## Lab 09: AWS Lambda with S3

Voici un **lab pas à pas** pour créer une fonction **AWS Lambda** déclenchée par un **événement S3**, qui lit un fichier CSV déposé dans un bucket et affiche les **10 premières lignes** dans les logs CloudWatch.

AWS S3 peut déclencher une fonction Lambda sur des événements de création d’objet, et S3 invoque Lambda de façon asynchrone avec les détails du bucket et de l’objet dans l’événement. AWS recommande aussi d’éviter les boucles si la fonction écrit dans le même bucket que celui qui la déclenche. ([docs.aws.amazon.com][1])

# Objectif du lab

À la fin du lab, vous aurez :

* un bucket **Amazon S3**
* une fonction **AWS Lambda** en Python
* un déclencheur **S3 Event Notification**
* un test réel par dépôt d’un fichier `.csv`
* l’affichage des **10 premières lignes** dans **CloudWatch Logs**

# Architecture

```text
Utilisateur -> Upload d’un fichier CSV dans S3
S3 -> déclenche un événement ObjectCreated
Lambda -> lit le fichier CSV depuis S3
Lambda -> écrit les 10 premières lignes dans CloudWatch Logs
```

# Prérequis

Il vous faut :

* les droits pour utiliser **S3**, **Lambda**, **IAM** et **CloudWatch Logs**
* un fichier CSV de test, par exemple :

```csv
id,nom,ville
1,Axel,Paris
2,Sarah,Lyon
3,Mehdi,Marseille
4,Lea,Toulouse
5,Nora,Nice
6,Hugo,Nantes
7,Ines,Lille
8,Adam,Bordeaux
9,Emma,Strasbourg
10,Yanis,Montpellier
11,Lina,Rennes
12,Zoe,Reims
```

# Étape 1 — Créer le bucket S3

Dans la console AWS :

1. Ouvrez **Amazon S3**
2. Cliquez sur **Create bucket**
3. Donnez un nom unique, par exemple :

   * `lab-09-esme-<prenom>`
4. Laissez les options par défaut
5. Cliquez sur **Create bucket**

Les notifications S3 peuvent être configurées pour des événements comme **ObjectCreated**, et S3 peut envoyer ces notifications à Lambda. ([docs.aws.amazon.com][2])

# Étape 2 — Créer la fonction Lambda

Dans la console AWS :

1. Ouvrez **AWS Lambda**
2. Cliquez sur **Create function**
3. Choisissez **Author from scratch**
4. Nom :

   * `read-csv-top10`
5. Runtime :

   * **Python 3.13** si disponible, sinon Python 3.12
6. Créez une nouvelle exécution role ou utilisez-en une existante
7. Cliquez sur **Create function**

# Étape 3 — Ajouter les permissions S3 à Lambda

Pour que Lambda lise l’objet S3, son **execution role** doit avoir des permissions S3, car AWS précise que si la fonction utilise le SDK AWS pour gérer des ressources S3, elle a besoin des permissions S3 dans son rôle d’exécution. ([docs.aws.amazon.com][1])

Dans le rôle IAM associé à la fonction, ajoutez une policy comme celle-ci en remplaçant le nom du bucket :

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
      "Resource": "arn:aws:s3:::lab-09-esme-*/*"
    }
  ]
}
```

Vous pouvez aussi ajouter `s3:ListBucket` si vous le souhaitez, même si ce lab n’en a pas besoin.

# Étape 4 — Coller le code Lambda

Dans l’éditeur de code de la fonction Lambda, remplacez le contenu par ce code :

```python
import boto3
import csv
import io
import urllib.parse

s3 = boto3.client("s3")

def lambda_handler(event, context):
    print("Event reçu :", event)

    # Un événement S3 contient généralement une liste Records
    record = event["Records"][0]
    bucket_name = record["s3"]["bucket"]["name"]
    object_key = urllib.parse.unquote_plus(record["s3"]["object"]["key"])

    print(f"Bucket : {bucket_name}")
    print(f"Objet  : {object_key}")

    # Lecture de l'objet S3
    response = s3.get_object(Bucket=bucket_name, Key=object_key)
    file_content = response["Body"].read().decode("utf-8")

    # Lecture CSV
    csv_buffer = io.StringIO(file_content)
    reader = csv.reader(csv_buffer)

    # Afficher les 10 premières lignes
    print("=== 10 premières lignes du CSV ===")
    for i, row in enumerate(reader):
        if i >= 10:
            break
        print(f"Ligne {i + 1}: {row}")

    return {
        "statusCode": 200,
        "body": f"Lecture des 10 premières lignes de {object_key} terminée."
    }
