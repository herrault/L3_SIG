# TD3 – Analyse spatiale à partir de données vecteurs et rasters

**Date :** 04-09-2025  
**Nom :** Herrault PA et Chardon V  

---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.  
- Explorer et analyser des données vectorielles et raster en rivière.
- Projection orthogonale et calculs de distance  
- Réaliser des opérations sur raster pour quantifier les interactions entre changement topo-bathymétriques et déplacement de la charge de fond.  
- Produire des résultats réutilisables pour des projets en géomorphologie fluviale.  

*Contexte : Vous avez obtenu un marché avec EDF dans le cadre d'une opération de restauration menées en rivière (injection sédimentaire). L'objectif est d'identifier et de quantifier les évolutions morphologiques après l'injection et de mesurer les distances de transport de galets équipés de traceurs (puces RFID).*  

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
   - `chenal en eau.gpkg` (polygones : usage, hauteur)  
   - `MNT_2022.tif` (raster, altitude du fond du chenal et topographie emmergée en 2022)  
   - `MNT_2023.tif` (raster, altitude du fond du chenal et tppographie emmergée en 2023)
---

## Séance 1 – Exploration et sélection

### 1.1 Exploration des données
- Ajoutez les quatre couches à QGIS.  
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `Galets_2022` → taille par classe  
  - `Galets_2023` → taille par classe, transparence 50%.  
  - `Cordon_sedimentaire.gpkg` → couleur jaune, transparence 80%.  

📌 **Pourquoi ?**  
La phase exploratoire est essentielle : avant toute analyse, il faut comprendre ce que contiennent les données. La symbologie thématique permet d’identifier visuellement des tendances (par ex. relation distance - taille des galets).  

### 1.2 Sélection par attributs et jointure attributaire

**Questions :**  
- Combien de galets ont été retrouvés par classe granulométrique lors de la deuxième campagne ?
- Quelle est la classe granulométrique qui a été la moins retrouvée (en %) lors de la deuxième campagne ? 
---
**Exercices :**  
- Exportez les traceurs détectés lors de la deuxième campagne et y incluant les coordonnées d'injection dans `Traitements` sous `Traceurs_P1.gpkg`.  

*Contexte : vous préparez un rapport présentant les résultats de l'études.*  
---

### 1.3 Calcul des distances de transport (je pense ici que je détaille un peu trop les étapes non ?)
- Créer une ligne centrale (centerline) à partir de la couche `chenal en eau.gpkg`
- Ajouter les coordonnées initiales 
- Réaliser une projection orthogonale des traceurs détectés lors des deux campagnes
 
- Combien de galets se sont déplacés de moins de 5 m entre les deux campagnes ?  
- Quelle est la distance minimale, moyenne, médiane et maximale des galets retrouvés lors de la période de suivi en supprimant les galets qui ont parcouru moins de 5 m ?  

- Sélectionnez les arbres situés à moins de 20 m des voies principales.  
- Sélectionnez les bâtiments situés dans un rayon de 50 m autour des arbres anciens.  

📌 **Pourquoi ?**  
La sélection spatiale permet de croiser des couches en fonction de leur position relative dans l’espace. C’est un outil puissant pour analyser des interactions concrètes, comme la proximité arbres/bâtiments ou arbres/voirie.  

**Questions :**  
- Combien d’arbres sont proches du réseau viaire ?  
- Combien de bâtiments se trouvent autour des arbres anciens ?  

---

## Séance 2 – Analyse et traitement avancé (Raster)

### 2.1 Limitation à la zone d’étude (Clip)
- Outil : **Vecteur > Outils de géotraitement > Découper (Clip)**  
- Découpez `arbres.shp` et `batiments.shp` avec `zone_etude.shp`.  
- Sauvegardez en :  
  - `arbres_zone_etude.shp`  
  - `batiments_zone_etude.shp`  

📌 **Pourquoi ?**  
Limiter l’analyse à la zone d’étude évite d’intégrer des données hors sujet. Cela réduit aussi la charge de calcul et garantit la cohérence des résultats.  

---

### 2.2 Regroupement d’entités similaires (Dissolve)
- Outil : **Vecteur > Outils de géotraitement > Dissolve**  
- Couche : `arbres_zone_etude.shp`  
- Attribut pour regrouper : `genre` ou `espèce`.  
- Sortie : `arbres_dissolve.shp`.  

📌 **Pourquoi ?**  
Le *Dissolve* fusionne les entités partageant un attribut commun. Ici, regrouper par espèce permet de simplifier la couche et de produire des statistiques globales (nombre ou surface par espèce).  

---

### 2.3 Analyse combinée (Union)
- Outil : **Vecteur > Outils de géotraitement > Union**  
- Couches : `arbres_dissolve.shp` et `batiments_zone_etude.shp`.  
- Sortie : `arbres_batiments_union.shp`.  

📌 **Pourquoi ?**  
L’union conserve toutes les géométries et tous les attributs des deux couches. Cela permet d’identifier les zones de chevauchement entre arbres et bâtiments et de quantifier les interactions.  

---

### 2.4 Création d’indicateurs écologiques-urbains
- Ajoutez un champ `vulnerable` dans `arbres.shp` :  
  - Arbres anciens proches de voies ou bâtiments = `oui`  
  - Sinon = `non`.  
- Ajoutez un champ `densite_arbres` dans `batiments.shp` :  
  - Nombre d’arbres dans un rayon de 50 m autour du bâtiment.  

📌 **Pourquoi ?**  
Créer de nouveaux attributs est une manière de transformer une observation spatiale en indicateur quantitatif. Ici, on passe d’une simple proximité à un diagnostic (arbres vulnérables, bâtiments bénéficiant d’une forte densité d’arbres).  

---

### 2.5 Cartographie finale
Réalisez une carte thématique incluant :  
- Arbres anciens et jeunes.  
- Bâtiments selon densité d’arbres à proximité.  
- Réseau viaire.  
- Limites de la zone d’étude.  

Ajoutez une légende claire, un titre, une échelle et une flèche du Nord.  

📌 **Pourquoi ?**  
La carte finale est la synthèse du travail : elle permet de communiquer efficacement les résultats à un commanditaire non spécialiste (ici, la Ville de Strasbourg).  

---

## 3. Bilan et réflexions
- Compréhension des différentes sélections (attributs, localisation, intersection).  
- Utilisation des outils de géotraitement (Clip, Dissolve, Union).  
- Création d’indicateurs combinant écologie et urbanisme.  
- Production de cartes et export de données vectorielles réutilisables.  

*Vous avez suivi le workflow complet d’un projet SIG appliqué à la ville, combinant écologie et aménagement.*  

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Conserver une copie des couches originales (ne jamais écraser les données sources).  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.  
