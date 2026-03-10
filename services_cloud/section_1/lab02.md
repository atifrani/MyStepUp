# Lab 02 : Mise en place des bonnes pratiques de sécurité avec AWS IAM

## Objectif du laboratoire

Dans ce laboratoire, les étudiants vont apprendre à **sécuriser un compte AWS en appliquant les bonnes pratiques recommandées par AWS Identity and Access Management (IAM)**.

À la fin de ce laboratoire, les étudiants seront capables de :

* sécuriser l’utilisateur **root**
* activer **l’authentification multi-facteurs (MFA)**
* créer un **utilisateur IAM**
* utiliser des **groupes IAM pour gérer les permissions**
* appliquer une **politique de mot de passe IAM**

Ces bonnes pratiques sont essentielles pour **sécuriser un environnement cloud AWS**.

# Partie 1 : Supprimer les accès programmatiques pour l’utilisateur root

## Pourquoi ?

Le compte **root** possède **tous les privilèges sur le compte AWS**.
Il est fortement recommandé de **ne jamais utiliser le compte root pour les tâches quotidiennes**.

AWS recommande de :

* utiliser le compte root uniquement pour les **tâches critiques**
* utiliser des **utilisateurs IAM pour l’administration quotidienne**

### Étapes

1. Se connecter à la **console AWS** avec le compte root.

2. Dans la barre de recherche, chercher : **IAM**

3. Dans le menu de gauche, cliquer sur : **Users**

4. Vérifier que **le compte root ne possède aucune Access Key**.

5. Si une clé d’accès root existe :

* supprimer la **Access Key**
* désactiver tout accès programmatique.


# Partie 2 : Activer le MFA pour l’utilisateur root

## Pourquoi ?

Le **MFA (Multi-Factor Authentication)** ajoute une couche de sécurité supplémentaire en demandant :

* le mot de passe
* un code temporaire généré par une application d’authentification.

Même si un mot de passe est compromis, **l’accès reste protégé**.

### Étapes

1. Cliquer sur le **nom du compte** en haut à droite.

2. Sélectionner : Security credentials

3. Dans la section **Multi-Factor Authentication (MFA)** :

Cliquer sur : **Assign MFA device**

4. Choisir : **Virtual MFA device**

5. Installer une application d’authentification sur le téléphone :

* Google Authenticator
ou  
* Microsoft Authenticator


6. Scanner le **QR code** affiché.

7. Entrer les deux codes MFA générés.

8. Cliquer sur **Assign MFA**.

Le **MFA est maintenant activé pour le compte root**.

# Partie 3 : Créer un utilisateur IAM

## Pourquoi ?

Il est recommandé de **ne pas utiliser le compte root pour gérer AWS**.
À la place, on crée des **utilisateurs IAM avec des permissions spécifiques**.

### Étapes

1. Aller dans le service : **IAM**

2. Cliquer sur : **Users**

3. Cliquer sur : **Create user**

4. Donner un nom : **user-admin**

5. Cocher : Provide user access to the AWS Management Console

6. Définir un mot de passe.

7. Cliquer sur **Next**.

# Partie 4 : Utiliser les groupes IAM pour gérer les permissions

## Pourquoi ?

Les **groupes IAM** permettent d’attribuer des permissions à plusieurs utilisateurs en même temps.

Plutôt que d’attacher les permissions directement aux utilisateurs, on les attribue aux **groupes**.

### Étapes

1. Dans la section **Permissions**, choisir : **Add user to group**

2. Cliquer sur : **Create group**  

Nom du groupe : **Administrators**

3. Attacher la politique suivante : **AdministratorAccess**

4. Cliquer sur : **Create group**

5. Ajouter l’utilisateur **user-admin** au groupe.

6. Cliquer sur **Create user**.

# Partie 5 : Appliquer une politique de mot de passe IAM

## Pourquoi ?

Une **politique de mot de passe** permet d’imposer des règles de sécurité aux utilisateurs.

Exemples :

* longueur minimale
* caractères spéciaux
* expiration des mots de passe

### Étapes

1. Aller dans le service : **IAM**

2. Dans le menu de gauche, cliquer sur : **Account settings**

3. Cliquer sur : **Edit password policy**

4. Configurer les règles suivantes :

Minimum password length : **8 caractères**

Activer :

* Require uppercase letters
* Require lowercase letters
* Require numbers
* Require non-alphanumeric characters

5. Cliquer sur : **Save changes**

# Vérification

À la fin du laboratoire, vérifier que :

* le **MFA est activé pour le compte root**
* aucun **accès programmatique root** n’est actif
* un **utilisateur IAM a été créé**
* l’utilisateur appartient à un **groupe**
* une **politique de mot de passe IAM est active**

# Résultat attendu

À la fin de ce laboratoire, les étudiants doivent être capables de :

* sécuriser le **compte root**
* utiliser **MFA**
* créer et gérer des **utilisateurs IAM**
* utiliser des **groupes IAM**
* appliquer une **politique de mot de passe**

Ces pratiques constituent les **bases de la sécurité sur AWS** et sont recommandées par le **AWS Well-Architected Framework**.