```

### Ce que fait ce code

* récupère le nom du bucket et la clé de l’objet depuis l’événement S3
* lit le fichier avec `boto3`
* parse le contenu avec le module standard `csv`
* affiche les 10 premières lignes dans les logs

# Étape 5 — Configurer le déclencheur S3

AWS indique que pour qu’un bucket S3 invoque une fonction Lambda, il faut configurer une notification d’événement sur le bucket et autoriser S3 à invoquer la fonction. Quand on configure le trigger via la console Lambda, la console met à jour la policy de la fonction pour permettre à S3 de l’appeler. ([docs.aws.amazon.com][1])

Dans Lambda :

1. Ouvrez votre fonction `read-csv-top10`
2. Cliquez sur **Add trigger**
3. Choisissez **S3**
4. Sélectionnez le bucket créé
5. Event type :

   * **All object create events**
6. Ajoutez éventuellement un suffixe :

   * `.csv`
7. Cochez l’avertissement
8. Cliquez sur **Add**

Le filtre suffixe `.csv` permet de ne traiter que les fichiers CSV.

# Étape 6 — Vérifier le risque de boucle

AWS recommande d’éviter qu’une fonction Lambda écrive dans le même bucket qui la déclenche, sinon cela peut créer une boucle d’exécution. ([docs.aws.amazon.com][1])

Dans ce lab :

* la Lambda **lit seulement** le fichier
* elle **n’écrit rien dans S3**

Donc il n’y a pas de boucle.

# Étape 7 — Créer un fichier CSV de test

Créez un fichier local `etudiants.csv` :

```csv
id,nom,ville
1,Axel,Paris
2,Sarah,Lyon
3,Mehdi,Marseille
4,Lea,Toulouse
5,Nora,Nice
6,Hugo,Nantes
7,Ines,Lille
8,Adam,Bordeaux
9,Emma,Strasbourg
10,Yanis,Montpellier
11,Lina,Rennes
12,Zoe,Reims
```

# Étape 8 — Déposer le fichier dans S3

Dans le bucket S3 :

1. Cliquez sur **Upload**
2. Sélectionnez `etudiants.csv`
3. Cliquez sur **Upload**

Comme l’événement S3 est de type **ObjectCreated**, l’upload déclenchera la fonction Lambda. Les notifications S3 sont conçues pour être délivrées **au moins une fois**, et sont généralement envoyées en quelques secondes, parfois un peu plus. ([docs.aws.amazon.com][2])

---

# Étape 9 — Consulter les logs CloudWatch

Dans Lambda :

1. Ouvrez votre fonction
2. Allez dans l’onglet **Monitor**
3. Cliquez sur **View CloudWatch logs**
4. Ouvrez le log stream le plus récent

Vous devriez voir quelque chose comme :

```text
Event reçu : {...}
Bucket : logbrain-csv-lab-123456
Objet  : test.csv
=== 10 premières lignes du CSV ===
Ligne 1: ['id', 'nom', 'ville']
Ligne 2: ['1', 'Axel', 'Paris']
Ligne 3: ['2', 'Sarah', 'Lyon']
Ligne 4: ['3', 'Mehdi', 'Marseille']
Ligne 5: ['4', 'Lea', 'Toulouse']
Ligne 6: ['5', 'Nora', 'Nice']
Ligne 7: ['6', 'Hugo', 'Nantes']
Ligne 8: ['7', 'Ines', 'Lille']
Ligne 9: ['8', 'Adam', 'Bordeaux']
Ligne 10: ['9', 'Emma', 'Strasbourg']
```

# Étape 10 — Vérifications utiles

Si rien ne se passe, vérifiez :

### 1. Le trigger S3 est bien attaché

Dans la fonction Lambda, l’onglet **Configuration** doit montrer le déclencheur S3.

### 2. La permission `s3:GetObject` existe

Sans cette permission, Lambda sera invoquée mais ne pourra pas lire le fichier.

### 3. Le suffixe `.csv` correspond bien

Si vous avez mis un filtre suffixe `.csv`, un fichier `etudiants.txt` ne déclenchera pas la Lambda.

### 4. Le fichier est encodé en UTF-8

Le code suppose un décodage UTF-8.

# Variante plus robuste

Voici une version un peu plus solide, qui :

* ignore les fichiers non CSV
* gère plusieurs enregistrements dans l’événement
* affiche proprement les erreurs

```python
import boto3
import csv
import io
import urllib.parse

s3 = boto3.client("s3")

def lambda_handler(event, context):
    print("Event reçu :", event)

    for record in event.get("Records", []):
        bucket_name = record["s3"]["bucket"]["name"]
        object_key = urllib.parse.unquote_plus(record["s3"]["object"]["key"])

        print(f"Bucket : {bucket_name}")
        print(f"Objet  : {object_key}")

        if not object_key.lower().endswith(".csv"):
            print("Fichier ignoré : ce n'est pas un CSV.")
            continue

        try:
            response = s3.get_object(Bucket=bucket_name, Key=object_key)
            file_content = response["Body"].read().decode("utf-8")

            csv_buffer = io.StringIO(file_content)
            reader = csv.reader(csv_buffer)

            print("=== 10 premières lignes du CSV ===")
            for i, row in enumerate(reader):
                if i >= 10:
                    break
                print(f"Ligne {i + 1}: {row}")

        except Exception as e:
            print(f"Erreur lors du traitement de {object_key}: {str(e)}")

    return {
        "statusCode": 200,
        "body": "Traitement terminé."
    }
```
# Résultat attendu

À la fin du lab :

* l’upload d’un `.csv` dans S3 déclenche la fonction
* Lambda lit le fichier depuis S3
* les **10 premières lignes** apparaissent dans **CloudWatch Logs**

# Nettoyage

Pour éviter les coûts inutiles, supprimez :

* la fonction Lambda `read-csv-top10`
* le bucket S3 et son contenu
* les éventuelles policies IAM spécifiques si elles ne servent plus

# Résumé rapide

1. créer un bucket S3
2. créer une Lambda Python
3. donner `s3:GetObject` à la Lambda
4. ajouter un trigger S3 `ObjectCreated`
5. uploader un fichier CSV
6. lire les logs CloudWatch
7. vérifier l’affichage des 10 premières lignes