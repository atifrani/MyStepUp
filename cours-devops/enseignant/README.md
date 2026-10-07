# 🧑‍🏫 Guide enseignant · Environnement de développement : Agilité, Git & DevOps

> Ce dossier est réservé au professeur : déroulés minutés, scripts de démonstration, réponses aux quiz et corrigés. Les supports étudiants (`../1.DEVOPS.md` à `../7.DBT.md`) ne contiennent **aucune réponse** : les corrections se font en séance.

> [!WARNING]
> Si le dépôt GitHub du cours est **public**, ce dossier l'est aussi, et les étudiants peuvent y lire les corrigés. Pour l'éviter, conserver `enseignant/` dans un dépôt privé séparé (ou hors de GitHub) et ne publier que les supports étudiants.

---

## 1. Vue d'ensemble

| Chapitre | Support étudiant | Guide enseignant | Durée | Séances |
|---|---|---|---|---|
| 1 · Introduction au DevOps | [1.DEVOPS.md](../1.DEVOPS.md) | [1.DEVOPS.md](1.DEVOPS.md) | 2h | 1 |
| 2 · EDI et VS Code | [2.EDI.md](../2.EDI.md) | [2.EDI.md](2.EDI.md) | 2h | 1 |
| 3 · Introduction à Git | [3.GIT.md](../3.GIT.md) | [3.GIT.md](3.GIT.md) | 3h | 2 × 1h30 |
| 4 · Introduction à GitHub | [4.GITHUB.md](../4.GITHUB.md) | [4.GITHUB.md](4.GITHUB.md) | 3h30 | 2 × 1h45 |
| 5 · Streamlit | [5.STREAMLIT.md](../5.STREAMLIT.md) | [5.STREAMLIT.md](5.STREAMLIT.md) | 4h | 2 × 2h |
| 6 · DuckDB | [6.DUCKDB.md](../6.DUCKDB.md) | [6.DUCKDB.md](6.DUCKDB.md) | 3h30 | 2h + 1h30 |
| 7 · dbt | [7.DBT.md](../7.DBT.md) | [7.DBT.md](7.DBT.md) | 4h | 2 × 2h |
| Projets d'évaluation | [exercice_evaluation.md](../exercice_evaluation.md), [data-project.md](../data-project.md) | voir [7.DBT.md](7.DBT.md), partie lancement des projets | | |

**Le fil rouge.** Les étudiants sont les nouvelles recrues de **DataCafé**, une start-up qui vend du café en ligne. Léa est la responsable technique ; Karim, Sofia et Tom sont leurs collègues. Chaque chapitre s'ouvre sur un court épisode (🎬) à raconter à la classe en une minute, et se termine sur le suivant. La page web `site-datacafe` (chapitre 3) passe sur GitHub et en équipe (chapitre 4), puis l'équipe construit une application de données (Streamlit, DuckDB, dbt) qui prépare directement les projets d'évaluation.

**Le format d'une séance.** Chaque support enchaîne des blocs repérés par un pictogramme. Le guide de chaque chapitre donne, séance par séance, le minutage bloc par bloc.

| Bloc | Ce que fait le professeur | Ce que font les étudiants |
|---|---|---|
| 💬 Tour de table / Débat | Pose les questions, fait réagir, note les idées au tableau | Répondent, discutent |
| 🎤 Cours | Présente la notion en s'appuyant sur la trace écrite (schémas, analogies 💡) | Écoutent, posent des questions |
| 👀 Démonstration | Fait la manipulation en direct au vidéoprojecteur, en commentant | Observent (sans taper) |
| 🛠️ À vous de jouer | Circule, débloque, oriente les rapides vers 🟡 et 🔴 | Reproduisent seuls, sur leur machine |
| 👥 En équipe | Lance le chrono, arbitre, note les points | Travaillent par équipes de 4 |
| 🧠 Quiz en classe | Projette les questions, fait voter, corrige avec le guide | Votent, expliquent leurs choix |
| 📌 À retenir | Résume en 2 minutes | Notent |
| 🏠 Avant la prochaine séance | Rappelle les installations et comptes à préparer | Préparent leur machine |

