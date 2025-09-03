# TD 1 – Explorer l'environnement, explorer les données

**Date :** 02-09-2025  
**Nom :** Herrault PA - Chardon V

## 🎯 Objectifs
- Découvrir et explorer l’interface QGIS.  
- Lire et organiser des données vectorielles et raster.  
- Réaliser des traitements simples et extraire des informations géométriques et attributaires de base.  

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD1_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees` (fichiers bruts fournis)  
   - `Traitements` (couches intermédiaires créées)  
   - `Resultats` (produits finaux)  
   - `Annexes` (exports de symbologie, captures d’écran, notes, etc.)  

*Cette organisation permet de **séparer les données sources** (jamais modifiées) des résultats produits pendant le TD et de travailler de manière reproductible.*

---

## 1. Importer et organiser les données vectorielles (Eurométropole)

L’objectif est de **savoir importer des shapefiles**, les stocker dans un **GeoPackage** central et les organiser en groupes logiques.

### 1.1 Jeux de données Strasbourg
1. Décompressez `data_TD_eurometropole` dans le dossier `Donnees`.  
2. Dans QGIS, créez un **GeoPackage** nommé `database.gpkg` dans `TD1_QGIS`.  
   - Le GeoPackage servira de **base de travail centralisée** pour toutes les couches vectorielles.  
   - Chaque shapefile importé y sera enregistré comme une couche pour faciliter l’export, la gestion et le partage.  
3. Importez les shapefiles suivants dans `database.gpkg` :  
   - Transport : `voie_ram`, `voie_ferree`, `grand_axe`, `station_tram`  
   - Eau : `surface_eau`, `cours_eau`  
   - Bâti : `batiment_public`, `bati_indiv`, `amenagt_es_vert`  
   - Administratif : `commune`, `limite_cus`  
4. Organisez-les dans des **groupes thématiques** (transport, eau, bâti, administratif).  
5. Vérifiez le **système de coordonnées (EPSG)** de chaque couche (**Clic droit > Propriétés > Source > Référence spatiale**) et assurez-vous qu’elles sont homogènes.  

**Questions Strasbourg :**  
- Quel est le système de coordonnées des shapefiles EMS ?  
- Quels shapefiles sont de type points ? lignes ? polygones ?  
- Combien de bâtiments individuels ? Combien de communes ?

---

## 2. Exploration des données vectorielles

L’objectif est de **comprendre le contenu de chaque couche** et d’appliquer des outils de visualisation, de sélection et d’export.

### 2.1 Table attributaire
- Pour chaque couche (ex. `grand_axe`, `batiment_public`, `commune`), ouvrez la table attributaire (**Clic droit sur la couche > Ouvrir la table attributaire**).  
- Observez les **champs, types de données et effectifs**.  
- Utilisez **Trier** et **Filtrer** pour explorer et répondre aux questions.

### 2.2 Symbologie
- Pour chaque couche, définissez une symbologie claire (**Clic droit > Propriétés > Symbologie > Valeurs uniques**) :  
  - Choisissez un champ (ex. `type_de_voie` pour `grand_axe`).  
  - Cliquez sur **Ajouter toutes les valeurs** et appliquez une palette adaptée.  
- Exportez la symbologie dans `Annexes`.

### 2.3 Étiquettes
- Affichez des étiquettes sur les couches concernées (**Clic droit > Propriétés > Étiquettes**) :  
  - `nom_station` pour `station_tram`  
  - `nom_commune` pour `commune`  
- Ajustez police, taille et couleur.

### 2.4 Sélection par attributs
- Menu : **Vecteur > Outils de recherche > Sélectionner par expression**  
- Exemples :  
  - Sélectionner les voies rapides (`grand_axe`)  
  - Sélectionner les bâtiments sportifs (`batiment_public`)  

**Questions :**  
- Combien de voies rapides ?  
- Combien de bâtiments sportifs ?  

### 2.5 Sélection par localisation
- Menu : **Vecteur > Outils de recherche > Sélection par localisation**  
- Exemple : sélectionner toutes les communes incluses dans `limite_cus`.  
- Vérifiez quelles entités sont sélectionnées et notez-les.

### 2.6 Importer des données CSV (stations VéloHop)
1. Téléchargez le fichier CSV des stations VéloHop et placez-le dans `Donnees`.  
2. Vérifiez les champs `la` (latitude) et `lg` (longitude) dans Excel/LibreOffice.  
3. Dans QGIS : **Clic droit sur le CSV > Importer comme couche de texte délimité**  
   - X = `lg`, Y = `la`, système EPSG:4326 (WGS84)  

**Question :** Quelle station dispose du plus grand nombre de vélos disponibles ?

### 2.7 Calculs géométriques
- Pour les couches polygonales ou linéaires, ajoutez un champ dans la table attributaire (**Calculatrice de champ**) :  
  - `surface` pour polygones (`bati_indiv`, `surface_eau`)  
  - `longueur` pour lignes (`grand_axe`, `cours_eau`)  
- Vérifiez avec **Clic droit sur le champ > Calculer la géométrie**.

**Questions :**  
- Quelle est la surface totale des gravières (`surface_eau`) ?  
- Quelle est la surface du Rhin (`cours_eau`) ?

### 2.8 Exporter des sous-ensembles
- Après une sélection, exportez la couche (**Clic droit > Exporter > Sauvegarder les entités sélectionnées sous…**) dans :  
  - `Traitements` pour couches intermédiaires  
  - `Resultats` pour résultats finaux

**Exercices :**  
- Exporter les jardins familiaux (`amenagt_es_vert`) → `Traitements`.  
- Exporter toutes les stations VéloHop → `Traitements`.  
- Exporter les stations VéloHop avec plus de 10 vélos → `Resultats`.

---

## 3. Exploration des données raster

L’objectif est de **comprendre la structure des rasters** et de manipuler leurs valeurs et symbologies.

### 3.1 Raster multi-bande (orthophoto)
- Importez `ortho_2018_CC48.tif`.  
- Examinez les métadonnées pour identifier **résolution, système de coordonnées et unité**.

**Questions :**  
- Quelle est la résolution du raster ?  
- Quel est le système de coordonnées et son unité ?  
- Valeurs des pixels aux coordonnées X=2050750, Y=7275678 ?

### 3.2 Raster mono-bande (hauteur des toits)
- Importez `hauteur_toits_CC48.tif`.  

**Questions :**  
- Quelle est la résolution ?  
- Combien de bandes contient-il ?  
- Que représentent les valeurs des pixels ?  

- Modifiez la symbologie (**Propriétés > Symbologie > Classification manuelle**) :  
  - Toits >100 m → rouge  
  - Toits 1–5 m → vert

### 3.3 Distances et coordonnées
- Identifiez la flèche de la cathédrale et la statue de la Place de la République.  
- Mesurez la distance à vol d’oiseau avec l’outil **Mesurer**.  
- Vérifiez les valeurs via les coordonnées et l’unité du système.

---

## 4. Bilan et réflexions
- Structuration d’un projet QGIS avec GeoPackage centralisé.  
- Exploration des données vectorielles : tables attributaires, symbologie, étiquetage.  
- Sélections par attributs et par localisation, export de sous-ensembles.  
- Calculs géométriques simples (surfaces, longueurs).  
- Exploration et symbolisation des rasters.  
- Utilisation d’outils de mesure et lecture de coordonnées.
