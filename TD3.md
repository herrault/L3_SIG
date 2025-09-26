# TD3 – Analyse spatiale à partir de données vectorielles et rasters // Rolling Stones

**Date :** 25-09-2025  
**Nom :** Herrault PA et Chardon V  

---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.
- Réaliser des projections et calculer de distance
- Réaliser des opérations arithmétiques sur des données raster.
- Manipuler, traiter et analyser des données vectorielles et raster en rivière.
- Adopter la bonne symbologie pour représenter des dynamiques spatiales et temporelles en géomorphologie fluviales de données rasters
- Développer un regard critique sur la précision des données utilisées  
- Produire des résultats réutilisables pour des projets en géomorphologie fluviale (mobilité de la charge de fond et évolutions topo-bathymétriques.  

*Contexte : Vous avez obtenu un marché avec EDF pour suivre les effets géomorphologiques d'une opération de restauration menées en rivière (injection sédimentaire). L'objectif est de quantifier les évolutions topo-bathymétriques du chenal actif et de calculer les distances de transport de galets équipés de traceurs (puces RFID).*  

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD3_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches rasters et vectorielles fournies :  
   - `GALET_2016.csv` (Id, X, Y)
   - `GALET_2017.csv` (Id, X, Y, GRANULO) 
   - `MNT_2016.tif` (raster, altitude du fond du chenal et topographie emmergée en 2022)  
   - `MNT_2017.tif` (raster, altitude du fond du chenal et tppographie emmergée en 2023)
---

### 1. Exploration des données
- Importer les deux fichiers .csv.
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `GALET_2016`→ couleur noire
  - `GALET_2017`→ couleur bleue
  - `Chenal_actif.gpkg` → contour bleu, transparence 80%.  

### 2. Sélection par attributs et jointure attributaire

**Questions :**  
- Combien de galets ont été retrouvés par classe granulométrique lors du deuxième suivi ?
- Quelle est la classe granulométrique qui a été la moins retrouvée (en %) lors de la deuxième campagne ?
  
---
*Contexte : vous préparez un rapport présentant les résultats significatifs de l'étude.*  
---

### 3. Mesure des distances de transport 
*Méthode 1
- Utiliser la fonction -> Vecteur -> Outils d'analyse -> Matrice des distances.
- Regardez la table d'attributs - quelle méthode pouvons-nous utiliser pour récupérer les bonnes associations Id? 
- Exporter le résultat dans `Traitements` sous `Distance_methode1.gpkg`.  
*Méthode 2
- Digitaliser le chenal actif de l'injection sédimentaire jusqu'au galet détecté le plus en aval. Exporter le résultat dans `Traitements` sous `Chenal_actif.gpkg`.  
- Créer une ligne centrale (centerline) à partir de la couche `Chenal_actif.gpkg` en utilisant la fonction skeleton
- Supprimer les polylignes indésirables.
- Agréger les polylignes. Exporter le résultat dans `Traitements` sous `Centerline.gpkg`.
- Découper votre centerline en polylines de 5 m avec l'outil 'Division des lignes par longeur maximale'. 
- Réaliser une projection orthogonale des traceurs sur la ligne centrale du chenal actif `GALET_2016.gpkg` - exporter le résultat dans `Traitements` sous `GALET_2016_orthogonale.gpkg`.
- Réaliser une projection orthogonale des traceurs sur la ligne centrale du chenal actif `GALET_2017.gpkg` - exporter le résultat dans `Traitements` sous `GALET_2017_orthogonale.gpkg`.  
- Calculer la distance euclidienne de chaque galet inclus dans la couche `GALET_P1.gpkg` - exporter dans `Traitements` sous `GALET_P1_euclidienne.gpkg`.
- Quelle est la distance minimale, moyenne, médiane et maximale des galets calculées durant P1 pour les deux méthodes de calcul ? Que remarquez-vous ?
- Réaliser la même opération en incluant uniquement les galets qui se sont déplacés au minimum de 5 m. Que constatez-vous ?

**Questions :**  
- Quelles est la classe granulométrique qui s'est déplacée en moyenne le plus loin ? 
- Pensez-vous que ces distances sont robustes statistiquement ?
- Si ce n'est pas le cas, quelle solution proposeriez-vous ?

---

### 4. Étude des évolutions topo-bathymétriques du chenal actif 
- Découpez `MNT_2016.gpkg` et `MNT_2017.gpkg` avec `chenal_actif.gpkg`.  
- Sauvegardez les deux couches dans `Traitements` sous`MNT_2016_clip.gpkg`; `MNT_2017_clip.gpkg` 
- Réaliser une soustraction des deux MNTs en utilisant la calculatrice raster pour identifier les évolutions topo-bathymétriques au cours du temps.
  - Sauvegardez le résultat dans `Traitements` sous `MNT_2016_2017.gpkg`
- Utiliser la bonne symbologie pour représenter les évolutions topo-bathymétriques par classe de 0,5 m. (couleur chaudes évolutions > 0 m; couleurs froides évolutions < 0 m)
 
- Supposons une incertitude de mesure en z (altitude) de 0,1 m pour chaque levé. Intégrer cette incertitude par la réalisation d'un nouveau raster à partir du raster `MNT_2016_2017.gpkg`.
- Enregistrez votre nouvelle couche raster dans `Traitements` sous `MNT_2016_2017_corr.gpkg`
- Utiliser la bonne symbologie pour représenter les évolutions topo-bathymétriques par classe de 0,5 m (couleur chaudes évolutions > 0 m; couleurs froides évolutions < 0 m)
- Que constatez-vous en comparant `MNT_2016_2017.gpkg` et `MNT_2016_2017_corr.gpkg`?
- Que constatez-vous en comparant la localisation des traceurs détectés en 2023 et les évolutions topo-bathymétriques observées entre 2022 et 2023 sur `MNT_2016_2017_corr.gpkg` ?
     
---

### 5. Cartographie finale
Réalisez une carte thématique incluant :  
- La position des traceurs installés en 2016 et détectés en 2017.
- Les évolutions topo-bathymétriques survenues entre les deux survols LiDAR topo-bathymétriques
- Limites du chenal actif
- Orthophotographie de la zone d'étude  

Ajoutez une légende claire, un titre, une échelle et une flèche du Nord.  

## 6. Bilan et réflexions
- Maitrise des jointure attributaires et spatiales
- Calculs de distance et projection orthogonale
- Calculs arithmétiques sur des données raster.  
- Production de cartes et export de données vectorielles et rasters réutilisables.
- Inclure des incertitudes de mesures dans les analyses spatiales

*Vous avez suivi le workflow complet d’un projet SIG en géomorphologie fluviale, combinant évolutions topo-bathymétriques et transport de la charge de fond.*  

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Conserver une copie des couches originales (ne jamais écraser les données sources).  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.
- Tenir compte des incertitudes des mesures dans les analyses spatiales
