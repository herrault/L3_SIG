# TD3 – Analyse spatiale à partir de données vectorielles

**Date :** 02-09-2025  
**Nom :** Herrault PA et Chardon V  

---

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
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches vectorielles fournies :  
   - `arbres.shp` (points : espèce, genre, ancien ou jeune)  
   - `batiments.shp` (polygones : usage, hauteur)  
   - `voirie.shp` (lignes : type de voie, nom)  
   - `zone_etude.shp` (polygone de délimitation de l’étude)
---

## Séance 1 – Exploration et sélection

### 1.1 Exploration des données
- Ajoutez les quatre couches à QGIS.  
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `arbres` → couleur par âge (jeune/ancien), forme par genre.  
  - `batiments` → couleur par usage, transparence 50%.  
  - `voirie` → couleur par type de voie.  
  - `zone_etude` → contour clair, transparence 30%.  

📌 **Pourquoi ?**  
La phase exploratoire est essentielle : avant toute analyse, il faut comprendre ce que contiennent les données. La symbologie thématique permet d’identifier visuellement des tendances (par ex. concentration d’arbres anciens dans certains quartiers).  

**À rendre :**  
- Exportez une vue pour chaque visualisation et collez-la dans un document Word (`Annexes`).  

**Questions :**  
- Combien d’arbres sont anciens ?  
- Combien de bâtiments publics et privés ?  
- Quelle est la longueur totale du réseau viaire ?  

---

### 1.2 Sélection par attributs
- Sélectionnez tous les arbres du genre *Quercus* (chênes).  
- Sélectionnez tous les bâtiments de plus de 20 m de hauteur.  

📌 **Pourquoi ?**  
Les sélections attributaires permettent de filtrer une couche selon les valeurs contenues dans la table. Cela sert à isoler des cas particuliers (ici, les chênes et les grands bâtiments) pour une analyse ciblée.  

**Exercices :**  
- Exportez les arbres *Quercus* dans `Traitements` sous `quercus.shp`.  
- Exportez les bâtiments >20 m dans `Traitements` sous `batiments_grands.shp`.  

*Contexte : vous préparez un rapport sur les chênes et les grands bâtiments pour orienter un projet de verdissement urbain.*  

---

### 1.3 Sélection par localisation
- Sélectionnez les arbres situés à moins de 20 m des voies principales.  
- Sélectionnez les bâtiments situés dans un rayon de 50 m autour des arbres anciens.  

📌 **Pourquoi ?**  
La sélection spatiale permet de croiser des couches en fonction de leur position relative dans l’espace. C’est un outil puissant pour analyser des interactions concrètes, comme la proximité arbres/bâtiments ou arbres/voirie.  

**Questions :**  
- Combien d’arbres sont proches du réseau viaire ?  
- Combien de bâtiments se trouvent autour des arbres anciens ?  

---

## Séance 2 – Analyse et traitement avancé

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
