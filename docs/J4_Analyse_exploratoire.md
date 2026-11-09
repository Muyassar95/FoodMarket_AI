# FoodMarket AI — Compte rendu J4

**Projet :** FoodMarket AI — Data Science & Pricing Intelligence  
**Journée :** J4 — Analyse exploratoire et visualisation  
**Date :** 9 novembre 2026  
**Catégorie pilote :** Céréales du petit-déjeuner  
**Technologies :** Python, Pandas, DuckDB, SQL, Plotly  
**Environnement :** JupyterLab — Windows 11

---

## 1. Objectifs atteints

### Objectif général

Construire une première analyse exploratoire des céréales du petit-déjeuner en combinant les informations produits d'Open Food Facts et les observations tarifaires d'Open Prices.

### Objectifs techniques

- [X] Vérifier le référentiel des céréales.
- [X] Réaliser la jointure SQL entre produits et prix.
- [X] Contrôler le nombre de correspondances.
- [X] Identifier les devises et les unités de prix.
- [X] Calculer les premières statistiques tarifaires.
- [X] Analyser les marques et les Nutri-Scores.
- [X] Produire des visualisations interactives avec Plotly.
- [X] Exporter les résultats pour les futures analyses.


## 2. Sources et données utilisées

### Open Food Facts

Informations exploitées :

- `code` : identifiant du produit.
- `product_name` : nom commercial.
- `brands` : marque déclarée.
- `nutrition_grades` : Nutri-Score.
- `quantity` : quantité ou conditionnement, si disponible.

**Fichier local :** `dim_cereales_j3.parquet`.

### Open Prices

Informations exploitées :

- `product_code` : identifiant permettant la jointure.
- `price` : prix observé.
- `date` : date d'observation.
- `currency` : devise, si disponible.
- `location_id` : identifiant du point de vente, si disponible.
- `price_is_discounted` : indicateur de promotion, si disponible.

**Fichier local :** `prices_j2.parquet`.

### Périmètre

L'analyse porte exclusivement sur les produits préalablement identifiés comme appartenant à la catégorie `en:breakfast-cereals`.

Les observations de prix non associées aux codes-barres de ces céréales sont exclues de la table analytique.

## 3. Méthodologie d'extraction et de traitement

### Étape 1 — Charg
