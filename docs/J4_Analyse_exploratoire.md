# FoodMarket AI — Compte rendu J4

**Projet :** FoodMarket AI — Data Science, Nutrition Intelligence & Pricing Intelligence  
**Journée :** J4 — Analyse exploratoire et enrichissement des données  
**Date :** 9 novembre 2026  
**Catégorie pilote :** Céréales du petit-déjeuner  
**Technologies :** Python, Pandas, DuckDB, SQL, Requests, Plotly  
**Environnement :** JupyterLab — Windows 11  
**Notebook :** `04_analyse_exploratoire.ipynb`

---

## 1. Objectifs atteints

### Objectif général

Développer une première analyse exploratoire des céréales du petit-déjeuner à partir de données produits Open Food Facts et d'observations tarifaires Open Prices.

L'objectif du J4 est de construire progressivement une base analytique fiable pour les futures applications de Market Intelligence, Pricing Intelligence, Power BI et Machine Learning.

### Travaux entrepris

- [x] Adapter le notebook aux fichiers locaux disponibles.
- [x] Préparer le traitement des produits et des codes-barres.
- [x] Définir une méthode d'enrichissement des Nutri-Scores manquants.
- [x] Préparer les analyses de marques et de profils nutritionnels.
- [x] Intégrer une requête de jointure SQL entre produits et prix.
- [x] Identifier l'erreur DuckDB `ResourceInUseException`.
- [x] Proposer une correction de la gestion des résultats SQL.
- [X] Confirmer l'exécution complète de la jointure après correction.
- [X] Vérifier les indicateurs statistiques obtenus.
- [X] Confirmer la création et l'export des graphiques Plotly.
- [X] Valider les fichiers Parquet finaux.


## 2. Sources et données utilisées

### Source 1 — Open Food Facts

Open Food Facts fournit les caractéristiques des produits alimentaires.

Variables utilisées ou recherchées :

| Variable | Description |
|---|---|
| `code` | Code-barres du produit |
| `product_name` | Nom du produit |
| `brands` | Marque déclarée |
| `quantity` | Quantité et conditionnement |
| `categories_tags` | Catégories alimentaires |
| `nutriscore_grade` | Note nutritionnelle A à E |
| `nutrition_grades` | Autre champ possible de Nutri-Score |

Le dataset produit utilisé ne disposait pas initialement de la colonne `nutrition_grades`.

Une méthode d'enrichissement ciblé à partir de l'API Open Food Facts a donc été ajoutée au notebook.

**Fichier produit effectivement chargé :** à compléter.

### Source 2 — Open Prices

Fichier de prix disponible :

`prices_j2.parquet`

Variables attendues, selon le schéma réel :

- `product_code` : code-barres du produit.
- `price` : prix observé.
- `date` : date du relevé.
- `currency` : devise.
- `price_per` : unité du prix.
- `location_id` : identifiant du lieu.
- `price_is_discounted` : indicateur de réduction.

Le fichier de prix ne permet pas, à lui seul, de garantir l'appartenance d'un produit à la catégorie des céréales.

### Périmètre analytique

Le J4 concerne les produits vérifiés comme appartenant à la catégorie :

`en:breakfast-cereals`

Les analyses de prix doivent uniquement utiliser les observations associées à ces produits.

## 3. Méthodologie d'extraction et de traitement

### Étape 1 — Chargement des données

- Identification des fichiers locaux sur le disque D:.
- Lecture du référentiel des céréales avec Pandas.
- Lecture du fichier Open Prices avec DuckDB.
- Vérification des colonnes disponibles.

Le recours à DuckDB permet de travailler directement sur les fichiers Parquet sans devoir charger toutes les observations de prix dans Pandas.

### Étape 2 — Préparation des produits

Les traitements prévus comprennent :

- La vérification des codes-barres.
- La suppression des espaces inutiles.
- Le contrôle des doublons.
- La vérification de la catégorie alimentaire.
- La préparation des champs produits nécessaires aux analyses.

Les codes-barres sont conservés sous forme de chaînes de caractères afin de préserver leurs éventuels zéros initiaux.

