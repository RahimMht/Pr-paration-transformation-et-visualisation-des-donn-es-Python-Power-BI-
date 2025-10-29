# 🛫 Analyse des Accidents d’Aviation – NTSB Dataset

## 📖 Contexte du projet

Ce projet s’appuie sur la base de données publique du **National Transportation Safety Board (NTSB)**, une agence fédérale américaine chargée d’enquêter sur les accidents de transport.  
Le jeu de données contient des informations sur les **accidents d’aviation civile** aux États-Unis, depuis **1962**, incluant les circonstances, les phases de vol, les conditions météo, le type d’aéronef et la gravité des incidents.

🔗 **Source officielle :** [NTSB Aviation Accident Database](https://www.ntsb.gov/Pages/AviationQuery.aspx)

---

## 🎯 Objectifs

L’objectif du projet est de comprendre les causes et conséquences des accidents d’avions à travers une approche **exploratoire, analytique et visuelle** :

- Identifier les modèles d’avions ou types de moteurs plus à risque  
- Analyser l’impact des conditions météorologiques  
- Étudier la relation entre le **nombre de moteurs** et la **probabilité de survie**  
- Déterminer les **phases de vol les plus dangereuses**

---

## 🧰 Technologies et bibliothèques utilisées

- **Python 3**
- **pandas**, **numpy** → nettoyage, manipulation et agrégation des données  
- **matplotlib**, **seaborn** → visualisation et analyse exploratoire  
- **scipy.stats** → analyse statistique et détection d’outliers  
- **catboost**, **LabelEncoder** → encodage des variables catégorielles  
- **Power BI** → exploration interactive et visualisation finale  

---

## ⚙️ Étapes principales du projet

### 1. Importation et inspection initiale
- Import du fichier `AviationData.csv` avec encodage `mac_roman`
- Création d’un tableau récapitulatif affichant :
  - le nom de chaque colonne  
  - le nombre de valeurs manquantes  
  - le type de données
- Application de `describe()` pour un aperçu statistique des variables numériques

### 2. Nettoyage et préparation des données
- Conversion de la colonne **Event.Date** en type `datetime`
- Création d’une colonne **Year** pour l’analyse temporelle  
- Suppression des colonnes non pertinentes pour l’étude  
- Remplacement des valeurs manquantes :
  - Par le **mode** pour les colonnes catégorielles  
  - Par des valeurs neutres pour certaines colonnes clés  
- Encodage des variables qualitatives avec **LabelEncoder**
- Codage numérique de la gravité des dommages :
  - `Substantial = 1`, `Minor = 0`, `Destroyed = 2`

### 3. Analyse exploratoire
- **Évolution temporelle des accidents (1982–2020)**  
  → regroupement des événements par année et visualisation linéaire  
- **Répartition mensuelle des accidents**  
  → extraction du mois depuis `Event.Date`, regroupement par `Month.Abbr`  
- **Phases de vol les plus risquées**  
  → regroupement par `Broad.phase.of.flight` et représentation en barres  
- **Analyse par gravité (fatal, grave, mineur)**  
  → regroupement et comparaison des tendances dans le temps  
- **Étude du nombre de moteurs et de la gravité**  
  → corrélation visuelle et statistique avec `Number.of.Engines`

### 4. Détection d’outliers et distribution
- Création de **boxplots** pour identifier les valeurs extrêmes  
- Nettoyage des données aberrantes selon leur distribution statistique  

---

## 💾 Exportation et préparation pour Power BI

Une fois les données nettoyées et structurées, elles ont été exportées dans un format exploitable pour Power BI afin de créer un tableau de bord interactif.

### 🔹 Code d’export Power BI

```python
# ================================
# 16. Préparation pour Power BI
# ================================

# Sélection des colonnes pertinentes
colonnes_selectionnees = [
    'Event.Id', 'Event.Date', 'Year', 'Month.Abbr',
    'Location', 'Country', 'Make', 'Model', 'Injury.Severity',
    'Aircraft.damage', 'Broad.phase.of.flight', 'Purpose.of.flight',
    'Weather.Condition', 'Engine.Type', 'Total.Fatal.Injuries',
    'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured',
    'Number.of.Engines'
]

# Création du DataFrame final
df_powerbi = Aviation[colonnes_selectionnees].copy()

# Nettoyage final
df_powerbi.drop_duplicates(inplace=True)
df_powerbi.fillna('Non Renseigné', inplace=True)

# Export CSV pour Power BI
df_powerbi.to_csv("Aviation_Cleaned_for_PowerBI.csv", index=False, encoding='utf-8')

print("✅ Données nettoyées et prêtes pour Power BI")