**Principe de rythme :** jamais plus de 15 à 20 minutes de 🎤 Cours d'affilée. Chaque notion suit la boucle **Cours → Démonstration → À vous de jouer**, puis un quiz court pour vérifier.

---

## 2. Préparation du cours

### Avant la première séance

- [ ] Constituer les **équipes de 4** (mêmes équipes que pour le projet d'évaluation), en mélangeant les niveaux : au moins un étudiant à l'aise avec l'informatique par équipe si possible.
- [ ] Préparer le **tableau des scores** des équipes (tableau blanc, slide ou tableur partagé).
- [ ] Choisir l'outil de quiz : cartons A/B/C/D, main levée, ou outil en ligne (Wooclap, Kahoot, Mentimeter). Les questions des supports peuvent être recopiées telles quelles.
- [ ] Envoyer aux étudiants la liste des **installations** à faire avant le chapitre 2 (VS Code) et le chapitre 3 (Git), et la création d'un **compte GitHub** avant le chapitre 4.

### Le poste du professeur

- VS Code, Git, Python, DuckDB et dbt installés et testés (les commandes DuckDB, dbt et Streamlit des chapitres 5 à 7 n'ont pas pu être exécutées lors de la rédaction : les jouer une fois avant la séance).
- Un compte GitHub de démonstration, connecté dans le navigateur et dans VS Code.
- Une police de terminal et d'éditeur **agrandie** pour le vidéoprojecteur (`Ctrl + +` dans VS Code), et un thème clair si la salle est lumineuse.
- Un dossier de démonstration vierge, pour refaire les manipulations « comme les étudiants ».

---

## 3. Le jeu des équipes 🏆

Le côté ludique du cours repose sur une compétition **bienveillante entre équipes**, sur toute la durée du cours. Les points ne comptent pas dans la note : ils servent à rythmer les séances et à motiver.

**Barème commun (à adapter librement) :**

| Action | Points |
|---|---|
| Bonne réponse d'équipe à une question de quiz | 1 pt |
| Première équipe à réussir un défi 👥 (puis 2ᵉ, 3ᵉ) | 3 / 2 / 1 pts |
| Défi 🔴 réussi par un membre de l'équipe | 2 pts |
| Aide efficace à une autre équipe (validée par le professeur) | 1 pt |
| Pull Request propre et relue (à partir du chapitre 4) | 1 pt |

Chaque guide de chapitre propose en plus un ou deux **défis d'équipe** spécifiques, avec leur barème.

**Les titres de chapitre** (🧱 Briseur de murs, 🛠️ Artisan de l'éditeur, ⏳ Maître du temps, 🤝 Joueur d'équipe, ☕📊 Barista de la data, 🕵️🦆 Détective des données, 🏗️ Architecte des données) peuvent être annoncés en fin de séance, par exemple pour l'équipe qui a le plus de points sur le chapitre. En fin de cours, l'équipe en tête du classement général reçoit le titre de **Maître DevOps de DataCafé** 🏆.

---

## 4. Gérer l'hétérogénéité

- **Binômes mixtes** pendant les 🛠️ : placer un étudiant à l'aise à côté d'un débutant, avec pour consigne « le débutant tape, l'expérimenté guide ».
- **Niveaux 🟢 🟡 🔴** : tout le monde fait le 🟢 ; ceux qui ont fini passent au 🟡 puis au 🔴 sans attendre. Ne corriger collectivement que le 🟢.
- **Référents** : à partir du chapitre 3, désigner un « référent Git » par équipe parmi les plus à l'aise, qui débloque ses coéquipiers avant d'appeler le professeur.
- **Encadrés 🆘** des supports : renvoyer d'abord les étudiants vers ces encadrés, ils couvrent les blocages les plus fréquents.

---

## 5. Antisèche et glossaire

- [MEMO-GIT.md](../MEMO-GIT.md) : toutes les commandes Git des chapitres 2 à 4. À projeter pendant les 🛠️ ou à distribuer imprimée.
- [GLOSSAIRE.md](../GLOSSAIRE.md) : tous les termes techniques, avec le chapitre où ils apparaissent.

La version précédente du cours, conçue pour l'autoformation (réponses repliées dans les supports, points d'expérience individuels), est conservée dans l'archive `cours-devops-git-autonomie.zip`.
