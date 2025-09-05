# TD3 – Analyse spatiale à partir de données vectorielles et rasters (Rolling Stones)

**Date :** 05-09-2025  
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

*Contexte : Vous avez obtenu un marché avec EDF pour suivre les effets géomorphologiques d'une opération de restauration menées en rivière (injection sédimentaire). L'objectif est de quantifier les évolutions topo-bathymétriques du chenal actifs et de calculer les distances de transport de galets équipés de traceurs (puces RFID).*  

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD3_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches rasters et vectorielles fournies :  
   - `Galets_2022.gpkg` (points : Id, classe granulométrique, longitude, latitude)
   - `Galets_2023.gpkg` (points : Id, longitude, latitude)
   - `Cordon_sedimentaire.gpkg` (polygone)  
   - `Chenal_actif.gpkg` (polygones : usage, hauteur)  
   - `MNT_2022.tif` (raster, altitude du fond du chenal et topographie emmergée en 2022)  
   - `MNT_2023.tif` (raster, altitude du fond du chenal et tppographie emmergée en 2023)
---

### 1. Exploration des données
- Ajoutez les quatre couches à QGIS.  
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `Galets_2022` → taille par classe  
  - `Galets_2023` → taille par classe, transparence 50%.  
  - `Cordon_sedimentaire.gpkg` → couleur jaune, transparence 80%.  

### 2. Sélection par attributs et jointure attributaire

**Questions :**  
- Combien de galets ont été retrouvés par classe granulométrique lors de la deuxième campagne ?
- Quelle est la classe granulométrique qui a été la moins retrouvée (en %) lors de la deuxième campagne ?
  
---
**Exercices :**  
- Exportez les traceurs détectés lors de la deuxième campagne et y incluant les coordonnées d'injection dans `Traitements` sous `Traceurs_P1.gpkg`.  
---
*Contexte : vous préparez un rapport présentant les résultats significatifs de l'étude.*  
---

### 3. Mesure des distances de transport des galets
- Créer une ligne centrale (centerline) à partir de la couche `Chenal_actif.gpkg`. Exporter le résultat dans `Traitements` sous `Centerline.gpkg`.  
- Réaliser une projection orthogonale sur la ligne centrale des traceurs inclus dans la couche `Traceurs_P1.gpkg` - exporter le résultat dans `Traitements` sous `Traceurs_P1_orthogonale.gpkg`.  
- Calculer la distance euclidienne de chaque galet inclus dans la couche `Traceurs_P1.gpkg` - exporter dans `Traitements` sous `Traceurs_P1_euclidienne.gpkg`.
- Quelle est la distance minimale, moyenne, médiane et maximale des galets calculées entre les deux levés pour les deux méthodes de calcul ? Que remarquez-vous ?
- Réaliser la même opération en incluant uniquement les galets qui se sont déplacés au minimum de 5 m. Que constatez-vous ?

**Questions :**  
- Quelles est la classe granulométrique qui s'est déplacée en moyenne le plus loin ? 
- Pensez-vous que ces distances sont robustes statistiquement ?
- Si ce n'est pas le cas, quelle solution proposeriez-vous ?

---

### 4. Étude des évolutions topo-bathymétriques du chenal actif 
- Découpez `MNT_2022.gpkg` et `MNT_2023.gpkg` avec `chenal_actif.gpkg`.  
- Sauvegardez les deux couches dans `Traitements` sous`MNT_2022_clip.gpkg`; `MNT_2023_clip.gpkg` 
- Réaliser une soustraction des deux MNTs en utilisant la calculatrice raster pour identifier les évolutions topo-bathymétrique au cours du temps.
  - Sauvegardez le résultat dans `Traitements` sous `MNT_2022_2023.gpkg`
- Utiliser la bonne symbologie pour représenter les évolutions topo-bathymétriques par classe de 0,5 m. (couleur chaudes évolutions > 0 m; couleurs froides évolutions < 0 m)
 
- Supposons une incertitude de mesure en z (altitude) de 0,1 m pour chaque levé. Intégrer cette incertitude par la réalisation d'un nouveau raster à partir du raster `MNT_2022_2023.gpkg`.
- Enregistrez votre nouvelle couche raster dans `Traitements` sous `MNT_2022_2023_corr.gpkg`
- Utiliser la bonne symbologie pour représenter les évolutions topo-bathymétriques par classe de 0,5 m (couleur chaudes évolutions > 0 m; couleurs froides évolutions < 0 m)
- Que constatez-vous en comparant `MNT_2022_2023.gpkg` et `MNT_2022_2023_corr.gpkg`?
- Que constatez-vous en comparant la localisation des traceurs détectés en 2023 et les évolutions topo-bathymétriques observées entre 2022 et 2023 sur `MNT_2022_2023_corr.gpkg` ?
     
---

### 5. Cartographie finale
Réalisez une carte thématique incluant :  
- La position initiale et de 2023 des traceurs détectés
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
