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

- ---

## J8 — Développement du dashboard Pricing & Nutrition Intelligence

**Date :** 23 novembre 2026  
**Notebook :** `08_dashboard_pricing.ipynb`  
**Application :** `app_j8.py`  
**Technologies :** Python, Pandas, Plotly, Streamlit, Parquet  
**Statut :** Dashboard fonctionnel — validation des indicateurs à finaliser.

### 1. Objectifs

- Transformer les données préparées au J7 en une application interactive.
- Centraliser les indicateurs de prix et les informations nutritionnelles.
- Développer des filtres par marque, magasin, période, Nutri-Score et promotion.
- Visualiser les comparaisons concurrentielles avec des effectifs minimaux.
- Préparer une application réutilisable pour les futures analyses FoodMarket AI.

### 2. Travaux réalisés

- Chargement des données produits et des observations tarifaires depuis les fichiers Parquet.
- Préparation des variables : prix/kg, dates, mois, marques, magasins et promotions.
- Développement d'une interface interactive avec **Streamlit**.
- Création de KPI : céréales référencées, produits avec prix/kg, relevés exploitables et magasins identifiés.
- Intégration des filtres interactifs dans une barre latérale.
- Programmation des analyses descriptives par marque, magasin et période.
- Intégration d'une analyse des promotions lorsque les prix de référence sont disponibles.
- Ajout d'une section Nutrition Intelligence avec comparaison des sucres et fibres selon le Nutri-Score.
- Mise en place d'un tableau de contrôle qualité et d'un export CSV des observations filtrées.

### 3. Résultats vérifiés

- **Application Streamlit lancée avec succès** dans le navigateur.
- Résolution du problème lié à l'utilisation de `__file__` dans JupyterLab.
- Identification des avertissements `missing ScriptRunContext`, provoqués par l'exécution de Streamlit directement dans un notebook.
- Séparation entre les traitements Jupyter (`.ipynb`) et l'application interactive (`.py`).
- Configuration de l'environnement Python et des bibliothèques nécessaires.


| Indicateur | Résultat |
|---|---|
| Céréales référencées | Oui |
| Produits avec prix/kg exploitable | Oui |
| Relevés de prix disponibles | Oui |
| Magasins identifiés | Oui |
| Marques disposant de relevés | Oui |
| Périodes disponibles | Oui |
| Promotions exploitables | Non |

### 4. Contrôles qualité et limites

- Conservation des prix manquants pour le diagnostic de couverture.
- Exclusion des prix/kg invalides des indicateurs commerciaux.
- Distinction entre prix ordinaires, promotionnels et statuts inconnus.
- Agrégation des observations par produit, magasin et mois.
- Application des seuils minimaux définis au J7 pour les comparaisons.
- Signalement des analyses dont les effectifs sont insuffisants.
- Vérification nécessaire de la cohérence des filtres et des KPI.

**Limite :** le dashboard décrit les observations Open Prices disponibles. Il ne constitue pas une mesure représentative de l'ensemble du marché agroalimentaire.

Les classements de marques et de magasins restent sensibles aux différences de produits et de périodes observés.

### 5. Difficultés et solutions

**Difficulté 1 — Variable `__file__` non définie**

- Cause : exécution du code destiné à un script Python dans JupyterLab.
- Solution : utilisation d'un chemin explicite dans le notebook, puis exécution de l'application dans un fichier `.py`.

**Difficulté 2 — Avertissements Streamlit**

- Erreurs rencontrées : `missing ScriptRunContext` et `Session state does not function`.
- Cause : fonctions Streamlit exécutées sans le serveur d'application local.
- Solution : lancement avec `python -m streamlit run app_j8.py`.

**Difficulté 3 — Organisation du code**

- Problème : coexistence des cellules Jupyter et des fonctionnalités Streamlit dans un même notebook.
- Solution : séparation des traitements analytiques et du code de l'application.

### 6. Preuves et livrables

**Fichiers principaux :**

- `notebooks/08_dashboard_pricing.ipynb`
- `app_j8.py`
- `data/processed/j8_dashboard_data.parquet`
- `requirements.txt`

**Fonctionnalités développées :**

- Dashboard interactif Streamlit.
- Filtres multidimensionnels.
- KPI commerciaux.
- Analyses des prix par marque, magasin et période.
- Analyse des promotions.
- Comparaisons nutritionnelles.
- Contrôles qualité.
- Export CSV.

**Validation restante :** vérifier les valeurs affichées, tester les filtres individuellement et confirmer les fichiers présents dans le dépôt GitHub.

### 7. Compétences acquises

- **Python / Pandas :** préparation, nettoyage, jointures et agrégations.
- **Plotly :** visualisations interactives.
- **Streamlit :** développement d'applications Data Science.
- **Data Engineering :** chargement et exploitation de fichiers Parquet.
- **Business Intelligence :** création de KPI et de filtres.
- **Data Quality :** contrôles de cohérence et gestion des effectifs insuffisants.
- **Environnement Python :** gestion des bibliothèques, scripts et exécution locale.

### 8. Prochaine étape — J9

**Thématique : Comparaisons tarifaires avancées et fiabilisation concurrentielle.**

Travaux prévus :

1. Mesurer la couverture tarifaire réelle du référentiel de céréales.
2. Identifier les mêmes produits observés dans plusieurs magasins.
3. Comparer les prix à code-barres identique et période comparable.
4. Calculer les écarts de prix en euros et en pourcentage.
5. Approfondir les comparaisons concurrentielles par marque.
6. Étudier les variations temporelles à produit et magasin constants.
7. Enrichir progressivement le dashboard Streamlit.
8. Consolider les indicateurs et les contrôles qualité.

### 9. Bilan personnel

Le J8 marque une évolution du projet FoodMarket AI : passage de notebooks d'analyse de données à une **première application interactive de Pricing & Nutrition Intelligence**.

Le développement du dashboard a permis de mieux comprendre la séparation entre les traitements de données, la logique métier et l'interface utilisateur.

**Résultat principal :** application Streamlit opérationnelle.

**Point à améliorer :** renforcer la fiabilité des comparaisons tarifaires, la cohérence des filtres et la validation des indicateurs.

---


---
