# Lab6 : Déployer un site web statique sur Amazon S3

Dans ce laboratoire, vous allez apprendre à **héberger un site web statique en utilisant Amazon S3**. Amazon S3 permet de configurer un **bucket comme serveur web statique** pour héberger des pages HTML, CSS, JavaScript et des images.

# Objectif du laboratoire

À la fin de ce laboratoire, vous serez capable de :

* créer un **bucket S3**
* activer l’option **Static Website Hosting**
* rendre le contenu du bucket **accessible publiquement**
* déployer un **site web statique**
* accéder au site via l’**URL S3 Website Endpoint**

# Partie 1 : Création du bucket S3

## Étape 1 : Accéder à la console S3

1. Connectez-vous à la **console AWS**.
2. Ouvrez le service **Amazon S3** :
   [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/)

## Étape 2 : Créer un bucket

1. Cliquez sur **Create bucket**.
2. Renseignez les informations suivantes :

**Bucket name :**

```
example.com
```

Le nom du bucket doit être **unique dans AWS**.

**Region :**

```
eu-west-3
```

3. Laissez les autres paramètres par défaut.
4. Cliquez sur **Create bucket**.

# Partie 2 : Activer l’hébergement web statique

## Étape 3 : Ouvrir les propriétés du bucket

1. Dans la liste des buckets, cliquez sur le **nom du bucket créé**.
2. Cliquez sur l’onglet :

```
Properties
```

## Étape 4 : Activer Static Website Hosting

1. Descendez jusqu’à la section :

```
Static website hosting
```

2. Cliquez sur **Edit**.
3. Sélectionnez :

```
Use this bucket to host a website
```

4. Activez :

```
Enable
```

5. Dans **Index document**, entrez :

```
index.html
```

6. Cliquez sur **Save changes**.


## Étape 5 : Récupérer l’URL du site

Dans la section **Static website hosting**, notez l’adresse :

```
Bucket website endpoint
```

Cette URL correspond à l’adresse publique de votre site web.

Exemple :

```
http://example.com.s3-website-eu-west-1.amazonaws.com
```

# Partie 3 : Autoriser l’accès public

Par défaut, Amazon S3 bloque l’accès public aux buckets. Pour héberger un site web, il faut autoriser l’accès public.

## Étape 6 : Modifier les paramètres Block Public Access

1. Dans votre bucket, cliquez sur l’onglet :

```
Permissions
```

2. Dans la section :

```
Block public access (bucket settings)
```

3. Cliquez sur **Edit**.
4. Décochez :

```
Block all public access
```

5. Cliquez sur **Save changes**.

# Partie 4 : Ajouter une Bucket Policy

Pour permettre à tous les utilisateurs d’accéder au contenu du site web, il faut ajouter une **Bucket Policy**.

## Étape 7 : Autoriser l’accès public aux objets
1. Allez dans l’onglet :

```
Objects
```

2. Sélectionnez tous les fichiers du site web (par exemple **index.html**, images, CSS, etc.).

3. Cliquez sur :

```
Actions
```

4. Sélectionnez :

```
Make public using ACL
```

5. Confirmez l’opération.

# Partie 5 : Déployer le site web

## Étape 8 : Télécharger le projet web

Téléchargez le projet [**webapp**](https://github.com/atifrani/aws_s3_static_website/edit/master/README.md) depuis GitHub.


## Étape 9 : Charger les fichiers dans S3

1. Ouvrez votre bucket.
2. Cliquez sur :

```
Upload
```

3. Ajoutez les fichiers du projet **webapp**.
4. Cliquez sur **Upload**.

Assurez-vous que le fichier principal s’appelle :

```
index.html
```

# Partie 6 : Tester le site web

## Étape 10 : Accéder au site

1. Retournez dans l’onglet :

```
Properties
```

2. Descendez jusqu’à **Static website hosting**.
3. Cliquez sur :

```
Bucket website endpoint
```

Le site web s’ouvrira dans un navigateur.

# Résultat attendu

Si la configuration est correcte :

* le site web s’affiche dans le navigateur
* les fichiers sont servis directement depuis **Amazon S3**

# Architecture obtenue

```
Utilisateur
      |
Navigateur Web
      |
S3 Static Website Endpoint
      |
Bucket S3
      |
index.html + fichiers du site
```

# Compétences acquises

À la fin de ce laboratoire, vous savez :

* créer un **bucket S3**
* configurer **Static Website Hosting**
* gérer l’accès public avec **Bucket Policy**
* déployer un **site web statique sur S3**

