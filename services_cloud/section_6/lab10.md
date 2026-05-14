## Lab 10: AWS Lambda with S3 and RDS

Cette fois, au lieu de seulement afficher les 10 premières lignes du CSV dans les logs, la fonction **AWS Lambda** va aussi **charger le contenu du fichier dans une base Amazon RDS for PostgreSQL**. Amazon S3 peut déclencher Lambda sur un événement de création d’objet, et S3 invoque la fonction de manière asynchrone. Pour qu’une Lambda accède à une instance RDS, la connectivité réseau et les règles de security groups doivent être correctement configurées.

# Objectif du lab

À la fin du lab, vous aurez :

1. un bucket **S3** dans lequel on dépose un fichier CSV,
2. une fonction **Lambda Python** déclenchée par un **S3 Event**,
3. une base **Amazon RDS for PostgreSQL**,
4. une table cible dans PostgreSQL,
5. une Lambda qui :

   * lit le CSV depuis S3,
   * affiche les 10 premières lignes dans les logs,
   * insère les lignes dans une table PostgreSQL.

Ce scénario repose sur des notifications S3 de type **ObjectCreated**, qui sont délivrées **au moins une fois**, généralement en quelques secondes. Il faut donc écrire une logique idempotente ou prévoir des garde-fous si le même fichier peut être traité plusieurs fois. 

# Architecture

```text
Upload CSV dans S3
        ↓
S3 Event Notification (ObjectCreated)
        ↓
AWS Lambda
        ↓
Lecture du fichier CSV depuis S3
        ↓
Affichage des 10 premières lignes dans CloudWatch Logs
        ↓
Insertion des données dans Amazon RDS for PostgreSQL
```

# Prérequis

Vous devez disposer :

* des droits sur **S3**, **Lambda**, **IAM**, **RDS**, **VPC** et **CloudWatch Logs**,
* d’un fichier CSV de test,


Par défaut, une instance RDS bloque les connexions entrantes tant que ses security groups n’autorisent pas explicitement l’accès. ([docs.aws.amazon.com][3])

---

# Étape 1 — Créer le bucket S3

Dans la console AWS :

1. Ouvrez **Amazon S3**
2. Cliquez sur **Create bucket**
3. Donnez un nom unique, par exemple :
   `logbrain-csv-to-rds-123456`
4. Laissez les options par défaut
5. Cliquez sur **Create bucket**

Les notifications S3 peuvent ensuite être configurées pour déclencher une Lambda sur les événements de création d’objet. ([docs.aws.amazon.com][2])

---

# Étape 2 — Créer l’instance Amazon RDS PostgreSQL

Dans la console AWS :

1. Ouvrez **Amazon RDS**
2. Cliquez sur **Create database**
3. Choisissez **Standard create**
4. Engine type : **PostgreSQL**
5. DB instance identifier : `csvlab-postgres`
6. Master username : par exemple `postgres`
7. Définissez un mot de passe fort
8. Choisissez une classe de base adaptée au lab, par exemple une petite instance de test
9. Choisissez votre **VPC**
10. Laissez l’instance en **Private** de préférence
11. Créez ou sélectionnez un **security group**
12. Cliquez sur **Create database**

Notez ensuite :

* l’endpoint de la base,
* le port, généralement `5432`,
* le nom d’utilisateur,
* le mot de passe,
* le nom de la base, par exemple `postgres` ou `csvlab`.

Pour qu’une application se connecte à RDS, il faut que les règles réseau autorisent explicitement le trafic vers le port PostgreSQL. ([docs.aws.amazon.com][3])

---

# Étape 3 — Créer la table cible dans PostgreSQL

Connectez-vous à votre base avec un client PostgreSQL, comme `psql`, DBeaver ou pgAdmin.

Créez une table simple pour le lab :

