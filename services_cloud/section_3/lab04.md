## 7. Démarrer et se connecter à une instance EC2 (pas à pas avec CloudShell)

Dans cette partie, nous allons apprendre à **lancer une instance EC2 Free Tier depuis la console AWS**, puis **s’y connecter en utilisant AWS CloudShell**.
CloudShell permet d’utiliser un **terminal directement depuis la console AWS**, sans installer d’outils sur votre ordinateur.

# Étape 1 : Accéder au service EC2

1. Connectez-vous à la **console AWS**.
2. Dans la barre de recherche des services, tapez :

```
EC2
```

3. Cliquez sur **EC2** pour accéder au tableau de bord.


# Étape 2 : Lancer une nouvelle instance

1. Dans le tableau de bord EC2, cliquez sur :

```
Launch instance
```

# Étape 3 : Choisir une AMI Free Tier

Une **AMI (Amazon Machine Image)** correspond au système d’exploitation de la machine virtuelle.

Dans la section **Application and OS Images (AMI)**, choisissez une **AMI compatible Free Tier**, par exemple :

```
Amazon Linux 2023 AMI (Free tier eligible)
```

Cette AMI est recommandée pour les laboratoires.

# Étape 4 : Choisir un type d’instance Free Tier

Dans la section **Instance type**, choisissez :

```
t2.micro
```

ou

```
t3.micro
```

Ces types d’instances sont **éligibles au Free Tier AWS**.


# Étape 5 : Configurer la paire de clés (Key Pair)

La **Key Pair** permet de se connecter à l’instance via SSH.

1. Dans la section **Key pair**, cliquez sur :

```
Create new key pair
```

2. Donnez un nom :

```
ec2-lab-key
```

3. Type :

```
RSA
```

4. Format :

```
.pem
```

5. Cliquez sur **Create key pair**.

Le fichier sera téléchargé sur votre ordinateur.

# Étape 6 : Configurer le réseau

Dans **Network settings** :

* Choisir votre **Default VPC**
* Choisir un **Public Subnet**
* Vérifier que l’option suivante est activée :

```
Auto-assign public IP
```

Cela permettra d’accéder à l’instance via Internet.

# Étape 7 : Configurer le Security Group

Dans la section **Firewall (Security groups)**, autoriser :

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | My IP     |
| HTTP | 80   | 0.0.0.0/0 |

Ces règles permettent :

* l’accès SSH à l’instance
* l’accès web au serveur si une application web est installée.

# Étape 8 : Lancer l’instance

Cliquez sur :

```
Launch instance
```

Attendez quelques secondes jusqu’à ce que l’instance soit **Running**.

# Étape 9 : Vérifier les informations de l’instance

1. Dans le menu EC2, cliquez sur :

```
Instances
```

2. Sélectionnez votre instance.

3. Notez les informations suivantes :

* **Public IPv4 address**
* **Instance ID**
* **Subnet**
* **Security Group**

# Étape 10 : Ouvrir AWS CloudShell

1. Dans la barre supérieure de la console AWS, cliquez sur l’icône :

```
CloudShell
```

2. Attendez que le terminal se lance.

CloudShell est un **terminal Linux préconfiguré avec AWS CLI**.

# Étape 11 : Télécharger la clé dans CloudShell

Dans CloudShell :

1. Cliquez sur **Upload file**.
2. Importez votre fichier :

```
ec2-lab-key.pem
```

Puis appliquez les permissions nécessaires :

```
chmod 400 ec2-lab-key.pem
```

# Étape 12 : Se connecter à l’instance EC2

Dans CloudShell, utilisez la commande SSH suivante :

```
ssh -i ec2-lab-key.pem ec2-user@public-ip
```

Remplacez **public-ip** par l’adresse IP publique de votre instance.

Exemple :

```
ssh -i ec2-lab-key.pem ec2-user@54.82.14.23
```

# Étape 13 : Vérifier la connexion

Si la connexion fonctionne, vous devriez voir un terminal similaire à :

```
[ec2-user@ip-10-0-1-25 ~]$
```

Vous êtes maintenant connecté à votre **instance EC2 dans le cloud AWS**.

## Étape 14 : Déployer une page web statique sur l’instance EC2

