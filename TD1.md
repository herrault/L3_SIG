# TD1 – Explorer l'environnement et les données de Strasbourg

**Date :** 02-09-2025  
**Nom :** Herrault PA - Chardon V

## 🎯 Objectifs
- Découvrir l’interface QGIS et ses fonctionnalités principales.  
- Importer et organiser des données vectorielles et raster.  
- Réaliser des traitements simples et extraire des informations géométriques et attributaires.  
- Savoir exporter des sous-ensembles de données et manipuler la symbologie.

---

## 0. Préparation de l’espace de travail
Avant de commencer, nous allons créer un espace de travail structuré :  

1. Créez un dossier principal `TD1_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees` → fichiers bruts fournis  
   - `Traitements` → couches intermédiaires créées  
   - `Resultats` → produits finaux  
   - `Annexes` → symbologie exportée, captures d’écran, notes  

> Cette organisation sépare les données sources des résultats produits et permet un travail reproductible.

---

## 1. Importer et organiser les données vectorielles (Eurométropole)

Nous allons importer les **shapefiles de l’Eurométropole de Strasbourg** et les stocker dans un **GeoPackage central** pour faciliter l’organisation et le partage.  

### 1.1 Jeux de données
1. Décompressez `data_TD_eurometropole` dans `Donnees`.  
2. Dans QGIS, créez un **GeoPackage** nommé `database.gpkg` dans `TD1_QGIS`.  
3. Importez les couches suivantes dans `database.gpkg` :  

| Thème      | Couches |
|------------|---------|
| Transport  | `voie_tram`, `voie_ferree`, `grand_axe`, `station_tram` |
| Eau        | `surface_eau`, `cours_eau` |
| Bâti       | `batiment_public`, `bati_indiv`, `amenagt_es_vert` |
| Administratif | `commune`, `limite_cus` |

4. Organisez-les en **groupes thématiques** dans le GeoPackage.  
5. Vérifiez que toutes les couches partagent le même **système de coordonnées EPSG** (`Clic droit > Propriétés > Source > Référence spatiale`).  

### Questions
- Quel est le système de coordonnées des shapefiles EMS ?  
- Identifiez les couches de type point, ligne, polygone.  
- Combien de bâtiments individuels sont présents ? Combien de communes ?

---

## 2. Exploration des données vectorielles

L’objectif est de découvrir le contenu de chaque couche et d’appliquer les outils de **visualisation, sélection et export**.

### 2.1 Tables attributaires
- Pour chaque couche (ex. `grand_axe`, `batiment_public`, `commune`) :  
  - **Clic droit > Ouvrir la table attributaire**  
  - Observez les champs, types de données et effectifs.  
  - Trier et filtrer pour explorer les données.  

### 2.2 Symbologie
- Définir une symbologie claire pour chaque couche :  
  - **Clic droit > Propriétés > Symbologie > Valeurs uniques**  
  - Choisir un champ pertinent pour catégoriser (ex. `type_voie` pour `grand_axe`)  
  - Ajouter toutes les valeurs et appliquer une palette adaptée.  
- Exportez la symbologie dans `Annexes` pour sauvegarder vos choix.  

### 2.3 Étiquettes
- Affichez des étiquettes pour mieux comprendre la localisation :  
  - `nom_station` pour `station_tram`  
  - `nom_commune` pour `commune`  
- Ajustez police, taille et couleur.  

### 2.4 Sélection par attributs
- **Vecteur > Outils de recherche > Sélectionner par expression**  
- Exemples ciblés :  
  - Sélectionner les **voies rapides** dans `grand_axe`  
  - Sélectionner les **bâtiments sportifs** dans `batiment_public`  

### Questions
- Combien de voies rapides sont présentes ?  
- Combien de bâtiments publics à vocation sportive ?

### 2.5 Sélection par localisation
- **Vecteur > Outils de recherche > Sélection par localisation**  
- Sélectionnez toutes les communes **incluses dans `limite_cus`**.  Attention à correctement sélectionner le bon critère topologique. 
- Vérifiez les entités sélectionnées spatialement et du point de vue attributaire.

### Question
- Combien de communes font partie intégrantes de la CUS ?

---

## 3. Importer des données CSV (stations VéloHop)
1. Téléchargez le fichier CSV des stations VéloHop et placez-le dans `Donnees`.  
2. Vérifiez les champs `lat` (latitude) et `lon` (longitude).  
3. Dans QGIS :  
   - **Couche > Ajouter une couche > Ajouter une couche de texte délimité**  
   - X = `lon`, Y = `lat`, EPSG:4326 (WGS84)  
4. Deux points à observer : (1) la couche est temporaire, il faut donc la rendre permanente; (2) le système de coordonnées est différent des autres couches
5. Pour effectuer cette tâche > **enregistrez-sous** > exportez cette couche sous 'stat_velhop_euro.shp' dans **Résultats** et changez le système pour celui des autres couches
6. Placez la sensuite dans votre database

### Question
- Quelle station possède le plus de vélos disponibles ?
- Avec la méthode employée précédemment, combien de stations sont localisées sur la commune de Strasbourg ?

---

## 4. Calculs géométriques
- Pour les couches polygonales ou linéaires (surface_eau, cours_eau, voie_tram)
  - Ajouter un champ (`surface` pour polygones, `longueur` pour lignes)  
  - Calculez les surfaces en hectares et les longueurs en mètres

### Questions
- Surface totale des gravières (`surface_eau`) ?  
- Surface du Rhin (`cours_eau`) ?
- Voie de tram avec la longueur la plus grande ?

---

## 5. Exporter des sous-ensembles
- Après sélection, exportez la couche : 

### Exercices
- Exporter les **jardins familiaux** (`amenagt_es_vert`) → `Résultats`  
- Exporter toutes les **etang**  → `Résultats`  
- Exporter les stations VéloHop avec **>10 vélos** et situées en dehors de Strasbourg → `Resultats`  

---

## 6. Exploration des rasters

### 6.1 Raster multi-bande (orthophoto)
- Importez `ortho_2018_CC48.tif`  
- Vérifiez **résolution, système de coordonnées et unités**  

### Questions
- Résolution du raster ?
- Nombre de bandes ?
- Système de coordonnées et unité ?  
- Valeurs des pixels aux coordonnées X=2050750, Y=7275678 ?

### 6.2 Raster mono-bande (hauteur des toits)
- Importez `hauteur_toits_CC48.tif`  

### Questions
- Résolution spatiale ?  
- Que représentent les valeurs ?

- Appliquez la symbologie suivante :  
  - Toits >50 m → rouge  
  - Toits 1–5 m → vert  

### 6.3 Distances et coordonnées
- Identifier la **flèche de la cathédrale** et la **statue de la Place de la République**  
- Mesurer la distance à vol d’oiseau avec l'outil règle
---

## 7. Bilan
- Organisation d’un projet QGIS avec **GeoPackage centralisé**  
- Exploration des données vectorielles et raster  
- Sélections par attributs et localisation  
- Calculs géométriques simples  
- Export et manipulation de sous-ensembles  
- Utilisation d’outils de mesure et lecture de coordonnées  

