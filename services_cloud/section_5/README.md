## Section 5 : Amazon Relational Database Service (Amazon RDS)

Dans cette section, nous allons découvrir **Amazon RDS (Relational Database Service)**, le service de bases de données relationnelles managées proposé par AWS.

Amazon RDS permet de **créer, exploiter et administrer une base de données relationnelle dans le cloud** sans avoir à gérer directement toute l’infrastructure associée, comme :

* l’installation du moteur de base de données
* les mises à jour logicielles
* les sauvegardes
* la haute disponibilité
* certaines opérations de maintenance

Ce service est particulièrement utile pour héberger des **bases de données applicatives** utilisées par des sites web, des applications métiers ou des services backend.

### Points qui seront étudiés

Dans cette section, nous allons détailler les points suivants :

1. **Introduction à RDS**
2. **Types de bases de données RDS**
3. **Tarification du service RDS**
4. **Lab 07: Lancer une base de données RDS**
5. **Lab 08 : Déployer une application Web sur EC2 connectée à une base de données RDS PostgreSQL**

### Point important à clarifier

Il faut noter que **Amazon RDS est un service de bases de données relationnelles**, donc orienté **SQL**.
Le terme **NoSQL** ne correspond pas à RDS, mais à d’autres services AWS comme :

* **Amazon DynamoDB**
* **Amazon DocumentDB**
* **Amazon Keyspaces**

Ainsi, dans cette section, lorsque nous parlerons de **types de bases de données RDS**, il faudra comprendre :

* les **différents moteurs relationnels supportés par RDS**
* et la distinction entre **SQL (RDS)** et **NoSQL (autres services AWS)**

### Objectifs pédagogiques

À la fin de cette section, les étudiants devront être capables de :

* comprendre le rôle d’**Amazon RDS**
* distinguer une base **relationnelle SQL** d’une base **NoSQL**
* identifier les principaux moteurs pris en charge par RDS
* comprendre les principes de facturation
* lancer une base de données RDS depuis la console AWS

--- 

## Introduction à Amazon RDS

**Amazon RDS (Relational Database Service)** est un service AWS qui permet de **créer, exploiter et administrer facilement des bases de données relationnelles dans le cloud**. Avec RDS, les utilisateurs peuvent déployer une base de données en quelques minutes sans avoir à gérer directement les tâches complexes liées à l’administration d’un serveur de base de données.

Dans une infrastructure traditionnelle, l’installation et la gestion d’une base de données nécessitent plusieurs opérations, comme :

* installer le moteur de base de données
* configurer le serveur
* gérer les sauvegardes
* appliquer les mises à jour et correctifs
* assurer la haute disponibilité

Amazon RDS automatise la plupart de ces tâches, permettant aux développeurs et aux administrateurs de **se concentrer sur le développement et l’utilisation de leurs applications** plutôt que sur la gestion de l’infrastructure.

### Fonctionnement d’Amazon RDS

Avec Amazon RDS, la base de données est exécutée sur une **instance de base de données** gérée par AWS. Cette instance comprend :

* la puissance de calcul (CPU)
* la mémoire (RAM)
* le stockage
* le moteur de base de données

L’utilisateur peut se connecter à la base de données en utilisant les outils habituels comme :

* **MySQL Workbench**
* **pgAdmin**
* **SQL Server Management Studio**
* **DBEAVER**
* ou directement via une application web.

### Avantages d’Amazon RDS

Amazon RDS offre plusieurs avantages importants :

* **Déploiement rapide** : création d’une base de données en quelques minutes
* **Gestion automatisée** : sauvegardes automatiques, mises à jour et maintenance
* **Haute disponibilité** : possibilité d’utiliser des architectures **Multi-AZ** pour la tolérance aux pannes
* **Scalabilité** : possibilité d’augmenter la capacité de stockage ou la puissance de calcul
* **Sécurité** : intégration avec **IAM**, **VPC**, **Security Groups** et chiffrement des données

### Cas d’utilisation

