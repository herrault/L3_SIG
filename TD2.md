# TD2 – Analyse spatiale à partir de données vectorielles

**Date :** 02-09-2025  
**Nom :** Herrault PA et Chardon V

## 🎯 Objectifs
- Maîtriser les outils de sélection spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.  
- Explorer et analyser des données vectorielles à Strasbourg.  
- Quantifier les interactions entre écologie urbaine (arbres) et aménagement (bâtiments, voirie).  
- Produire des résultats réutilisables pour des projets urbains et écologiques.

---

## 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD2_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Traitements`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches vectorielles :  
   - `arbres.shp` (points : espèce, genre, ancien ou jeune)  
   - `batiments.shp` (polygones : usage, hauteur)  
   - `voirie.shp` (lignes : type de voie, nom)  
   - `zone_etude.shp` (polygone de délimitation de l’étude)  

---

## Séance 1 – Exploration et sélection

### 1.1 Exploration des données
- Ajoutez les quatre couches à QGIS.  
- Examinez les tables attributaires : types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie :  
  - `arbres` → couleur par âge (jeune/ancien), forme par genre.  
  - `batiments` → couleur par usage, transparence 50%.  
  - `voirie` → couleur par type de voie.  
  - `zone_etude` → contour clair, transparence 30%.  

**Questions :**  
- Combien d’arbres sont anciens ?  
- Combien de bâtiments publics et privés ?  
- Quelle est la longueur totale du réseau viaire ?  

---

### 1.2 Sélection par attributs
- Sélectionnez tous les arbres du genre *Quercus* (chênes).  
- Sélectionnez tous les bâtiments de plus de 20 m de hauteur.  

**Exercices :**  
- Exportez les arbres *Quercus* dans `Traitements` en `quercus.shp`.  
- Exportez les bâtiments >20 m dans `Traitements` en `batiments_grands.shp`.  

---

### 1.3 Sélection par localisation
- Sélectionnez les arbres situés à moins de 20 m des voies principales.  
- Sélectionnez les bâtiments situés dans un rayon de 50 m autour des arbres anciens.  

**Questions :**  
- Combien d’arbres sont proches du réseau viaire ?  
- Combien de bâtiments se trouvent autour des arbres anciens ?  

---

### 1.4 Boîte à outils – Intersections
- Outil : **Vecteur > Outils de géotraitement > Intersection**  
- Intersection entre `arbres.shp` et `zone_risque.shp` (optionnel, ou créer un buffer autour des voies).  
- Résultat : arbres situés dans des zones spécifiques (proximité voirie ou zones sensibles).  

**Exercice :**  
- Créez un buffer de 10 m autour des voies principales et sélectionnez les arbres à l’intérieur.  

---

### 1.5 Synthèse de la séance 1
- Carte thématique des arbres anciens et jeunes en fonction de la proximité des voies.  
- Export des couches sélectionnées et préparation pour la séance suivante.  

---

## Séance 2 – Analyse et traitement avancé

### 2.1 Réutilisation des couches
- Réutilisez les couches exportées `quercus.shp` et `batiments_grands.shp`.  
- Vérifiez les systèmes de coordonnées et unifiez-les si nécessaire (EPSG 2154 par exemple).  

---

### 2.2 Calcul de statistiques spatiales
- Outil : **Vecteur > Analyse > Statistiques de zone**  
- Exemple : surface de bâtiments par zone, densité d’arbres par parcelle ou quartier.  

**Exercices :**  
- Calculer le nombre d’arbres par parcelle ou par quartier.  
- Calculer la surface totale des bâtiments >20 m.  

---

### 2.3 Outils de la boîte à outils
- **Buffer** : créer des zones tampon autour des arbres anciens (20 m, 50 m).  
- **Distance minimale** : calculer la distance entre chaque arbre ancien et le bâtiment le plus proche.  
- **Jointure spatiale** : relier `arbres.shp` et `batiments.shp` pour savoir quels arbres sont proches de bâtiments publics.  

**Questions :**  
- Quels arbres sont à moins de 10 m d’un bâtiment public ?  
- Quels bâtiments sont à moins de 20 m des arbres anciens ?  

---

### 2.4 Création d’indicateurs écologiques-urbains
- Créez un champ `vulnerable` dans `arbres.shp` : arbres anciens proches de voies ou bâtiments = `oui`, sinon `non`.  
- Créez un champ `densite_arbres` dans `batiments.shp` : nombre d’arbres dans un rayon de 50 m autour du bâtiment.  

**Exercice :**  
- Exporter la couche `arbres_vulnerables.shp` dans `Resultats`.  
- Exporter la couche `batiments_densite.shp` dans `Resultats`.  

---

### 2.5 Opérations vectorielles avancées : Clip, Dissolve et Union

#### a) Découpage (Clip)
**Objectif :** limiter les arbres et bâtiments à la zone d’étude.  

1. Menu : **Vecteur > Outils de géotraitement > Découper (Clip)**  
2. Couche à découper : `arbres.shp` ou `batiments.shp`  
3. Couche de découpage : `zone_etude.shp`  
4. Nom de sortie : `arbres_zone_etude.shp` et `batiments_zone_etude.shp`  

**Questions :**  
- Combien d’arbres restent dans la zone d’étude après le Clip ?  
- Combien de bâtiments sont inclus dans la zone ?  

---

#### b) Regroupement (Dissolve)
**Objectif :** fusionner des entités similaires pour simplifier l’analyse.  

1. Menu : **Vecteur > Outils de géotraitement > Dissolve**  
2. Couche : `arbres_zone_etude.shp`  
3. Attribut pour regrouper : `espèce` ou `genre`  
4. Nom de sortie : `arbres_dissolve.shp`  

**Questions :**  
- Combien d’entités avant et après Dissolve ?  
- Quels avantages pour l’analyse des espèces dans la zone d’étude ?  

---

#### c) Union
**Objectif :** combiner arbres et bâtiments pour analyser les interactions.  

1. Menu : **Vecteur > Outils de géotraitement > Union**  
2. Couches à unir : `arbres_dissolve.shp` et `batiments_zone_etude.shp`  
3. Nom de sortie : `arbres_batiments_union.shp`  

**Questions :**  
- Combien d’entités au total après l’union ?  
- Quels attributs des deux couches sont conservés dans la couche combinée ?  
- Quelles interactions écologie/urbanisme pouvez-vous identifier grâce à cette union ?  

---

### 2.6 Cartographie finale
- Créez une carte thématique montrant :  
  - Arbres anciens et jeunes.  
  - Bâtiments selon densité d’arbres à proximité.  
  - Réseau viaire.  
  - Limites de la zone d’étude.  
- Ajouter légende, titre, échelle et Nord.  

---

## 3. Bilan et réflexions
- Compréhension des différentes sélections (attributs, localisation, intersection).  
- Utilisation des outils de géotraitement (buffer, distance, jointure spatiale, Clip, Dissolve, Union).  
- Création d’indicateurs combinant écologie et urbanisme.  
- Production de cartes et export de données vectorielles réutilisables.

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Garder des copies des couches originales pour éviter de perdre des informations.  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.
