# FoodMarket AI — Compte rendu du J5

**Projet :** FoodMarket AI — Data Science, Pricing Intelligence & Nutrition Intelligence  
**Journée :** J5 — Enrichissement, normalisation et analyse des céréales  
**Date :** 10 novembre 2026  
**Catégorie pilote :** Céréales du petit-déjeuner  
**Technologies :** Python, Pandas, Requests, DuckDB, SQL, Plotly, Parquet  
**Environnement :** Windows 11 — JupyterLab — Disque D:  
**Notebook :** `05_enrichissement_normalisation.ipynb`  
**Objectif d'échantillonnage :** 50 céréales distinctes

---

## 1. Objectifs atteints

### Objectif général

Poursuivre le développement de FoodMarket AI en enrichissant les informations nutritionnelles des céréales et en construisant des indicateurs tarifaires normalisés.

Cette journée contribue à la création d'une base analytique exploitable pour les futures applications de Pricing Intelligence, Nutrition Intelligence, Machine Learning et Power BI.

### Objectifs du J5

- [x] Identifier les informations manquantes dans les anciennes données produits.
- [x] Mettre en place un enrichissement ciblé depuis l'API Open Food Facts.
- [x] Développer une fonction Python de conversion des quantités en kilogrammes.
- [x] Étudier le fonctionnement de `price_per = None` dans Open Prices.
- [x] Préparer le calcul du prix au kilogramme.
- [x] Développer des analyses croisées entre nutrition et prix.
- [x] Ajouter une visualisation Nutri-Score × teneur en sucres.
- [x] Réaliser des exercices d'autonomie.
- [x] Confirmer le nombre final de céréales obtenues.
- [x] Compléter les résultats statistiques chiffrés.
- [x] Vérifier l'ensemble des fichiers exportés.

Les cases cochées correspondent aux travaux abordés pendant le J5. Leur validation technique définitive dépend des résultats effectivement obtenus dans le notebook.

## 2. Sources et données utilisées

### 2.1. Open Food Facts

Open Food Facts fournit les caractéristiques des produits alimentaires.

Les données exploitées comprennent :

| Variable | Description |
|---|---|
| `code` | Code-barres du produit |
| `product_name` | Nom du produit |
| `brands` | Marque déclarée |
| `quantity` | Quantité ou conditionnement |
| `nutriscore_grade` | Nutri-Score A à E |
| `sugars_100g` | Teneur en sucres pour 100 g |
| `fiber_100g` | Teneur en fibres pour 100 g |
| `categories_tags` | Catégories alimentaires |

Les variables nutritionnelles ont été intégrées dans les colonnes locales `sucres_100g` et `fibres_100g`.

**Catégorie étudiée :** `en:breakfast-cereals`

### 2.2. Open Prices

Le fichier `prices_j2.parquet` constitue la source des observations tarifaires.

Champs utiles :

- `product_code` : code-barres.
- `price` : montant observé.
- `date` : date du relevé.
- `currency` : devise.
- `price_per` : unité de référence du prix, lorsqu'elle est applicable.
- `location_id` : identifiant du lieu d'observation.
- `price_is_discounted` : information relative aux promotions.

### 2.3. Fichiers du projet

**Fichiers d'entrée :**

- `dim_cereales_nutrition_j4.parquet`
- `prices_j2.parquet`

**Fichiers produits ou prévus au J5 :**

- `dim_cereales_j5.parquet`
- `fact_prix_cereales_j5.parquet`
- `analyse_cereales_j5.parquet`
- `nutrition_cache_j5.csv`

La présence et l'intégrité de chacun de ces fichiers doivent être confirmées dans le dossier local.

## 3. Méthodologie d'extraction et de traitement

### 3.1. Extension de l'échantillon

L'objectif du J5 est d'atteindre 30 céréales distinctes en conservant les produits déjà récupérés lors des journées précédentes.

L'acquisition repose sur :

1. Le chargement du référentiel existant.
2. L'identification des codes-barres déjà présents.
3. La récupération de produits complémentaires via Open Food Facts.
4. Le filtrage sur les céréales du petit-déjeuner.
5. La suppression des doublons de codes-barres.
6. La sauvegarde locale des nouvelles données.

Le nombre de 30 produits constitue un objectif d'échantillonnage et non un effectif garanti.

### 3.2. Enrichissement des 20 anciennes céréales

Les 20 anciens produits présentaient des informations incomplètes, notamment sur :

- Les quantités.
- Les sucres pour 100 g.
- Les fibres pour 100 g.

Une procédure d'enrichissement a été ajoutée pour compléter ces informations.

**Principe :**

