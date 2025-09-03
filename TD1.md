# TD1 – Explorer l'environnement, explorer les données

**Date :** 06/02/2022  
**Nom :** Herrault PA - Chardon V

## Objectifs
- Découverte de l’interface du logiciel QGIS ;  
- Charger, explorer, enregistrer des données.

---

## 1. Données
Les données utilisées pour le TD sont dans le dossier compressé `data_TD1`, à retrouver sur Moodle.  
Vous devez le décompresser où vous voulez.  
Il s’agit de données thématiques recouvrant le territoire de trois communes du département Français des Pyrénées Orientales (routes, bâti, végétation …).

**Question :** Quel est le format des données contenues dans le dossier du TD1 ? Cherchez sur Internet à quoi correspondent les différents formats (`.shp`, `.shx`, etc.).

- `.shp` : stocke les entités géographiques. Il s'agit du shapefile proprement-dit, utilisé pour visualiser les données dans QGIS.  
- `.dbf` : stocke les données attributaires (tableur consultable avec Excel ou LibreOffice). Ne pas modifier le fichier original.  
- `.shx` : stocke les index des entités géométriques contenues dans le `.shp`.  
- `.prj` : stocke le système de projection des données géométriques.

**Note :** Pour avoir l’intégralité des fonctionnalités, une couche vectorielle doit au minimum avoir `.shp`, `.dbf`, `.shx` et `.prj`.

---

## 2. Créer un espace de travail
1. Créez un dossier nommé `TD1`.  
2. Copiez les données décompressées précédemment dans ce dossier.  

**Règles de dénomination des fichiers :**  
- Pas d'espaces dans les noms (`Parcelle en herbe` → `Parcelle_en_herbe`)  
- Pas de caractères accentués (`Propriétaire` → `Proprietaire`)  
- Pas de caractères spéciaux (`& : ; / \ @…`)  
- Pas de chiffre comme premier caractère (`1990_habitat` → `Habitat_1990`)  

---

## 3. Connecter l’espace de travail dans QGIS
1. Lancez **QGIS 3.x**.  
2. Dans le panneau **Navigateur**, ajoutez le dossier `TD1` en tant que source de données.  
3. La source apparaît maintenant dans QGIS, vous permettant de naviguer et charger les fichiers.

---

## 4. Créer une GeoPackage et organiser les données
1. Dans QGIS : **Clic droit sur `TD1` → Nouvelle GeoPackage**.  
2. Nommez-la `database.gpkg`. Cette GeoPackage servira de **base de travail centralisée**.  
3. Créez deux **groupes** (ou classes) à l’intérieur de la GeoPackage :  
   - `lineaire` → pour les entités linéaires  
   - `surfacique` → pour les entités surfaciques  

**Astuce :** Vérifiez le système de coordonnées (EPSG) de chaque couche avant import.  

4. Importez les fichiers `.shp` dans les groupes correspondants.  
5. Renommez les couches dans l’arborescence et observez comment QGIS gère les fichiers.

---

## 5. Explorer les données
1. **Créer une carte** : `Projet → Nouvelle Carte`.  
2. Ajoutez vos couches depuis la GeoPackage en les glissant dans le panneau **Couches**.  
3. Enregistrez votre projet (`Projet → Enregistrer sous`).

### 5.1 Table attributaire
- Clic droit sur une couche → **Ouvrir la table attributaire**.  
- Observez champs et types de données.  

### 5.2 Symbologie
- Clic droit sur la couche → **Propriétés > Symbologie > Valeurs uniques**.  
- Choisissez un champ et **Ajouter toutes les valeurs**.  
- Appliquez une palette de couleurs.  

### 5.3 Étiquettes
- Clic droit sur la couche → **Propriétés > Étiquettes > Simple**.  
- Affichez `nom_station` ou `nom_commune`.  

### 5.4 Sélection par attributs
- Menu : **Sélection → Sélection par expression**  
- Exemples :  
  - Voies rapides (`grand_axe`)  
  - Bâtiments sportifs (`batiment_public`)  

**Questions :**  
- Quelle est la distance entre le centre-ville de Torreilles et Villelongue-de-la-Salanque ?  
- Quelle est la longueur et la nature du tronçon `TRONROUT0000000038686356` ?  

### 5.5 Sélection par localisation
- Menu : **Vecteur → Outils de géotraitement → Sélection par localisation**  
- Exemple : communes à l’intérieur de la couche `limite_cus`.  

### 5.6 Importer un CSV (stations VéloHop)
1. Placez le fichier CSV dans `Donnees`.  
2. **Couches → Ajouter une couche → Ajouter une couche de texte délimité**  
   - X = `lg`  
   - Y = `la`  
   - EPSG = 4326 (WGS84)  

**Question :** Quelle station dispose du plus grand nombre de vélos disponibles ?

### 5.7 Calcul géométrique
- Ouvrir la table attributaire → **Calculatrice de champ**  
- Ajouter un champ `surface` ou `longueur` et calculer la géométrie.  

**Questions :**  
- Surface totale des gravières ?  
- Surface du Rhin ?  

### 5.8 Exporter des sous-ensembles
- Sélection → **Couches → Exporter → Enregistrer les entités sélectionnées sous…**  

**Exercices :**  
- Exporter jardins familiaux → `Traitements`  
- Exporter stations VéloHop → `Traitements`  
- Exporter stations VéloHop >10 vélos → `Resultats`

---

## 6. Exploration des données raster
### 6.1 Raster multi-bande (orthophoto)
- Importez `ortho_2018_CC48.tif`.  
- Vérifiez résolution, système de coordonnées, unité et valeurs des pixels (outil **Identifier**).  

**Questions :**  
- Résolution du raster ?  
- Système de coordonnées et unité ?  
- Valeurs aux coordonnées X=2050750 Y=7275678 ?

### 6.2 Raster mono-bande (hauteur des toits)
- Importez `hauteur_toits_CC48.tif`.  
- Vérifiez résolution, nombre de bandes, et interprétation des valeurs.  
- Symbologie → Classification manuelle :  
  - >100 m → rouge  
  - 1–5 m → vert  

### 6.3 Distances et coordonnées
- Identifier la flèche de la cathédrale et la statue de la Place de la République.  
- Mesurer distance à vol d’oiseau (outil **Mesurer**).  
- Vérifier distance à partir des coordonnées et unité du système.

---

## 7. Bilan
- Organisation de projet QGIS avec GeoPackage.  
- Exploration des couches vectorielles (attributs, symbologie, étiquettes).  
- Sélection par attributs et localisation, export de sous-ensembles.  
- Calculs géométriques (surface, longueur).  
- Exploration et symbologie des rasters.  
- Mesures et lecture de coordonnées.

