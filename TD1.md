# TD 1 – Explorer l'environnement, explorer les données

**Date :** 02-09-2025  
**Nom :** Herrault PA - Chardon V

## 🎯 Objectifs
- Découvrir et explorer l’interface QGIS.  
- Lire et organiser des données vectorielles et raster.  
- Réaliser des traitements simples et extraire des informations géométriques/attributaires de base.  

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD1_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees` (fichiers bruts fournis)  
   - `Traitements` (couches intermédiaires créées)  
   - `Resultats` (produits finaux)  
   - `Annexes` (exports de symbologie, captures d’écran, notes, etc.)  

Cette organisation permettra de **séparer les données sources** (jamais modifiées) des résultats produits pendant le TD.  

---

## 1. Importer et organiser les données vectorielles

L’objectif de cette partie est de savoir **importer des données shapefiles** et de les organiser dans un **GeoPackage** pour faciliter leur gestion.

### 1.1 Jeux de données Pyrénées
1. Décompressez `data_TD_pyrenees` dans le dossier `Donnees`.  
2. Dans QGIS, créez un **GeoPackage** nommé `database.gpkg` dans `TD1_QGIS`.  
   - Ce fichier servira de **base de travail centralisée**.  
   - Chaque shapefile importé y sera enregistré comme une couche.  
3. Importez les shapefiles dans `database.gpkg`.  
   - Regroupez visuellement les couches dans deux groupes QGIS :  
     - `lineaire` (routes, cours d’eau, etc.)  
     - `surfacique` (communes, zones naturelles, etc.)  
4. Vérifiez le **système de coordonnées (EPSG)** de chaque couche.  

**Questions Pyrénées :**  
- Quel est le système de coordonnées des couches importées ?  
- Quelle est la distance entre Torreilles et Villelongue-de-la-Salanque ?  
- Quelle est la longueur et la nature du tronçon `TRONROUT0000000038686356` ?  

### 1.2 Jeux de données Strasbourg
1. Décompressez `data_TD_eurometropole` dans `Donnees`.  
2. Importez dans QGIS les couches suivantes :  
   - `voie_ram`, `voie_ferree`, `surface_eau`, `station_tram`, `limite_cus`, `grand_axe`, `cours_eau`, `commune`, `batiment_public`, `bati_indiv`, `amenagt_es_vert`.  
3. Organisez-les dans des groupes logiques (par exemple : transport, eau, bâti, administratif).  

**Questions Strasbourg :**  
- Quel est le système de coordonnées des shapefiles EMS ?  
- Quels shapefiles sont de type points ? lignes ? polygones ?  
- Combien de bâtiments individuels ? Combien de communes ?  

---

## 2. Exploration des données vectorielles

L’objectif est de **comprendre le contenu** des couches grâce aux tables attributaires, symbologies, sélections et exports.

### 2.1 Table attributaire
- Faites un clic droit sur une couche → **Ouvrir la table attributaire**.  
- Observez les champs, types de données et effectifs.  
- Utilisez **Trier** ou **Filtrer** pour explorer le contenu.  

### 2.2 Symbologie
- Clic droit sur une couche → **Propriétés > Symbologie > Valeurs uniques**.  
- Choisissez un champ, cliquez sur **Ajouter toutes les valeurs**, appliquez une palette.  
- Exportez la symbologie dans `Annexes`.  

### 2.3 Étiquettes
- Clic droit sur la couche → **Propriétés > Étiquettes**.  
- Affichez `nom_station` (stations de tram) ou `nom_commune`.  
- Modifiez police, taille, couleur si nécessaire.  

### 2.4 Sélection par attributs
- Menu : **Vecteur > Outils de recherche > Sélectionner par expression**.  
- Exemples :  
  - Sélectionner les voies rapides (`grand_axe`).  
  - Sélectionner les bâtiments sportifs (`batiment_public`).  

**Questions :**  
- Combien de voies rapides ?  
- Combien de bâtiments sportifs ?  

### 2.5 Sélection par localisation
- Menu : **Vecteur > Outils de recherche > Sélection par localisation**.  
- Exemple : sélectionner toutes les communes incluses dans `limite_cus`.  

### 2.6 Importer des données CSV (stations VéloHop)
1. Téléchargez les stations VéloHop et placez le fichier dans `Donnees`.  
2. Ouvrez le CSV avec Excel/LibreOffice et vérifiez les champs `la` (latitude) et `lg` (longitude).  
3. Dans QGIS : **Clic droit sur le CSV > Importer comme couche de texte délimité**.  
   - X = `lg`  
   - Y = `la`  
   - Système de coordonnées : WGS84 (EPSG:4326).  

**Question :** Quelle station dispose du plus grand nombre de vélos disponibles ?  

### 2.7 Calculs géométriques
- Ouvrez la table attributaire → **Calculatrice de champ**.  
- Ajoutez un champ `surface` ou `longueur` selon le type de géométrie.  
- Menu : **Clic droit sur le champ > Calculer la géométrie** pour vérifier.  

**Questions :**  
- Quelle est la surface totale des gravières ?  
- Quelle est la surface du Rhin ?  

### 2.8 Exporter des sous-ensembles
- Après une sélection → **Clic droit > Exporter > Sauvegarder les entités sélectionnées sous…**.  

**Exercices :**  
- Exporter les jardins familiaux → `Traitements`.  
- Exporter toutes les stations VéloHop → `Traitements`.  
- Exporter les stations VéloHop avec plus de 10 vélos → `Resultats`.  

---

## 3. Exploration des données raster

Objectif : comprendre la structure des données raster (résolution, système de coordonnées, valeurs de pixels) et les manipuler pour en extraire de l’information.

### 3.1 Raster multi-bande (orthophoto)
- Importez `ortho_2018_CC48.tif`.  
- Ouvrez les propriétés pour examiner les métadonnées.  

**Questions :**  
- Quelle est la résolution du raster ?  
- Quel est son système de coordonnées et son unité ?  
- Quelles sont les valeurs de pixels aux coordonnées X=2050750, Y=7275678 ?  

### 3.2 Raster mono-bande (hauteur des toits)
- Importez `hauteur_toits_CC48.tif`.  

**Questions :**  
- Quelle est la résolution du raster ?  
- Combien de bandes contient-il ?  
- Que représentent les valeurs des pixels ?  

- Modifiez la symbologie : **Propriétés > Symbologie > Classification manuelle**.  
- Créez des classes colorées, par exemple :  
  - Toits >100 m → rouge  
  - Toits entre 1–5 m → vert  

### 3.3 Distances et coordonnées
- Identifiez la flèche de la cathédrale et la statue de la Place de la République.  
- Mesurez la distance à vol d’oiseau avec l’outil **Mesurer**.  
- Vérifiez le calcul à partir des coordonnées et de l’unité du système.  

---

## 4. Bilan et réflexions
- Organisation d’un projet QGIS et structuration des données dans un GeoPackage.  
- Exploration des données vectorielles (tables attributaires, symbologie, étiquetage).  
- Sélections par attributs et localisation, exports de sous-ensembles.  
- Calculs géométriques simples (surfaces, longueurs).  
- Exploration et symbolisation de données raster.  
- Manipulation d’outils de mesure et de lecture de coordonnées.  

