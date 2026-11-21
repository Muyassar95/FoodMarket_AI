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
## J7 — Extension à 100 céréales & Pricing Intelligence concurrentielle

**Date :** 21 novembre 2026  
**Notebook :** `07_pricing_intelligence.ipynb`  
**Statut :** À valider après exécution des contrôles finaux  
**Technologies :** Python, Pandas, DuckDB, SQL, Plotly, Open Food Facts, Open Prices

### 1. Objectifs

- Étendre l'échantillon de **70 à 100 céréales distinctes**, soit 30 nouvelles références.
- Consolider les données nutritionnelles et les quantités normalisées.
- Développer une première analyse concurrentielle des prix au kilogramme.
- Étudier les différences de prix par marque, magasin, période et promotion.
- Appliquer des seuils minimaux pour limiter les interprétations fondées sur des effectifs insuffisants.

### 2. Travaux et traitements prévus

- Acquisition complémentaire de céréales via l'API Open Food Facts.
- Vérification des codes-barres, suppression des doublons et conservation de la provenance des produits.
- Extraction des quantités en grammes et conversion en kilogrammes.
- Identification des conditionnements ambigus nécessitant une vérification.
- Jointure des données produits avec les observations Open Prices à l'aide de DuckDB.
- Calcul des prix normalisés en €/kg lorsque les données le permettent.
- Préparation des statistiques descriptives et des visualisations interactives.

**Ces traitements constituent le programme du J7 ; leur réalisation effective doit être confirmée dans le notebook.**

### 3. Résultats et indicateurs

| Indicateur | Résultat |
|---|---|
| Références initiales J6 | 70 |
| Nouvelles références visées | 30 |
| Total visé J7 | 100 |
| Références uniques obtenues | 100 |
| Produits avec quantité exploitable | 99 |
| Produits avec prix/kg disponible | 99 |
| Magasins identifiés | 15 |
| Marques comparables | 5 |
| Promotions vérifiables | 14 |

### 4. Méthodologie de Pricing Intelligence

Les comparaisons reposent sur les prix observés, normalisés au kilogramme lorsque le poids du produit et la nature du prix sont suffisamment établis.

**Seuils exploratoires retenus :**

| Analyse | Conditions minimales |
|---|---|
| Marques | 10 relevés, 3 produits et 2 magasins |
| Magasins | 10 relevés et 3 produits |
| Périodes mensuelles | 10 relevés, 3 produits et 2 magasins |
| Promotions | 5 observations appariées exploitables, 3 produits et 2 magasins |

Ces seuils sont des règles internes de filtrage, et non des garanties de représentativité statistique.

Les comparaisons entre magasins privilégient les mêmes codes-barres sur des périodes communes. Les analyses temporelles privilégient les couples produit–magasin observés sur des mois consécutifs.

### 5. Contrôles qualité et limites

- Vérifier la correspondance entre codes-barres et produits.
- Conserver les prix et quantités d'origine.
- Signaler les lots et conditionnements ambigus.
- Écarter les prix invalides des indicateurs normalisés.
- Contrôler les devises, dates et lieux d'observation.
- Distinguer les promotions des prix ordinaires.
- Vérifier les effectifs avant toute comparaison.
- Documenter les groupes insuffisamment représentés.

**Limite principale :** les observations Open Prices ne constituent pas un échantillon représentatif de l'ensemble du marché. Les résultats décrivent uniquement les relevés disponibles.

### 6. Difficultés et points de vigilance

- Disponibilité variable des produits via l'API Open Food Facts.
- Risques de doublons lors de l'acquisition complémentaire.
- Conditionnements exprimés dans différents formats.
- Couverture tarifaire potentiellement faible pour certaines céréales.
- Informations de magasin et de promotions parfois incomplètes.
- Risque de comparer des produits ou des périodes non équivalents.

Les comparaisons insuffisamment documentées doivent être signalées plutôt que présentées comme des conclusions concurrentielles.

### 7. Preuves et livrables

**Notebook :**
- `notebooks/07_pricing_intelligence.ipynb`

**Données à vérifier :**
- `data/processed/dim_cereales_j7.parquet`
- `data/processed/fact_prix_cereales_j7.parquet`
- `data/processed/j7_stats_marques.parquet`
- `data/processed/j7_stats_magasins.parquet`
- `data/processed/j7_stats_mois.parquet`
- `data/processed/j7_promotions.parquet`

**Visualisations prévues :**
- Prix médian au kilogramme par marque.
- Comparaison des magasins admissibles.
- Évolution descriptive des prix par mois.
- Analyse des promotions vérifiables.

### 8. Compétences mobilisées

- **Python / Pandas :** extraction, nettoyage et normalisation.
- **SQL / DuckDB :** jointures, agrégations, médianes et fonctions de fenêtrage.
- **Data Quality :** validation, traçabilité et contrôle des valeurs atypiques.
- **Statistiques descriptives :** comparaison d'échantillons et gestion des faibles effectifs.
- **Plotly :** visualisations interactives.
- **Pricing Intelligence :** construction d'indicateurs concurrentiels.


---
