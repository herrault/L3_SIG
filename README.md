# TD 1 Explorer l'environnement, explorer les données

**Date :** …  
**Nom :** …  

## 🎯 Objectifs
- Découvrir et explorer l’interface QGIS.  
- Manipuler des données vectorielles et raster.  
- Réaliser des traitements simples et extraire des informations géométriques.  
- Répondre aux questions pratiques pour se familiariser avec les SIG.  

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Traitements`  
   - `Resultats`  
   - `Annexes`  


---

## 1. Importer les données vectorielles (shapefiles)

### 1.1 Pyrénées (TD1)
1. Décompressez `data_TD1` dans `Donnees`.  
2. Lancez **QGIS 3.34** et connectez le dossier `Donnees`.  
3. Créez un **GeoPackage** nommé `database.gpkg` dans `TD_QGIS`.  
4. Ajoutez deux groupes à l’intérieur : `lineaire` et `surfacique`.  
5. Importez les shapefiles linéaires et surfaciques dans les groupes correspondants.  
6. Vérifiez le **système de coordonnées (EPSG)** de chaque couche.  

**Questions Pyrénées :**  
- Quel est le système de coordonnées des couches ?  
- Quelle est la distance entre Torreilles et Villelongue-de-la-Salanque ?  
- Quelle est la longueur et la nature du tronçon `TRONROUT0000000038686356` ?  

### 1.2 Strasbourg (TD2)
1. Téléchargez les shapefiles de l’Eurométropole de Strasbourg (data.strasbourg.eu) et décompressez-les dans `Donnees`.  
2. Importez : `voie_tram`, `voie_ferree`, `surface_eau`, `station_tram`, `limite_cus`, `grand_axe`, `cours_eau`, `commune`, `batiment_public`, `bati_indiv`, `amenagt_es_vert`.  

**Questions Strasbourg :**  
- Quel est le système de coordonnées des shapefiles EMS ?  
- Quels shapefiles sont points ? lignes ? polygones ?  
- Combien de bâtiments individuels ? Combien de communes ?

---

## 2. Exploration des données vectorielles

### 2.1 Table attributaire
- Ouvrez les tables attributaires et observez les champs.  
- Trier et filtrer pour répondre aux questions.  

### 2.2 Symbologie
- Clic droit sur une couche → **Propriétés > Symbologie > Valeurs uniques**.  
- Choisir un champ, **Ajouter toutes les valeurs**, et appliquer une palette de couleurs.  
- Exporter/importer la symbologie dans `Annexes`.  

### 2.3 Étiquettes
- Clic droit sur la couche → **Étiquettes** → afficher `nom_station` ou `nom_commune`.  
- Modifier police, taille, couleur si nécessaire.  

### 2.4 Sélection par attributs
- Sélectionner voies rapides (`grand_axe`) et bâtiments sportifs (`batiment_public`).  
- Sélection manuelle possible via l’outil **Sélectionner des entités**.  

**Questions :**  
- Nombre de voies rapides ?  
- Nombre de bâtiments sportifs ?  

### 2.5 Sélection par localisation
- Outil : **Vecteur > Outils de recherche > Sélection par localisation**.  
- Exemple : sélectionner toutes les communes à l’intérieur de `limite_cus`.

### 2.6 Import CSV (Strasbourg)
1. Téléchargez les stations VelHop et placez le dans `Donnees`.  
2. Ouvrez avec Excel/LibreOffice et vérifiez les champs latitude (`la`) et longitude (`lg`).  
3. Dans QGIS : clic droit → **Afficher XY**, X=longitude, Y=latitude, système EPSG GPS.  

**Question :** Quelle station a le plus de vélos disponibles ?  

### 2.7 Calcul géométrique
- Table attributaire → **Ajouter un champ** → calculer `surface` ou `longueur`.  
- Clic droit sur le champ → **Calculer la géométrie**.  

**Questions :**  
- Surface totale des gravières ?  
- Surface du Rhin ?  

### 2.8 Export de sous-ensembles
- Sélection → **Créer couche à partir des entités sélectionnées** → Export.  

**Exercices :**  
- Exporter jardins familiaux → `Traitements`.  
- Exporter stations VelHop → `Traitements`.  
- Exporter stations VelHop avec >10 vélos → `Resultats`.

---

## 3. Exploration des données raster

### 3.1 Raster multi-bande (ortho)
- Importer `ortho_2018_CC48.tif`.  
- Questions :  
  - Résolution du raster ?  
  - Système de coordonnées et unité ?  
  - Valeurs des pixels aux coordonnées X=2050750 Y=7275678 ?  

### 3.2 Raster mono-bande (hauteur_toits)
- Importer `hauteur_toits_CC48.tif`.  
- Questions :  
  - Résolution du raster ?  
  - Nombre de bandes ?  
  - Que représentent les valeurs ?  
- Symbologie : **Classification manuelle**, ajuster classes et couleurs.  

**Exercice :** colorer toits >100m en rouge, 1–5m en vert.  

### 3.3 Distances et coordonnées
- Identifier flèche cathédrale et statue Place de la République.  
- Mesurer distance à vol d’oiseau avec outil **Mesurer**.  
- Calculer distance à partir des coordonnées et unité du système.

---

## 4. Bilan et réflexions
- Création et organisation d’un projet QGIS.  
- Exploration et symbologie des données vectorielles.  
- Export de sous-ensembles vectoriels.  
- Exploration et symbolisation de données raster.  
- Calcul géométrique et distances.




