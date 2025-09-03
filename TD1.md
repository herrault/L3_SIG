# TD1                              						                		 06/02/2022
**Objectifs :**
- Découverte de l’interface du logiciel ;
- Charger, explorer, enregistrer des données.

## 1 Données
Les données utilisées pour le TD sont dans le dossier compressé « data_TD1 », à retrouver sur Moodle.
Vous devez le décompresser, où vous voulez.
Il s’agit de données thématiques recouvrant le territoire de trois communes du département Français des Pyrénées Orientales (routes, bâti, végétation …).

- Quel est le format des données contenues dans le dossier du TD1 ? Cherchez sur Internet à quoi correspondent les différents formats. (.shp, .shx, etc.).
  - *.shp : stocke les entités géographiques. Il s'agit du shapefile proprement-dit. Le fichier principal qui lie les autres ensemble. C’est ce fichier que vous allez utiliser dans ArcMap pour visualiser les données.
  - *.dbf (DataBaseFile) : stocke les données attributaires. Il s’agit d’un fichier de type tableur que vous pouvez consulter avec Excel ou Libre Office. 
    Note : Il est d’ailleurs parfois intéressant de consulter, copier et utiliser les données contenues dans le .dbf. Mais attention à ne pas modifier le fichier .dbf original.
  - *.shx : stocke les index (identifiants) des entités géométriques contenues dans le fichier .shp
  - *.prj : stocke le système de projection des données géométriques.
    Note : Pour avoir l’intégralité de ses fonctionnalités, une couche géographique en mode vectoriel doit avoir au moins les fichiers .shp, .dbf, .shx et .prj. Il est possible de les supprimer, mais la couche aura alors des fonctionnalités restreintes (e.g. pas possible d’accéder aux valeurs attributaires sans fichier .dbf, pas possible de calculer des distances, de mettre une échelle ou une flèche du Nord sans fichier .prj). Ces formats sont à l’origine des formats propriétaires de ESRI, la compagnie qui a fondé et développé ArcGIS. Ces formats sont devenus les formats standards de création et d’échange de l’information géographique en mode vectoriel. Tous les logiciels SIG (QGIS par exemple) sont capables de prendre en charge des fichiers .shp, .shx, .dbf etc.

## 2 Créer un espace de travail
Nous allons créer un espace de travail (un dossier) adapté avant de commencer à travailler. Vous pouvez par exemple créer un dossier nommé ‘TD1’. Tout votre travail devra être enregistré dans le nouvel espace de travail ainsi défini au cours de la séance. Copiez les données décompressées précédemment dans votre espace de travail (dossier ‘TD1’).

Note : Quand on enregistre des couches, il faut respecter certaines règles de dénomination.
- Pas d'espace dans les noms de fichiers : Parcelle en herbe => Parcelle_en_herbe
- Pas de caractères accentués dans les noms de fichiers => Propriétaire => Proprietaire
- Pas de caractères spéciaux (& : ; / \ @...) : uniquement des lettres et des chiffres
- Pas de chiffre comme premier caractère : 1990_habitat => Habitat_1990

- Une fois le dossier de travail créé, il faut le connecter à **QGIS**.

- Lancer QGIS.

- Cliquer sur ‘Navigateur’ puis **Ajouter un dossier** pour connecter votre dossier de travail à QGIS.

- Choisir votre dossier de travail, par exemple ‘TD1’.

- Dans le panneau « Navigateur », votre dossier de travail est maintenant disponible.

- QGIS est maintenant connecté à votre dossier de travail.

- Nous allons y créer un **GeoPackage**. Clic droit sur l’espace de travail (dossier ‘TD1’) → **Nouveau > GeoPackage**.

- Nommez le GeoPackage ‘database.gpkg’.

- Le GeoPackage est vide. Avant d’y importer les données téléchargées, nous allons créer des **couches séparées** pour mieux organiser les entités.

Note : Un GeoPackage doit contenir des données homogènes. Avant d’importer des fichiers de forme, vérifiez qu’ils partagent tous la même projection géographique en faisant un clic droit sur les couches dans le Navigateur puis « Propriétés > Source > Système de coordonnées ».
⇒ Notez l’identifiant EPSG du système de coordonnées

- Nous créons deux groupes de couches dans le GeoPackage : un nommé ‘lineaire’ et un nommé ‘surfacique’, dans lesquels nous importerons respectivement les entités de type linéaire et de type surfacique.

A chaque fois, vous devrez indiquer le bon système de coordonnées. Retrouvez alors le système de coordonnées des couches grâce à l’identifiant EPSG noté précédemment.

