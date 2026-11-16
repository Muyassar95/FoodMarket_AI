# FoodMarket AI — Journal de progression

**Projet :** FoodMarket AI — Data Science, Pricing & Nutrition Intelligence  
**Catégorie pilote :** Céréales du petit-déjeuner  
**Environnement :** Python, Pandas, DuckDB, Plotly, Open Food Facts, Open Prices

---

## J6 — Extension et enrichissement des céréales

**Mise à jour :** 16 novembre 2026  
**Notebook :** `06_extension_echantillon.ipynb`  
**Objectif :** Étendre progressivement l'échantillon d'environ 30 à 70 céréales, en renforçant les classes Nutri-Score sous-représentées.

### 1. Travaux réalisés

- Préparation d'une acquisition ciblée sur les Nutri-Scores A, B, C, D et E.
- Utilisation de l'API Open Food Facts pour récupérer de nouvelles références.
- Mise en place de fonctions Python réutilisables pour l'extraction et le nettoyage.
- Préparation de la fusion des produits du J5 avec les nouvelles acquisitions du J6.
- Identification de plusieurs formats de conditionnement : `311g`, `375 g`, `12 oz (340 g)`, `250g ℮ 8.8 oz`.
- Ajout d'une méthode Pandas avec expression régulière pour extraire les quantités en grammes.
- Préparation de la conversion des grammes en kilogrammes pour les analyses tarifaires.

### 2. Résultats vérifiés

Un extrait de **20 nouvelles références ciblées** a été obtenu.

Répartition observée dans cet extrait :

| Nutri-Score | Nombre |
|---|---:|
| A | 0 |
| B | 6 |
| C | 1 |
| D | 6 |
| E | 7 |
| **Total** | **20** |

Les données présentées contiennent notamment :

- Codes-barres et noms des produits.
- Marques et quantités.
- Nutri-Scores.
- Teneurs en sucres et en fibres.
- Provenance de l'échantillonnage (`CIBLE_J6`).

**Constat :** les observations ciblées renforcent notamment les catégories D et E, mais la classe C reste peu représentée dans cet extrait.

### 3. Contrôles qualité et limites

- Vérifier que chaque référence appartient réellement à la catégorie des céréales du petit-déjeuner.
- Préserver les codes-barres comme chaînes de caractères.
- Éliminer les doublons entre J5 et J6.
- Vérifier les quantités extraites dans les conditionnements complexes.
- Distinguer le poids total du produit et le poids d'une portion.
- Conserver les valeurs nutritionnelles manquantes sans les inventer.
- Documenter le biais lié à la sélection ciblée par Nutri-Score.

**Point de vigilance :** `2 biscuits 38g` permet d'extraire 38 g, mais cette valeur nécessite une vérification avant d'être utilisée comme poids total du produit.

### 4. Preuves et livrables

**Notebook de travail :**
- `notebooks/06_extension_echantillon.ipynb`

**Données à vérifier après export :**
- `data/processed/dim_cereales_j6.parquet`
- `data/processed/j6_echantillonnage.csv`
- `data/processed/fact_prix_cereales_j6.parquet`

**Visualisations prévues :**
- Répartition des Nutri-Scores J5/J6.
- Distribution des sucres selon le Nutri-Score.
- Distribution des fibres selon le Nutri-Score.

### 6. Compétences mobilisées

Python, Pandas, API REST, JSON, expressions régulières, nettoyage de données, gestion des valeurs manquantes, échantillonnage et préparation des analyses statistiques.

---
--
*Journal évolutif : les prochaines journées seront ajoutées dans ce même fichier, sans multiplier les comptes rendus.*
