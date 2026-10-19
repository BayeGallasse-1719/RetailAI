# RetailAI Intelligence Platform

> Plateforme intelligente de données commerciales  
> Data Engineering • MLOps • Business Intelligence • IA générative

**Dataset** : [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
**Durée MVP** : 6 jours  
**Objectif** : Concevoir et réaliser une plateforme Data moderne avec une rigueur compatible avec un contexte professionnel.

---

## Table des matières

- [1. Vue d'ensemble](#1-vue-densemble)
- [2. Architecture](#2-architecture)
- [3. Stack technique](#3-stack-technique)
- [4. Prérequis](#4-prérequis)
- [5. Installation](#5-installation)
- [6. Structure du projet](#6-structure-du-projet)
- [7. Modules fonctionnels](#7-modules-fonctionnels)
- [8. Configuration](#8-configuration)
- [9. Utilisation](#9-utilisation)
- [10. Tests et CI/CD](#10-tests-et-cicd)
- [11. Principes d'ingénierie](#11-principes-dingénierie)
- [12. Perspectives](#12-perspectives)

---

## 1. Vue d'ensemble

RetailAI Intelligence Platform est une plateforme de données de bout en bout destinée à une entreprise de commerce électronique. Elle permet de :

- Ingester et transformer des données commerciales (dataset Olist)
- Produire des indicateurs métiers fiables (couche Gold)
- Détecter des anomalies de ventes via Machine Learning
- Interroger des documents internes via un système RAG
- Combiner requêtes analytiques SQL et recherche documentaire dans un assistant conversationnel
- Industrialiser l'ensemble avec Docker, orchestration et CI/CD

Le projet met en œuvre une **architecture Medallion** (Bronze / Silver / Gold) et respecte des principes d'ingénierie d'entreprise : idempotence, tests de données, traçabilité, séparation des responsabilités et documentation des trade-offs.

---

## 2. Architecture


┌─────────────────────────────────────────────────────────────────────┐
│                        Couche Présentation                          │
│   Metabase (BI)  │  FastAPI + Assistant  │  n8n (automatisations)   │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                        Couche Services                              │
│              FastAPI (endpoints SQL + RAG + hybrid)                 │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                     Couche Intelligence                             │
│   IsolationForest + MLflow   │   RAG (pgvector + Groq)              │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                      Couche Données                                 │
│  Bronze (raw)  →  Silver (cleaned)  →  Gold (analytics)             │
│                     PostgreSQL + pgvector                           │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                   Orchestration & Ingestion                         │
│         Apache Airflow (DAGs data + ML)  │  dbt Core                │
│                    Sources : CSV Olist                              │
└─────────────────────────────────────────────────────────────────────┘
```

### Flux principaux

1. **Pipeline Data** (Airflow)  
   CSV Olist → validation → Bronze → dbt (Silver/Gold) → tests qualité

2. **Pipeline ML** (Airflow)  
   Données Gold → préparation → IsolationForest → tracking MLflow → artefact

3. **RAG**  
   Documents métier → chunking → embeddings locaux → pgvector → génération Groq

4. **Assistant hybride**  
   Question utilisateur → routage (SQL / RAG / hybride) → réponse + sources

---

## 3. Stack technique

| Domaine              | Technologie                  | Rôle                                      |
|----------------------|------------------------------|-------------------------------------------|
| Base de données      | PostgreSQL + pgvector        | Stockage relationnel + embeddings         |
| Transformation       | dbt Core                     | Modélisation Silver/Gold + tests          |
| Orchestration        | Apache Airflow               | Pipelines data et ML                      |
| Automatisation       | n8n                          | Workflows métier / webhooks               |
| Business Intelligence| Metabase                     | Tableaux de bord                          |
| Machine Learning     | scikit-learn (IsolationForest)| Détection d'anomalies                    |
| MLOps                | MLflow                       | Tracking d'expériences + artefacts        |
| LLM                  | Groq (API)                   | Génération de réponses                    |
| Embeddings           | sentence-transformers        | Vectorisation locale (zéro coût)          |
| API                  | FastAPI                      | Exposition des services                   |
| Conteneurs           | Docker Compose               | Environnement reproductible               |
| CI/CD                | GitHub Actions               | Tests automatisés                         |

---

## 4. Prérequis

- Ubuntu (ou équivalent) avec **16 Go RAM minimum**
- Docker & Docker Compose
- Python 3.11+
- Git
- Compte Groq (clé API gratuite)
- (Optionnel) Compte Kaggle pour télécharger le dataset Olist

---

## 5. Installation

### 5.1 Cloner le dépôt

```bash
git clone <url-du-repo>
cd retailai-intelligence-platform
```

### 5.2 Configuration des secrets

```bash
cp .env.example .env
```

Renseigner au minimum :

```env
POSTGRES_USER=retailai
POSTGRES_PASSWORD=********
POSTGRES_DB=retailai
GROQ_API_KEY=gsk_********
```

> **Important** : les secrets ne doivent jamais être commités (`.env` est dans `.gitignore`).

### 5.3 Lancer l'environnement

```bash
docker compose up -d
```

Services démarrés :
- PostgreSQL + pgvector
- Airflow (webserver + scheduler)
- Metabase
- MLflow
- FastAPI
- n8n

### 5.4 Initialisation

```bash
# Création des schémas Medallion
make init-db

# Chargement des données Olist (Bronze)
make ingest

# Exécution des transformations dbt
make dbt-run

# Indexation des documents (RAG)
make index-docs
```

---

## 6. Structure du projet

```
retailai-intelligence-platform/
├── airflow/
│   ├── dags/
│   │   ├── retail_data_pipeline.py
│   │   └── retail_ml_pipeline.py
│   └── plugins/
├── dbt/
│   ├── models/
│   │   ├── bronze/
│   │   ├── silver/
│   │   └── gold/
│   ├── tests/
│   └── dbt_project.yml
├── api/
│   ├── app/
│   │   ├── main.py
│   │   ├── routers/
│   │   ├── services/          # SQL, RAG, hybrid
│   │   └── schemas/
│   └── requirements.txt
├── ml/
│   ├── train_anomaly.py
│   └── evaluate.py
├── rag/
│   ├── ingest_docs.py
│   ├── chunking.py
│   └── embeddings.py
├── docs/
│   ├── architecture.md
│   ├── adr/                   # Architecture Decision Records
│   └── data_dictionary.md
├── tests/
├── .github/workflows/
├── docker-compose.yml
├── Makefile
├── .env.example
└── README.md
```

---

## 7. Modules fonctionnels

### 7.1 Ingestion & Architecture Medallion

| Couche  | Contenu                                      | Outil      |
|---------|----------------------------------------------|------------|
| Bronze  | Données brutes Olist (types minimaux)        | Python     |
| Silver  | Nettoyage, dédoublonnage, cohérence          | dbt        |
| Gold    | Tables analytiques (CA, ventes, retours…)    | dbt        |

Sources Olist utilisées : `orders`, `order_items`, `customers`, `products`, `order_payments`, `order_reviews`, `sellers`, `geolocation`.

Le pipeline d'ingestion est **idempotent**.

### 7.2 Transformations dbt

```bash
cd dbt
dbt run --select silver
dbt run --select gold
dbt test
```

Tests de qualité inclus : unicité des clés, non-nullité, relations, plages de valeurs.

### 7.3 Orchestration Airflow

- **DAG `retail_data_pipeline`** : extraction → Bronze → dbt → tests
- **DAG `retail_ml_pipeline`** : préparation → entraînement → MLflow → évaluation

Accès UI Airflow : `http://localhost:8080`

### 7.4 Business Intelligence (Metabase)

Connecté à la couche Gold. Indicateurs principaux :

- Chiffre d'affaires, nombre de commandes, panier moyen
- Évolution des ventes journalières
- Performance par catégorie / produit
- Signaux de retour (proxy via statuts `canceled` / `unavailable`)
- Dates signalées comme anomalies

Accès : `http://localhost:3000`

### 7.5 Machine Learning & MLOps

- Modèle : **IsolationForest** (détection d'anomalies sur séries de ventes)
- Tracking : MLflow (paramètres, métriques, artefacts)
- DAG dédié pour réentraînement reproductible

Accès MLflow : `http://localhost:5000`

### 7.6 Système RAG

1. Extraction et chunking des documents métier
2. Embeddings locaux (`sentence-transformers`)
3. Stockage vectoriel dans PostgreSQL (pgvector)
4. Récupération des passages pertinents + génération via Groq
5. Citations des sources dans les réponses

### 7.7 API & Assistant hybride

Endpoints principaux :

| Méthode | Endpoint              | Description                          |
|---------|-----------------------|--------------------------------------|
| GET     | `/health`             | Santé de l'API                       |
| POST    | `/query/sql`          | Requête analytique contrôlée         |
| POST    | `/query/rag`          | Question documentaire                |
| POST    | `/query/hybrid`       | Routage intelligent SQL + RAG        |
| GET     | `/anomalies`          | Liste des anomalies détectées        |

Documentation interactive : `http://localhost:8000/docs`

### 7.8 Automatisation (n8n)

Au moins un workflow métier :  
Webhook → appel API FastAPI → notification (ex. : alerte anomalie).

---

## 8. Configuration

Toutes les variables sensibles passent par le fichier `.env` :

| Variable            | Description                     |
|---------------------|---------------------------------|
| `POSTGRES_*`        | Connexion PostgreSQL            |
| `GROQ_API_KEY`      | Clé API Groq                    |
| `AIRFLOW_*`         | Configuration Airflow           |
| `MLFLOW_TRACKING_URI` | URI du serveur MLflow         |

Les définitions métier (ex. : calcul du CA) sont documentées dans `docs/data_dictionary.md`.

---

## 9. Utilisation

### Lancer un pipeline complet

```bash
# Via Makefile
make pipeline-data
make pipeline-ml

# Ou via Airflow UI
```

### Interroger l'assistant

```bash
curl -X POST http://localhost:8000/query/hybrid \
  -H "Content-Type: application/json" \
  -d '{"question": "Quelle catégorie génère le plus de CA et quelles sont ses conditions de retour ?"}'
```

### Consulter les dashboards

- Metabase : http://localhost:3000
- MLflow : http://localhost:5000
- Airflow : http://localhost:8080

---

## 10. Tests et CI/CD

### Tests locaux

```bash
make test                 # tests unitaires + intégration
make dbt-test             # tests de données dbt
```

### CI (GitHub Actions)

À chaque push / pull request :

- Lint (ruff / black)
- Tests unitaires
- Vérification dbt
- Build Docker

Les secrets (clés API) sont stockés dans les GitHub Secrets.

---

## 11. Principes d'ingénierie

Ces principes sont appliqués dès le MVP :

1. **Idempotence** — toute réexécution d'un pipeline produit le même résultat
2. **Séparation des responsabilités** — Airflow = data/ML ; n8n = automatisations métier
3. **Données testées** — tests dbt + documentation des règles métier
4. **Secrets hors Git** — `.env` + variables d'environnement uniquement
5. **SQL contrôlé** — l'assistant n'a jamais d'accès arbitraire à la base (vues Gold + whitelist)
6. **Mesurer avant d'optimiser** — pas de Kafka/Spark sans besoin justifié
7. **Documenter les trade-offs** — Architecture Decision Records (ADR) légers
8. **Qualité > quantité** — une fonctionnalité complète vaut mieux que trois amorcées

---

## 12. Perspectives (post-MVP)

| ID  | Évolution                              | Horizon approximatif |
|-----|----------------------------------------|----------------------|
| E1  | Streaming (Kafka + consumer)           | ~2 semaines          |
| E2  | MLOps avancé (registry, non-régression)| 2-3 semaines         |
| E3  | Observabilité (logs, métriques, alertes)| 1-2 semaines        |
| E4  | Authentification et multi-tenancy      | selon besoin         |

---

## Licence

Projet réalisé dans un cadre d'apprentissage et de montée en compétence professionnelle.  
Dataset Olist : licence CC BY-NC-SA 4.0.

---

**Contact / Auteur**  
Projet mené avec l'objectif de développer une expertise concrète en conception et réalisation de plateformes Data en contexte entreprise.

