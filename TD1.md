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
   - `Resultats` → produits finaux  
   - `Annexes` → symbologie exportée, captures d’écran, notes  

> Cette organisation sépare les données sources des résultats produits et permet un travail reproductible.

> Les données dont nous aurons besoin sont disponibles ici : https://seafile.unistra.fr/f/0470fa22594d4baabca7/

---

## 1. Importer et organiser les données vectorielles (Eurométropole)

Nous allons importer les **shapefiles de l’Eurométropole de Strasbourg** et les stocker dans un **GeoPackage central** pour faciliter l’organisation et le partage.  

### 1.1 Jeux de données
1. Décompressez `data_TD_eurometropole` dans `Donnees`.  
2. Dans QGIS, créez un **GeoPackage** nommé `database_strasbourg.gpkg` dans `TD1_QGIS`.  
3. Importez les couches suivantes dans `database_strasbourg.gpkg` :  

| Thème      | Couches |
|------------|---------|
| Transport  | `voie_tram`, `voie_ferree`, `grand_axe`, `station_tram` |
| Eau        | `surface_eau`, `cours_eau` |
| Bâti       | `batiment_public`, `bati_indiv`, `amenagt_es_vert` |
| Administratif | `commune`, `limite_cus` |

4. Vérifiez que toutes les couches partagent le même **système de coordonnées EPSG** (`Clic droit > Propriétés > Source > Systeme de coordonnées`).
5. Importez ces données dans vos couches à visualiser et organisez les en groupes thématiques (Transport, etc) dans votre bloc de couches

### Questions
- Quel est le système de coordonnées des shapefiles EMS ?  Quelle est sa particularité ?
- Identifiez les couches de type point, ligne, polygone.  
- Combien de bâtiments individuels sont présents ? Combien de communes ? Combien existe-t'il de voies de tram ?

---

## 2. Exploration des données vectorielles

L’objectif est de découvrir le contenu de chaque couche et d’appliquer les outils de **visualisation, sélection et export**.

### 2.1 Tables attributaires
- Pour chaque couche (`grand_axe`, `batiment_public`, `commune`) :  
  - **Clic droit > Ouvrir la table attributaire**  
  - Observez les champs, types de données et effectifs.  
  - Trier et filtrer pour explorer les données avec l'outil de filtrage (icone entonnoir disponible depuis la table attributaire)
 
### Question
- Combien existe-t-il de voies rapides ?
- Combien de bâtiments appartenant à LA POSTE, sont de type industriels et technologiques ?
- Combien de communes ne figurent pas dans la CUS ?

### 2.2 Symbologie
- Définir une symbologie claire pour les couches ('voie_tram',  `amenagt_es_vert`,  `grand_axe`)
  - **Clic droit > Propriétés > Symbologie 
  - Choisir un champ pertinent pour catégoriser le type de voie de tram, le type d'aménagement vert et le type de grand axe)
  - Ajouter toutes les valeurs et appliquer une palette adaptée.  
- Pour chaque symbologie définie, exportez la à partir du menu **style** et exportez la dans **Annexes**

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
  - Sélectionner les **bâtiments historiques et militaire** dans `batiment_public`

### Questions
- Combien de voies rapides sont présentes ?  
- Combien de bâtiments publics à vocation sportive ?
- Combien de bâtiments historique et militaire ?

### 2.5 Sélection par localisation
- **Vecteur > Outils de recherche > Sélection par localisation**  
- Sélectionnez toutes les communes **incluses dans `limite_cus`**.  Attention à correctement sélectionner le bon critère topologique. 
- Vérifiez les entités sélectionnées spatialement et du point de vue attributaire.

### Question
- Combien de communes font partie intégrantes de la CUS ?
- Dans la même logique, sélectionnez les gravières localisées hors commune de Strasbourg ?

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
- Représentez en cercles proportionnels le nombre de vélos disponible par station pour la commune de strasbourg uniquement 

---

## 4. Calculs géométriques
- Pour les couches polygonales ou linéaires (surface_eau, cours_eau, voie_tram)
  - Ajouter un champ (`surface` pour polygones, `longueur` pour lignes)  
  - Calculez les surfaces en hectares et les longueurs en mètres à l'aide du calculateur de champ (

### Questions
- Surface totale des gravières (`surface_eau`) ?  
- Surface du Rhin (`cours_eau`) ?
- Quelle est l'identifiant de la Voie de tram avec la longueur la plus grande ?

---

## 5. Exporter des sous-ensembles
- Après sélection, exportez la couche : 

### Exercices
- Exporter les **jardins familiaux** (`amenagt_es_vert`) → `Résultats`  
- Exporter toutes les **etangs**  → `Résultats`  
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
  - Autre → jaune

### 6.3 Distances et coordonnées
- Identifier la **flèche de la cathédrale** et la **statue de la Place de la République**  
- Mesurer la distance à vol d’oiseau avec l'outil Règle (en mètres)
- Si vous effectuez maintenant un triangle reliant ces deux points avec le sommet du palais universitaire, quelle est la distance parcourue en km ?
---

## 7. Bilan
- Organisation d’un projet QGIS avec **GeoPackage centralisé**  
- Exploration des données vectorielles et raster  
- Sélections par attributs et localisation  
- Calculs géométriques simples  
- Export et manipulation de sous-ensembles  
- Utilisation d’outils de mesure et lecture de coordonnées  