```sql
CREATE TABLE IF NOT EXISTS imported_contacts (
    id SERIAL PRIMARY KEY,
    source_file TEXT NOT NULL,
    row_number INTEGER NOT NULL,
    col1 TEXT,
    col2 TEXT,
    col3 TEXT,
    col4 TEXT,
    col5 TEXT,
    inserted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Cette structure est volontairement générique pour simplifier le chargement de CSV. Si votre CSV a un schéma fixe, vous pouvez créer une table métier avec des colonnes nommées précisément.

---

# Étape 4 — Créer la fonction Lambda

Dans la console AWS :

1. Ouvrez **AWS Lambda**
2. Cliquez sur **Create function**
3. Choisissez **Author from scratch**
4. Nom : `s3-csv-to-rds`
5. Runtime : **Python 3.12** ou **Python 3.11**
6. Créez un rôle d’exécution ou utilisez-en un existant
7. Cliquez sur **Create function**

Comme la Lambda devra parler à RDS, elle devra être placée dans la **même VPC** que l’instance RDS, avec des security groups compatibles. AWS propose d’ailleurs une configuration assistée pour connecter Lambda et RDS, et recommande **RDS Proxy** pour les usages à forte concurrence. ([docs.aws.amazon.com][4])

---

# Étape 5 — Configurer la connectivité réseau entre Lambda et RDS

Dans la Lambda :

1. Ouvrez **Configuration**
2. Allez dans **VPC**
3. Cliquez sur **Edit**
4. Sélectionnez la **même VPC** que celle de RDS
5. Choisissez des subnets permettant à Lambda d’atteindre la base
6. Associez un security group à la Lambda

Ensuite, dans le security group de RDS, ajoutez une règle **inbound** :

* Type : PostgreSQL
* Port : `5432`
* Source : **le security group de la Lambda**

C’est le point réseau le plus important du lab. Sans cette règle, la Lambda ne pourra pas se connecter à PostgreSQL. Par défaut, RDS bloque toutes les connexions entrantes. ([docs.aws.amazon.com][3])

---

# Étape 6 — Ajouter les permissions IAM à la Lambda

Le rôle IAM de la Lambda doit pouvoir lire les objets S3. Ajoutez une policy du type :

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadCsvFromS3",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::logbrain-csv-to-rds-123456/*"
    }
  ]
}
```

Si la fonction écrit seulement dans CloudWatch Logs et lit S3, aucune permission IAM spéciale n’est requise pour PostgreSQL, car la connexion à RDS se fait via le réseau et les identifiants de base de données, pas via une API RDS pour les requêtes SQL. Pour S3, AWS indique bien que le rôle d’exécution doit autoriser les actions S3 nécessaires. ([docs.aws.amazon.com][1])

---

# Étape 7 — Définir les variables d’environnement de la Lambda

Dans **Configuration > Environment variables**, ajoutez :

```text
DB_HOST=<endpoint RDS>
DB_PORT=5432
DB_NAME=postgres
DB_USER=postgres
DB_PASSWORD=<mot de passe>
DB_TABLE=imported_contacts
```

Pour un vrai projet, il vaut mieux stocker le mot de passe dans **AWS Secrets Manager** plutôt qu’en variable d’environnement, mais pour un lab simple, cette approche est acceptable.

---

# Étape 8 — Ajouter la dépendance PostgreSQL à la Lambda

Le runtime Python de Lambda n’inclut pas `psycopg2` par défaut. AWS précise que les dépendances Python doivent être intégrées au package `.zip` ou fournies via une **Lambda layer**. ([docs.aws.amazon.com][5])

Pour ce lab, vous avez deux choix :

## Option simple pour le lab

Créer un package ZIP contenant :
```
mkdir package
 
pip install \
  --platform manylinux2014_x86_64 \
  --target=package \
  --implementation cp \
  --python-version 3.13 \
  --only-binary=:all: \
  psycopg2-binary

  cd .\package\  

  zip -r ../load-s3-rds.zip .      
```
## Option plus propre

Créer une **Lambda layer** contenant la dépendance PostgreSQL, puis l’attacher à la fonction.

Pour rester simple ici, on va supposer que vous empaquetez la dépendance dans le ZIP de déploiement.

---

# Étape 9 — Code Python de la Lambda

Voici une version complète qui :

* lit le CSV depuis S3,
* affiche les 10 premières lignes,
* insère chaque ligne dans PostgreSQL.

