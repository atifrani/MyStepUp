# Section 1 : Introduction au cloud AWS

## 1.1 Fondamentaux du Cloud Computing

* Définition du cloud computing

* Modèles de services :
    * IaaS (Infrastructure as a Service)
    * PaaS (Platform as a Service)
    * SaaS (Software as a Service)

* Modèles de déploiement :
    * Cloud public
    * Cloud privé
    * Cloud hybride

## 1.2 Infrastructure globale AWS

* Regions
* Availability Zones
* Edge Locations

## 1.3 Concepts clés du Cloud AWS

* Pay-as-you-go (paiement à l’usage)
* Elasticité
* Scalabilité
* Haute disponibilité

## 1.4 Moyens d'accès à AWS

* Console AWS
* AWS CLI
* SDK AWS

## 1.5 lab1: Création d’un compte AWS et découverte de la console

* Création d’un compte AWS Free Tier 
* Exploration de la console AWS
* Activation des alertes Free Tier 
* Création d'un budget

---

## 1.1 Fondamentaux du Cloud Computing

### Introduction : Le modèle client–serveur dans l’informatique moderne

La grande majorité des systèmes informatiques modernes repose sur un modèle d’architecture appelé **client–serveur**. Dans ce modèle, deux rôles principaux interagissent :

* **Le client** : l’utilisateur ou l’application qui fait une demande.

* **Le serveur** : le système qui traite la demande et fournit une réponse ou un service.

Pour comprendre ce principe, on peut prendre l’exemple simple illustré dans l’image.

![alt text](../../images/client_serveur.png)

Dans cette scène, un **client se présente au comptoir d’un café et commande une boisson**. Le client exprime une demande (par exemple un milkshake). La personne au comptoir reçoit la commande, la traite, puis prépare et fournit la boisson demandée.

Cette interaction illustre parfaitement le fonctionnement du modèle client–serveur :

* Le **client** correspond à la personne qui passe la commande.

* Le **serveur** correspond au système derrière le comptoir qui reçoit et traite la demande.

* La **commande** représente la requête envoyée.

* La **boisson préparée** représente la réponse retournée au client.

Dans le domaine informatique, ce fonctionnement est similaire. Par exemple :

* Un **navigateur web** agit comme un client.

* Un **serveur web** héberge un site ou une application.

* Le **navigateur** envoie une **requête HTTP** au **serveur**.

* Le **serveur** renvoie une **page web** ou **des données**.

![alt text](../../images/client_serveur_web.png)

Le **cloud computing**, et notamment les plateformes comme **AWS, GCP et Azure**, s’appuie largement sur ce modèle. La différence principale est que les serveurs ne sont plus hébergés dans l’infrastructure locale d’une entreprise, mais dans de ***grands centres de données accessibles via Internet**.

### Définition : Qu’est-ce que le Cloud Computing ?

Le terme **Cloud Computing** provient de deux mots anglais :

* **Cloud**, qui signifie *nuage*
* **Computing**, qui signifie *informatique*

Littéralement, le **Cloud Computing** peut donc être traduit par **« informatique en nuage »**.

Le **Cloud Computing** désigne un modèle informatique qui permet d’accéder, via Internet, à des **ressources informatiques partagées et configurables**, disponibles **à la demande et en libre-service**.

Ces ressources peuvent inclure :

* des **réseaux**
* des **serveurs**
* du **stockage**
* des **applications**
* différents **services informatiques**

![alt text](../../images/cloud_computing.png)

Dans ce modèle, les ressources sont **fournies rapidement aux utilisateurs**, sans nécessiter d’investissement préalable dans une infrastructure matérielle. Les utilisateurs peuvent **provisionner ou libérer ces ressources selon leurs besoins**, et leur utilisation est généralement **facturée à l’usage**.

Ainsi, le Cloud Computing permet aux entreprises et aux développeurs de **consommer des ressources informatiques comme un service**, de manière flexible, scalable et accessible depuis n’importe où via Internet.

