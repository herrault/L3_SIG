# TD3bis – Analyse spatiale à partir de données vectorielles et rasters 

**Date :** 20-10-2026 
**Nom :** Herrault PA et Chardon V  
Accès données : https://seafile.unistra.fr/d/a5a7ed788ad246428146/
---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- 
 

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD3_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches rasters et vectorielles fournies :  
   - `Communes_emprise` (Id, X, Y)
   - `OCSGE_2019_68` (Occupation du sol en 2019 du Haut-Rhin) 
   - `MNT_20M_68` (raster, MNT du Haut-Rhin, résolution 20 m (MNT généré à partir du RGE 5 m))  
   - `` (raster, altitude du fond du chenal et tppographie emmergée en 2017)
---

### 1. Exploration des données
- Importer les deux fichiers .csv.
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `GALET_2016`→ couleur noire
  - `GALET_2017`→ couleur bleue
  - `Chenal_actif.gpkg` → contour bleu, transparence 80%.  