```python
import os
import io
import csv
import boto3
import urllib.parse
import psycopg2

s3 = boto3.client("s3")

DB_HOST = os.environ["DB_HOST"]
DB_PORT = int(os.environ.get("DB_PORT", "5432"))
DB_NAME = os.environ["DB_NAME"]
DB_USER = os.environ["DB_USER"]
DB_PASSWORD = os.environ["DB_PASSWORD"]
DB_TABLE = os.environ.get("DB_TABLE", "imported_contacts")

def get_connection():
    return psycopg2.connect(
        host=DB_HOST,
        port=DB_PORT,
        dbname=DB_NAME,
        user=DB_USER,
        password=DB_PASSWORD
    )

def lambda_handler(event, context):
    print("Event reçu :", event)

    conn = None
    try:
        conn = get_connection()
        conn.autocommit = False

        with conn.cursor() as cur:
            for record in event.get("Records", []):
                bucket_name = record["s3"]["bucket"]["name"]
                object_key = urllib.parse.unquote_plus(record["s3"]["object"]["key"])

                print(f"Bucket : {bucket_name}")
                print(f"Objet  : {object_key}")

                if not object_key.lower().endswith(".csv"):
                    print("Fichier ignoré : ce n'est pas un CSV.")
                    continue

                response = s3.get_object(Bucket=bucket_name, Key=object_key)
                file_content = response["Body"].read().decode("utf-8")

                csv_buffer = io.StringIO(file_content)
                reader = csv.reader(csv_buffer)

                print("=== 10 premières lignes du CSV ===")
                rows_to_insert = []

                for i, row in enumerate(reader):
                    if i < 10:
                        print(f"Ligne {i + 1}: {row}")

                    # On normalise à 5 colonnes max pour la table du lab
                    normalized = row[:5] + [None] * (5 - len(row[:5]))

                    rows_to_insert.append((
                        object_key,
                        i + 1,
                        normalized[0],
                        normalized[1],
                        normalized[2],
                        normalized[3],
                        normalized[4],
                    ))

                insert_sql = f"""
                    INSERT INTO {DB_TABLE}
                    (source_file, row_number, col1, col2, col3, col4, col5)
                    VALUES (%s, %s, %s, %s, %s, %s, %s)
                """

                cur.executemany(insert_sql, rows_to_insert)
                print(f"{len(rows_to_insert)} lignes insérées dans {DB_TABLE}")

        conn.commit()

        return {
            "statusCode": 200,
            "body": "Traitement terminé avec succès."
        }

    except Exception as e:
        if conn:
            conn.rollback()
        print(f"Erreur : {str(e)}")
        raise

    finally:
        if conn:
            conn.close()
```

Ce code est adapté à un CSV générique. Pour un schéma métier, il est préférable de mapper explicitement les colonnes du CSV vers les colonnes PostgreSQL.

---

# Étape 10 — Créer le package ZIP de déploiement

Sur une machine Linux ou macOS, dans un dossier vide :

```bash
mkdir lambda-csv-rds
cd lambda-csv-rds
pip install psycopg2-binary -t .
```

Créez ensuite le fichier `lambda_function.py` avec le code ci-dessus, puis :

```bash
zip -r lambda-csv-rds.zip .
```

AWS indique que, pour une Lambda Python déployée en `.zip`, le fichier du handler et les dépendances doivent se trouver à la racine du package, ou être fournis via une layer. ([docs.aws.amazon.com][5])

Chargez ensuite ce ZIP dans votre fonction Lambda via **Upload from > .zip file**.

---

# Étape 11 — Configurer le déclencheur S3

Dans la console Lambda :

1. Ouvrez la fonction `s3-csv-to-rds`
2. Cliquez sur **Add trigger**
3. Choisissez **S3**
4. Sélectionnez votre bucket
5. Event type : **All object create events**
6. Ajoutez un suffixe `.csv`
7. Validez

Amazon S3 invoque Lambda de manière asynchrone et transmet dans l’événement les informations sur le bucket et l’objet créé. AWS recommande aussi d’éviter une boucle infinie si la fonction écrit dans le même bucket déclencheur ; ici, la Lambda lit dans S3 et écrit dans RDS, donc ce risque n’existe pas. ([docs.aws.amazon.com][1])

