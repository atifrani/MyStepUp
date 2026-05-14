# Lab : Déployer une application web serverless avec AWS Lambda, DynamoDB et API Gateway

## Objectif du laboratoire

Dans ce laboratoire, les étudiants vont apprendre à déployer une **application web serverless complète** en utilisant :

* **AWS Lambda** (logique applicative)
* **Amazon DynamoDB** (base de données NoSQL)
* **Amazon API Gateway** (exposition de l’application via HTTP)

À la fin du laboratoire, l’application sera accessible via une **URL publique**.

# Architecture du laboratoire

```text
Navigateur Web
      |
API Gateway
      |
AWS Lambda (Python)
      |
DynamoDB
```

# Partie 1 : Création du rôle IAM pour Lambda

## Étape 1 : Accéder à IAM

1. Connectez-vous à la **console AWS**
2. Sélectionnez la région :

```text
eu-west-3
```

3. Ouvrez le service **IAM**

## Étape 2 : Créer un rôle

1. Cliquez sur **Roles**
2. Cliquez sur :

```text
Create role
```

3. Sélectionnez :

```text
Lambda
```

4. Cliquez sur **Next**

---

## Étape 3 : Ajouter les permissions

Sélectionnez la politique suivante :

```text
AWSLambdaBasicExecutionRole
AmazonDynamoDBFullAccess
```

5. Cliquez sur **Next**

## Étape 4 : Finaliser le rôle

Nom du rôle :

```text
dynamolambdarole
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
formularwebapp
```

* **Runtime**

```text
Python 3.9
```

* **Execution role**

```text
Use existing role → dynamolambdarole
```

4. Cliquez sur **Create function**

## Étape 7 : Déployer le code

1. Téléchargez le projet :

```text
https://github.com/atifrani/lambda_dynamodb_webapp
```

2. Dans Lambda :

* Cliquez sur **Upload from**
* Choisissez **.zip file**
* Importez le fichier

La fonction Lambda contient :

* la logique métier
* l’interface HTML de l’application

# Partie 3 : Création de la table DynamoDB

## Étape 8 : Accéder à DynamoDB

1. Ouvrez le service :

```text
DynamoDB
```

2. Cliquez sur **Tables → Create table**

## Étape 9 : Configurer la table

* **Table name**

```text
formular
```

* **Partition key**

```text
email (String)
```

👉 Cette clé identifie de manière unique chaque enregistrement.

3. Laissez les paramètres par défaut (Free Tier compatible)
4. Cliquez sur **Create table**

# Partie 4 : Création de l’API Gateway

## Étape 10 : Créer une API

1. Ouvrez **API Gateway**
2. Cliquez sur :

```text
Build
```

3. Sélectionnez :

```text
REST API
```

## Étape 11 : Configurer l’API

* **API Name**

```text
dynamowebapi
```

* **Endpoint type**

```text
Regional
```

Cliquez sur **Create API**

## Étape 12 : Créer une méthode GET

1. Cliquez sur **Actions → Create Method**
2. Sélectionnez :

```text
GET
```

3. Configuration :

* Integration type : Lambda
* Activer : **Lambda Proxy integration**
* Lambda function : `squarewebapp`

4. Cliquez sur **Save**

5. Autorisez API Gateway à appeler Lambda

## Étape 13 : Créer une méthode POST

Répétez les mêmes étapes pour :

```text
POST
```

## Étape 14 : Déployer l’API

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

## Étape 15 : Récupérer l’URL

Copiez l’URL :

```text
Invoke URL
```

Exemple :

```text
https://xxxx.execute-api.eu-west-3.amazonaws.com/dev
```

# Partie 5 : Tester l’application

## Étape 16 : Accéder à l’application

1. Ouvrez un navigateur
2. Accédez à :

```text
Invoke URL
```

## Résultat attendu

* une interface web (formulaire) s’affiche
* les données saisies sont envoyées à **Lambda**
* Lambda enregistre les données dans **DynamoDB**
* les données sont persistées sans serveur

# Vérification

L’étudiant doit vérifier que :

* Lambda s’exécute correctement
* DynamoDB contient les données
* API Gateway répond correctement
* aucun message d’erreur dans **CloudWatch Logs**

# Nettoyage des ressources

À la fin du laboratoire :

* supprimer la fonction **Lambda**
* supprimer l’API Gateway
* supprimer la table **DynamoDB**
* supprimer le rôle IAM si nécessaire

# Compétences acquises

À la fin de ce laboratoire, les étudiants sont capables de :

* créer une application **serverless complète**
* utiliser **DynamoDB comme base NoSQL**
* connecter Lambda à DynamoDB
* exposer une API via **API Gateway**
* comprendre une architecture moderne **cloud-native et scalable**
