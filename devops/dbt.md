# Introduction à dbt (Data Build Tool)

## Qu'est-ce que dbt ?

dbt (Data Build Tool) est un outil open source permettant de transformer des données en utilisant uniquement SQL.

Il est aujourd'hui l'un des outils les plus utilisés dans les projets Analytics Engineering.

L'idée principale est simple :

* les données brutes sont stockées dans une base de données ;
* dbt exécute des transformations SQL ;
* les résultats sont stockés sous forme de tables ou de vues ;
* toutes les transformations sont versionnées dans Git.

## Pourquoi utiliser dbt ?

Dans un projet classique, les transformations SQL sont souvent dispersées :

```text
script1.sql
script_final.sql
script_final_v2.sql
script_final_v2_ok.sql
```

Il devient difficile de :

* comprendre les dépendances ;
* reproduire les traitements ;
* collaborer ;
* documenter les transformations.

dbt apporte :

* la gestion des dépendances ;
* la documentation automatique ;
* les tests de qualité ;
* l'intégration Git ;
* l'automatisation des transformations.

## Architecture dbt

```text
Sources
(CSV, Parquet, Base SQL)
          │
          ▼

      DuckDB (Tables brutes Bronze)

          │
          ▼

        dbt (transformation)

          │
          ▼

   DuckDB (Tables analytiques Silver/Gold)
```


# Installation de dbt

## Installation avec pip

* Créer un environnement virtuel :

```bash
python -m venv venv
```

* Activation :

- Windows :

```bash
venv\Scripts\activate
```

- Linux / macOS :

```bash
source venv/bin/activate
```

* Installation :

```bash
pip install dbt-core
pip install dbt-duckdb
```

## Vérification

```bash
dbt --version
```

Exemple :

```text
Core:
  - installed: 1.10.x

Plugins:
  - duckdb: 1.10.x
```

# Création d'un projet dbt

Créer un nouveau projet :

```bash
dbt init duckdb_demo
```

Se déplacer dans le projet :

```bash
cd duckdb_demo
```

Structure générée :

```text
duckdb_demo/
├── models/
├── seeds/
├── tests/
├── macros/
├── dbt_project.yml
└── profiles.yml
```

# Configuration de DuckDB avec dbt

Créer le fichier :

```text
~/.dbt/profiles.yml
```

Configuration :

```yaml
duckdb_demo:
  target: dev

  outputs:
    dev:
      type: duckdb
      path: duckdb_demo.duckdb
      threads: 4
```

## Vérification de la connexion

```bash
dbt debug
```

Résultat attendu :

```text
Connection test: OK
```

# Premier modèle dbt

Créer :

```text
models/customers.sql
```

```sql
SELECT
    1 AS id,
    'Alice' AS customer_name

UNION ALL

SELECT
    2,
    'Bob'  AS customer_name
```

## Exécution

```bash
dbt run
```

dbt crée automatiquement la table :

```text
customers
```

dans DuckDB.

# Utilisation de données CSV avec dbt

Créer :

```text
seeds/customers.csv
```

```csv
id,name,city
1,Alice,Paris
2,Bob,Lyon
3,Charlie,Marseille
```

## Chargement dans DuckDB

```bash
dbt seed
```
dbt importe automatiquement le CSV.

## Vérification

```bash
dbt run
```

Les données sont maintenant disponibles dans DuckDB.

# Référencer un modèle avec ref()

Créer :

```text
models/customers_by_city.sql
```

```sql
SELECT
    city,
    COUNT(*) AS nb_customers
FROM {{ ref('customers') }}
GROUP BY city
```

## Pourquoi utiliser ref() ?

Sans ref :

```sql
FROM customers
```

Avec ref :

```sql
FROM {{ ref('customers') }}
```

Avantages :

* gestion automatique des dépendances ;
* traçabilité ;
* exécution dans le bon ordre ;
* documentation.

# 20. Les tests de qualité de données

Créer :

```text
models/schema.yml
```

```yaml
version: 2

models:
  - name: customers

    columns:
      - name: id

        tests:
          - unique
          - not_null
```

## Exécution des tests

```bash
dbt test
```

dbt vérifie :

* qu'il n'y a pas de doublons ;
* qu'il n'y a pas de valeurs nulles.

## Pourquoi tester les données ?

Les tests permettent de détecter :

* des erreurs d'import ;
* des données corrompues ;
* des régressions dans les pipelines.

Dans les entreprises, ces tests sont souvent exécutés automatiquement dans GitHub Actions.

# Documentation automatique

Générer la documentation :

```bash
dbt docs generate
```

Lancer le serveur :

```bash
dbt docs serve
```

Une interface web est générée automatiquement.

Elle permet de visualiser :

* les modèles ;
* les colonnes ;
* les dépendances ;
* les tests.


# Cas pratique : Pipeline Analytics

Nous disposons des fichiers :

```text
sales_2024_01.csv
sales_2024_02.csv
sales_2024_03.csv
```

Objectif :

Créer une table analytique consolidée.

## Étape 1 : Import

```bash
dbt seed
```

## Étape 2 : Modèle intermédiaire **sales_all.sql**

```sql
SELECT *
FROM {{ ref('sales_2024_01') }}

UNION ALL

SELECT *
FROM {{ ref('sales_2024_02') }}

UNION ALL

SELECT *
FROM {{ ref('sales_2024_03') }}
```

## Étape 3 : Table analytique

```sql
SELECT
    customer_id,
    SUM(amount) AS revenue
FROM {{ ref('sales_all') }}
GROUP BY customer_id
```

## Étape 4 : Exécution

```bash
dbt run
```

## Étape 5 : Validation

```bash
dbt test
```

# 23. dbt et GitHub

Dans un projet professionnel :

```text
GitHub
   │
   ▼

Pull 

   │
   ▼

dbt run

   │
   ▼

dbt test

   │
   ▼

Validation
```

Chaque modification :

* est revue ;
* est testée ;
* est tracée.

Cette approche constitue la base du DevOps/DataOps moderne.

# 24. Exercice pratique dbt

Créer un projet dbt utilisant DuckDB.

Objectifs :

1. Importer le dataset Titanic.
2. Créer un modèle `passengers`.
3. Créer un modèle `survival_stats`.
4. Ajouter un test sur l'identifiant des passagers.
5. Générer la documentation.
6. Versionner le projet dans GitHub.

Livrables :

* dépôt GitHub ;
* projet dbt fonctionnel ;
* documentation générée ;
* résultats des tests.