- Importer les données linéaire et surfaciques dans les groupes correspondants du GeoPackage.

Astuce : Les icônes permettent d’identifier rapidement les types d’entités. 

- Maintenant que le GeoPackage est créé, votre Navigateur doit ressembler à ça :

Renommez l’une des couches dans l’arborescence du Navigateur (pas dans le GeoPackage). Ouvrez l’explorateur Windows. Que constatez-vous ? Comment QGIS gère les données ?

L’ensemble des fichiers composant la couche géographique a pris le nouveau nom que vous avez défini. Le Navigateur gère tous les fichiers composant une couche comme un seul bloc de données cohérent. 

Renommez l’une des couches dans le GeoPackage. Ouvrez le GeoPackage dans l’explorateur Windows. Que constatez-vous ?

## 3 Explorer les données avec QGIS
Pour visualiser les données, nous devons créer une carte.
- Dans l’onglet **Projet**, cliquez sur **Nouvelle Carte**. Une vue « Carte » s’affiche. La couche « Carte » apparaît également dans le panneau **Couches**. Elle correspond au fond de carte par défaut dans QGIS.
- Ajoutez les données de votre GeoPackage à votre carte, en glissant les couches depuis le Navigateur jusque sur la vue « Carte ».
- Enregistrez votre projet. Dans l’onglet **Projet**, cliquez sur **Enregistrer sous**. Donnez un nom à votre projet, par exemple TD1.
Sous QGIS, un fichier de projet stocke la mise en forme, les options d’affichage et le chemin vers les données, mais pas les données en elles-mêmes. Celles-ci restent stockées dans le répertoire de travail. Toute modification des données d’origine au cours de la réalisation d’un projet cartographique résulte en une modification des données dans le répertoire de travail.

- Le panneau **Couches** contient la table des matières. Elle permet de visualiser les couches présentes dans le projet, de les organiser et de rendre ou non visible les couches. Il existe plusieurs modes de visualisation des données dans la table des matières. Essayez en l’un ou l’autre.

- Enregistrez votre projet dans la **barre d’outils accès rapide**.

- Fermez QGIS. Ouvrez l’explorateur Windows. Retrouvez votre fichier de projet. Double-cliquez dessus. Que se passe-t-il ?

- Modifiez l’ordre des couches dans le panneau **Couches** pour les rendre mieux visibles.

- À l’aide de l’outil **Identifier les entités**, retrouvez la commune de Torreilles dans la couche « communes ».

- Vous pouvez également interroger des entités grâce à l’outil **Identifier les entités**. Pour cela, sélectionnez une couche dans le panneau Couches, puis cliquez sur une entité. Une fenêtre s’ouvre et vous informe du contenu de la table attributaire de l’entité correspondante.

- Mesurez la distance entre le centre-ville de Torreilles et le centre-ville de Villelongue-de-la-Salanque au moyen de l’outil **Mesurer**.
⇒ Notez la distance.

- Quelle est la longueur du tronçon de route dont l’ID est TRONROUT0000000038686356 ? Quelle est la NATURE de ce tronçon ?

- En double cliquant sur une couche dans le panneau **Couches**, vous aurez accès aux propriétés des données. Retrouvez le système de projection de la donnée. Quel est-il ? Quelle est son unité ? Quelle est l’extension de la couche affichée ?

Dans l’onglet **Source**, cliquez sur **Définir la source de données**.

Une fenêtre de dialogue s’ouvre. Dans cette fenêtre, double cliquez sur une autre couche que celle que vous utilisez à l’origine (par exemple, si vous regardiez les propriétés de la couche « communes », sélectionnez la couche « batiments »). Que se passe-t-il ?  

La couche « batiments » est affichée à la place de la couche « communes ». Il est ainsi possible de changer le fichier de formes qui va être affiché pour la visualisation. Cette option est pratique dans le cas de grands projets : il est souvent nécessaire de bouger, reclasser des données. Si QGIS perd le chemin du fichier de formes à afficher, il pourra redéfinir le chemin de stockage des données.

- Annulez ensuite cette manipulation.

- Les couches chargées dans QGIS ont un aspect par défaut. Afin de rendre la visualisation du territoire plus réaliste et plus conforme aux conventions, il faut changer leur symbologie.
Cliquez sur l’onglet **Apparence**, cliquez sur le bouton **Symbologie**, et choisissez des représentations plus adaptées pour chaque couche.

- Terminez par enregistrer votre projet.
Vous pouvez par exemple obtenir quelque chose comme ça (style carte topographique) :