Amazon RDS est utilisé pour de nombreuses applications, par exemple :

* bases de données pour **applications web**
* systèmes de **gestion d’utilisateurs**
* applications **e-commerce**
* applications **mobiles**
* systèmes d’information d’entreprise

Ainsi, Amazon RDS permet aux entreprises de **déployer et gérer facilement des bases de données relationnelles fiables et évolutives dans le cloud AWS**.

## Types de bases de données dans Amazon RDS

Amazon RDS prend en charge plusieurs **moteurs de bases de données relationnelles**. Ces moteurs utilisent le langage **SQL (Structured Query Language)** pour manipuler les données. Chaque moteur possède ses propres caractéristiques et est adapté à différents types d’applications.

### Principaux moteurs de bases de données supportés par RDS

#### 1. MySQL

**MySQL** est l’un des moteurs de bases de données relationnelles les plus populaires au monde. Il est largement utilisé pour les **applications web**.

Caractéristiques principales :

* open source
* facile à utiliser
* compatible avec de nombreux frameworks web
* très utilisé avec des applications **PHP, Java, Python ou Node.js**

Exemples d’utilisation :

* sites web
* blogs
* applications e-commerce
* applications SaaS

#### 2. PostgreSQL

**PostgreSQL** est une base de données relationnelle open source reconnue pour sa **fiabilité et ses fonctionnalités avancées**.

Caractéristiques principales :

* support avancé des requêtes SQL
* extensible
* support des données complexes (JSON, géospatiales)

Exemples d’utilisation :

* applications analytiques
* systèmes d’information complexes
* applications nécessitant des requêtes avancées

#### 3. MariaDB

**MariaDB** est une version dérivée de MySQL, développée par la communauté open source.

Caractéristiques principales :

* compatible avec MySQL
* meilleures performances dans certains cas
* open source

Exemples d’utilisation :

* applications web
* plateformes e-commerce
* systèmes nécessitant une compatibilité MySQL

#### 4. Oracle Database

**Oracle Database** est un moteur de base de données relationnelle commercial utilisé principalement dans les **grandes entreprises**.

Caractéristiques principales :

* très performant
* fonctionnalités avancées pour les systèmes critiques
* haute sécurité

Exemples d’utilisation :

* applications financières
* ERP
* systèmes bancaires

#### 5. Microsoft SQL Server

**Microsoft SQL Server** est un moteur de base de données relationnelle développé par Microsoft.

Caractéristiques principales :

* forte intégration avec l’écosystème Microsoft
* outils d’administration avancés
* support des applications .NET

Exemples d’utilisation :

* applications d’entreprise
* systèmes Windows
* applications utilisant **.NET**

#### 6. Amazon Aurora

**Amazon Aurora** est une base de données relationnelle développée par AWS, compatible avec **MySQL et PostgreSQL**.

Caractéristiques principales :

* haute performance
* haute disponibilité
* stockage distribué et scalable
* performances supérieures à MySQL standard

Exemples d’utilisation :

* applications cloud modernes
* plateformes web à forte charge
* systèmes nécessitant une haute disponibilité

### Différence entre SQL et NoSQL

Les bases de données utilisées avec **Amazon RDS** sont des bases **relationnelles SQL**. Elles utilisent des **tables structurées avec des relations entre les données**.

En revanche, AWS propose également des bases de données **NoSQL**, utilisées pour des applications nécessitant une grande flexibilité ou des volumes de données très importants.

Exemples de bases NoSQL dans AWS :

* **Amazon DynamoDB**
* **Amazon DocumentDB**
* **Amazon Keyspaces**

Ces services ne font pas partie d’Amazon RDS.

### Résumé des moteurs RDS

| Moteur     | Type               | Usage principal                      |
| ---------- | ------------------ | ------------------------------------ |
| MySQL      | Open source        | Applications web                     |
| PostgreSQL | Open source avancé | Applications complexes               |
| MariaDB    | Compatible MySQL   | Applications web                     |
| Oracle     | Commercial         | Grandes entreprises                  |
| SQL Server | Microsoft          | Applications .NET                    |
| Aurora     | AWS                | Applications cloud haute performance |

