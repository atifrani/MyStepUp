# Lab 08 : Déployer une application Web sur EC2 connectée à une base de données RDS PostgreSQL

## Objectif du laboratoire

Dans ce laboratoire, les étudiants vont apprendre à **déployer une application web sur une instance EC2** qui se connecte à une **base de données Amazon RDS PostgreSQL**.

Ce laboratoire permet de mettre en pratique plusieurs services AWS étudiés dans le cours :

* **Amazon EC2**
* **Amazon RDS**
* **VPC et Security Groups**
* **Application web PHP**
* **Connexion à une base de données PostgreSQL**

# Architecture du laboratoire

L’architecture déployée sera la suivante :

```
Utilisateur (Navigateur)
        |
        |
   Internet
        |
      EC2
 (Apache + PHP)
        |
        |
     RDS PostgreSQL
```

L’application web est hébergée sur **EC2**, tandis que les données sont stockées dans **Amazon RDS PostgreSQL**.

# Partie 1 : Créer une instance EC2 (Free Tier)

## Étape 1 : Créer une instance EC2

1. Connectez-vous à la **console AWS**
2. Ouvrez le service :

```
EC2
```

3. Cliquez sur :

```
Launch instance
```

## Étape 2 : Configuration de l’instance

Configurer les paramètres suivants :

**Nom de l’instance**

```
lab-webapp-ec2
```

**AMI**

Choisir une AMI **Free Tier**, par exemple :

```
Amazon Linux 2023
```

**Type d’instance**

```
t2.micro ou t3.micro
```

(compatible Free Tier)

**Région**

```
eu-west-3
```

---

## Étape 3 : Configuration du Security Group

Créer un **Security Group** avec les règles suivantes :

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | My IP     |
| HTTP | 80   | 0.0.0.0/0 |

Ces règles permettent :

* la connexion SSH
* l’accès web à l’application

## Étape 4 : Connectez vous à l'EC2

Depuis l'instance, cliquez sur **Connet**

Selectionnez l'onglet **EC2 Instance Connect**

Cliquez sur **Connect**

# Partie 2 : Installer les composants sur EC2

## Étape 5 : Installer Apache

```
sudo yum install httpd -y
```

Démarrer le serveur web :

```
sudo service httpd start
```

## Étape 6 : Installer PHP

Installer PHP :

```
sudo yum install php php-pgsql -y
```

Vérifier l’installation :

```
php -v
```

# Partie 3 : Cloner le code de l’application

## Étape 7 : Installer Git

```
sudo yum install git -y
```

## Étape 9 : Cloner le projet

Cloner le dépôt GitHub :

```
git clone https://github.com/atifrani/ec2_rds_webapp.git
```

# Partie 4 : Créer une base de données RDS PostgreSQL

Cette partie est optionnelle si vous avez réalisé le [**lab07**](https://github.com/atifrani/MyStepUp/blob/main/services_cloud/section_5/lab07.md)

## Étape 10 : Créer une instance RDS

Dans la console AWS :

1. Ouvrir le service :

```
Amazon RDS
```

2. Cliquer sur :

```
Create database
```

## Étape 11 : Configuration de la base

Choisir les paramètres suivants :

**Engine**

```
PostgreSQL
```

**Template**

```
Free Tier
```

**DB instance identifier**

```
lab-postgres-db
```

**Master username**

```
postgres
```

**Password**

Choisir un mot de passe sécurisé.

## Étape 12 : Configuration réseau

Configurer :

* **Region**

```
eu-west-3
```

* **Public access**

```
Yes
```

Configurer le **Security Group** pour autoriser le port :

```
5432
```

depuis l’instance EC2.

# Partie 5 : Initialiser la base de données

## Étape 13 : Se connecter à la base PostgreSQL

Depuis EC2 :

```
psql -h ENDPOINT-RDS -U postgres -d postgres
```

Remplacer **ENDPOINT-RDS** par l’endpoint de votre base.

## Étape 14 : Exécuter le script SQL

Cette partie est optionnelle si vous avez réalisé le [**lab07**](https://github.com/atifrani/MyStepUp/blob/main/services_cloud/section_5/lab07.md)

Dans le projet cloné, exécuter le script :

```
employees.sql
```

Par exemple :

```
psql -h ENDPOINT-RDS -U postgres -d postgres -f employees.sql
```

Cela crée la table **employees** et les données initiales.

---

# Partie 6 : Configurer la connexion à la base

## Étape 15 : Modifier les fichiers de configuration

Sur EC2, modifier les fichiers suivants :

```
insert.php
find.php
```

Mettre à jour les informations de connexion :

```
Host
User
Password
Port
Database name
```

Exemple :

```
host = endpoint-rds
user = postgres
password = yourpassword
port = 5432
database = postgres
```

# Partie 7 : Déployer l’application

## Étape 16 : Copier l’application dans Apache

Copier le répertoire **app** dans le dossier du serveur web :

```
sudo mv ec2_rds_webapp/app /var/www/html
```

---

## Étape 17 : Redémarrer Apache

```
sudo systemctl restart httpd
```

# Partie 8 : Tester l’application

## Étape 18 : Accéder à l’application

Dans votre navigateur :

```
http://PUBLIC-IP/app/insert.php
```

Cette page permet d’insérer un employé dans la base de données.

Pour rechercher un employé :

```
http://PUBLIC-IP/app/find.php
```

# Résultat attendu

Si la configuration est correcte :

* l’application web est accessible depuis le navigateur
* les données sont stockées dans **Amazon RDS PostgreSQL**
* EC2 communique correctement avec la base de données

# Vérification

L’étudiant doit vérifier que :

* l’instance **EC2 est accessible**
* Apache fonctionne
* la base **RDS PostgreSQL est disponible**
* le port **5432 est ouvert entre EC2 et RDS**
* les données sont bien insérées dans la table **employees**

# Nettoyage des ressources

À la fin du laboratoire :

1. **Terminer l’instance EC2**
2. **Supprimer la base de données RDS**

Cela permet d’éviter toute consommation de ressources AWS.
