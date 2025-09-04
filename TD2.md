# TD2 – Analyse spatiale à partir de données vectorielles // La question des vieux arbres à Strasbourg

**Date :** 02-09-2025  
**Nom :** Herrault PA et Chardon V  

---

## 🎯 Objectifs
- Maîtriser les outils de sélection spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.  
- Explorer et analyser des données vectorielles à Strasbourg.  
- Quantifier les interactions entre écologie urbaine (arbres) et aménagement (bâtiments, voirie).  
- Produire des résultats réutilisables pour des projets urbains et écologiques.  

📌 **Contexte ?**  

Les vieux arbres en milieu urbain remplissent des fonctions écologiques essentielles : régulation thermique, filtration de l’air, rétention des eaux pluviales, support de biodiversité. Leur morphologie (couronne étendue, racines superficielles) et leur longévité les rendent sensibles aux pressions physiques et environnementales liées à l’urbanisation. La proximité des infrastructures, notamment la voirie et les bâtiments, influence directement leur état de santé et leur pérennité. Ces éléments modifient les conditions pédologiques, hydriques et lumineuses, et peuvent entraîner un stress mécanique ou physiologique. L’analyse de la répartition des vieux arbres en ville nécessite donc une prise en compte systématique du contexte bâti et des aménagements urbains.*  

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

## Partie 1 – Exploration et sélection

### 1.1 Exploration des données

- De la même manière que dans le TD1, créez un geopackage nommée database_arbre et placez vos couches à l'intérieur
- Examinez les tables attributaires : notez les types de données, champs disponibles, nombre d’entités.  
- Modifiez la symbologie pour améliorer la lecture cartographique :  
  - `arbres` → couleur par âge (jeune/ancien), forme par genre.  
  - `batiments` → couleur par usage, transparence 50%.  
  - `voirie` → couleur par type de voie.  
  - `zone_etude` → contour gris foncé, transparence 30%.  

**À faire :**  
- Exportez une vue (Projet > Exporter/Importer) pour chaque visualisation et collez-la dans un document Word (`Annexes`).  

**Questions :**  

- Combien d’arbres sont anciens ?  
- Combien de bâtiments publics et privés ?  
- Quelle est la longueur totale du réseau viaire ?  

---

### 1.2 Sélection par attributs

📌 **Contexte ?**  

Les arbres anciens comme les Quercus sont à la fois des refuges de biodiversité et appartiennent au patrimoine paysager. Leur proximité avec des infrastructures routières (pollution, risques mécaniques) ou avec des bâtiments (chute, conflits racinaires) peut cependant poser des problèmes de gestion et de sécurité. Les commandes qui suivent permettent donc d’identifier les situations de cohabitation sensible entre patrimoine naturel et infrastructures humaines.

- Sélectionnez tous **les vieux arbres du genre *Quercus* (chênes)** et exportez les `Résultats` sous `quercus_vieux.shp`.  
- Sélectionnez tous **les bâtiments publics et privés de plus de 20 m de hauteur**. Après avoir ouvert le menu **Edition**, dans un champ nommé **type_taille** (type = entier), codez en 1 ceux ayant une taille > 20m et les autres en 2. Sauvegardez vos résultats et
bouclez votre menu d'Edition. 

---

### 1.3 Sélection par localisation

- Sélectionnez les vieux quercus situés **à moins de 20 m des rues** et exportez les ('vieux_quercus_risques.shp')
- Sélectionnez les grands bâtiments situés dans un rayon de 50 m autour des arbres anciens et exportez les ('grands_bat_quercus_risques.shp')

**Questions :**  

- Parmi les vieux arbres, combien de chênes (quercus) en sont pas en situation de conflit avec la voirie  ?  
- Combien de bâtiments de grande taille se trouvent autour des arbres anciens ?

**Exercice :**   

- Produisez une cartographie des arbres urbains (tous genres confondus, à Strasbourg) faisant apparaitre les jeunes et les vieux (avec symbologie différente). Les vieux arbres situées à moins de 20m
des rues et ruelles sont considérés comme **à risque** et doivent être représentés comme tels. Les vieux arbres situés dans un rayon de 50m autour des bâtiments de grande taille (> 15m)
sont considérés comme **en danger**. Ceux qui cumuleraient les deux profils (a risque et en danger) doivent être labelisés comme **prioritaire** alors que les autres sont à représenter comme
**non problématique**.
- Avant de démarrer, réfléchissez aux champs, aux types, à la hiérachie des informations à représenter. 

---

## Partie 2 – Analyse et traitement avancé

📌 **Contexte ?**  

La seconde partie vise à introduire de nouvelles fonctionnalités plus avancés (regroupement, fusion, agrrégation, etc) pour tirer des partie des intéractions entre plusieyrs couches. 

### 2.1 Limitation à la zone d’étude (Clip)

- Outil : **Vecteur > Outils de géotraitement > Découper (Clip)**  
- Découpez `arbres.shp` et `batiments.shp` avec `zone_etude.shp`.  
- Sauvegardez en :  
  - `arbres_zone_etude.shp`  
  - `batiments_zone_etude.shp`  

---

### 2.2 Regroupement d’entités similaires (Dissolve)

Le *Dissolve* fusionne les entités partageant un attribut commun. Par exep, regrouper par espèce permet de simplifier la couche et de produire des statistiques globales 

- Outil : **Vecteur > Outils de géotraitement > Dissolve**  
- Couche : `arbres_zone_etude.shp`  
- Attribut pour regrouper : `genre` ou `espèce`.  
- Sortie : `arbres_dissolve.shp`.  

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


