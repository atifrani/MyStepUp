# Lab : Déployer une application Web serverless avec AWS Lambda et RDS PostgreSQL

## Objectif du laboratoire

Dans ce laboratoire, les étudiants vont apprendre à déployer une **application web serverless** utilisant :

* **AWS Lambda** (logique applicative)
* **Amazon RDS PostgreSQL** (base de données)
* **Amazon API Gateway** (exposition de l’application via HTTP)

À la fin du lab, l’application sera accessible via une **URL publique**.

# Architecture du laboratoire

```text
Navigateur Web
      |
API Gateway
      |
AWS Lambda (Python)
      |
RDS PostgreSQL
```

# Partie 1 : Création du rôle IAM pour Lambda

## Étape 1 : Accéder au service IAM

1. Connectez-vous à la **console AWS**
2. Sélectionnez la région :

```text
eu-west-3
```

3. Ouvrez le service **IAM**

---

## Étape 2 : Créer un rôle IAM

1. Cliquez sur **Roles**
2. Cliquez sur :

```text
Create role
```

3. Choisissez :

```text
Lambda
```

4. Cliquez sur **Next**

## Étape 3 : Ajouter les permissions

Sélectionnez les politiques suivantes :

```text
AWSLambdaBasicExecutionRole
AmazonRDSFullAccess
```

5. Cliquez sur **Next**

## Étape 4 : Finaliser le rôle

Nom du rôle :

```text
aws_lambda_rds
```

6. Cliquez sur **Create role**

# Partie 2 : Création de la fonction Lambda

## Étape 5 : Créer la fonction

1. Ouvrez le service **AWS Lambda**
2. Cliquez sur :

```text
Create function
```

3. Choisissez :

```text
Author from scratch
```

## Étape 6 : Configurer la fonction

* **Function name**

```text
lambda_formular
```

* **Runtime**

```text
Python 3.8 (ou version disponible)
```

* **Execution role**

```text
Use an existing role → aws_lambda_rds
```

4. Cliquez sur **Create function**

## Étape 7 : Déployer le code

1. Téléchargez le projet :

```text
https://github.com/atifrani/lambda_formular
```

2. Dans Lambda :

* Cliquez sur **Upload from**
* Choisissez **.zip file**
* Importez le projet

## Étape 8 : Ajouter les layers

1. Dans la section **Layers**, cliquez sur :

```text
Add a layer
```

2. Choisissez :

```text
Specify an ARN
```

3. Ajoutez les layers suivants :

```text
arn:aws:lambda:eu-west-1:898466741470:layer:psycopg2-py38:1
arn:aws:lambda:eu-west-1:580247275435:layer:LambdaInsightsExtension:33
```
Ces layers permettent :

* la connexion à PostgreSQL (psycopg2)
* le monitoring (Lambda Insights)

# Partie 3 : Création de la table PostgreSQL

## Étape 9 : Se connecter à la base RDS

Utilisez un outil comme :

```text
DBeaver
```

## Étape 10 : Créer la table

1. Ouvrez le fichier :

```text
contacts.sql
```

2. Exécutez le script SQL

## Étape 11 : Vérifier la table

Exécutez :

```sql
SELECT * FROM contacts;
```

## Étape 12 : Configurer la connexion dans Lambda

Dans le code Lambda, mettez à jour :

```text
Host
User
Password
Port (5432)
Database name
```

# Partie 4 : Création de l’API Gateway

**Amazon API Gateway** est un service AWS qui permet de **créer, publier, sécuriser et gérer des API** à grande échelle.

**Une API (Application Programming Interface)** est une interface qui permet à différentes applications de communiquer entre elles. **API Gateway** joue le rôle d’intermédiaire entre **les clients (navigateur, application mobile, IoT)** et **les services backend (comme AWS Lambda, EC2 ou RDS)**.

* **Rôle principal d’API Gateway**:

API Gateway permet de :

* recevoir des requêtes HTTP (GET, POST, etc.)
* les router vers des services backend (Lambda, EC2…)
* retourner une réponse au client

* **Fonctionnalités clés:**

* Gestion des API REST et HTTP
* Intégration avec AWS Lambda (serverless)
* Sécurisation des API (IAM, tokens, authentification)
* Gestion du trafic (throttling, limitation)
* Monitoring et logs via CloudWatch

## Étape 13 : Créer une API

1. Ouvrez **API Gateway**
2. Cliquez sur :

```text
Build
```

3. Choisissez :

```text
REST API
```

## Étape 14 : Configurer l’API

* **API Name**

```text
contacts
```

* **Endpoint type**

```text
Regional
```

Cliquez sur **Create API**

## Étape 15 : Créer une méthode GET

1. Cliquez sur **Actions → Create Method**
2. Choisissez :

```text
GET
```

3. Configuration :

* Integration type : Lambda
* Lambda function : `lambda_formular`
* Activer : **Lambda Proxy integration**

4. Cliquez sur **Save**

5. Autorisez l’accès à Lambda

## Étape 16 : Créer une méthode POST

Répétez les mêmes étapes pour :

```text
POST
```

## Étape 17 : Déployer l’API

1. Cliquez sur :

```text
Actions → Deploy API
```

2. Configurez :

* Stage :

```text
dev
```

3. Cliquez sur **Deploy**

## Étape 18 : Récupérer l’URL

Copiez l’URL :

```text
Invoke URL
```

Exemple :

```text
https://xxxx.execute-api.eu-west-3.amazonaws.com/dev
```

# Partie 5 : Tester l’application

## Étape 19 : Accéder à l’application

1. Ouvrez un navigateur
2. Accédez à :

```text
Invoke URL
```

## Résultat attendu

* une interface web s’affiche (formulaire)
* les données sont envoyées à Lambda
* Lambda écrit les données dans **RDS PostgreSQL**
* les données peuvent être consultées depuis la base

# Vérification

L’étudiant doit vérifier que :

* Lambda est correctement exécuté
* API Gateway fonctionne (GET/POST)
* la base RDS contient les données
* aucune erreur dans **CloudWatch Logs**

# Nettoyage des ressources

À la fin du laboratoire :

* supprimer la fonction **Lambda**
* supprimer l’API Gateway
* supprimer la base **RDS**
* supprimer le rôle IAM si non utilisé

# Compétences acquises

À la fin de ce laboratoire, l’étudiant est capable de :

* créer une **application serverless**
* connecter **Lambda à RDS PostgreSQL**
* exposer une API avec **API Gateway**
* comprendre une architecture moderne **event-driven et scalable**
