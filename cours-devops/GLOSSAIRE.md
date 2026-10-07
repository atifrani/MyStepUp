# 📖 Glossaire

> Un mot vous bloque ? Il est sûrement ici, expliqué simplement. Le chapitre où il apparaît pour la première fois est indiqué entre parenthèses.
> 💡 Utilisez `Ctrl + F` (ou `Cmd + F` sur Mac) pour chercher un mot dans cette page.

| Terme | Définition simple |
|---|---|
| **Agilité** (ch. 1) | Façon de mener un projet par petites étapes (sprints), en livrant souvent et en s'adaptant aux retours du client. Née du **Manifeste Agile** en 2001. |
| **Anti-pattern** (ch. 1) | Une idée qui semble bonne au départ, mais qui produit finalement l'inverse de l'effet recherché. |
| **Backlog** (ch. 1) | La liste priorisée de tout ce qu'il reste à faire sur un produit. |
| **Branche** / *branch* (ch. 4) | Une ligne de travail parallèle dans un dépôt Git, pour développer une nouveauté sans toucher à la version principale. |
| **Build** (ch. 1) | Étape où l'on assemble le code de tous les développeurs en un produit qui fonctionne. |
| **CALMS** (ch. 1) | Les 5 piliers du DevOps : Culture, Automatisation, Lean, Mesure, Sharing (partage). |
| **CI/CD** (ch. 1) | *Intégration continue / Déploiement continu* : à chaque modification, le code est automatiquement testé (CI), puis mis en production si tout va bien (CD). |
| **Clone** (ch. 4) | Copie complète d'un dépôt distant (fichiers + historique) sur votre ordinateur. Commande `git clone`. |
| **Commit** (ch. 3) | Une « photo » de l'état de vos fichiers à un instant donné, avec un message qui la décrit. Un point de sauvegarde. |
| **Conflit** (ch. 4) | Situation où deux personnes ont modifié la même partie d'un fichier : Git demande à un humain de choisir la bonne version. |
| **Conteneur** (ch. 1) | Une « boîte » (par exemple Docker) qui embarque une application et tout ce dont elle a besoin pour fonctionner partout de la même façon. |
| **Cycle en V** (ch. 1) | Méthode de projet traditionnelle : tout spécifier, puis tout développer, puis tout tester, puis tout livrer d'un coup. |
| **Débogueur** (ch. 2) | Outil qui exécute le code pas à pas pour trouver les erreurs (*bugs*). |
| **Déploiement** / *deploy* (ch. 1) | Installer une nouvelle version d'une application sur les serveurs. |
| **Dépôt** / *repository*, *repo* (ch. 3) | Le dossier d'un projet suivi par Git, avec tout son historique (rangé dans le dossier caché `.git`). |
| **Dépôt distant** / *remote* (ch. 4) | Une copie du dépôt hébergée ailleurs, par exemple sur GitHub. Son surnom habituel est `origin`. |
| **DevOps** (ch. 1) | Culture et ensemble de pratiques qui rapprochent les développeurs (DEV) et les opérationnels (OPS) pour livrer plus vite et plus sûrement. |
| **DORA** (ch. 1) | Programme de recherche qui mesure la performance DevOps avec 4 indicateurs : fréquence de déploiement, délai de mise en production, taux d'échec, temps de restauration. |
| **EDI** / *IDE* (ch. 2) | Environnement de Développement Intégré : logiciel qui regroupe tous les outils du développeur (éditeur, compilateur, débogueur...). Exemple : VS Code. |
| **Extension** (ch. 2) | Module qu'on ajoute à VS Code pour lui donner de nouvelles fonctions. |
| **Fork** (ch. 4) | Copie du dépôt de quelqu'un d'autre dans **votre** compte GitHub, pour y contribuer. |
| **Fusion** / *merge* (ch. 4) | Réunir le travail d'une branche dans une autre. |
| **Git** (ch. 3) | Logiciel de gestion de versions, créé par Linus Torvalds en 2005. |
| **GitHub** (ch. 4) | Site web qui héberge des dépôts Git et permet de collaborer (Pull Requests, Issues, Projects...). |
| **GitHub Flow** (ch. 4) | Méthode de travail : une branche par fonctionnalité → Pull Request → relecture → fusion dans `main`. |
| **`.gitignore`** (ch. 5) | Fichier qui liste ce que Git doit ignorer (fichiers temporaires, secrets, environnements virtuels...). |
| **HEAD** (ch. 3) | Le « marque-page » de Git : le commit sur lequel vous vous trouvez en ce moment. |
| **Index** / *staging area* (ch. 3) | La « salle d'attente » où l'on prépare ce qui partira dans le prochain commit (`git add`). |
| **Issue** (ch. 4) | Un ticket sur GitHub : bug, idée ou tâche à réaliser. |
| **Kanban** (ch. 1) | Tableau de suivi des tâches en colonnes : « À faire / En cours / Terminé ». |
| **Lean** (ch. 1) | Principe venu de Toyota : produire sans gaspillage, par petits lots, en s'améliorant en continu. |
| **`main`** / **`master`** (ch. 3) | Le nom de la branche principale d'un dépôt (`master` est l'ancien nom). |
| **Markdown** (ch. 2) | Format de texte simple pour écrire des documents mis en forme (`# titre`, `**gras**`...). Les fichiers ont l'extension `.md`. |
| **Mur de la confusion** (ch. 1) | La séparation entre les équipes DEV et OPS, causée par leurs objectifs opposés. |
| **Open source** (ch. 2) | Logiciel dont le code source est public, que chacun peut lire, utiliser et améliorer. |
| **Production** (ch. 1) | L'environnement « réel » où l'application est utilisée par les clients. |
| **Pull** (ch. 4) | Récupérer les commits du dépôt distant et les intégrer à son dépôt local. Commande `git pull`. |
| **Pull Request** / **PR** (ch. 4) | Demande de relecture et de fusion d'une branche, sur GitHub. |
| **Push** (ch. 4) | Envoyer ses commits locaux vers le dépôt distant. Commande `git push`. |
| **README** (ch. 4) | Fichier `README.md` qui présente un projet ; GitHub l'affiche sur la page d'accueil du dépôt. |
| **Release** (ch. 1 et 4) | Une version stable et publiée d'un logiciel. |
| **SHA-1** / *hash* (ch. 3) | L'identifiant unique d'un commit (40 caractères, souvent abrégés aux 7 premiers). |
| **Silo** (ch. 1) | Une équipe isolée qui travaille dans son coin, sans communiquer avec les autres. |
| **Sprint** (ch. 1) | Période courte (souvent 2 semaines) au bout de laquelle une équipe agile livre quelque chose qui fonctionne. |
| **Tag** (ch. 3) | Étiquette qui donne un nom lisible à un commit important, par exemple `v1.0`. |
| **Terminal** (ch. 2) | Fenêtre où l'on donne des ordres à l'ordinateur en tapant du texte. |
| **Time-to-Market** (ch. 1) | Le temps nécessaire pour qu'une idée devienne un produit disponible pour les clients. |
| **VS Code** (ch. 2) | Visual Studio Code : éditeur de code gratuit de Microsoft, le plus utilisé au monde. |
