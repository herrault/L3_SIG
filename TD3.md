# TD3 – Analyse spatiale à partir de données vecteurs et rasters

**Date :** 04-09-2025  
**Nom :** Herrault PA et Chardon V  

---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.
- Réaliser des projections et calculer de distance
- Réaliser des opérations arithmétiques sur des données raster.
- Manipuler, traiter et analyser des données vectorielles et raster en rivière.
- Développer un regard critique sur la précision des données utilisées  
- Produire des résultats réutilisables pour des projets en géomorphologie fluviale (mobilité de la charge de fond et évolutions topo-bathymétriques.  

*Contexte : Vous avez obtenu un marché avec EDF dans le cadre d'une opération de restauration menées en rivière (injection sédimentaire). L'objectif est de quantifier les évolutions topo-bathymétriques du chenal actifs et de calculer les distances de transport de galets équipés de traceurs (puces RFID).*  

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

📌 **Pourquoi ?**  
La phase exploratoire est essentielle : avant toute analyse, il faut comprendre ce que contiennent les données. La symbologie thématique permet d’identifier visuellement des tendances (par ex. relation distance - taille des galets).  

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

### 3. Calcul des distances de transport et extraction de métriques
- Créer une ligne centrale (centerline) à partir de la couche `Chenal_actif.gpkg`
- Réaliser une projection orthogonale sur la ligne centrale des traceurs inclus dans la couche `Traceurs_P1.gpkg` - exporter le résultat dans `Traitements` sous `Traceurs_P1_orthogonale.gpkg`.  
- Calculer la distance euclidienne de chaque galet inclus dans la couche `Traceurs_P1.gpkg` - exporter dans `Traitements` sous `Traceurs_P1_euclidienne.gpkg`.
- Quelle est la distance minimale, moyenne, médiane et maximale des galets retrouvés entre les deux levés pour les deux méthodes de calcul des distances?
- Réaliser la même opération en incluant uniquement les galets qui se sont déplacés au minimum de 5m. Que constatez-vous ?

**Questions :**  
- Quelles est la classe granulométrique qui s'est déplacée en moyenne le plus loin ? 
- Pensez-vous que ces distances sont robustes statistiquement ?
- Si pas le cas, quelle solution proposeriez-vous ?

---

### 4. Mesure et étude des évolutions topo-bathymétriques du chenal actif 
- Découpez `MNT_2022.gpkg` et `MNT_2023.gpkg` avec `chenal_actif.gpkg`.  
- Sauvegardez les deux couches dans `Traitements` sous`MNT_2022_clip.gpkg`; `MNT_2023_clip.gpkg` 
- Réaliser une soustraction des deux MNT en utilisant la calculatrice raster.
  - Sauvegardez le résultat dans `Traitements` sous `MNT_2022_2023_clip.gpkg`
 
**Questions :**   
- Supposons une incertitude de mesure de 0,1 m pour chaque levé. Intégrer cette incertitude par la réalisation d'un nouveau raster à partir du raster `MNT_2022_2023_clip.gpkg`.
- Enregistrez votre nouvelle couche raster dans `Traitements` sous `MNT_2022_2023_clip_corr.gpkg`
- Quelle est la différence de superficie entre les deux rasters ?
- Que constatez-vous en comparant la localisation des traceurs et les évolutions topo-bathymétriques ?
     
---

### 5. Cartographie finale
Réalisez une carte thématique incluant :  
- La position initiale et de 2023 des traceurs détectés
- Les évolutions topo-bathymétriques survenues entre les deux survols LiDAR topo-bathymétriques
- Limites du chenal actif
- Orthophotographie de la zone d'étude  

Ajoutez une légende claire, un titre, une échelle et une flèche du Nord.  

📌 **Pourquoi ?**  
La carte finale est la synthèse du travail : elle permet de communiquer efficacement les résultats à un commanditaire non spécialiste (ici, EDF).  

---

## 3. Bilan et réflexions
- Compréhension des différentes sélections (attributs, localisation, intersection).  
- Calcul de distances.  
- Calculs mathémaatiques sur des données raster.  
- Production de cartes et export de données vectorielles réutilisables.  

*Vous avez suivi le workflow complet d’un projet SIG en géomorphologie fluviale, combinant évolutions topo-bathymétriques et transport de la charge de fond.*  

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Conserver une copie des couches originales (ne jamais écraser les données sources).  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.
- Tenir compte des incertitudes des mesures dans les analyses
