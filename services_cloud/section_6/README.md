## Section 6 : Amazon Lambda

Dans cette section, nous allons découvrir **Amazon Lambda**, le service de calcul proposé par AWS qui permet d’exécuter du code sans avoir à provisionner ni gérer de serveurs.

Avec Lambda, l’utilisateur n’a plus besoin de se soucier de l’infrastructure sous-jacente (serveurs, systèmes d’exploitation, capacité, etc.). Il suffit simplement de fournir le code, et AWS se charge automatiquement de son exécution.

## Fonctionnement de AWS Lambda

AWS Lambda exécute le code sur **une infrastructure de calcul hautement disponible et entièrement gérée par AWS (SAAS)**. Le service prend en charge automatiquement :

* la gestion des serveurs
* la maintenance du système d’exploitation
* le provisionnement des ressources
* la scalabilité automatique (adaptation à la charge)
* la journalisation (logs) via CloudWatch


## Principe clé

Avec AWS Lambda :

* vous ne gérez pas l’infrastructure
* vous écrivez uniquement le code
* vous payez uniquement lorsque le code s’exécute

## Langages supportés

AWS Lambda prend en charge plusieurs langages de programmation, notamment :

* Python
* Java
* Node.js
* C#
* Go

## Quand utiliser AWS Lambda

AWS Lambda est particulièrement adapté aux applications qui doivent être **hautement scalables**, c’est-à-dire capables de :

* **monter en charge automatiquement** lorsque la demande augmente
* **s’arrêter complètement (scale to zero)** lorsqu’il n’y a pas d’activité

Ce modèle permet d’optimiser les coûts, car vous ne payez que lorsque votre code est exécuté.

### Cas d’utilisation courants

#### 1. Traitement de fichiers

Lambda peut être utilisé pour traiter automatiquement des fichiers stockés dans **Amazon S3**.

Exemple :

* Un utilisateur télécharge une image dans un bucket S3
* Cela déclenche automatiquement une fonction Lambda
* La fonction redimensionne l’image ou génère une miniature

👉 Ce traitement se fait **en temps réel, sans intervention manuelle**.

#### 2. Traitement de flux de données (stream processing)

Lambda permet de traiter des données en continu (temps réel), par exemple :

* suivi de l’activité des utilisateurs
* traitement des transactions
* analyse des clics (clickstream)
* nettoyage et filtrage de données
* analyse de logs
* données issues des objets connectés (IoT)

👉 Idéal pour les applications nécessitant une **réactivité immédiate**.


#### 3. Applications web

Lambda peut être combiné avec d’autres services AWS (comme **API Gateway**) pour créer des **applications web serverless**.

Avantages :

* montée en charge automatique
* haute disponibilité
* aucune gestion de serveur

#### 4. Backends IoT

Lambda permet de construire des **backends pour les objets connectés (IoT)**.

Exemple :

* un capteur envoie des données
* Lambda traite ces données
* les stocke ou déclenche une action

#### 5. Backends mobiles

Lambda peut être utilisé avec **Amazon API Gateway** pour créer des **API backend** pour applications mobiles.

Fonctions possibles :

* authentification des utilisateurs
* traitement des requêtes API
* communication avec une base de données

## Créer sa première fonction Lambda

AWS permet de créer et tester rapidement une fonction Lambda directement depuis la **console AWS**.

Dans cet exemple, nous allons créer une fonction simple qui :

* reçoit un objet JSON contenant :

  * `length`
  * `width`
* calcule l’aire (length × width)
* retourne le résultat au format JSON

### Étapes de création d’une fonction Lambda

#### Étape 1 : Accéder au service Lambda

1. Connectez-vous à la **console AWS**
2. Ouvrez le service **AWS Lambda**
3. Accédez à la section :

```
Functions
```

#### Étape 2 : Créer une fonction

1. Cliquez sur :

```
Create function
```

2. Sélectionnez :

```
Author from scratch
```

#### Étape 3 : Configurer la fonction

Dans la section **Basic information** :

* **Function name** :

```
myLambdaFunction
```

* **Runtime** :

```
Python 3.13
```

* **Architecture** :

```
x86_64
```

3. Cliquez sur :

```
Create function
```

### Étape 4 : Comprendre le rôle d’exécution (IAM Role)

Lors de la création de la fonction, AWS crée automatiquement un **rôle IAM (execution role)**.

Ce rôle permet à la fonction Lambda :

* d’accéder à certains services AWS
* d’écrire des logs dans **Amazon CloudWatch**

👉 Sans ce rôle, la fonction ne pourrait pas interagir avec les ressources AWS.

### Étape 5 : Exemple de fonction simple

Vous pouvez modifier le code de la fonction pour calculer une surface :

```python
import json

def lambda_handler(event, context):
    length = event['length']
    width = event['width']
    
    area = length * width
    
    return {
        'statusCode': 200,
        'body': {
            'area': area
        }
    }
```

### Étape 6 : Tester la fonction

1. Cliquez sur **Test**
2. Créez un événement de test avec le JSON suivant :

```json
{
  "length": 5,
  "width": 3
}
```

3. Exécutez la fonction

Résultat attendu :

```json
{
  "area": 15
}
```