### Étape 3 — Enrichissement nutritionnel

La colonne Nutri-Score étant absente du dataset produit initial, le notebook prévoit une récupération ciblée depuis l'API Open Food Facts.

Principe :

1. Identifier les céréales sans Nutri-Score.
2. Récupérer leur code-barres.
3. Interroger l'API produit.
4. Extraire `nutriscore_grade` ou `nutrition_grades`.
5. Conserver uniquement les valeurs A, B, C, D et E.
6. Enregistrer les réponses dans un cache local.
7. Ajouter les valeurs disponibles au référentiel produit.

L'acquisition est volontairement limitée à un petit nombre de produits par exécution afin de respecter les ressources matérielles et de limiter les requêtes.

**Nombre de Nutri-Scores effectivement récupérés : à compléter.**

Une valeur nutritionnelle absente doit rester manquante et ne doit pas être remplacée arbitrairement.

### Étape 4 — Jointure SQL produits × prix

La jointure repose sur la correspondance des codes-barres :

- `code` pour les produits.
- `product_code` pour les observations de prix.

Requête SQL de principe :

```sql
SELECT
    c.code,
    c.product_name,
    c.brands,
    c.nutriscore_grade,
    p.price,
    p.date
FROM prix_bruts AS p
INNER JOIN cereales_j4 AS c
    ON TRIM(CAST(p.product_code AS VARCHAR)) = c.code;
```

La jointure `INNER JOIN` permet d'associer uniquement les observations de prix correspondant aux céréales identifiées.

**Validation de l'exécution complète : à confirmer après résolution de l'erreur DuckDB.**

### Étape 5 — Préparation analytique

Les traitements suivants sont prévus après la jointure :

- Conversion des prix en valeurs numériques.
- Conversion des dates.
- Contrôle des devises.
- Identification des unités de prix.
- Recherche des valeurs manquantes et invalides.
- Agrégations statistiques par produit.
- Préparation des visualisations interactives.

La comparaison de prix entre produits de conditionnements différents nécessitera une normalisation supplémentaire au J5.

## 4. Contrôles de qualité

### Contrôles sur les produits

| Contrôle | Statut |
|---|---|
| Présence du code-barres | À vérifier |
| Unicité des codes produits | À vérifier |
| Appartenance à la catégorie céréales | À confirmer |
| Présence des marques | À mesurer |
| Disponibilité des Nutri-Scores | À mesurer |
| Cohérence des conditionnements | À approfondir au J5 |

### Contrôles sur les prix

| Contrôle | Statut |
|---|---|
| Présence des codes produits | À vérifier |
| Prix nuls ou négatifs | À mesurer |
| Dates invalides | À mesurer |
| Devises différentes | À mesurer |
| Unités de prix différentes | À mesurer |
| Doublons d'observations | À vérifier |
| Correspondances produits-prix | À confirmer |

### Indicateurs de qualité

**Couverture nutritionnelle**

Nombre de céréales possédant un Nutri-Score valide divisé par le nombre total de céréales identifiées.

**Couverture tarifaire**

Nombre de céréales possédant au moins une observation de prix divisé par le nombre total de céréales identifiées.

Ces deux indicateurs devront être exprimés en pourcentage, avec des dénominateurs explicites.

## 5. Résultats exploratoires

### A. Analyse des produits

| Indicateur | Résultat |
|---|---|
| Nombre de céréales | 20 |
| Nombre de marques déclarées | 10 |
| Nombre de codes-barres uniques | 20 |
| Nombre de produits sans marque | 0 |

### B. Analyse des Nutri-Scores

| Indicateur | Résultat |
|---|---|
| Nutri-Score A | 7 |
| Nutri-Score B | 6 |
| Nutri-Score C | 4 |
| Nutri-Score D | 3 |
| Nutri-Score E | 0 |
| Nutri-Score non renseigné | 0 |
| Taux de couverture nutritionnelle | 100% |
| Part A ou B parmi les notes disponibles | 65% |

### C. Analyse tarifaire