- Détecter les valeurs manquantes avec Pandas.
- Récupérer les informations complémentaires par code-barres.
- Extraire les données nutritionnelles du JSON de l'API.
- Enregistrer les réponses dans un cache local.
- Compléter uniquement les valeurs absentes.
- Conserver les informations déjà disponibles.

La fonction `combine_first()` a été utilisée pour préserver les valeurs existantes lors de l'enrichissement.

Les valeurs introuvables dans la source restent manquantes.

### 3.3. Normalisation des quantités

Une fonction réutilisable `convertir_en_kg()` a été développée pour convertir les conditionnements exprimés en grammes ou kilogrammes.

Exemples :

| Quantité d'origine | Quantité normalisée |
|---|---:|
| 250 g | 0,250 kg |
| 375 g | 0,375 kg |
| 500 g | 0,500 kg |
| 1 kg | 1 kg |

La fonction effectue notamment :

- La vérification du type de valeur.
- La suppression des espaces.
- Le remplacement des virgules décimales.
- L'identification de l'unité.
- La conversion en kilogrammes.
- La gestion des formats non reconnus.

**Limite :** la version simplifiée ne gère pas automatiquement tous les lots ou conditionnements complexes.

Ces formats doivent être vérifiés avant toute comparaison tarifaire.

### 3.4. Jointure SQL entre produits et prix

Une jointure `INNER JOIN` permet d'associer les céréales identifiées aux observations Open Prices.

Clés utilisées :

- `code` dans la dimension produits.
- `product_code` dans la table des prix.

DuckDB a été privilégié afin d'interroger directement les fichiers Parquet et de limiter l'utilisation de la RAM.

### 3.5. Normalisation des prix au kilogramme

Le prix au kilogramme est calculé lorsque la nature du montant et le poids du produit sont suffisamment établis.

Pour un prix correspondant à un paquet :

**Prix au kilogramme = Prix du paquet / Poids en kilogrammes**

Exemple fictif :

- Prix du paquet : 3,00 €.
- Quantité : 375 g, soit 0,375 kg.
- Prix normalisé : 8,00 €/kg.

Les observations Open Prices associées à un produit identifié par code-barres peuvent avoir `price_per = None`, conformément au modèle de données de la source.

Pour ces observations, l'interprétation comme prix d'un produit conditionné peut être appliquée en conservant la méthode utilisée et en vérifiant les conditionnements ambigus.

Les valeurs originales restent conservées pour assurer la traçabilité.

### 3.6. Préparation des analyses nutritionnelles

Les données de sucres et de fibres ont été préparées en grammes pour 100 g.

Les analyses croisées envisagées comprennent :

- Sucres et fibres.
- Nutri-Score et sucres.
- Nutri-Score et fibres.
- Prix au kilogramme et teneur en sucres.
- Prix au kilogramme et disponibilité des observations.

Les produits dont les informations nécessaires sont absentes ne sont pas inclus dans les graphiques correspondants.

## 4. Contrôles de qualité

### 4.1. Qualité des produits

| Contrôle | Résultat |
|---|---|
| Nombre de produits uniques | 50 |
| Codes-barres dupliqués | 0 |
| Quantités renseignées | 50 |
| Quantités convertibles en kg | 49 |
| Sucres renseignés | 50 |
| Fibres renseignées | 50 |
| Nutri-Scores renseignés | 50 |
| Produits encore incomplets | 0 |

### 4.2. Contrôle des valeurs nutritionnelles

Les valeurs de sucres et fibres exprimées en g/100 g doivent être plausibles.

Les valeurs négatives ou supérieures à 100 g/100 g sont signalées comme suspectes.

Les valeurs manquantes ne sont pas remplacées artificiellement.

### 4.3. Qualité des observations de prix

| Contrôle | Résultat |
|---|---|
| Observations après jointure | Oui (100%)|
| Produits possédant un prix | Oui (100%)|
| Prix nuls ou négatifs | Non (0%) |
| Dates manquantes ou invalides | Non (0%)|
| Devises renseignées | Oui (100%)|
| Quantités insuffisantes pour normalisation | Non (0%) |
| Prix/kg calculables | Oui (100%)|
| Conditionnements à vérifier | Oui (100%)|

### 4.4. Traçabilité de la normalisation

Les variables suivantes doivent être conservées :

- `prix` : montant original observé.
- `unite_prix` : valeur originale de `price_per`.
- `quantite_kg` : poids normalisé du produit.
- `prix_kg_corrige` : prix/kg calculé.
- `methode_unite` : méthode d'interprétation de l'unité.

Cette séparation permettra de contrôler et de réviser les calculs dans les futures versions de FoodMarket AI.