---

# Étape 12 — Créer un fichier CSV de test

Créez localement un fichier `test.csv` :

```csv
prenom,nom,ville,email,entreprise
Axel,Tifrani,Paris,axel@example.com,Logbrain
Sarah,Martin,Lyon,sarah@example.com,DataCorp
Mehdi,Ali,Marseille,mehdi@example.com,CloudOps
Lea,Durand,Toulouse,lea@example.com,InsightAI
Nora,Petit,Nice,nora@example.com,BlueScale
Hugo,Robert,Nantes,hugo@example.com,Quantix
Ines,Moreau,Lille,ines@example.com,StackFlow
Adam,Garcia,Bordeaux,adam@example.com,VisionData
Emma,Roux,Strasbourg,emma@example.com,CoreBI
Yanis,Faure,Montpellier,yanis@example.com,MLHub
Lina,Andre,Rennes,lina@example.com,NorthGrid
Zoe,Lambert,Reims,zoe@example.com,NextLake
```

---

# Étape 13 — Uploader le CSV dans S3

Dans le bucket S3 :

1. Cliquez sur **Upload**
2. Sélectionnez `test.csv`
3. Cliquez sur **Upload**

L’événement **ObjectCreated** va déclencher la Lambda. Les notifications S3 sont délivrées au moins une fois, donc dans un environnement réel il faut prévoir la possibilité d’un retraitement. ([docs.aws.amazon.com][2])

---

# Étape 14 — Vérifier les logs CloudWatch

Dans Lambda :

1. Ouvrez l’onglet **Monitor**
2. Cliquez sur **View CloudWatch logs**
3. Ouvrez le log stream le plus récent

Vous devez voir :

* le bucket,
* la clé de l’objet,
* les 10 premières lignes du CSV,
* le nombre de lignes insérées.

---

# Étape 15 — Vérifier les données dans PostgreSQL

Reconnectez-vous à PostgreSQL et exécutez :

```sql
SELECT *
FROM imported_contacts
ORDER BY id DESC
LIMIT 20;
```

Vous devriez retrouver les lignes du CSV insérées dans la table.

Vous pouvez aussi compter :

```sql
SELECT COUNT(*) FROM imported_contacts;
```

---

# Dépannage

## La Lambda est déclenchée, mais rien n’est inséré

Le problème est souvent réseau : la Lambda n’est pas dans la bonne VPC, ou le security group RDS n’autorise pas le security group Lambda sur le port 5432. RDS bloque par défaut les connexions entrantes tant qu’une règle explicite n’existe pas. ([docs.aws.amazon.com][3])

## Erreur `No module named psycopg2`

La dépendance n’est pas dans le ZIP ou la layer. Lambda n’inclut pas `psycopg2` par défaut ; il faut empaqueter la dépendance ou utiliser une layer. ([docs.aws.amazon.com][5])

## Erreur sur le mot de passe ou l’endpoint

Vérifiez les variables d’environnement `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`.

## Le fichier est traité plusieurs fois

Les notifications S3 sont livrées au moins une fois. En production, prévoyez une table de suivi des fichiers traités, une contrainte d’unicité, ou un mécanisme d’idempotence. ([docs.aws.amazon.com][2])

---

# Améliorations recommandées pour une version plus proche de la production

Pour un lab, la connexion directe à RDS est acceptable. En production, AWS recommande souvent **RDS Proxy** pour les fonctions Lambda qui ouvrent beaucoup de connexions courtes. Cela réduit le risque d’épuiser les connexions PostgreSQL. Il est aussi préférable d’utiliser **Secrets Manager** pour les identifiants de base de données plutôt que des variables d’environnement en clair. ([docs.aws.amazon.com][6])

---

# Résumé

Dans ce lab, vous avez :

1. créé un bucket S3,
2. créé une base RDS PostgreSQL,
3. créé une table cible,
4. créé une Lambda Python,
5. configuré la VPC et les security groups,
6. ajouté la dépendance PostgreSQL,
7. connecté S3 comme déclencheur,
8. chargé un CSV dans S3,
9. affiché les 10 premières lignes dans les logs,
10. inséré les données dans PostgreSQL.