## Introduction à Amazon DynamoDB

**Amazon DynamoDB** est un service de base de données **NoSQL entièrement managé** proposé par AWS. Contrairement aux bases de données relationnelles, DynamoDB permet de stocker et de gérer des données de manière **flexible, scalable et très performante**, sans avoir à gérer l’infrastructure.

DynamoDB est conçu pour offrir :

* une **latence très faible (millisecondes)**
* une **scalabilité automatique**
* une **haute disponibilité**
* une gestion entièrement **serverless**

### Fonctionnement de DynamoDB

DynamoDB stocke les données sous forme de :

* **tables**
* **items (éléments)** → équivalent d’une ligne
* **attributes (attributs)** → équivalent des colonnes

Cependant, contrairement aux bases relationnelles, les données ne sont pas strictement structurées. Chaque item peut avoir des attributs différents.

### Exemple de structure

```text
Table : Users

Item 1 :
{
  user_id: 1,
  name: "Alice",
  age: 25
}

Item 2 :
{
  user_id: 2,
  name: "Bob",
  email: "bob@example.com"
}
```

👉 Les deux items n’ont pas exactement les mêmes champs.

---

## Cas d’utilisation de DynamoDB

DynamoDB est particulièrement adapté pour :

* applications web à forte charge
* applications mobiles
* jeux en ligne
* IoT (objets connectés)
* systèmes nécessitant des performances élevées

---

## Différence entre DynamoDB et RDS PostgreSQL

Il est essentiel de bien comprendre la différence entre une base **NoSQL (DynamoDB)** et une base **relationnelle SQL (RDS PostgreSQL)**.

### 1. Modèle de données

| DynamoDB                         | RDS PostgreSQL                      |
| -------------------------------- | ----------------------------------- |
| NoSQL                            | Relationnel (SQL)                   |
| Données flexibles (schéma libre) | Schéma structuré (tables, colonnes) |
| Pas de relations complexes       | Relations entre tables (JOIN)       |

👉 DynamoDB est plus flexible, PostgreSQL est plus structuré.

---

### 2. Scalabilité

| DynamoDB                        | RDS PostgreSQL                          |
| ------------------------------- | --------------------------------------- |
| Scalabilité automatique         | Scalabilité manuelle (verticale)        |
| Conçu pour des charges massives | Limité par les ressources de l’instance |

👉 DynamoDB est idéal pour les applications à très grande échelle.

---

### 3. Performance

| DynamoDB                   | RDS PostgreSQL                                    |
| -------------------------- | ------------------------------------------------- |
| Latence très faible (ms)   | Bonne performance mais dépend de l’infrastructure |
| Optimisé pour accès rapide | Optimisé pour requêtes complexes                  |

👉 DynamoDB est optimisé pour la vitesse, PostgreSQL pour la complexité des requêtes.

---

### 4. Requêtes

| DynamoDB                      | RDS PostgreSQL                  |
| ----------------------------- | ------------------------------- |
| Pas de SQL (requêtes simples) | Utilise SQL (requêtes avancées) |
| Pas de JOIN                   | Support des JOIN                |

👉 PostgreSQL est plus adapté pour les analyses complexes.

---

### 5. Gestion

| DynamoDB                    | RDS PostgreSQL                   |
| --------------------------- | -------------------------------- |
| Serverless (aucune gestion) | Gestion partielle (instance RDS) |
| Pas de maintenance          | Maintenance gérée mais présente  |

---

### 6. Cas d’usage

| DynamoDB                | RDS PostgreSQL                |
| ----------------------- | ----------------------------- |
| Applications temps réel | Applications métier           |
| Données non structurées | Données relationnelles        |
| IoT, gaming, mobile     | ERP, CRM, systèmes financiers |

---

## Résumé

* **DynamoDB** :

  * base NoSQL
  * très scalable
  * rapide
  * flexible
  * idéale pour applications modernes à grande échelle

* **RDS PostgreSQL** :

  * base relationnelle SQL
  * adaptée aux données structurées
  * support des relations complexes
  * idéale pour applications métier

👉 Le choix entre DynamoDB et RDS dépend principalement :

* du **type de données**
* du **niveau de scalabilité**
* de la **complexité des requêtes**
* des **besoins applicatifs**
