# Projet d'évaluation : Airbnb Analytics Platform

## Contexte

Airbnb souhaite mettre en place une plateforme analytique permettant :
Vous pouvez vous référez au [lab02](https://github.com/atifrani/MyStepUp/blob/main/architecture_data/lab02.md)

* d'analyser les logements ;
* d'analyser les hôtes ;
* d'analyser les avis clients ;
* d'étudier l'impact des nuits de pleine lune sur les avis ;
* de mettre à disposition des indicateurs via une application Streamlit.

L'entreprise a décidé d'utiliser :

* DuckDB comme moteur analytique ;
* dbt pour les transformations ;
* GitHub pour le versioning ;
* Streamlit pour la restitution.

## Architecture cible:

                     GitHub

                        │

                        ▼

                dbt Project

                        │

      ┌─────────────────┼─────────────────┐

      ▼                 ▼                 ▼

   Bronze            Silver             Gold

      ▼                 ▼                 ▼

   DuckDB         Data Quality      Data Products

                        │

                        ▼

                  Streamlit

                        │

                        ▼

                Business Users


Le jeux de données est disponible [ici](https://github.com/atifrani/MyStepUp/tree/main/data)
├── data/
│   ├── hosts.csv
│   ├── reviews.csv
│   ├── listings.json
│   └── seed_full_moon_dates.csv

## Étapes à Suivre

* 1 : Création du projet
* 2 : Ingestion couche Bronze
* 3 : Ingestion/transformation couche Silver
* 4 : Vérification qualité des données
* 5 : Ingestion/transformation couche Gold
* 6 : Dashboard Streamlit

Le projet doit être réalisé en binôme, avec une répartition équitable des tâches. Les livrables identiques entre groupes seront considérés comme du plagiat et sanctionnés en conséquence.

📬 Soumission
Envoyez votre livrable avec intitulé [Votre_Promo]_Evaluation_DevOps_2026 à l'adresse suivante : axel@logbrain.fr