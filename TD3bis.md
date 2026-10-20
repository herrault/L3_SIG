# TD3bis – Analyse spatiale à partir de données vectorielles et rasters 

**Date :** 20-10-2026 
**Nom :** Herrault PA et Chardon V  
Accès données : https://seafile.unistra.fr/d/a5a7ed788ad246428146/
---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.
- Manipuler, traiter et analyser des données rasters
- Produire de nouvelles métriques à partir d'un MNT
- Extraire des informations de données rasters
---

### 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD3bis_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches rasters et vectorielles fournies :  
   - `Communes_emprise` (Couche vectorielle, emprise des communes françaises)
   - `OCSGE_2019_68` (Couche vectorielles, Occupation du sol en 2019 du Haut-Rhin) 
   - `MNT_20m_68` (Couche raster, MNT du Haut-Rhin, résolution 20 m (MNT généré à partir du RGE 5 m))  
---

### 1. Exploration et préparation des données
 - Importer la couche `OCSGE_2019_68`
 - Examinez la table attributaire
 - Modifiez la symbologie pour améliorer la lecture cartographique au niveau 3  
   - Dans symbologie, charger le style de couche de l'IGN à l'aide du fichier `OCSGE_GE2_symbologie_niv3`
 - À partir de la couche `Communes_emprise`, faire une extraction uniquement des communes du Haut-Rhin en enregistrer cette nouvelle couche dans `Resultats` sous le nom `Communes_emprise_68`
   - Pour faire cette tâche, aidez-vous de cette expression `"insee" LIKE '68%'`

### 2. Révision de plusieurs manipulations sur des données vectorielles à l'échelle du Haut-Rhin (lib_n3)
Pour réaliser cette partie, utilisez l'outil de géotraitement `intersection` dans un premier temps
 - Quelle est la commune qui concentre le plus d'exploitations agricoles ?
 - Quelle est la superficie moyenne en ha de bâti continu à l'échelle de la commune ?
 - Quelle est la commune qui concentre le plus de bâtis continus et d’Équipements sportifs de loisirs et campings ?
 - Combien de bâtis (tous types confondus) à l'échelle du Haut-Rhin (tous type) se trouvent à moins de 2,5 km de cours et voies d'eau ?

### 3. Exploration d'un MNT, recodage d'une donnée raster
 -  Modifier la symbologie de sorte à avoir trois classes d'altitude pour représenter les trois grandes unités de relief (typologie simplifiée) de la zone géographique étudiée
    - classe 1 : altitude < 200 m : Plaine d'Alsace
    - classe 2 : altitude 200 m - 350 m : Piémont
    - classe 3 : altitude > 350 m : Massif vosgien
 - Recoder une données raster à l'aide de l'outil reclassification par table Code  = 1 pour altitude < 200m; Code = 2 pour altitude 200m - 350m et code = 3 pour altitude > 350 m et enregistrer cette nouvelle couche dans `Resultats` sous le nom `MNT_reclassification`
 - Réalisez une vectorisation du raster `MNT_reclassification` en utilisant l'outil Polygoniser
 - À l'aide de la calculatrice de champs créez une nouvelle colonne `Unité_relief` de type texte. Pour chaque code, indiquez le nom de l'unité de relief.
 - Dans quelle unité de relief sont implantées en majorité les vergers traditionnels ? 
      
### 4. Exploration, traitements et analyses sur données Rasters & Vectorielles - Étagement de la végétation
 -  Réalisez une requête attributaire pour extraire uniquement les classes suivantes (lib_n3)  `Forêts de conifères`, `Forêts de feuillus`,  `Forêts mixtes`,  `Pelouses et pâturages de montagne` et enregistrer cette nouvelle couche dans `Resultats` sous le nom `Vegetation`
 - À l'aide de l'outil `statistique de zone`, calculez pour chaque polygone la valeur minimale et maximale de l'altitude. Enregistrez cette nouvelle couche dans `Resultats` sous le nom `Statistique_Vegetation`.
 - Calculez la valeur moyenne minimale et la valeur moyenne maximale de l'altitude par classe de végétation. 
 - Que pouvez-vous en conclure sur l'étagement moyen de la végétation ?
 - Spatialement observez vous des particularités locales / phénomènes locaux sur l'étagement de la végétation ? 
    

### 5. Exploration, traitements et analyses sur données Rasters & Vectorielles - Les Vignes
 - Le modèle numérique de terrain permet de calculer d'autres métriques utiles pour étudier l'espace géographique. Nous allons étudier les terrains sur lesquels les vignes sont implantées. 
    - Calculer la valeur de pente à l'échelle du pixel à partir de l'outil Raster -> Analyse -> Pente
      - Enregistrer cette nouvelle couche dans `Resultats` sous le nom `Raster_Pente_20m_68`
        
    - Calculer la valeur d'exposition à l'échelle du pixel à partir de l'outil Raster -> Analyse -> Exposition
      - Enregistrer cette nouvelle couche dans `Resultats` sous le nom `Raster_Exposition_20m_68`
        
- À partir des trois couches rasters `MNT_20m_68`, `Raster_Pente_20m_68` et `Raster_Exposition_20m_68`, étudiez les caractéristiques topographiques et d'exposition des terrains où le vignoble du Haut-Rhin est implanté ?
  - Pour réaliser les étapes suivantes, récupérez l'occupation du sol de type vignes à partir de la couche `OCSGE_2019_68`. 
  - À l'aide de l'outil `statistique de zone`, calculez pour chaque polygone la valeur minimale, moyenne et maximale de l'altitude, de la pente et de l'exposition. Enregistrez les trois nouvelles couches dans `Resultats`
  - Réalisez une jointure attributaire pour rassembler l'ensemble des résultats dans une seule couche vectorielle et enregistrer cette nouvelle couche dans `Resultats` sous le nom `Statistiques_vignes_68`
  - Refaire les étapes précédentes en utilisant l'outil `statistique de zone` en utilisant le traitement par lot
  - Résumez l'information à l'échelle de la commune (min_altitude/ moy_altitude / max_altitude / min_pente / moy_pente / max_pente / min_exposition/ moy_exposition/ max_exposition. Aidez-vous de l'outil `Statistiques par catégories`
  - Quelle est la commune où les vignes sont implantées à la plus haute altitude ?
  - Quelle est la commune où les vignes sont implantées sur les pentes les plus importantes ? 
 
    
     
     