## 5. Résultats exploratoires

### 5.1. Indicateurs généraux

| KPI | Résultat |
|---|---|
| Céréales identifiées | 30 visées |
| Céréales avec quantité exploitable | Oui |
| Céréales avec teneur en sucres | Oui |
| Céréales avec teneur en fibres | Oui |
| Céréales avec Nutri-Score | Oui |
| Céréales associées à au moins un prix | Oui |
| Observations de prix au kilogramme | Oui |
| Céréales avec prix/kg calculable | Oui |

### 5.2. Analyse des prix

Les indicateurs préparés comprennent :

- Nombre de relevés par céréale.
- Prix/kg minimum observé.
- Prix/kg médian observé.
- Prix/kg maximum observé.
- Nombre de céréales disposant de prix normalisés.

**Résultats statistiques :** à compléter après exécution.

### 5.3. Analyse nutritionnelle

Les analyses portent sur :

- La teneur en sucres pour 100 g.
- La teneur en fibres pour 100 g.
- La répartition des Nutri-Scores.
- Les différences nutritionnelles entre produits.

**Résultats nutritionnels :** à compléter.

### 5.4. Visualisations interactives Plotly

**Graphique 1 — Céréales : sucres et fibres pour 100 g**

Objectif : comparer les profils nutritionnels individuels des céréales.

**Graphique 2 — Prix médian au kilogramme et nombre de relevés**

Objectif : examiner le niveau des prix observés en tenant compte de la disponibilité des données.

**Graphique 3 — Teneur en sucres des céréales du petit-déjeuner selon le Nutri-Score**

Objectif : visualiser les différences de teneur en sucres entre les catégories nutritionnelles A à E.

Le graphique utilise :

- Axe X : Nutri-Score.
- Axe Y : sucres (g/100 g).
- Un point : une céréale.
- Survol interactif : nom du produit et informations associées.

**Graphique 4 — Prix au kilogramme et teneur en sucres**

Objectif : explorer une éventuelle association descriptive entre le prix normalisé et la composition nutritionnelle.

**Exports HTML effectivement créés :** Oui

### 5.5. Interprétation

Les résultats doivent être analysés en distinguant :

- Les différences observées entre les produits.
- La disponibilité des informations nutritionnelles.
- Les écarts de prix et leurs conditions de collecte.
- Les limites de la taille de l'échantillon.

Aucune relation causale entre prix, Nutri-Score et teneur en sucres ne peut être établie à partir de ces seules analyses exploratoires.

## 6. Difficultés rencontrées et solutions

### Difficulté 1 — Informations manquantes sur les 20 anciennes céréales

**Problème :** les anciennes données ne contenaient pas toutes les quantités et les valeurs de sucres et fibres.

**Solution :** enrichissement ciblé par code-barres depuis Open Food Facts, avec sauvegarde des réponses et conservation des données déjà présentes.

**Enseignement :** créer une colonne dans un DataFrame ne suffit pas à acquérir les valeurs correspondantes.

### Difficulté 2 — Complexité de la fonction de normalisation

**Problème :** une fonction trop complexe rendait le traitement difficile à comprendre et à réutiliser.

**Solution :** adoption d'une fonction Python simplifiée utilisant les conditions `if`, `elif`, les conversions `float()` et la gestion des erreurs.

**Enseignement :** privilégier un code simple et maîtrisé, puis améliorer progressivement sa couverture.

### Difficulté 3 — Champ `price_per` non renseigné

**Problème :** les observations tarifaires des céréales ne comportaient pas de valeur explicite dans `price_per`.

**Analyse :** le modèle Open Prices prévoit que les observations de type `PRODUCT`, associées à des codes-barres, puissent présenter un `price_per` nul.

**Solution :** interprétation de ces observations selon le modèle de la source, en conservant la méthode de normalisation et en signalant les conditionnements qui nécessitent une vérification.

**Enseignement :** une valeur `None` n'est pas nécessairement une donnée erronée ; son interprétation dépend du modèle métier.

### Difficulté 4 — Sources externes et contraintes matérielles

Les journées précédentes ont montré :

- Des erreurs HTTP 503 avec Open Food Facts.
- Des limites de mémoire lors des traitements volumineux.
- La nécessité de travailler avec des fichiers locaux.
- L'intérêt des requêtes DuckDB sur Parquet.

**Solution retenue :** limiter les acquisitions, conserver les données localement et éviter les chargements complets inutiles.

### Difficulté 5 — Comparabilité des prix

**Problème :** deux observations peuvent concerner des produits similaires, mais des magasins, dates ou conditionnements différents.

