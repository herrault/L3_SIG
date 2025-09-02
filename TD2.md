# TD2 – Analyse spatiale à partir de données vectorielles

**Date :** 02-09-2025  
**Nom :** Herrault PA et Chardon V

## 🎯 Objectifs
- Maîtriser les outils de sélection spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.  
- Explorer et analyser des données vectorielles à Strasbourg.  
- Quantifier les interactions entre écologie urbaine (arbres) et aménagement (bâtiments, voirie).  
- Produire des résultats réutilisables pour des projets urbains et écologiques.

*Contexte : Vous êtes consultant pour la ville de Strasbourg. Votre mission est d’analyser la répartition des arbres en lien avec le bâti et le réseau viaire, afin d’identifier les zones prioritaires pour la gestion urbaine et la biodiversité.*

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

* Exportez la vue pour chaque visualisation et collez la dans un .doc.*

**Questions :**  
- Combien d’arbres sont anciens ?  
- Combien de bâtiments publics et privés ?  
- Quelle est la longueur totale du réseau viaire ?

* Répondez aux questions dans ce même .doc.*

---

### 1.2 Sélection par attributs
- Sélectionnez tous les arbres du genre *Quercus* (chênes).  
- Sélectionnez tous les bâtiments de plus de 20 m de hauteur.  

**Exercices :**  
- Exportez les arbres *Quercus* dans `Traitements` en `quercus.shp`.  
- Exportez les bâtiments >20 m dans `Traitements` en `batiments_grands.shp`.  

*Vous préparez un rapport sur les chênes et les grands bâtiments pour orienter un projet de verdissement urbain.*

---

### 1.3 Sélection par localisation
- Sélectionnez les arbres situés à moins de 20 m des voies principales.  
- Sélectionnez les bâtiments situés dans un rayon de 50 m autour des arbres anciens.  

**Questions :**  
- Combien d’arbres sont proches du réseau viaire ?  
- Combien de bâtiments se trouvent autour des arbres anciens ?  

*Vous analysez l’impact du réseau viaire sur la végétation et la proximité du bâti sur les arbres.*

---

## Séance 2 – Analyse et traitement avancé

### 2.1 Limitation à la zone d’étude (Clip)
- Outil : **Vecteur > Outils de géotraitement > Découper (Clip)**  
- Découpez `arbres.shp` et `batiments.shp` avec `zone_etude.shp`.  
- Noms de sortie :  
  - `arbres_zone_etude.shp`  
  - `batiments_zone_etude.shp`  

*Justification : Seules les entités situées dans la zone étudiée sont pertinentes pour l’analyse.*

**Questions :**  
- Combien d’arbres restent dans la zone d’étude ?  
- Combien de bâtiments y sont inclus ?  

---

### 2.2 Regroupement d’entités similaires (Dissolve)
- Outil : **Vecteur > Outils de géotraitement > Dissolve**  
- Couche : `arbres_zone_etude.shp`  
- Attribut pour regrouper : `genre` ou `espèce`  
- Nom de sortie : `arbres_dissolve.shp`  

*Justification : Simplifier la couche pour visualiser la répartition des espèces et faciliter les statistiques.*

**Questions :**  
- Combien d’entités avant et après Dissolve ?  
- Quels avantages pour l’analyse des espèces dans la zone d’étude ?  

---

### 2.3 Analyse combinée (Union)
- Outil : **Vecteur > Outils de géotraitement > Union**  
- Couches à unir : `arbres_dissolve.shp` et `batiments_zone_etude.shp`  
- Nom de sortie : `arbres_batiments_union.shp`  

*Justification : Identifier les zones où arbres et bâtiments interagissent. Cela aide à repérer les zones sensibles, les conflits potentiels et les opportunités de verdissement.*

**Questions :**  
- Combien d’entités au total après l’union ?  
- Quels attributs des deux couches sont conservés ?  
- Quelles interactions écologie/urbanisme pouvez-vous identifier ?  

---

### 2.4 Création d’indicateurs écologiques-urbains
- Créez un champ `vulnerable` dans `arbres.shp` : arbres anciens proches de voies ou bâtiments = `oui`, sinon `non`.  
- Créez un champ `densite_arbres` dans `batiments.shp` : nombre d’arbres dans un rayon de 50 m autour du bâtiment.  

**Exercice :**  
- Exporter la couche `arbres_vulnerables.shp` dans `Resultats`.  
- Exporter la couche `batiments_densite.shp` dans `Resultats`.  

*Ces indicateurs sont utiles pour décider des actions d’entretien et de protection des arbres en ville.*

---

### 2.5 Cartographie finale
- Carte thématique montrant :  
  - Arbres anciens et jeunes.  
  - Bâtiments selon densité d’arbres à proximité.  
  - Réseau viaire.  
  - Limites de la zone d’étude.  
- Ajouter légende, titre, échelle et Nord.  

*Vous présentez un rapport visuel comme un consultant SIG urbain.*

---

## 3. Bilan et réflexions
- Compréhension des différentes sélections (attributs, localisation, intersection).  
- Utilisation des outils de géotraitement (Clip, Dissolve, Union, buffer, distance, jointure spatiale).  
- Création d’indicateurs combinant écologie et urbanisme.  
- Production de cartes et export de données vectorielles réutilisables.  

*Vous avez suivi le workflow complet d’un projet SIG appliqué à la ville, combinant écologie et aménagement.*

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Garder des copies des couches originales pour éviter de perdre des informations.  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.

