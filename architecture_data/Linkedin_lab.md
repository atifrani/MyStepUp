
# 🧊 Projet : Analyse des Offres d'Emploi LinkedIn avec Snowflake

Chaque jour, des milliers d'entreprises et de particuliers se tournent vers LinkedIn à la recherche de talents.
Le jeu de données que vous allez explorer contient plusieurs milliers d'offres d’emploi.
Pour pouvoir interagir avec les différents jeux de données aux formats CSV et JSON, vous commencerez par charger chaque fichier dans une table de la base de données Snowflake.

## 🎯 Objectif
Ce projet vise à évaluer votre capacité à manipuler des données en utilisant Snowflake et streamlit. Vous exploiterez un ensemble de données provenant de LinkedIn pour effectuer des analyses pertinentes sur le marché de l'emploi.

## 📁 Jeu de Données
Les fichiers sont disponibles dans le bucket S3 public suivant : **s3://snowflake-lab-bucket/**

Vous y trouverez les fichiers suivants :

* benefits.csv : Ce fichier contient les avantages associés à chaque offre d'emploi.

* companies.json : Ce fichier fournit des informations détaillées sur chaque entreprise ayant publié une offre d'emploi, incluant le nom de l'entreprise, son site web, sa description, sa taille, sa localisation, et d'autres détails pertinents.

* company_industries.json : Ce fichier répertorie les secteurs d'activité associés à chaque entreprise.

* company_specialities.json : Ce fichier liste les spécialités associées à chaque entreprise.

* employee_counts.csv : Ce fichier contient le nombre d'employés et de followers pour chaque entreprise.

* job_industries.json : Ce fichier indique les secteurs d'activité associés à chaque offre d'emploi.

* job_postings.csv : Ce fichier fournit des informations détaillées sur chaque offre d'emploi, incluant le titre du poste, la description, le salaire, le type de contrat, la localisation, et d'autres informations pertinentes.

* job_skills.csv : Ce fichier contient les compétences associées à chaque offre d'emploi.


Chaque fichier contient des informations spécifiques détaillées dans la description des colonnes ci-dessous.  

**Jobs_posting :** 

|Column |                      Description  |
|--------|-----------------------------------|  
|job_id                    | The job ID as defined by LinkedIn (https://www.linkedin.com/jobs/view/{job_id})|
|company_name               | nmae for the company associated with the job posting (maps to companies.csv)  |
|title	                   | Job title  |
|description	           |     Job description  |
|max_salary	               | Maximum salary  |
|med_salary	               | Medium salary  |
|min_salary	               | Minimum salary  |
|pay_period	               | Pay period for salary (Hourly, Monthly, Yearly)  |
|formatted_work_type	   |     Type of work (Fulltime, Parttime, Contract)  |
|location	               | Job location  |
|applies	               |     Number of applications that have been submitted  |
|original_listed_time	   | Original time the job was listed  |
|remote_allowed	           | Whether job permits remote work  |
|views	                   | Number of times the job posting has been viewed  |
|job_posting_url	       |     URL to the job posting on a platform  |
|application_url	       |     URL where applications can be submitted |  
|application_type	       | Type of application process (offsite, complex/simple onsite)  |
|expiry	                   | Expiration date or time for the job listing  |
|closed_time	           |     Time to close job listing  |
|formatted_experience_level | job experience level (entry, associate, executive, etc)  |
|skills_desc	           |    Description detailing required skills for job  |
|listed_time	           |   Time when the job was listed  |
|posting_domain	           | Domain of the website with application  |
|sponsored	               |Whether the job listing is sponsored or promoted  |
|work_type	               | Type of work associated with the job  |
|currency	               | Currency in which the salary is provided  |
|compensation_type	       | Type of compensation for the job  |




**Benefits :** 

|Column |                      Description  |
|--------|-----------------------------------|  
|job_id	    |The job ID|
|inferred	|Whether the benefit was explicitly tagged or inferred through text by LinkedIn|
|type	    |Type of benefit provided (401K, Medical Insurance, etc)|



**Companies :**

|Column |                      Description  |
|--------|-----------------------------------|  
|company_id	    |The company ID as defined by LinkedIn|
|name	        |Company name|
|description	|Company description|
|company_size	|Company grouping based on number of employees (0 Smallest - 7 Largest)|
|state	        |State of company headquarters|
|country	    |Country of company headquarters|
|city	        |City of company headquarters|
|zip_code	    |ZIP code of company's headquarters|
|address	    |Address of company's headquarters|
|url	        |Link to company's LinkedIn page|



**Employee_counts :**   

|Column |                      Description  |
|--------|-----------------------------------|  
|company_id	The |company ID|
|employee_count	|Number of employees at company|
|follower_count	|Number of company followers on LinkedIn|
|time_recorded	|Unix time of data collection|


**Job_Skills :**

|Column |                      Description  |
|--------|-----------------------------------| 
|job_id	    |The job ID (references jobs table and primary key)|
|skill_abr	|The skill abbreviation|



**Job_Industries :**  

|Column |                      Description  |
|--------|-----------------------------------| 
|job_id	        |The job ID (references jobs table and primary key)|
|industry_id	|The industry ID |


**Company_specialities :**

|Column |                      Description  |
|--------|-----------------------------------| 
|company_id	|The company ID (references companies table and primary key)|
|speciality	|The speciality name|


**Company_industries :**

|Column |                      Description  |
|--------|-----------------------------------| 
|company_id	|The company ID (references companies table and primary key)|
|industry	|The industry ID |



## 🛠️ Prérequis
Création d'un compte Snowflake gratuit (120 jours) via ce lien si vous n'avez pas de compte.

Connaissances de base en SQL, Python et manipulation de fichiers CSV/JSON.

Utilisation exclusive de scripts pour toutes les opérations (pas d'interface graphique).

# 🔧 Étapes à Suivre
Création de la base de données : Créez une base nommée **linkedin**.

Création d'un stage externe : Configurez un stage pointant vers le bucket S3 fourni.

Définition des formats de fichiers : Spécifiez les formats appropriés pour les fichiers CSV et JSON.

Création des tables : Créez les tables correspondantes aux fichiers, en respectant les schémas fournis.

Chargement des données : Utilisez la commande COPY INTO pour importer les données depuis le stage.

Transformations nécessaires : Effectuez les transformations requises pour assurer la cohérence et l'intégrité des données.

## 📊 Analyse des Données
Réalisez les analyses suivantes :

1. Top 10 des titres de postes les plus publiés par industrie.

2. Top 10 des postes les mieux rémunérés par industrie.

3. Répartition des offres d’emploi par taille d’entreprise.

4. Répartition des offres d’emploi par secteur d’activité.

5. Répartition des offres d’emploi par type d’emploi (temps plein, stage, temps partiel).

Pour chaque analyse, créez une visualisation appropriée en utilisant **Streamlit** directement dans Snowflake.

📄 Livrable Attendu
Un dépôt github détaillant :

1. Les commandes SQL utilisées, avec explications.

2. Le code Streamlit pour chaque visualisation, accompagné des résultats obtenus.

3. Les problèmes rencontrés et les solutions apportées.

4. Des commentaires explicatifs pour chaque étape.

Le projet doit être réalisé en binôme, avec une répartition équitable des tâches. Les livrables identiques entre groupes seront considérés comme du plagiat et sanctionnés en conséquence.

## 📬 Soumission
Envoyez votre livrable avec intitulé **ESME_EVALUATION_ARCHITECTURE_BIGDATA_2026** à l'adresse suivante : axel@logbrain.fr
