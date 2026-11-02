# FoodMarket AI : Cadrage du projet

**Nom :** FoodMarket AI  
**Domaine :** FoodTech, Data Science, Pricing Intelligence & Artificial Intelligence  
**Date de lancement :** 02 novembre 2026  
**Durée de la V1 :** 60 jours  
**Vision à long terme :** entreprise technologique évolutive, sans date de fin imposée.

## 1. Présentation du projet

FoodMarket AI est un projet de Data Science et d'intelligence artificielle consacré à l'analyse des produits alimentaires, des prix, des marques et de la concurrence.

Il vise à transformer des données alimentaires en informations utiles à la prise de décision des entreprises agroalimentaires et des distributeurs.

## 2. Deux objectifs complémentaires

### Objectif 1 — Portfolio Data Scientist

Développer en 60 jours une première version professionnelle, reproductible et démontrable sur GitHub, LinkedIn et en entretien.

Compétences à valoriser :
- Python et analyse statistique
- SQL et traitement des données
- Power BI et Data Visualization
- Machine Learning
- Initiation au Deep Learning et au NLP

### Objectif 2 — Entreprise SaaS B2B

Transformer progressivement FoodMarket AI en une plateforme commerciale de Market Intelligence et Pricing Intelligence.

Clients potentiels : industriels agroalimentaires, marques, distributeurs et cabinets d'études.

L'objectif est de développer à terme des services d'analyse, de prévision et de surveillance concurrentielle, avec une possibilité d'expansion internationale.

## 3. Périmètre initial

**Catégorie pilote : céréales du petit-déjeuner.**

Sources de données :
- **Open Food Facts :** produits, marques, ingrédients, informations nutritionnelles et Nutri-Score.
- **Open Prices :** observations de prix, dates, points de vente et informations disponibles sur les produits.

Identifiant de rapprochement : code-barres EAN/GTIN.

Les analyses devront tenir compte des différences de conditionnement, de période et de lieu de vente. Les données ouvertes ne seront pas considérées comme représentatives de l'ensemble du marché.

## 4. Technologies

| Domaine | Technologies |
|---|---|
| Programmation | Python, Jupyter Notebook |
| Traitement | Pandas, NumPy |
| Base de données et SQL | DuckDB, SQL |
| Visualisation | Power BI, Matplotlib |
| Machine Learning | Scikit-learn, XGBoost |
| Deep Learning et NLP | PyTorch, Transformers |
| Big Data | Parquet, DuckDB, PySpark si nécessaire |
| Versionnement | Git, GitHub |
| Application future | Streamlit, API, Cloud |

**Priorité V1 : Python + SQL + Power BI.**

Les fonctionnalités avancées seront ajoutées progressivement lorsqu'elles apporteront une valeur analytique mesurable.

## 5. Analyses prévues

1. Explorer et nettoyer les données alimentaires.
2. Étudier la répartition des produits par marque.
3. Comparer les caractéristiques nutritionnelles.
4. Analyser les prix disponibles et les différences entre enseignes.
5. Calculer des indicateurs comparables, notamment les prix au kilogramme lorsque les données le permettent.
6. Construire un tableau de bord interactif Power BI.
7. Expérimenter le clustering, la prédiction et la similarité des produits.

## 6. Livrables de la V1

- Notebooks Python documentés.
- Jeux de données nettoyés et échantillonnés.
- Requêtes SQL reproductibles.
- Tableau de bord Power BI.
- Premières expérimentations de Machine Learning.
- Documentation GitHub et résultats analytiques.
- Présentation professionnelle pour LinkedIn et les entretiens.

## 7. Organisation GitHub

```text
foodmarket-ai/
├── README.md
├── requirements.txt
├── .gitignore
├── docs/
│   └── J0_CADRAGE.md
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── sql/
├── powerbi/
├── src/
├── reports/
│   └── figures/
└── tests/
```

Les fichiers de données volumineux ou sensibles ne seront pas publiés directement sur GitHub.

## 8. Feuille de route — 60 jours

| Période | Travaux |
|---|---|
| J0 | Cadrage et objectifs |
| J1–J7 | Acquisition et nettoyage des données avec Python |
| J8–J15 | Analyse exploratoire et statistiques |
| J16–J25 | SQL, DuckDB et préparation des données |
| J26–J35 | Tableaux de bord Power BI |
| J36–J45 | Premiers modèles de Machine Learning |
| J46–J55 | NLP et similarité des produits, selon l'avancement |
| J56–J60 | Tests, documentation et valorisation du portfolio |

## 9. Contraintes et méthode

- Ordinateur Windows avec 16 Go de RAM.
- GPU AMD/Intel : privilégier les traitements CPU compatibles.
- Utiliser des échantillons raisonnables pour commencer.
- Éviter le téléchargement intégral des bases volumineuses.
- Prévoir environ 30 à 45 minutes de travail quotidien.
- Documenter les sources, les limites et les résultats.

## 10. Vision à long terme

FoodMarket AI pourra évoluer vers une plateforme intégrant :

- Pricing Intelligence et suivi concurrentiel.
- Prévisions de prix et analyses prédictives.
- Similarité et classification intelligente des produits.
- Market Intelligence multi-catégories.
- API et tableaux de bord professionnels.
- Services SaaS B2B avec abonnements.
- Déploiement international et nouvelles technologies.

**La période de 60 jours concerne uniquement la première version. FoodMarket AI est conçu comme un projet évolutif sur plusieurs décennies, sans date de fin imposée.**

## 11. Prochaine étape — J1

Créer le premier notebook `01_decouverte_donnees.ipynb` pour :

1. Importer Pandas et Requests.
2. Interroger l'API Open Food Facts.
3. Récupérer un échantillon de céréales.
4. Explorer les colonnes et les premières statistiques.
5. Découvrir les observations disponibles dans Open Prices.
6. Enregistrer les premiers résultats en CSV.

**Statut J0 : cadrage rédigé — prêt pour GitHub.**