Ces différents moteurs permettent aux utilisateurs de **choisir la base de données la plus adaptée aux besoins de leur application**.


## Tarification du service Amazon RDS

Amazon RDS utilise un modèle de **facturation à l’usage (pay-as-you-go)**. Cela signifie que vous payez uniquement pour les ressources que vous utilisez. Le coût total dépend de plusieurs éléments liés à l’infrastructure de la base de données et à son utilisation.

La tarification de RDS repose principalement sur les composants suivants :

### 1. Le type et la taille de l’instance

Le coût dépend du **type d’instance RDS** choisi. L’instance représente la puissance de calcul utilisée pour exécuter la base de données.

Chaque instance inclut :

* le **CPU**
* la **mémoire RAM**
* les **performances réseau**

Exemple de types d’instances :

* `db.t3.micro` (souvent éligible au Free Tier)
* `db.t3.small`
* `db.m5.large`

Plus l’instance est puissante, plus son coût est élevé.

### 2. Le stockage de la base de données

RDS facture également la **capacité de stockage utilisée par la base de données**.

Le stockage est généralement basé sur **Amazon EBS**. Les options principales sont :

* **General Purpose SSD (gp2 / gp3)** : stockage standard pour la plupart des applications
* **Provisioned IOPS SSD (io1 / io2)** : stockage haute performance
* **Magnetic Storage** : ancienne option moins utilisée

Le coût dépend :

* du **nombre de gigaoctets (GB) utilisés**
* du **type de stockage choisi**

### 3. Les opérations d’entrées/sorties (I/O)

Certaines configurations de stockage facturent également les **opérations d’entrées/sorties (I/O)**.

Ces opérations correspondent aux **lectures et écritures de données sur le disque**.

Les applications qui effectuent beaucoup d’accès à la base de données peuvent générer davantage de coûts.

### 4. Les sauvegardes (Backups)

Amazon RDS propose des **sauvegardes automatiques**.

Les sauvegardes incluses gratuitement correspondent généralement à **la taille de la base de données active**.

Cependant, un coût supplémentaire peut apparaître si :

* le volume de sauvegardes dépasse la taille de la base
* des **snapshots manuels supplémentaires** sont créés.

### 5. Le transfert de données

Les coûts peuvent également dépendre du **transfert de données réseau**.

* le transfert **entrant vers AWS est généralement gratuit**
* le transfert **sortant vers Internet peut être facturé**

### 6. La haute disponibilité (Multi-AZ)

Si l’option **Multi-AZ** est activée pour améliorer la disponibilité de la base de données, AWS crée **une instance de secours dans une autre zone de disponibilité**.

Cette configuration augmente la fiabilité mais **double généralement le coût de l’instance**, car deux instances sont utilisées.


### 7. Le Free Tier AWS

AWS propose une offre **Free Tier** permettant d’utiliser Amazon RDS gratuitement dans certaines limites pendant les premiers mois.

Exemple typique :

* **750 heures par mois d’instance db.t3.micro**
* **20 Go de stockage**
* **20 Go de sauvegardes**

Cette offre est idéale pour :

* les laboratoires
* les environnements de test
* l’apprentissage d’AWS

### Résumé des éléments facturés

| Élément              | Description                                  |
| -------------------- | -------------------------------------------- |
| Instance DB          | puissance de calcul utilisée                 |
| Stockage             | volume de données stockées                   |
| I/O                  | opérations de lecture et écriture            |
| Sauvegardes          | stockage des backups                         |
| Transfert de données | trafic réseau sortant                        |
| Multi-AZ             | instance secondaire pour haute disponibilité |

Comprendre ces éléments permet de **choisir une configuration adaptée aux besoins de l’application tout en maîtrisant les coûts d’utilisation d’Amazon RDS**.