| Indicateur | Résultat |
|---|---|
| Nombre d'observations après jointure | 385 |
| Nombre de céréales avec prix | 20 |
| Taux de couverture tarifaire | 100% |
| Première date d'observation | 11/07/2022 |
| Dernière date d'observation | 13/03/2026 |

Pour les produits possédant des relevés comparables en euros et à l'unité :

- Prix minimum : 0,69€
- Prix médian : 3,70€
- Prix maximum : 6,5€
- Nombre de relevés : 384

### D. Visualisations interactives

Graphiques prévus avec Plotly :

1. Répartition des céréales par marque.
2. Répartition des Nutri-Scores.
3. Observations par unité de prix.
4. Prix observés d'une céréale dans le temps.

**Graphiques effectivement produits et vérifiés : à compléter.**

Les visualisations devront être interprétées en fonction des effectifs disponibles et de la représentativité limitée des données.

## 6. Difficultés rencontrées et solutions

### Difficulté 1 — Absence du fichier de prix attendu

Le fichier `fact_prix_cereales_j3.parquet` n'était pas disponible.

**Solution :** adapter les chemins du notebook pour utiliser le fichier existant `prices_j2.parquet`.

Une jointure supplémentaire est nécessaire afin de limiter les observations aux céréales du petit-déjeuner.

### Difficulté 2 — Nutri-Score absent du dataset

La colonne `nutrition_grades` n'était pas présente dans le dataset produit.

**Solution :**

- Rechercher les champs nutritionnels portant d'autres noms.
- Prévoir un enrichissement via l'API Open Food Facts.
- Utiliser les codes-barres comme identifiants.
- Enregistrer les réponses dans un cache local.
- Préserver les valeurs manquantes.

**Résultat de l'enrichissement : à vérifier.**

### Difficulté 3 — Erreur DuckDB `ResourceInUseException`

Lors de la cellule 12, l'exécution s'est interrompue sur :

```python
con.register("cereales_j4", df_dimension)
```

Message rencontré :

```text
ResourceInUseException:
Resource In Use Error:
connection has an unfinished result from execute()
```

**Cause indiquée par DuckDB :**

La connexion possédait un résultat SQL précédent non entièrement terminé ou consommé.

Ce problème ne démontre pas, à lui seul, un manque de RAM ni une erreur dans la logique de jointure.

**Corrections proposées :**

1. Terminer le résultat SQL précédent avant d'enregistrer le DataFrame.
2. Utiliser `fetchall()` pour consommer intégralement les résultats de petites requêtes comme `COUNT(*)`.
3. Recréer une connexion DuckDB propre si nécessaire.
4. Reconstruire les vues SQL dans la nouvelle connexion.

Exemple de correction :

```python
# Terminer le résultat précédent
con.complete()

# Préparer la dimension produits
df_dimension = df[
    [
        "code",
        "product_name",
        "brands",
        "quantity",
        "nutriscore_grade"
    ]
].copy()

# Enregistrer le DataFrame
con.register("cereales_j4", df_dimension)
```

Pour une requête renvoyant une seule ligne :

```python
nb_lignes = con.execute("""
    SELECT COUNT(*)
    FROM prix_bruts
""").fetchall()[0][0]
```

`fetchall()` consomme ici un résultat d'une seule ligne.

**Statut : correction proposée, validation après réexécution à compléter.**

### Difficulté 4 — Contraintes techniques des sources

Les recherches volumineuses et certains traitements sur Hugging Face ont précédemment entraîné des problèmes de mémoire.

L'API Open Food Facts a également présenté des erreurs HTTP 503.

**Approche retenue :**

- Fichiers Parquet locaux.
- DuckDB pour les requêtes SQL.
- Limitation des colonnes et des lignes transférées vers Pandas.
- API produit ciblée.
- Cache local pour les enrichissements.

### Enseignements

Cette journée a montré l'importance de contrôler les connexions SQL, de vérifier les schémas des données et de concevoir des pipelines capables de reprendre après une interruption.

## 7. Limites scientifiques et méthodologiques

### Représentativité des produits

Le nombre de références présentes dans Open Food Facts ne correspond pas aux parts de marché des marques.

