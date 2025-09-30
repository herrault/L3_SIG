# TD2 – Analyse spatiale à partir de données vectorielles // La question des vieux arbres à Strasbourg

**Date :** 30-09-2025  
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

Les données dont nous aurons besoin sont disponibles ici : https://seafile.unistra.fr/d/6407ede3dd604ed2863f/

MNT : https://seafile.unistra.fr/d/93c58499e53a47f3a1d8/

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
- Combien de bâtiments de grande taille se trouvent autour des arbres anciens ? Quel est leur surface moyenne ?

### 1.4 Cartographie finale 

- Produisez une cartographie des arbres urbains (tous genres confondus, à Strasbourg) faisant apparaitre les jeunes et les vieux (avec symbologie différente). Les vieux arbres situées à moins de 20m
des rues et ruelles sont considérés comme **à risque** et doivent être représentés comme tels. Les vieux arbres situés dans un rayon de 50m autour des bâtiments de grande taille (> 15m)
sont considérés comme **en danger**. Ceux qui cumuleraient les deux profils (a risque et en danger) doivent être labelisés comme **prioritaire** alors que les autres sont à représenter comme
**non problématique**.
- Avant de démarrer, réfléchissez aux champs, aux types, à la hiérachie des informations à représenter. 

---

## Partie 2 – Analyse et traitement avancé

📌 **Contexte ?**  

La seconde partie vise à introduire de nouvelles fonctionnalités plus avancés (regroupement, fusion, aggrégation, etc) pour tirer des partie des intéractions d'une ou plusieurs couches. 

### 2.1 Limitation à la zone d’étude (Clip)

- Outil : **Vecteur > Outils de géotraitement > Couper (Clip)**  
- Découpez `arbres.shp`, `batiments.shp` et `zone_etude.shp`.  
- Sauvegardez en :  
  - `arbres_zone_etude.shp`  
  - `batiments_zone_etude.shp`
  - `voirie_zone_etude.shp`
---

### 2.2 Regroupement d’entités similaires (Aggrégation)

L' *Aggrégation* fusionne les entités partageant un attribut commun et permet d'effectuer des statistiques sur la base de ce regroupement.

- **Vecteur > Outils de géotraitement > Aggrégation *  
- Couche : `arbres_zone_etude.shp`

**À faire :**  

- Effectuer un regroupement par genre et comptez le nombre d'individus par genre ainsi que le nombre d'espèce différentes. Exportez votre résultat ('arbres_aggreges.shp') puis inspectez votre table. 

### 2.3 Analyse combinée (Union et Intersection)

L’union conserve toutes les géométries et tous les attributs des deux couches. Cela permet par exemple d’identifier les zones de chevauchement entre les arbres et la voirie et de quantifier les interactions.  

- **Vecteur > Outils de géotraitement > Union**  
- Couches : `arbres_zone_etude.shp` et `voirie_zone_etude.shp`.  

- **À faire :**  

- la voirie étant représentée par des entités linéaires, nous allons appliquer une zone tampon pour simuler une largeur de route. Appliquez un tampon carré de 5m et exportez le résultat sous 'voirie_tampon.shp'.
- Réalisez maintenant une union entre vos arbres (1ere couche) et cette nouvelle voirie (2nde couche). Qu'observez vous spatialement et du point de vue attributaire ?
- Reproduisez le même travail en utilisant l'outil d'**intersection** ? Obtenez vous le même résultat ?

---

### 2.4 Résumés statistiques

Un autre outil intéressant s'appuie sur le résumé statistique pour calculer à partir d'un champ sans réaliser d'opérations spatiales nécessairement. 

- **Boite à outils Traitement > Outils généraux pour les vecteurs > Résumés statistiques**
- Couches : 'voirie.shp'

- **À faire :**  

- A l'aide de cet outil, calculez la moyenne, la somme et le maximum de longueur de linéaire par type de voie 

---

### 2.5 Cartographie finale
 
- Produisez une cartographie des arbres urbains (tous genres confondus, à Strasbourg) en distinguant **les jeunes** et **les anciens** (symbologie différente).  
- Les vieux arbres situés à moins de 20 m des rues principales sont considérés comme **arbres de bordure**.  
- Les vieux arbres situés dans un rayon de 50 m autour des bâtiments de grande taille (> 15 m) sont considérés comme **arbres sensibles**.  
- Les vieux arbres qui cumulent ces deux situations (bordure + sensibles) doivent être identifiés comme **arbres cumulés**.  
- Tous les autres vieux arbres doivent apparaître comme **non concernés**.  

📌 **Étape supplémentaire :**  
- Réalisez une **Intersection** entre les arbres anciens et la couche de voirie tamponnée (5 m).  
- À partir de cette intersection, identifiez les arbres directement situés sur l’emprise potentielle de la voirie.  
- Ajoutez une catégorie supplémentaire dans votre légende : **arbres en conflit direct avec la voirie**.  

**À faire :**  
- Réfléchissez aux champs attributaires nécessaires pour stocker ces informations et à la hiérarchie des catégories.  
- Représentez les différentes classes avec une symbologie adaptée (par exemple : dégradés de couleurs ou symboles distincts).
- Mettez la en page
- Exportez votre carte finale au format image (`Resultats/cartographie_finale.png`) 

---

## 3. Bilan et réflexions
- Compréhension des différentes sélections (attributs, localisation, intersection).  
- Utilisation des outils de géotraitement (Clip, Dissolve, Union).  
- Création d’indicateurs combinant écologie et urbanisme.  
- Production de cartes et export de données vectorielles réutilisables.  

---

## Astuces et recommandations
- Toujours vérifier le système de coordonnées avant d’exporter ou de traiter les données.  
- Conserver une copie des couches originales (ne jamais écraser les données sources).  
- Documenter chaque étape dans le panneau **Propriétés > Métadonnées**.  


