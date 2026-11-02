# FoodMarket AI

### Data Science • Machine Learning • Deep Learning • Pricing Intelligence • FoodTech

**FoodMarket AI** est un projet de Data Science et d'intelligence artificielle consacré à l'analyse des marchés agroalimentaires, des produits, des prix, des marques et de la concurrence.

L'objectif est de développer progressivement une plateforme intelligente d'aide à la décision pour les entreprises agroalimentaires et les distributeurs.

Le projet poursuit deux ambitions complémentaires :

- **Court terme :** construire en 60 jours un portfolio professionnel de Data Scientist, valorisable sur GitHub, LinkedIn et lors d'entretiens.
- **Long terme :** transformer FoodMarket AI en une plateforme SaaS B2B de Market Intelligence et Pricing Intelligence, évolutive à l'international et sans date de fin imposée.

## 1. Objectifs du projet

- Collecter et structurer des données alimentaires ouvertes.
- Analyser les produits, les marques et leurs caractéristiques nutritionnelles.
- Étudier les prix et le positionnement concurrentiel.
- Développer des tableaux de bord interactifs.
- Appliquer des méthodes statistiques et de Machine Learning.
- Explorer le Deep Learning et le traitement automatique du langage (NLP).
- Construire progressivement des fonctionnalités d'intelligence artificielle à valeur ajoutée.

## 2. Sources de données

**Open Food Facts**

Base collaborative contenant des informations sur les produits alimentaires : marques, ingrédients, catégories, valeurs nutritionnelles et Nutri-Score.

Site : https://world.openfoodfacts.org

**Open Prices**

Base ouverte permettant d'étudier les observations de prix alimentaires selon les produits, les lieux et les dates disponibles.

Site : https://prices.openfoodfacts.org

Les données seront rapprochées lorsque cela est possible grâce aux codes-barres EAN/GTIN.

**Catégorie pilote : les céréales du petit-déjeuner.**

Les données ouvertes ne constituent pas nécessairement un échantillon représentatif des ventes ou de l'ensemble du marché.

## 3. Technologies utilisées et envisagées

| Domaine | Technologies |
|---|---|
| Programmation | Python |
| Analyse de données | Pandas, NumPy |
| Environnement | Jupyter Notebook |
| Bases de données | SQL, DuckDB |
| Visualisation | Power BI, Matplotlib |
| Machine Learning | Scikit-learn, XGBoost |
| Deep Learning | PyTorch |
| NLP | Transformers, Sentence Transformers |
| Traitement de données volumineuses | Parquet, DuckDB, PySpark si nécessaire |
| Application web future | Streamlit |
| Versionnement | Git, GitHub |
| Cloud futur | Selon les besoins du projet |

**Priorité initiale : Python + SQL + Power BI.**

Les autres technologies seront intégrées progressivement selon les besoins et les résultats des expérimentations.

## 4. Analyses et fonctionnalités prévues

### Analyse exploratoire

- Répartition des produits par marque.
- Comparaison des profils nutritionnels.
- Analyse des ingrédients et des catégories.
- Étude des valeurs manquantes et de la qualité des données.

### Pricing Intelligence

- Comparaison des prix entre produits et enseignes.
- Calcul des prix au kilogramme lorsque les données le permettent.
- Étude des écarts de prix selon les observations disponibles.
- Analyse du positionnement concurrentiel.

### Machine Learning

- Segmentation des produits par clustering.
- Expérimentation de modèles prédictifs.
- Identification de groupes de produits comparables.
- Évaluation des performances des modèles.

### Deep Learning et NLP

- Analyse textuelle des ingrédients.
- Représentation vectorielle des descriptions de produits.
- Recherche de produits similaires.
- Expérimentation de modèles Transformers préentraînés.

## 5. Organisation du dépôt

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

Les fichiers volumineux ne seront pas directement intégrés au dépôt GitHub.

## 6. Feuille de route — Version initiale

| Période | Objectif |
|---|---|
| J0 | Cadrage du projet |
| J1–J7 | Acquisition et préparation des données |
| J8–J15 | Analyse exploratoire et statistiques |
| J16–J25 | SQL, DuckDB et structuration des données |
| J26–J35 | Création du tableau de bord Power BI |
| J36–J45 | Expérimentations de Machine Learning |
| J46–J55 | NLP et similarité des produits |
| J56–J60 | Tests, documentation et présentation finale |

Cette feuille de route est indicative et pourra évoluer selon les résultats obtenus.

## 7. Livrables attendus

- Notebooks Python reproductibles et documentés.
- Données nettoyées et structurées.
- Analyses statistiques et requêtes SQL.
- Tableau de bord Power BI interactif.
- Expérimentations de Machine Learning et NLP.
- Visualisations et conclusions métier.
- Documentation technique et méthodologique.

## 8. Vision à long terme

FoodMarket AI a vocation à évoluer vers une plateforme professionnelle proposant :

- La surveillance des prix et des marchés.
- L'analyse concurrentielle automatisée.
- Des prévisions et recommandations fondées sur les données.
- Des outils d'analyse des produits alimentaires.
- Des API et tableaux de bord pour les entreprises.
- Des services SaaS B2B avec abonnements.
- Une extension progressive à plusieurs catégories alimentaires et marchés internationaux.

Le projet est conçu pour évoluer sur plusieurs décennies, au rythme des innovations technologiques et des besoins des entreprises.

## 9. État d'avancement

**Projet lancé le 1er novembre 2026.**

- [x] J0 — Définition du périmètre et des objectifs
- [ ] J1 — Acquisition des premières données
- [ ] Analyse exploratoire
- [ ] Structuration SQL
- [ ] Tableau de bord Power BI
- [ ] Premiers modèles de Machine Learning
- [ ] Expérimentations NLP
- [ ] Publication de la V1

## 10. Documentation

Le cadrage détaillé du projet est disponible dans [J0 — Cadrage du projet](docs/J0_CADRAGE.md).

---

**FoodMarket AI — Transformer les données alimentaires en intelligence décisionnelle grâce à la Data Science et à l'intelligence artificielle.**