Une fois connecté à l’instance EC2 via **CloudShell**, nous allons installer un **serveur web Apache (httpd)** et déployer une **application web statique** depuis un dépôt GitHub.

# 1. Installer le serveur web Apache

Dans le terminal de l’instance EC2, exécutez la commande suivante pour installer **Apache HTTP Server** :

```bash
sudo yum install httpd -y
```

Cette commande installe le serveur web Apache qui permettra d’héberger des pages web sur votre instance EC2.

# 2. Installer Git

Nous allons utiliser **Git** pour récupérer le code source de l’application web.

Installez Git avec la commande suivante :

```bash
sudo yum install git -y
```

# 3. Démarrer le service Apache

Après l’installation, démarrez le serveur web Apache :

```bash
sudo service httpd start
```

Vous pouvez vérifier que le service fonctionne correctement avec :

```bash
sudo service httpd status
```

# 4. Télécharger l’application web depuis GitHub

Nous allons maintenant cloner une application web statique depuis GitHub.

Exécutez la commande suivante :

```bash
git clone https://github.com/atifrani/ec2_webapp.git
```

Cette commande télécharge le code source dans un dossier nommé :

```
ec2_webapp
```

# 5. Vérifier le dossier du serveur web

Le répertoire par défaut utilisé par Apache pour servir les pages web est :

```
/var/www/html
```

Vérifiez son contenu avec :

```bash
ls /var/www/html
```

# 6. Déployer l’application dans le serveur web

Déplacez le dossier de l’application dans le répertoire du serveur web :

```bash
sudo mv ec2_webapp /var/www/html
```

# 7. Vérifier le déploiement

Vérifiez que l’application est bien présente dans le dossier web :

```bash
ls /var/www/html
```

Vous devriez voir le dossier :

```
ec2_webapp
```

# 8. Tester l’application web

Ouvrez un navigateur et entrez l’adresse suivante :

```
http://Public-IP/ec2_webapp
```

Remplacez **Public-IP** par l’adresse IP publique de votre instance EC2.

Exemple :

```
http://54.82.14.23/ec2_webapp
```

## Étape 15 : Supprimer l’instance EC2 (nettoyage des ressources)

À la fin du laboratoire, il est important de **supprimer l’instance EC2** afin d’éviter toute consommation de ressources inutile, même si vous utilisez une instance **Free Tier**.

Cette étape permet de **terminer proprement le laboratoire et libérer les ressources AWS**.

### 1. Accéder au service EC2

1. Retournez dans la **console AWS**.
2. Dans la barre de recherche, tapez :

```
EC2
```

3. Cliquez sur **EC2** pour accéder au tableau de bord.

### 2. Accéder à la liste des instances

1. Dans le menu de gauche, cliquez sur :

```
Instances
```

2. Sélectionnez l’instance que vous avez créée pendant le laboratoire.

### 3. Terminer l’instance

1. Cliquez sur le bouton :

```
Instance state
```

2. Sélectionnez :

```
Terminate instance
```

3. Confirmez l’action.

### 4. Vérifier la suppression

Après quelques secondes, l’état de l’instance devrait passer à :

```
Terminated
```

Une fois dans cet état, l’instance est définitivement supprimée et ne génère plus de ressources de calcul.

### 5. Vérification finale (optionnel)

Pour s’assurer que toutes les ressources sont correctement nettoyées, vous pouvez vérifier :

* qu’aucune **instance EC2** n’est encore active
* qu’aucune **Elastic IP** n’est attachée
* que le **volume EBS associé est supprimé automatiquement**

# Résultat attendu

À la fin de ce laboratoire, les étudiants doivent être capables de :

* lancer une **instance EC2 Free Tier**
* choisir une **AMI Free Tier**
* configurer un **Security Group**
* utiliser **AWS CloudShell**
* se connecter à une **instance EC2 via SSH**
* installer un **serveur web Apache**
* utiliser **Git pour récupérer du code**
* déployer une **application web statique sur EC2**
* accéder à l’application via **Internet**
* arrêter et supprimer une **instance EC2**
* comprendre l’importance du **nettoyage des ressources cloud**
* éviter des **coûts inutiles dans AWS**.
