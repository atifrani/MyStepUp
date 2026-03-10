# Lab 03 : Découverte des composants réseau AWS (VPC, Subnets, Internet Gateway et Security Groups)

## Objectif du laboratoire

L’objectif de ce laboratoire est de permettre aux étudiants de **découvrir et manipuler les principaux composants réseau d’AWS** directement depuis la **console AWS**.

À la fin de ce laboratoire, les étudiants seront capables de :

* identifier un **VPC**
* explorer et créer des **subnets**
* comprendre le rôle d’une **Internet Gateway**
* configurer un **Security Group**

Ce laboratoire est volontairement **exploratoire** afin de permettre aux étudiants de **se familiariser avec l’interface AWS et les composants réseau de base**.

# Partie 1 : Découverte du VPC

Le **VPC (Virtual Private Cloud)** est le réseau virtuel dans lequel toutes les ressources AWS sont déployées.

### Étapes

1. Connectez-vous à la **console AWS**.

2. Dans la barre de recherche des services, recherchez :

```
VPC
```

3. Cliquez sur le service **VPC**.

4. Dans le menu de gauche, cliquez sur :

```
Your VPCs
```

5. Identifiez le **Default VPC** créé automatiquement par AWS.

6. Cliquez sur ce VPC et observez les informations suivantes :

* CIDR Block
* IPv4 CIDR
* Region
* State

7. Cliquez sur l’onglet **Resource map** pour visualiser les ressources réseau associées au VPC.

# Partie 2 : Exploration des Subnets

Les **subnets** permettent de segmenter le VPC en plusieurs sous-réseaux.

### Étapes

1. Dans le menu de gauche du service **VPC**, cliquez sur :

```
Subnets
```

2. Identifiez les **subnets du Default VPC**.

3. Observez les informations suivantes :

* Availability Zone
* CIDR Block
* VPC associé
* nombre d’adresses IP disponibles

# Partie 3 : Découverte de l’Internet Gateway

Une **Internet Gateway** permet à un VPC de communiquer avec Internet.

### Étapes

1. Dans le service **VPC**, cliquez sur :

```
Internet Gateways
```

2. Vérifiez l’existence d’une **Internet Gateway attachée au Default VPC**.

3. Sélectionnez cette Internet Gateway.

4. Dans le menu **Actions**, cliquez sur :

```
Detach from VPC
```

5. Confirmez l’opération.

6. Vérifiez que l’état de la passerelle devient :

```
Detached
```

7. Dans le menu **Actions**, cliquez sur :

```
Attach to VPC
```

8. Sélectionnez le **Default VPC**.

9. Confirmez l’opération.

# Partie 4 : Découverte des Security Groups

Les **Security Groups** agissent comme un **pare-feu virtuel** pour les ressources AWS.

### Étapes

1. Dans le service **VPC**, cliquez sur :

```
Security Groups
```

2. Identifiez le **Security Group par défaut du VPC**.

3. Cliquez dessus et observez :

* les **Inbound rules**
* les **Outbound rules**

4. Cliquez sur :

```
Create security group
```

5. Configurez les paramètres suivants :

Nom :

```
lab-web-sg
```

Description :

```
Security group pour serveur web
```

VPC :

```
Default VPC
```

6. Ajoutez une règle **Inbound** :

Type :

```
HTTP
```

Source :

```
0.0.0.0/0
```

7. Ajoutez une seconde règle :

Type :

```
SSH
```

Source :

```
My IP
```

8. Cliquez sur :

```
Create security group
```


# Vérification

À la fin du laboratoire, vérifiez que :

* vous avez identifié le **Default VPC**
* vous avez exploré les **subnets**
* vous avez manipulé une **Internet Gateway**
* vous avez créé un **Security Group**

# Résultat attendu

À la fin de ce laboratoire, les étudiants doivent être capables de :

* naviguer dans le **service VPC**
* identifier les **composants réseau AWS**
* comprendre le rôle de chaque composant
* manipuler les **ressources réseau depuis la console AWS**