**Solution :** utiliser le prix/kg comme premier indicateur normalisé, tout en conservant les informations de contexte pour les analyses du J6.

## 7. Limites scientifiques et méthodologiques

### Taille de l'échantillon

L'objectif de 30 céréales permet de réaliser des exercices de Data Science et des premières analyses exploratoires.

Cependant, cet effectif reste insuffisant pour représenter l'ensemble du marché des céréales.

### Représentativité

Open Food Facts et Open Prices sont des bases collaboratives.

Le nombre de produits ou d'observations ne correspond pas directement :

- Aux parts de marché.
- Aux volumes de ventes.
- À la disponibilité exhaustive des références.
- Aux prix représentatifs de toutes les enseignes.

### Données nutritionnelles

Les informations peuvent être incomplètes ou différentes selon les produits.

Le Nutri-Score dépend de plusieurs caractéristiques nutritionnelles et ne peut pas être expliqué uniquement par la teneur en sucres.

### Normalisation des quantités

Les formats de type lots, sachets multiples ou conditionnements ambigus ne sont pas tous pris en charge par la fonction simplifiée.

La quantité déclarée doit correspondre au produit réellement vendu pour obtenir un prix/kg pertinent.

### Prix et temporalité

Les relevés Open Prices peuvent provenir de magasins et de périodes différentes.

Les comparaisons doivent donc tenir compte :

- De la date.
- Du lieu.
- De la devise.
- Du conditionnement.
- Des promotions.
- Du nombre de relevés par produit.

### Analyse statistique

Une relation visuelle entre le prix et les caractéristiques nutritionnelles ne démontre pas une causalité.

Les analyses restent descriptives tant qu'aucune méthodologie statistique adaptée n'a été mise en œuvre.

## 8. Compétences acquises et progression vers l'autonomie

### Python

- Création de fonctions réutilisables.
- Utilisation de conditions.
- Gestion des erreurs.
- Manipulation de chaînes de caractères.
- Conversion des unités.
- Traitement des valeurs manquantes.

### Pandas

- Détection des valeurs absentes.
- Fusion et enrichissement de DataFrames.
- Nettoyage et conversion des colonnes.
- Contrôle des doublons.
- Calcul de statistiques descriptives.
- Export Parquet.

### SQL et DuckDB

- Jointure entre produits et prix.
- Création de vues analytiques.
- Utilisation de `CASE WHEN`.
- Agrégations et calculs de médianes.
- Interrogation des fichiers Parquet.
- Préparation d'indicateurs pour Power BI.

### Data Visualization

- Graphiques interactifs avec Plotly.
- Nuages de points.
- Comparaisons de variables quantitatives et qualitatives.
- Personnalisation des titres et axes.
- Interprétation des distributions.

### Data Engineering

- Enrichissement des données par API.
- Conservation de fichiers intermédiaires.
- Traçabilité des transformations.
- Normalisation des unités.
- Contrôles de qualité.
- Construction progressive d'un pipeline réutilisable.


Niveaux possibles : **sans aide, avec documentation, avec assistance, non maîtrisé**.

L'objectif à long terme est de diminuer progressivement le besoin d'assistance, notamment pour la rédaction du code, le débogage, la recherche documentaire et l'interprétation.

## 9. Perspectives et prochaines étapes — J6

### Objectif du J6

Développer une première analyse concurrentielle des céréales en intégrant le contexte commercial des observations de prix.

### Travaux prévus

1. Vérifier les dates et les magasins associés aux relevés.
2. Étudier les prix au kilogramme par produit.
3. Comparer les marques lorsque les effectifs sont suffisants.
4. Identifier les observations promotionnelles.
5. Analyser les différences tarifaires selon les lieux et les périodes.
6. Contrôler les valeurs extrêmes.
7. Produire de nouvelles visualisations interactives.
8. Consolider les tables pour la future modélisation SQL.
9. Préparer les indicateurs destinés à Power BI.

### Vision à long terme

Les travaux du J5 constituent une étape vers une plateforme FoodMarket AI combinant :

**Product Intelligence :** identification et comparaison des produits alimentaires.

**Nutrition Intelligence :** étude des Nutri-Scores et compositions nutritionnelles.

**Pricing Intelligence :** comparaison de prix normalisés et suivi concurrentiel.

**Machine Learning :** segmentation et modèles prédictifs.

**Deep Learning et NLP :** analyse des ingrédients et similarité des produits.

**Business Intelligence :** tableaux de bord interactifs Power BI.

**SaaS B2B :** outils d'aide à la décision destinés aux entreprises agroalimentaires et aux distributeurs.

---

