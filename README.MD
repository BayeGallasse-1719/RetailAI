# RetailAI Intelligence Platform
### Enterprise Data Platform — Data Engineering, Machine Learning & Generative AI

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-336791.svg)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Container-Docker-2496ED.svg)](https://www.docker.com/)
[![Machine Learning](https://img.shields.io/badge/AI-Machine%20Learning-orange.svg)]()
[![Generative AI](https://img.shields.io/badge/AI-Generative%20AI-purple.svg)]()
[![Status](https://img.shields.io/badge/Status-In%20Development-yellow.svg)]()

---

## 1. Présentation du projet

**RetailAI Intelligence Platform** est un projet de portfolio visant à concevoir et
développer une plateforme intégrée de données et d'intelligence artificielle
appliquée au commerce électronique.

Le projet s'appuie sur le dataset public Olist pour construire une chaîne de
traitement de données, développer plusieurs modèles de Machine Learning,
exploiter des documents métier à travers un système RAG et exposer les
fonctionnalités via une API.

L'objectif est de reproduire une démarche proche de celle utilisée dans un
projet professionnel : comprendre le besoin métier, concevoir une architecture,
préparer les données, expérimenter, évaluer, tester, documenter et industrialiser.

Le projet est développé progressivement, avec une attention particulière portée
à la qualité du code, à la reproductibilité des résultats, à la traçabilité des
expériences et à la compréhension des choix techniques.

> **Statut :** projet en cours de développement. Les composants seront considérés
> comme terminés uniquement après leur implémentation et leur validation.

---

## 2. Objectifs

### 2.1. Data Engineering

- Concevoir un pipeline d'ingestion de données avec Python.
- Stocker les données dans PostgreSQL.
- Organiser les données en couches Bronze, Silver et Gold.
- Transformer les données avec dbt Core.
- Automatiser les traitements avec Apache Airflow.
- Ajouter des contrôles de qualité et des journaux d'exécution.
- Rendre les traitements reproductibles et maîtriser les relances.

### 2.2. Data Science et Machine Learning

Développer plusieurs cas d'usage à partir des données Olist.

#### A. Prédiction des retards de livraison

Construire un modèle permettant d'estimer le risque de retard d'une commande
à partir des informations disponibles au moment de la prédiction.

Algorithmes envisagés :
- Régression logistique ;
- Random Forest ;
- Gradient Boosting.

L'évaluation prendra en compte les déséquilibres éventuels entre classes,
les métriques adaptées au problème et les risques de fuite de données.

#### B. Segmentation des clients

Identifier des groupes de clients à partir de leurs comportements d'achat.

Approches envisagées :
- Analyse RFM : récence, fréquence et montant des achats ;
- K-Means ;
- Clustering hiérarchique ;
- DBSCAN, si pertinent.

L'objectif est de produire des segments interprétables et utiles pour l'analyse
commerciale, et non simplement de créer des groupes mathématiques.

#### C. Prévision des ventes

Construire une série temporelle agrégée à partir des commandes et comparer
plusieurs approches de prévision.

Approches envisagées :
- Prévision naïve servant de référence ;
- Modèle statistique, par exemple ARIMA si les données le permettent ;
- Random Forest avec variables calendaires et variables retardées.

L'évaluation reposera notamment sur MAE et RMSE, avec une séparation
chronologique des données.

#### D. Détection d'anomalies

Repérer les variations inhabituelles dans les volumes de commandes ou les ventes.

Approches envisagées :
- Méthodes statistiques ;
- Isolation Forest.

Une anomalie statistique ne sera pas automatiquement considérée comme une fraude.
Les résultats devront être examinés et interprétés.

### 2.3. MLOps et suivi des expériences

- Suivre les paramètres et métriques avec MLflow.
- Enregistrer les résultats des expériences.
- Sauvegarder les modèles retenus.
- Tester le chargement et l'utilisation des modèles.
- Documenter les protocoles d'entraînement et d'évaluation.
- Favoriser la reproductibilité des expériences.

### 2.4. IA générative et RAG

Développer un système de question-réponse documentaire fondé sur une approche
RAG (*Retrieval-Augmented Generation*), sans utiliser de framework RAG dédié.

Le pipeline prévu comprend :

1. Extraction du texte depuis des documents PDF ou DOCX.
2. Découpage du texte en segments.
3. Génération d'embeddings via un service externe.
4. Stockage des vecteurs dans PostgreSQL avec pgvector.
5. Recherche des segments pertinents.
6. Construction du contexte transmis au modèle de langage.
7. Génération d'une réponse avec l'API Groq.
8. Restitution des sources et gestion des questions insuffisamment documentées.

La qualité du système sera évaluée sur un ensemble de questions de test,
notamment des questions avec réponse connue, ambiguë ou absente du corpus.

**Important :** Groq sera utilisé pour la génération des réponses.
Le fournisseur d'embeddings sera sélectionné et testé séparément.

### 2.5. Développement et industrialisation

- Exposer les fonctionnalités avec FastAPI.
- Construire des tableaux de bord avec Metabase.
- Automatiser certains processus métier avec n8n.
- Tester les composants avec pytest.
- Mettre en place des contrôles automatisés avec GitHub Actions.
- Conteneuriser les services avec Docker Compose.
- Documenter l'installation, l'architecture et les limites du système.

---

## 3. Données utilisées

Le projet utilise le dataset public **Brazilian E-Commerce Public Dataset by Olist**,
disponible sur Kaggle :

https://www.kaggle.com/olistbr/brazilian-ecommerce

Le dataset contient des informations historiques relatives au commerce
électronique, notamment :

- Commandes ;
- Clients ;
- Produits ;
- Vendeurs ;
- Paiements ;
- Articles commandés ;
- Avis clients ;
- Informations géographiques ;
- Traductions de catégories de produits.

Les fichiers réellement disponibles seront inventoriés au début du projet.
Les traitements seront adaptés aux colonnes et aux relations effectivement
présentes dans la version téléchargée.

### Limites des données

Les données Olist sont historiques et ne représentent pas nécessairement les
conditions actuelles du commerce électronique.

Les modèles développés dans ce projet constituent des études expérimentales.
Leurs performances ne garantissent pas des résultats équivalents en production.

---

## 4. Architecture cible

L'architecture est organisée autour de trois ensembles :
la plateforme de données, les services de Machine Learning et le système
d'IA générative.

### 4.1. Pipeline de données

```text
Dataset Olist (CSV)
        |
        v
Python — Ingestion
        |
        v
PostgreSQL — Bronze
        |
        v
dbt Core — Transformations
        |
        +---------------------+
        |                     |
        v                     v
Silver / Gold             Tests de qualité
        |
        +---------------------+
        |          |          |
        v          v          v
    Metabase      ML       FastAPI
                   |
                   v
                 MLflow
```

Apache Airflow assurera l'orchestration des traitements de données et,
progressivement, des pipelines de Machine Learning.

### 4.2. Pipeline RAG

```text
Documents PDF / DOCX
        |
        v
Extraction du texte
        |
        v
Découpage en segments
        |
        v
Génération d'embeddings via API externe
        |
        v
PostgreSQL + pgvector
        |
        v
Recherche de segments pertinents
        |
        v
Construction du contexte
        |
        v
API Groq — Génération de réponse
        |
        v
Réponse avec sources
```

La logique de recherche, de récupération et de construction du contexte sera
implémentée en Python, sans framework RAG dédié.

### 4.3. Assistant hybride

L'assistant devra distinguer deux catégories de questions :

- **Questions analytiques :** réponses fondées sur les indicateurs et les données
  structurées, via des requêtes autorisées.
- **Questions documentaires :** réponses fondées sur les documents indexés
  dans le système RAG.

L'accès analytique sera limité à des opérations en lecture seule et à des
requêtes contrôlées. Le modèle de langage ne devra pas disposer d'un accès
libre à la base de données.

---

## 5. Technologies prévues

| Domaine | Technologies |
|---|---|
| Langage | Python |
| Base de données | PostgreSQL |
| Recherche vectorielle | pgvector |
| Ingestion | Python, pandas |
| Transformation | dbt Core |
| Orchestration | Apache Airflow |
| Machine Learning | scikit-learn |
| Analyse des données | pandas, NumPy |
| Suivi des expériences | MLflow |
| IA générative | API Groq |
| Embeddings | Fournisseur externe à sélectionner |
| RAG | Python, implémentation manuelle |
| API | FastAPI, Pydantic |
| Visualisation | Metabase |
| Automatisation métier | n8n |
| Tests | pytest |
| Qualité du code | Ruff ou outil équivalent |
| Conteneurisation | Docker, Docker Compose |
| CI | GitHub Actions |
| Versionnement | Git, GitHub |

Les technologies seront intégrées progressivement. Leur présence dans cette
liste décrit l'architecture cible, et non un état d'avancement déjà validé.

---

## 6. Organisation du projet

Structure cible proposée :

```text
retailai-intelligence/
├── data/
│   ├── raw/
│   └── processed/
├── src/
│   ├── ingestion/
│   ├── ml/
│   ├── rag/
│   └── api/
├── tests/
├── dbt/
├── airflow/
│   └── dags/
├── dashboards/
├── docs/
├── scripts/
├── docker-compose.yml
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

Cette structure pourra évoluer selon les besoins réels du projet.

Les données brutes, les secrets, les environnements virtuels et les artefacts
volumineux ne devront pas être versionnés sans justification.

---

## 7. Plan de réalisation

Le projet est planifié sur **15 jours, à raison de 4 heures par jour**,
soit environ 60 heures de travail.

| Jour | Objectif |
|---|---|
| 1 | Cadrage, environnement, Git et structure du projet |
| 2 | Exploration des données Olist et PostgreSQL |
| 3 | Pipeline d'ingestion Python |
| 4 | Qualité des données et tests |
| 5 | dbt et modélisation analytique |
| 6 | Prédiction des retards de livraison |
| 7 | Segmentation des clients |
| 8 | Prévision des ventes et détection d'anomalies |
| 9 | MLflow et reproductibilité des expériences |
| 10 | Orchestration avec Airflow |
| 11 | Dashboard Metabase et API analytique |
| 12 | Ingestion documentaire et indexation RAG |
| 13 | Recherche RAG, génération Groq et évaluation |
| 14 | Assistant hybride et automatisation n8n |
| 15 | Tests finaux, CI, documentation et démonstration |

Ce calendrier constitue un objectif de travail. Les tâches seront validées sur
la base de résultats observables et non uniquement selon le temps passé.

---

## 8. Qualité, tests et reproductibilité

La qualité du projet sera évaluée sur plusieurs dimensions.

### Données
- Comparaison des volumes source et cible ;
- Contrôles des valeurs manquantes ;
- Détection des doublons ;
- Vérification des clés et relations ;
- Reproductibilité de l'ingestion ;
- Tests des transformations dbt.

### Machine Learning
- Présence d'une baseline ;
- Comparaison de plusieurs algorithmes ;
- Stratégie de validation adaptée ;
- Prévention des fuites de données ;
- Métriques pertinentes ;
- Analyse des erreurs ;
- Traçabilité avec MLflow.

### RAG
- Traçabilité des sources ;
- Pertinence des segments récupérés ;
- Citations vérifiables ;
- Tests de questions connues, ambiguës et inconnues ;
- Gestion des contextes insuffisants ;
- Protection des clés API.

### API et industrialisation
- Tests des endpoints ;
- Gestion des erreurs ;
- Documentation OpenAPI ;
- Tests automatisés ;
- Vérification de la configuration ;
- Intégration continue ;
- Instructions reproductibles d'installation.

---

## 9. Sécurité et configuration

Les clés API ne doivent jamais être inscrites directement dans le code.

La configuration sera gérée au moyen de variables d'environnement et d'un
fichier `.env` ignoré par Git.

Un fichier `.env.example` pourra documenter les variables nécessaires sans
contenir de secrets réels.

Exemple indicatif :

```dotenv
GROQ_API_KEY=
EMBEDDINGS_API_KEY=
DATABASE_URL=
```

Les noms définitifs dépendront de l'implémentation.

Le compte utilisé par l'assistant analytique devra être limité aux opérations
autorisées. Les requêtes arbitraires et les accès non nécessaires seront évités.

---

## 10. Critères de réussite

Le projet sera considéré comme présentable lorsque les points suivants auront
été validés :

- [ ] Le dépôt est organisé et documenté.
- [ ] Les données Olist sont chargées dans PostgreSQL.
- [ ] Le pipeline d'ingestion peut être relancé de façon contrôlée.
- [ ] Les contrôles de qualité sont exécutables.
- [ ] Les modèles dbt sont testés et documentés.
- [ ] Les pipelines peuvent être exécutés depuis Airflow.
- [ ] Plusieurs algorithmes ont été comparés pour les cas d'usage ML retenus.
- [ ] Les expériences sont enregistrées dans MLflow.
- [ ] Les résultats ML sont interprétés et leurs limites documentées.
- [ ] Le dashboard présente des indicateurs vérifiés.
- [ ] Le RAG retrouve des segments pertinents et restitue leurs sources.
- [ ] L'assistant utilise Groq et gère les questions sans réponse suffisante.
- [ ] Les endpoints FastAPI sont testés.
- [ ] Le workflow n8n est démontré.
- [ ] Les tests automatisés sont exécutables.
- [ ] La CI est configurée et son résultat est vérifiable.
- [ ] Aucun secret n'est versionné.
- [ ] La démonstration peut être reproduite à partir de la documentation.

---

## 11. Évolutions futures

Les fonctionnalités suivantes sont envisagées après stabilisation du MVP.

### V2 — Streaming et amélioration des modèles
- Simulation d'événements de commandes ;
- Kafka et consommateur Python ;
- Mesure de la latence et du débit ;
- Amélioration de la recherche RAG ;
- Évaluation plus approfondie des modèles ;
- Suivi de la dérive des données.

### V3 — Déploiement et observabilité
- Déploiement sur un serveur ou dans le cloud ;
- Gestion plus avancée des secrets ;
- Authentification et autorisations API ;
- Logs structurés et alertes ;
- Surveillance des performances.

### V4 — Évolutions Big Data
- Étude de Spark si les volumes ou les traitements le justifient ;
- Comparaison des architectures ;
- Mesures de performance ;
- Justification technique du choix d'architecture.

Ces évolutions ne font pas partie des critères obligatoires du MVP.

---

## 12. Discipline et journal d'apprentissage

Ce projet est également un exercice de rigueur et d'autonomie.

À la fin de chaque session, les éléments suivants seront consignés :

1. Travail réalisé ;
2. Concepts compris ;
3. Difficultés rencontrées ;
4. Méthodes de diagnostic utilisées ;
5. Résultats vérifiés ;
6. Prochaines actions.

Une fonctionnalité ne sera pas considérée comme terminée uniquement parce
qu'elle fonctionne une fois. Elle devra être testée, comprise et documentée
à un niveau adapté à son importance.

---

## 13. Limites connues

- Les données Olist sont historiques.
- Les modèles seront évalués dans un contexte expérimental.
- Les performances observées ne garantissent pas celles d'un système réel.
- La disponibilité des embeddings dépend du fournisseur externe sélectionné.
- Le déploiement cloud n'est pas garanti dans le périmètre initial.
- Le streaming et Spark restent des évolutions conditionnelles.
- Les performances du RAG dépendront du corpus, de la stratégie de recherche
  et du modèle de génération.

---

## 14. Ressources

- Dataset Olist :
  https://www.kaggle.com/olistbr/brazilian-ecommerce

- Python :
  https://www.python.org/

- PostgreSQL :
  https://www.postgresql.org/

- dbt :
  https://docs.getdbt.com/

- Apache Airflow :
  https://airflow.apache.org/docs/

- scikit-learn :
  https://scikit-learn.org/stable/

- MLflow :
  https://mlflow.org/docs/latest/

- FastAPI :
  https://fastapi.tiangolo.com/

- Groq :
  https://console.groq.com/docs

- Docker :
  https://docs.docker.com/

---

## 15. Auteur et statut

**Projet :** RetailAI Intelligence Platform  
**Domaine :** Data Engineering, Data Science, Machine Learning et IA générative  
**Statut :** En cours de développement  
**Planning initial :** 15 jours × 4 heures  
**Données :** Brazilian E-Commerce Public Dataset by Olist

Ce projet est réalisé dans un objectif d'apprentissage pratique, de consolidation
des compétences techniques et de développement de méthodes de travail adaptées
aux projets professionnels.

L'objectif final est de construire une solution reproductible, testée,
documentée et suffisamment maîtrisée pour pouvoir en expliquer l'architecture,
les choix techniques, les résultats et les limites.