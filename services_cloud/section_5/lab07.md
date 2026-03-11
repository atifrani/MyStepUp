# Lab : Lancer une base de données Amazon RDS PostgreSQL Free Tier depuis la console AWS

## Objectif du laboratoire

Dans ce laboratoire, les étudiants vont apprendre à **créer une base de données relationnelle PostgreSQL avec Amazon RDS (Free Tier)** à partir de la **console AWS**.

À la fin du laboratoire, l’étudiant sera capable de :

* accéder au service **Amazon RDS**
* choisir le moteur **PostgreSQL**
* utiliser la configuration **Free Tier**
* configurer l’instance RDS
* lancer la base de données
* récupérer les informations de connexion

# Partie 1 : Accéder au service RDS

## Étape 1 : Ouvrir Amazon RDS

1. Connectez-vous à la **console AWS**
2. Dans la barre de recherche des services, tapez :

```
RDS
```

3. Cliquez sur **Amazon RDS**

# Partie 2 : Créer une base de données

## Étape 2 : Démarrer la création

Dans le tableau de bord RDS :

1. Cliquez sur :

```
Create database
```

## Étape 3 : Choisir la méthode de création

Dans **Choose a database creation method**, sélectionnez :

```
Standard create
```

Cette option permet d’avoir accès à tous les paramètres de configuration.


## Étape 4 : Choisir le moteur PostgreSQL

Dans **Engine options**, sélectionnez :

```
PostgreSQL
```

PostgreSQL est une base de données relationnelle **open source robuste et très utilisée dans les applications modernes**.


## Étape 5 : Choisir le template Free Tier

Dans la section **Templates**, sélectionnez :

```
Free tier
```

Cela permet d’utiliser une configuration adaptée aux laboratoires.


# Partie 3 : Configuration de l’instance

## Étape 6 : Définir l’identifiant de la base

Dans **DB instance identifier**, entrez par exemple :

```
lab-rds-postgres
```

## Étape 7 : Définir les identifiants administrateur

Dans **Credentials Settings** :

**Master username**

```
postgres
```

**Master password**

Choisissez un mot de passe sécurisé.

Confirmez le mot de passe dans **Confirm password**.


## Étape 8 : Vérifier la classe d’instance

Dans **DB instance class**, vérifiez que l’instance sélectionnée est :

```
db.t3.micro
```

Cette instance est **éligible au Free Tier AWS**.

## Étape 9 : Vérifier le stockage

Dans **Storage**, laissez les paramètres par défaut :

* **Storage type** : General Purpose SSD
* **Allocated storage** : 20 GiB

Cette configuration est incluse dans le Free Tier.


# Partie 4 : Configuration réseau

## Étape 10 : Choisir le VPC

Dans **Connectivity**, sélectionnez votre **VPC**.

Exemple :

```
Default VPC
```

## Étape 11 : Accès public

Dans **Public access**, choisissez selon le contexte du laboratoire :

Pour tester facilement :

```
Yes
```

Pour une configuration plus sécurisée (recommandée en production) :

```
No
```

## Étape 12 : Configurer le Security Group

Dans **VPC security group**, choisissez un **Security Group** autorisant le port PostgreSQL :

```
5432
```

Exemple :

* autoriser l’accès depuis l’instance EC2
* ou depuis l’adresse IP de l’étudiant

## Étape 13 : Choisir la zone de disponibilité

Laissez la configuration par défaut ou choisissez une **Availability Zone**.

# Partie 5 : Configuration supplémentaire

## Étape 14 : Nom initial de la base

Dans **Additional configuration**, définissez le nom de la base :

```
studentdb
```

## Étape 15 : Paramètres supplémentaires

Pour un laboratoire simple :

* laissez **Multi-AZ** désactivé
* conservez les paramètres par défaut
* laissez les **backups automatiques** activés

# Partie 6 : Lancer la base de données

## Étape 16 : Créer la base

Cliquez sur :

```
Create database
```

AWS commence la création de l’instance PostgreSQL.

La création peut prendre **5 à 10 minutes**.

# Partie 7 : Vérifier l’état de la base

## Étape 17 : Attendre que la base soit disponible

Dans la liste des bases RDS, vérifiez que le statut devient :

```
Available
```

## Étape 18 : Récupérer les informations de connexion

Cliquez sur votre base de données et notez les informations suivantes :

* **Endpoint**
* **Port**
* **DB name**
* **Master username**

Exemple :

```
Endpoint : lab-rds-postgres.xxxxxx.eu-west-1.rds.amazonaws.com
Port : 5432
```

Ces informations seront utilisées pour se connecter à la base.

# Résultat attendu

À la fin du laboratoire, l’étudiant doit disposer :

* d’une base **Amazon RDS PostgreSQL**
* d’une configuration **Free Tier**
* d’un **endpoint de connexion**
* d’une base prête à être utilisée par une application

# Vérification

L’étudiant doit vérifier que :

* le moteur est **PostgreSQL**
* la configuration est **Free Tier**
* l’instance est **db.t3.micro**
* le port **5432** est autorisé
* le statut de la base est **Available**


# Étape 19 : Créer la table employee

Dans le terminal PostgreSQL, exécutez la commande suivante :

```sql
create table if not exists employee 
(
    id varchar(256),
    fname varchar(256),
    lname varchar(256),
    gender varchar(256),
    age integer,
    location varchar(256)
)
```

Cette table contient :

* un **identifiant**
* un **nom d’employé**
* un **prenom d’employé**
* un **sexe de l'employé**
* un **age de l'employé**
* un **adresse de l'employé**

# Étape 20 : Insérer des données dans la table

Insérez quelques employés dans la table :

```sql
INSERT INTO employee (id, fname,lname, sexe, age, location)
VALUES ('1F001', 'Alice', 'Dupont','F', 40, 'CA');

INSERT INTO employee (id, fname,lname, sexe, age, location)
VALUES ('2H001', 'Bob', 'Macarty', 'H', 33, 'CA');

INSERT INTO employee (id, fname,lname, sexe, age, location)
VALUES ('1F002', 'Charlie', 'Lapointe','F', 28, 'NYK');
```

# Étape 21 : Vérifier les données

Pour vérifier que les données ont été correctement insérées :

```sql
SELECT * FROM employee;
```

# Compétences acquises

À la fin de ce laboratoire, les étudiants savent :

* lancer une base **PostgreSQL avec Amazon RDS**
* utiliser la configuration **Free Tier**
* configurer la connectivité réseau
* récupérer les informations de connexion
* Connexion à la base de donnée
* créer une table employee.
