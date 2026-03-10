# Lab 1 : Création d’un compte AWS Free Tier et découverte de la console

## Objectif du laboratoire

L’objectif de ce premier laboratoire est de permettre aux étudiants de :

* créer un **compte AWS Free Tier**
* découvrir **l’interface de la console AWS**
* identifier les principaux **services cloud proposés par AWS**
* comprendre l’organisation de l’environnement AWS

Ce laboratoire constitue la **première étape pour pouvoir utiliser les services AWS dans les travaux pratiques suivants**.

# Partie 1 : Création d’un compte AWS Free Tier

AWS propose une offre appelée **Free Tier**, qui permet d’utiliser certains services gratuitement pendant une période limitée ou avec des quotas mensuels gratuits.

### Étape 1 : Accéder au site AWS

Ouvrir le site officiel :

[https://aws.amazon.com/free](https://aws.amazon.com/free)

Cliquer sur **Create a Free Account**.

### Étape 2 : Création du compte

Remplir les informations demandées :

* **Adresse email**
* **Mot de passe**
* **Nom du compte AWS**

Cliquer sur **Verify email address**.

### Étape 3 : Informations de contact

Choisir :

* **Personal** (compte personnel pour les étudiants)

Puis renseigner :

* nom et prénom
* adresse
* numéro de téléphone
* pays

Accepter les **conditions d’utilisation AWS**.

### Étape 4 : Informations de paiement

AWS demande une **carte bancaire** afin de vérifier l’identité de l’utilisateur.

Important :

* la création du compte est gratuite
* l’utilisation du **Free Tier** ne génère pas de frais si les quotas sont respectés


### Étape 5 : Vérification du numéro de téléphone

AWS demande une **vérification par téléphone**.

Entrer :

* votre numéro de téléphone
* le code reçu par SMS.

### Étape 6 : Choix du plan de support

Choisir :

**Basic Support – Free**

Puis cliquer sur **Complete sign up**.

# Partie 2 : Connexion à la console AWS

Une fois le compte créé, accéder à la **console de gestion AWS**.

Lien :

[https://console.aws.amazon.com](https://console.aws.amazon.com)

Entrer :

* l’adresse email
* le mot de passe

Vous accédez alors à la **AWS Management Console**.

# Partie 3 : Découverte de l’interface AWS

La console AWS permet d’accéder à **plus de 200 services cloud**.

### Éléments principaux de l’interface

1. **Barre de recherche des services**

Permet de rechercher rapidement un service AWS.

Exemples :

* EC2
* S3
* Lambda
* RDS

2. **Liste des services**

Les services sont classés par catégories :

* Compute
* Storage
* Database
* Networking
* Security
* Analytics
* Machine Learning

3. **Sélecteur de région**

En haut à droite de la console se trouve le **sélecteur de région**.

Exemple :

```
Europe (Paris)
```

Les ressources AWS sont **créées dans une région spécifique**.

4. **Menu du compte utilisateur**

Permet de :

* gérer les paramètres du compte
* accéder à la facturation
* configurer la sécurité

# Partie 4 : Configuration d’une alerte de coût (Zero Spend Alert)

Avant d’utiliser AWS, il est fortement recommandé de configurer une **alerte de facturation** afin d’éviter toute dépense accidentelle.

### Objectif

Configurer une alerte lorsque les dépenses atteignent **0 USD** afin d’être averti immédiatement si un service payant est utilisé.

### Étape 1 : Accéder au service Billing

Dans la barre de recherche AWS, rechercher :

**Billing**

Puis cliquer sur **Billing Dashboard**.

### Étape 2 : Accéder aux Budgets AWS

Dans le menu de gauche :

Cliquer sur :

**Budgets**

Puis cliquer sur :

**Create budget**

### Étape 3 : Choisir le type de budget

Sélectionner :

**Cost Budget**

Puis cliquer sur **Next**.

### Étape 4 : Configurer le budget

Configurer les paramètres suivants :

Budget name :

Zero-Spend-Budget

Budget amount : 0 USD

Period : Monthly

### Étape 5 : Configurer l’alerte

Ajouter une alerte :

Alert threshold : 100 %

Puis ajouter une **adresse email** pour recevoir la notification.

### Étape 6 : Créer le budget

Cliquer sur :

**Create budget**

AWS enverra alors un **email de notification si des coûts apparaissent sur votre compte**.