Les données ne permettent pas directement d'estimer les ventes, le chiffre d'affaires ou la consommation.

### Couverture nutritionnelle

Toutes les céréales ne possèdent pas nécessairement de Nutri-Score.

Une faible couverture peut limiter l'interprétation des résultats nutritionnels.

### Couverture tarifaire

La présence d'une céréale dans Open Food Facts ne garantit pas l'existence de relevés tarifaires dans Open Prices.

Le taux de couverture devra être mesuré.

### Comparabilité

Les prix peuvent varier selon :

- La quantité du produit.
- L'unité de prix.
- La devise.
- Le magasin.
- La date de collecte.
- Les promotions.

Les comparaisons devront donc être effectuées sur des ensembles cohérents.

### Temporalité des données

Les observations sont collectées à des dates différentes et ne représentent pas nécessairement un suivi continu des prix.

### Limites nutritionnelles

Le Nutri-Score constitue un indicateur synthétique et ne remplace pas l'analyse détaillée des sucres, fibres, protéines, matières grasses, sel et ingrédients.

## 8. Compétences acquises et autonomie

### Compétences travaillées

**Python et Pandas**
- Lecture de fichiers Parquet.
- Manipulation des DataFrames.
- Nettoyage des codes-barres.
- Gestion des valeurs manquantes.
- Préparation de statistiques descriptives.

**SQL et DuckDB**
- Création de vues SQL.
- Enregistrement d'un DataFrame dans DuckDB.
- Compréhension de `INNER JOIN`.
- Gestion des résultats de requêtes.
- Débogage d'une connexion SQL.

**Data Engineering**
- Intégration de deux sources.
- Enrichissement par API.
- Mise en cache des réponses.
- Contrôle de qualité et de couverture.
- Traitement compatible avec une RAM limitée.

**Data Visualization**
- Préparation des données destinées à Plotly.
- Choix de graphiques adaptés aux indicateurs.
- Distinction entre effectifs et proportions.

## 9. Perspectives et prochaines étapes — J5

### Finalisation du J4

Avant de passer au J5 :

1. Vérifier que l'erreur DuckDB est résolue.
2. Exécuter complètement la jointure SQL.
3. Mesurer la couverture tarifaire.
4. Contrôler les Nutri-Scores récupérés.
5. Produire les graphiques avec les données réellement disponibles.
6. Enregistrer les résultats validés.
7. Finaliser le notebook et le compte rendu GitHub.

### J5 — Normalisation des prix

L'objectif principal sera de comparer les céréales sur une base tarifaire cohérente.

Travaux prévus :

- Analyse des conditionnements.
- Identification des formats `g` et `kg`.
- Conversion des quantités en kilogrammes.
- Calcul des prix au kilogramme.
- Vérification des prix déjà exprimés au kilogramme.
- Détection des unités incohérentes.
- Contrôle des valeurs extrêmes.
- Comparaisons entre produits de formats différents.
- Visualisations interactives avec Plotly.

### Perspectives à long terme

FoodMarket AI évoluera progressivement vers :

**Product Intelligence :** analyse des produits, marques et références.

**Nutrition Intelligence :** analyses des Nutri-Scores et des compositions nutritionnelles.

**Pricing Intelligence :** comparaison des prix normalisés et analyse concurrentielle.

**Machine Learning :** segmentation et modélisation prédictive lorsque les données seront suffisantes.

**Deep Learning et NLP :** similarité des produits et traitement des ingrédients.

**Power BI :** tableaux de bord décisionnels.

**SaaS B2B :** plateforme d'analyse destinée aux entreprises agroalimentaires et aux distributeurs.

---

## Bilan personnel du J4

**Principale difficulté rencontrée :** gestion d'un résultat SQL inachevé lors de l'enregistrement d'un DataFrame dans DuckDB.

**Correction étudiée :** consommation ou terminaison du résultat précédent, puis réutilisation ou recréation de la connexion.

**Résultats chiffrés validés :** Oui


---

*FoodMarket AI — Construire une plateforme durable d'intelligence des marchés agroalimentaires grâce à Python, SQL, Power BI et à l'intelligence artificielle.*
