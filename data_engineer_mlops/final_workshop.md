### Evaluation Data Enginneering et Machine Learning

**Titre :** Data Engineering et au Machine Learning avec Snowflake

#### Contexte

Les plateformes de données Snowflake propose une plateforme unifiée qui permet d’ingérer, transformer, analyser et entraîner des modèles de machine learning directement sur les données stockées dans la data plateforme, sans déplacer les données vers des systèmes externes.

Grâce à **Snowpark**, **Snowflake ML** et les **Snowflake Notebooks**, les data engineers et data scientists peuvent collaborer pour construire des pipelines de données et développer des modèles ML en utilisant des bibliothèques open source telles que **scikit-learn, XGBoost ou PyTorch**, tout en bénéficiant de l’élasticité et de la gouvernance de Snowflake.

Dans ce workshop, vous allez utiliser Snowflake comme plateforme complète de **Data Engineering et Machine Learning** en développant vos modèles ML directement dans l’environnement Snowflake.

# Objectifs

* Charger et préparer un dataset dans Snowflake
* Utiliser les **notebooks** comme environnement de développement 
* Utiliser **Snowpark** pour manipuler les données
* Construire un pipeline de **data engineering**
* Entraîner des **modèles de machine learning** dans Snowflake
* Évaluer les performances des **modèles de machine learning**
* Optimiser un **modéle de machine learning** avec des **Hyperparameters**
* Stocker voter meilleur modéle dans un registry
* Utiliser votre meilleur modéle pour l'inference
* Construire une application permettant aux utilisateurs métier d’interagir avec le modèle

# Scénario du Workshop

Vous êtes **data engineer / data scientist** dans une entreprise qui souhaite exploiter ses données analytiques pour créer un modèle prédictif.

Les données sont stockées dans **un bucket S3**, et votre objectif est de construire un pipeline complet permettant de :

1. Charger et explorer les données
2. Explorer vos données
3. Préparer les features nécessaires au modèle
4. Entraîner des modèles de machine learning
4. Évaluer ses performances
6. Optimiser les performances avec des hyperparameteres
7. Stocker le modéle le plus performant dans un registry
8. Utiliser votre modéle le plus performance pour l'inférence
9. Utiliser Streamlit pour interagir avec le modèle

L’ensemble du workflow devra être réalisé **directement dans Snowflake**, sans exporter les données vers un environnement externe.

# Étapes du Workshop

## 1. Configuration de l’environnement

* Accéder à Snowflake
* Configurer un **Snowflake Notebook**
* Vérifier les dépendances Python nécessaires (scikit-learn, pandas, etc.)


## 2. Ingestion et exploration des données

* Charger un dataset dans une table Snowflake
* Explorer les données avec Snowpark
* Identifier la variable cible et les variables explicatives

## 3. Exploration des données

* Comprendre votre dataset
* Comprendre la correlation entre les colonnes

## 4. Préparation des données

* Séparer les **features (X)** et la **variable cible (y)**
* Effectuer les transformations nécessaires (normalisation, encodage, etc.)
* Préparer les datasets **train / test**

## 5. Entraînement des modèles

* Charger les données depuis Snowflake dans un environnement Python
* Utiliser une bibliothèque ML (ex : scikit-learn)
* Entraîner un premier modèle (ex : Logistic Regression ou XGBoost)

## 6. Évaluation du modèle

* Calculer les métriques de performance
    * Accuracy
    * Precision
    * Recall
* Comparer les résultats du modèle
* Identifier les axes d’amélioration

## 7. Optimisation du modèle

* Identifier les hyperparamètres importants du modèle
* Tester différentes configurations
* Utiliser des techniques d’optimisation comme Grid Search ou Random Search
* Comparer les performances des différents modèles
* Calculer les métriques de performance
    * Accuracy
    * Precision
    * Recall
* Sélectionner le meilleur modèle

## 8. Stockage du meilleur modèle dans le Model Registry

* Enregistrer le modèle dans le Snowflake Model Registry
* Ajouter des métadonnées (version, métriques, description)
* Gérer différentes versions du modèle
* Identifier le modèle prêt pour la production

## 9. Utilisation du modèle pour les inférences

* Charger le modèle depuis le Model Registry
* Appliquer le modèle sur de nouvelles données
* Générer des prédictions (inference) directement dans Snowflake
* Stocker les résultats dans une table Snowflake

## 10. Construction d’une application Streamlit pour interagir avec le modèle

Afin de rendre le modèle accessible aux utilisateurs, vous allez développer une application Streamlit directement dans Snowflake.Cette application permettra de :

* Saisir des données via une interface utilisateur
* Envoyer ces données au modèle enregistré
* Générer une prédiction en temps réel
* Afficher le résultat de manière interactive

# Livrables attendus

À la fin du workshop, chaque groupe devra me founir un dépôt github avec :

* Un **Notebook Snowflake** contenant le pipeline ML complet
* Un **modèle entraîné** sur le dataset
* Une **analyse des performances du modèle** dans le README.