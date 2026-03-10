# Section 2 : Réseau AWS

## Concepts réseau dans le cloud

* **VPC (Virtual Private Cloud)**
* **Subnets**
* **CIDR**
* **Route Tables**
* **Internet Gateway**
* **NAT Gateway**
* **Security Groups**
* **Network ACL**

### Architecture réseau AWS

Architecture simple :

```
Internet
   |
Internet Gateway
   |
Public Subnet ---- Bastion Host
   |
Private Subnet --- Application Servers
```

### Lab 3 : Structure réseau aws

explorer la structure réseau AWS de votre compte.
* **VPC**
* **subnets**
* **route table**
* **internet gateway**

### Lab 4 : Déploiement d’une architecture web

Architecture cible :

```
Internet
   |
EC2 instances
```

---

## Introduction : Qu’est-ce qu’un VPC ?

Dans AWS, le **VPC (Virtual Private Cloud)** est l’un des éléments fondamentaux de l’architecture réseau. Il permet aux utilisateurs de créer un **réseau virtuel privé dans le cloud AWS** dans lequel ils peuvent déployer leurs ressources.

Un **Virtual Private Cloud (VPC)** est donc un **réseau virtuel dédié à votre compte AWS**, qui est **logiquement isolé des autres réseaux présents dans l’infrastructure AWS**. Cette isolation garantit que les ressources déployées dans votre VPC sont séparées de celles des autres clients AWS.

À l’intérieur d’un VPC, vous pouvez lancer différents types de ressources AWS, par exemple :

* des **instances Amazon EC2**
* des **bases de données**
* des **services applicatifs**
* des **systèmes de stockage**

![alt text](../../images/vpc.png)

Lors de la création d’un compte AWS, un **VPC par défaut (Default VPC)** est automatiquement créé dans chaque région. Ce VPC par défaut est préconfiguré pour permettre de **lancer rapidement des ressources AWS sans configuration réseau complexe**.


## Les Subnets (Sous-réseaux) dans AWS

Après avoir créé un **VPC (Virtual Private Cloud)**, il est possible d’y ajouter un ou plusieurs **subnets (sous-réseaux)**. Les subnets permettent de **diviser le réseau du VPC en plusieurs segments plus petits** afin d’organiser les ressources et de mieux contrôler le trafic réseau.

Un **subnet** est une **plage d’adresses IP appartenant à votre VPC**. Les ressources AWS, telles que les **instances Amazon EC2**, doivent être déployées dans un subnet spécifique.

### Règles importantes concernant les subnets

Dans AWS, plusieurs règles doivent être respectées lors de la création des subnets :

* Chaque **subnet doit appartenir à une seule zone de disponibilité (Availability Zone)**.
* Un subnet **ne peut pas s’étendre sur plusieurs zones de disponibilité**.
* Les blocs CIDR des subnets **doivent être inclus dans le bloc CIDR du VPC**.
* Si plusieurs subnets existent dans un VPC, leurs **plages d’adresses IP ne doivent pas se chevaucher**.

Cette organisation permet de créer des architectures réseau structurées, par exemple :

* **subnets publics** pour les ressources accessibles depuis Internet
* **subnets privés** pour les bases de données ou les serveurs internes.

### Exemple

Supposons que vous créez un VPC avec le bloc CIDR suivant :

```
10.0.0.0/24
```

Ce bloc couvre les adresses IP :

```
10.0.0.0 → 10.0.0.255
```

Il permet donc de disposer de **256 adresses IP**.

Ce bloc peut ensuite être divisé en plusieurs subnets. Par exemple :

* **Subnet 1 : 10.0.0.0/25** → pour les adresses 10.0.0.0 - 10.0.0.127:  128 adresses IP 
* **Subnet 2 : 10.0.0.128/25** → pour les adresses 10.0.0.128 - 10.0.0.255: 128 adresses IP

Chaque subnet pourra ensuite être associé à une **zone de disponibilité différente** afin d’améliorer la **disponibilité et la tolérance aux pannes des applications**.

![alt text](../../images/subnets.png)