### Les modèles de services du Cloud Computing

Le **cloud computing** offre de nombreuses possibilités d’utilisation selon les besoins des entreprises et des développeurs. Pour structurer ces usages, on distingue généralement **trois grands modèles de services cloud**.

Ces modèles se différencient principalement par **le niveau de gestion pris en charge par le fournisseur cloud** et par **le niveau de contrôle laissé à l’utilisateur**.

Les trois modèles principaux sont :

* **IaaS (Infrastructure as a Service)**
* **PaaS (Platform as a Service)**
* **SaaS (Software as a Service)**

Chaque modèle correspond à un **niveau d’abstraction différent**.

#### 1. IaaS – Infrastructure as a Service

Le modèle **IaaS** fournit aux utilisateurs une **infrastructure informatique virtuelle** accessible via Internet.

Le fournisseur cloud met à disposition :

* des **machines virtuelles**
* du **stockage**
* du **réseau**
* des **ressources de calcul**

L’utilisateur garde le contrôle sur :

* le **système d’exploitation**
* les **applications**
* la **configuration du serveur**

Dans ce modèle, l’entreprise gère donc encore une partie importante de l’infrastructure logicielle.

**Exemple:**  

Une entreprise souhaite héberger son application web.

Elle utilise **Amazon EC2 (service de serveur virtuel)** pour créer un serveur virtuel :

1. Elle lance une **machine virtuelle EC2**
2. Elle installe **Linux**
3. Elle installe un **serveur web (Apache ou Nginx)**
4. Elle déploie son application

Dans ce cas :

* **AWS fournit l’infrastructure**
* **l’entreprise gère le serveur et l’application**

#### 2. PaaS – Platform as a Service

Le modèle **PaaS** fournit une **plateforme complète de développement et de déploiement d’applications**.

Le fournisseur cloud gère :

* l’infrastructure
* le système d’exploitation
* les serveurs
* les mises à jour
* la scalabilité

L’utilisateur se concentre uniquement sur :

* le **code de l’application**
* la **logique métier**

Ce modèle simplifie fortement le développement et le déploiement d'applications.

**Exemple:**

Un développeur souhaite déployer une application web sans gérer les serveurs.

Il utilise **AWS Amplify** :

1. Il développe son application web
2. Il téléverse son code sur amplify
3. AWS déploie automatiquement l'application

Dans ce cas :

* **AWS gère l’infrastructure et la plateforme**
* **le développeur gère uniquement son application**

#### 3. SaaS – Software as a Service

Le modèle **SaaS** correspond à un logiciel complet accessible directement via Internet.

L’utilisateur n’a rien à installer ni à gérer. Le fournisseur cloud s’occupe de :

* l’infrastructure
* la plateforme
* l’application
* les mises à jour
* la maintenance

L’utilisateur utilise simplement le service via :

* un **navigateur web**
* une **application**

**Exemple:**  

Un utilisateur utilise **Microsoft 365**.

Il peut :

* créer des documents
* les modifier
* les partager en ligne

Sans installer de logiciel ni gérer de serveur.

Dans ce cas :

* **tout est géré par le fournisseur**
* **l’utilisateur consomme uniquement le service**

# Résumé des trois modèles

| Modèle | Ce que gère l’utilisateur       | Ce que gère le fournisseur  |
| ------ | ------------------------------- | --------------------------- |
| IaaS   | OS, applications, configuration | infrastructure              |
| PaaS   | application                     | plateforme + infrastructure |
| SaaS   | utilisation du logiciel         | tout le reste               |

Ces trois modèles représentent différents **niveaux d’abstraction du cloud** :

* **IaaS** → contrôle maximal
* **PaaS** → simplification du développement
* **SaaS** → utilisation directe d’un service logiciel.

![alt text](../../images/models_cloud.png)