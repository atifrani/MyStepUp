# Lab : Héberger une page web sur EC2 et afficher une image stockée dans un bucket S3 public

## Objectif du laboratoire

Dans ce laboratoire, l’étudiant va :

* lancer une **application web HTML simple** sur une **instance EC2**
* créer un **bucket S3**
* rendre une **image accessible publiquement**
* afficher cette image dans la page web hébergée sur EC2

Ce laboratoire permet d’illustrer l’intégration entre **Amazon EC2** et **Amazon S3**.

# Architecture du laboratoire

L’architecture mise en place est la suivante :

```text
Utilisateur
   |
Navigateur Web
   |
EC2 (serveur Apache + page HTML)
   |
Image appelée depuis un bucket S3 public
```

La page HTML est hébergée sur **EC2**, mais l’image est stockée dans **Amazon S3**.

# Partie 1 : Créer un bucket S3

## Étape 1 : Ouvrir le service S3

1. Connectez-vous à la **console AWS**
2. Recherchez le service :

```text
S3
```

3. Cliquez sur **Create bucket**

## Étape 2 : Créer le bucket

Renseignez :

* **Bucket name** : un nom unique, par exemple :

```text
lab-ec2-s3-image-votreprenom
```

* **AWS Region** : choisissez la même région que votre EC2

```
eu-west-3
```

Laissez les autres paramètres par défaut.

Cliquez sur **Create bucket**.

# Partie 2 : Charger une image dans S3

## Étape 3 : Ouvrir le bucket

1. Cliquez sur le bucket créé
2. Cliquez sur **Upload**

## Étape 4 : Ajouter l’image

1. Sélectionnez une image depuis votre ordinateur
2. Cliquez sur **Upload**

# Partie 3 : Rendre l’image publique

## Étape 5 : Désactiver le blocage d’accès public du bucket

1. Dans le bucket, allez dans l’onglet **Permissions**
2. Dans **Block public access**, cliquez sur **Edit**
3. Décochez le blocage d’accès public
4. Confirmez l’opération
5. Cliquez sur **Save changes**

## Étape 6 : Rendre l’objet public

1. Revenez dans l’onglet **Objects**
2. Sélectionnez l’image
3. Cliquez sur **Actions**
4. Choisissez **Make public using ACL**

L’image devient accessible publiquement.

## Étape 7 : Récupérer l’URL publique de l’image

1. Cliquez sur l’image
2. Copiez **l’Object URL**

Exemple :

```text
https://lab-ec2-s3-image-votreprenom.s3.amazonaws.com/photo.jpg
```
Conservez cette URL, elle sera utilisée dans la page HTML.

# Partie 4 : Se connecter à l’instance EC2

## Étape 8 : Allez dans l'instance EC2

Dans la console AWS, cliquez sur l'instance EC2.

## Étape 9 : Se connecter à EC2

Depuis l'instance, cliquez sur **Connet**

Selectionnez l'onglet **EC2 Instance Connect**

Cliquez sur **Connect**

# Partie 5 : Installer Apache sur EC2

## Étape 10 : Installer le serveur web

Dans le terminal EC2, exécutez :

```bash
sudo yum install httpd -y
```

## Étape 11 : Démarrer Apache

```bash
sudo service httpd start
```

# Partie 6 : Créer la page HTML

## Étape 12 : Créer un fichier HTML

Exécutez la commande suivante :

```bash
sudo nano /var/www/html/index.html
```

Ajoutez le contenu suivant : (Remplacez **URL-IMAGE-S3** par l'url de votre image dans le bucket S3)  

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Lab EC2 et S3</title>
</head>
<body>
    <h1>Application web hébergée sur EC2</h1>
    <p>L’image ci-dessous est stockée dans un bucket S3 public.</p>
    <img src="URL-IMAGE-S3" alt="Image depuis S3" width="500">
</body>
</html>
```

# Partie 7 : Tester l’application

## Étape 13 : Ouvrir la page web

Dans un navigateur, entrez l’adresse suivante :

```text
http://PUBLIC-IP
```

Remplacez `PUBLIC-IP` par l’adresse IP publique de l’instance EC2.

# Résultat attendu

La page web doit s’afficher avec :

* un titre HTML
* un texte descriptif
* l’image chargée depuis le **bucket S3 public**

Cela montre que :

* le **frontend HTML** est servi par **EC2**
* l’image est lue directement depuis **Amazon S3**

# Vérifications

L’étudiant doit vérifier que :

* le bucket S3 existe
* l’image est publique
* l’URL de l’image fonctionne seule dans le navigateur
* Apache fonctionne sur EC2
* la page HTML affiche bien l’image

# Nettoyage des ressources

À la fin du laboratoire :

* supprimer l’instance **EC2** si elle n’est plus utilisée
* supprimer l’image du **bucket S3**
* supprimer le **bucket S3** si nécessaire

# Compétences acquises

À la fin de ce laboratoire, l’étudiant sera capable de :

* créer un **bucket S3**
* téléverser une image dans S3
* configurer un accès public sur un objet S3
* héberger une page web HTML sur **EC2**
* intégrer une ressource distante S3 dans une application web simple

Je peux maintenant vous rédiger une **version plus guidée pas à pas pour étudiants**, avec sections **Questions / Observations / Résultat attendu**.