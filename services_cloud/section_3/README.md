# Section 3 : Introduction à Amazon EC2

Dans le cloud AWS, les applications ont besoin de ressources de **calcul** pour s’exécuter. AWS fournit plusieurs services de calcul, dont le plus fondamental est **Amazon EC2 (Elastic Compute Cloud)**.

Amazon EC2 permet de créer et de gérer des **machines virtuelles dans le cloud**. Ces machines peuvent être utilisées pour héberger :

* des applications web
* des services backend
* des bases de données
* des environnements de développement ou de test

Contrairement à l’infrastructure traditionnelle, où les serveurs doivent être achetés et installés physiquement, EC2 permet de **créer un serveur en quelques minutes** et de payer uniquement pour le temps d’utilisation.

Cette section présente les concepts essentiels liés à l’utilisation d’Amazon EC2 :

* **Elastic Compute Cloud (EC2)**
* **Amazon Machine Images (AMI)**
* **Types d’instances EC2**
* **Volumes de stockage EBS**
* **Utilisation des Security Groups**
* **Adressage IP des instances EC2**
* **Démarrer et se connecter à une instance EC2**

Ces éléments constituent les **briques fondamentales du déploiement d’applications sur AWS**.

---

# 1. Elastic Compute Cloud (EC2)

**Amazon EC2 (Elastic Compute Cloud)** est un service AWS qui permet de **lancer des machines virtuelles appelées instances** dans le cloud.

Une **instance EC2** fonctionne comme un serveur classique :

* elle possède un **système d’exploitation**
* elle dispose de **CPU, mémoire et stockage**
* elle peut exécuter des **applications**

Les avantages principaux d’EC2 sont :

* **création rapide de serveurs**
* **scalabilité** (ajout ou suppression de ressources)
* **facturation à l’usage**
* **intégration avec les autres services AWS**

# 2. Amazon Machine Images (AMI)

Une **AMI (Amazon Machine Image)** est un **modèle de machine virtuelle** utilisé pour lancer une instance EC2.

Une AMI contient généralement :

* le **système d’exploitation** (Linux, Windows, etc.)
* des **logiciels préinstallés**
* les **configurations nécessaires au démarrage**

AWS propose plusieurs AMI prêtes à l’emploi, par exemple :

* **Amazon Linux**
* **Ubuntu**
* **Windows Server**
* **Red Hat Enterprise Linux**

Les utilisateurs peuvent également créer leurs **propres AMI personnalisées**.

# 3. Types d’instances EC2

Lors de la création d’une instance EC2, il est nécessaire de choisir un **type d’instance**. Ce type définit les ressources disponibles pour la machine virtuelle.

Chaque type d’instance détermine :

* la **puissance CPU**
* la **mémoire RAM**
* la **capacité réseau**
* la **performance de stockage**

Les types d’instances sont généralement regroupés par catégories :

| Catégorie         | Utilisation                                  |
| ----------------- | -------------------------------------------- |
| General Purpose   | Applications classiques                      |
| Compute Optimized | Applications intensives en calcul            |
| Memory Optimized  | Applications nécessitant beaucoup de mémoire |
| Storage Optimized | Applications nécessitant un stockage rapide  |

Exemple :

```
t2.micro
```

Cette instance est souvent utilisée pour les **tests et le Free Tier AWS**.

# 4. Les volumes de stockage EBS

Les instances EC2 utilisent généralement un stockage appelé **Amazon EBS (Elastic Block Store)**.

Un **volume EBS** est un **disque virtuel attaché à une instance EC2**.

Il fonctionne de manière similaire à un **disque dur dans un serveur physique**.

Les caractéristiques principales d’EBS :

* stockage **persistant**
* possibilité d’augmenter la taille du volume
* snapshots pour sauvegarde
* haute disponibilité

Même si l’instance EC2 est arrêtée, **les données stockées sur EBS sont conservées**.

# 5. Utilisation des Security Groups

Les **Security Groups (SG)** sont utilisés pour contrôler l’accès réseau aux instances EC2.

Ils fonctionnent comme un **pare-feu virtuel**.

Les Security Groups permettent de définir :

* le trafic **entrant autorisé**
* le trafic **sortant autorisé**

Par exemple :

| Type  | Port | Source                    |
| ----- | ---- | ------------------------- |
| SSH   | 22   | adresse IP administrateur |
| HTTP  | 80   | Internet                  |
| HTTPS | 443  | Internet                  |

Les Security Groups permettent donc de **sécuriser l’accès aux instances EC2**.

# 6. Adressage IP des instances EC2

Chaque instance EC2 possède une ou plusieurs **adresses IP**.

On distingue généralement deux types d’adresses :

### Adresse IP privée

* utilisée pour la communication **à l’intérieur du VPC**
* non accessible depuis Internet

### Adresse IP publique

* utilisée pour l’accès **depuis Internet**
* peut changer lorsque l’instance est arrêtée et redémarrée

AWS propose également des **Elastic IP**, qui sont des **adresses IP publiques statiques**.


# Conclusion

Amazon EC2 constitue l’un des services les plus importants d’AWS. Il permet aux entreprises et aux développeurs de **déployer rapidement des serveurs dans le cloud**, tout en gardant un contrôle total sur leur environnement.

La compréhension des concepts suivants est essentielle pour travailler avec EC2 :

* AMI
* types d’instances
* stockage EBS
* Security Groups
* adressage IP
* connexion aux instances

Ces éléments seront utilisés dans les **travaux pratiques suivants**, où les étudiants lanceront et configureront leurs premières instances EC2.
