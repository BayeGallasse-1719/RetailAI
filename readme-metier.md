# 🛒 RetailAI Intelligence Platform

> Plateforme intelligente de données commerciales — Data Engineering • MLOps • BI • IA générative

---

## Contexte métier

Les entreprises de commerce électronique et de distribution génèrent chaque jour d’importants volumes de données : commandes, clients, produits, paiements, avis et documents internes (politiques de retour, conditions de livraison, procédures, etc.).

Pourtant, ces données restent souvent **dispersées** entre plusieurs systèmes, fichiers et documents non structurés. Cette fragmentation entraîne des difficultés concrètes :

- Production lente et manuelle d’indicateurs fiables
- Suivi difficile des performances commerciales
- Détection tardive des anomalies ou comportements inhabituels
- Accès laborieux aux connaissances métier contenues dans les documents internes
- Prise de décision freinée par l’absence d’une vision consolidée

Dans de nombreuses organisations, les analystes passent encore une part significative de leur temps à collecter, nettoyer et consolider les données avant de pouvoir en extraire de la valeur.

**RetailAI Intelligence Platform** a été conçue pour répondre à ce constat : centraliser les données, automatiser les traitements, garantir leur qualité et permettre un accès simple et intelligent à l’information — à la fois structurée et documentaire.

---

## Problématique

Comment construire une plateforme Data moderne capable de :

1. Collecter automatiquement des données provenant de sources hétérogènes
2. Garantir leur qualité, leur cohérence et leur traçabilité
3. Produire des indicateurs métiers fiables et actionnables
4. Détecter des anomalies commerciales de façon proactive
5. Permettre aux utilisateurs d’interroger **à la fois** les données analytiques et les documents internes via une interface conversationnelle
6. Assurer la reproductibilité, l’évolutivité et l’industrialisation des traitements

Tout cela dans un cadre réaliste, reproductible et aligné sur les pratiques d’entreprise.

---

## Valeur ajoutée

Contrairement à de nombreux projets qui se concentrent sur une seule brique (dashboard, modèle ML ou chatbot), RetailAI adopte une **approche systémique**. Elle reproduit le fonctionnement réel d’une plateforme Data moderne et montre comment les différentes couches collaborent pour résoudre un problème métier concret.

### Pour les équipes métiers
- Accès rapide et fiable aux indicateurs commerciaux
- Visualisation claire des performances
- Réduction des tâches manuelles de reporting
- Accès simplifié aux procédures et documents internes via le langage naturel

### Pour les équipes Data
- Architecture structurée (Medallion : Bronze / Silver / Gold)
- Contrôles de qualité automatisés
- Pipelines reproductibles et orchestrés
- Suivi des expériences Machine Learning
- Environnement industrialisable (Docker + CI/CD)

### Pour les décideurs
- Meilleure visibilité sur l’activité commerciale
- Identification précoce des anomalies
- Accélération de la prise de décision
- Centralisation de la connaissance métier

---

## Cas d’usage métier

La plateforme permet de répondre à des questions concrètes telles que :

**Analyse commerciale**  
Quelle catégorie génère actuellement le plus de chiffre d’affaires ?  
Quels sont les produits les plus performants ce mois-ci ?  
Quel est le taux de retour (ou d’annulation) par catégorie ?

**Détection d’anomalies**  
Certaines journées présentent-elles des ventes inhabituelles ?  
Existe-t-il des comportements anormaux nécessitant une investigation ?

**Recherche documentaire**  
Quelles sont les conditions de remboursement d’un produit ?  
Quels sont les délais de livraison prévus par la politique interne ?

**Questions hybrides (SQL + RAG)**  
Quelle catégorie génère le plus de ventes et quelles sont ses conditions de retour ?

Cette dernière capacité illustre la combinaison d’une requête analytique et d’une recherche documentaire augmentée (RAG) au sein d’un même assistant.

---

## Pourquoi ce projet

RetailAI n’est pas une simple démonstration technique. Il reproduit les principales briques d’une plateforme Data moderne en entreprise :

| Domaine              | Brique présente                          |
|----------------------|------------------------------------------|
| Data Engineering     | Ingestion + architecture Medallion       |
| Transformation       | dbt + tests de qualité                   |
| Orchestration        | Apache Airflow                           |
| Business Intelligence| Metabase                                 |
| Machine Learning     | Détection d’anomalies                    |
| MLOps                | MLflow                                   |
| IA générative        | RAG (PostgreSQL + pgvector)              |
| API                  | FastAPI                                  |
| Automatisation métier| n8n                                      |
| Industrialisation    | Docker + GitHub Actions                  |

L’objectif n’est pas d’empiler des technologies, mais de démontrer comment elles collaborent pour résoudre un problème métier réel, avec une rigueur méthodologique compatible avec un contexte professionnel.

---

## Approche et intention

Ce projet a été conçu comme un **exercice de montée en compétence structurée**, et non comme un simple projet portfolio.

L’ambition est double :

1. **Maîtriser concrètement** les technologies et pratiques utilisées dans les plateformes Data modernes (Data Engineering, MLOps, BI, RAG, industrialisation).
2. **Tester et renforcer la rigueur** d’exécution sur la durée : cadrage, priorisation, qualité, documentation, reproductibilité et capacité à livrer une solution cohérente et démontrable.

Chaque choix technique est motivé par un besoin métier ou un principe d’architecture.  
Chaque livrable vise à être complet, testé et compréhensible par un autre développeur ou un responsable technique.

L’objectif final n’est pas seulement de « montrer des outils », mais de développer une **capacité réelle à concevoir et réaliser ce type de plateforme en contexte professionnel**.

---

## Compétences démontrées

À travers ce projet, je mets en pratique et consolide ma capacité à :

- Concevoir une architecture Data de bout en bout orientée métier
- Mettre en place une architecture Medallion (Bronze / Silver / Gold)
- Développer et orchestrer des pipelines de données avec Apache Airflow
- Construire et tester des modèles de transformation avec dbt
- Mettre en œuvre des pratiques MLOps (tracking, reproductibilité) avec MLflow
- Développer un système RAG basé sur PostgreSQL et pgvector
- Exposer les services via une API FastAPI
- Industrialiser la solution avec Docker et GitHub Actions
- Travailler avec une exigence de qualité, de traçabilité et de documentation compatible avec un environnement d’entreprise

---

*Ce README raconte une histoire : celle d’un problème business réel, d’une architecture pensée pour y répondre, et d’une exécution menée avec rigueur.*