# TD3bis – Analyse spatiale à partir de données vectorielles et rasters 

**Date :** 20-10-2026 
**Nom :** Herrault PA et Chardon V  
Accès données : https://seafile.unistra.fr/d/a5a7ed788ad246428146/
---

## 🎯 Objectifs
- Maîtriser les outils de sélection / jointure spatiale et attributaire.  
- Manipuler les outils de la boîte à outils de traitement QGIS.
- Manipuler, traiter et analyser des données rasters
- Produire de nouvelles métriques à partir d'un MNT
- Extraire des informations de données rasters
---

### 0. Préparation de l’espace de travail
1. Créez un dossier principal `TD3bis_QGIS`.  
2. Créez les sous-dossiers :  
   - `Donnees`  
   - `Resultats`  
   - `Annexes`  
3. Placez dans `Donnees` les couches rasters et vectorielles fournies :  
   - `Communes_emprise` (Couche vectorielle, emprise des communes françaises)
   - `OCSGE_2019_68` (Couche vectorielles, Occupation du sol en 2019 du Haut-Rhin) 
   - `MNT_20m_68` (Couche raster, MNT du Haut-Rhin, résolution 20 m (MNT généré à partir du RGE 5 m))  
---

### 1. Exploration et préparation des données
 - Importer la couche `OCSGE_2019_68`
 - Examinez la table attributaire
 - Modifiez la symbologie pour améliorer la lecture cartographique au niveau 3  
   - Dans symbologie, charger le style de couche de l'IGN à l'aide du fichier `OCSGE_GE2_symbologie_niv3`
 - À partir de la couche `Communes_emprise`, faire une extraction uniquement des communes du Haut-Rhin en enregistrer cette nouvelle couche dans `Resultats` sous le nom `Communes_emprise_68`
   - Pour faire cette tâche, aidez-vous de cette expression `"insee" LIKE '68%'`

### 2. Révision de plusieurs manipulations sur des données vectorielles (lib_n3)
Pour réaliser cette partie, utilisez l'outil de géotraitement `intersection` dans un premier temps
 - Quelle est la commune qui concentre le plus d'exploitations agricoles ?
 - Quelle est la superficie moyenne en ha de bâti continu à l'échelle de la commune ?
 - Quelle est la commune qui concentre le plus de bâtis continus et d’Équipements sportifs de loisirs et campings ?
 - Combien de bâtis (tous types confondus) à l'échelle du Haut-Rhin (tous type) se trouvent à moins de 2,5 km de cours et voies d'eau ?

### 3. Exploration d'un MNT, recodage d'une donnée raster et vectorisation
 -  Modifier la symbologie de sorte à avoir trois classes d'altitude pour représenter les trois grandes unités de relief de la zone géographique étudiée
    - classe 1 : altitude < 200 m : Plaine d'Alsace
    - classe 2 : altitude 200 m - 350 m : Piémont
    - classe 3 : altitude > 350 m : Massif vosgien
 - Recoder une donnée raster à l'aide de l'outil reclassification par table Code  = 1 pour altitude < 200m; Code = 2 pour altitude 200m - 350m et code = 3 pour altitude > 350 m et enregistrer cette nouvelle couche dans `Resultats` sous le nom `MNT_reclassification`
 - Réalisez une vectorisation du raster `MNT_reclassification` en utilisant l'outil Polygoniser
 - À l'aide de la calculatrice de champs créez une nouvelle colonne `Unité_relief` de type texte. Pour chaque code, indiquez le nom de l'unité de relief.
 - Dans quelle unité de relief sont implantées en majorité les vergers ? 
      
### 4. Exploration, traitements et analyses sur données Rasters & Vectorielles - Étagement de la végétation
 -  Réalisez une requête attributaire pour extraire uniquement les classes suivantes (lib_n3)  `Forêts de conifères`, `Forêts de feuillus`,  `Forêts mixtes`,  `Pelouses et pâturages de montagne` et enregistrer cette nouvelle couche dans `Resultats` sous le nom `Vegetation`
 - À l'aide de l'outil `statistique de zone`, calculez pour chaque polygone la valeur minimale et maximale de l'altitude. Enregistrez cette nouvelle couche dans `Resultats` sous le nom `Statistique_Vegetation`.
 - Calculez la valeur moyenne minimale et la valeur moyenne maximale de l'altitude par classe de végétation. 
 - Que pouvez-vous en conclure sur l'étagement de la végétation ? 
    

### 5. Modéliser les déplacements des espèces en fonction des contraintes topographiques et d'occupation du sol

Maintenant que vous connaissez la structure de ces couches, nous allons
voir comment les utiliser pour étudier les déplacements des espèces à
travers le paysage.

Les démarches visant à préserver les espèces s'appuient notamment sur
des données spatiales afin de comprendre comment les différents éléments
du paysage peuvent contraindre leurs déplacements. Il s'agit ainsi
d'évaluer si ces éléments ont des effets positifs ou négatifs sur leur
capacité à trouver des ressources et des espaces de quiétude. Ces deux
dimensions seront représentées par deux scénarios de friction :
`friction_ressource.tif` et `friction_energie.tif`.

Le principe de connectivité du paysage est donc central, mais il dépend
également des caractéristiques propres aux organismes biologiques,
notamment de leur capacité de dispersion. Il est important de tenir
compte de ces deux aspects. Un ongulé sera, par exemple, davantage
affecté par les paysages présentant de fortes pentes que par
l'occupation du sol elle-même. À l'inverse, de petits mammifères
terrestres, qui se déplacent sur de plus courtes distances, peuvent être
très dépendants du couvert végétal, mais potentiellement moins affectés
par la topographie.

Nous allons mettre en œuvre ce principe sur un site d'étude situé dans
le Haut-Rhin.

#### 5.1. Préparer les données : découper le MNT et l'occupation du sol

La première étape consiste à découper le modèle numérique de terrain
(MNT) et la couche d'occupation du sol sur le territoire de la zone
d'étude.

À l'aide du fichier `emprise`, utilisez : - l'outil **Couper** pour la
couche vectorielle d'occupation du sol ; - l'outil **Découper un raster
selon une couche masque** pour le MNT.

Nommez les fichiers exportés respectivement `ocs_ge_emprise.gpkg` et
`mnt_emprise.tif`.

#### 5.2. Calculer le raster de pente

Pour modéliser la connectivité et la capacité de déplacement des
espèces, une partie de la contrainte sera représentée par la
topographie. Puisque nous travaillons sur les mammifères terrestres, la
pente constitue un élément important pour caractériser les contraintes
que le relief peut imposer à leurs déplacements.

À partir de `mnt_emprise.tif`, utilisez l'outil **Pente** de GDAL afin
de calculer une couche de pente. Exportez le résultat sous le nom
`pente_emprise.tif`.

Représentez ensuite la pente à l'aide d'un gradient continu afin
d'observer sa répartition sur le site d'étude. Répondez aux questions
suivantes : - Dans quelles zones observe-t-on les pentes les plus fortes
? - Quelles sont, en moyenne, les valeurs de pente observées dans la
vallée de Munster ?

#### 5.3. Reclasser le raster de pente en valeurs de friction

L'objectif est maintenant de convertir les valeurs de pente en valeurs
de friction, en fonction de la contrainte qu'elles peuvent représenter
pour le déplacement des espèces.

À l'aide de l'outil **Reclassification par table**, reclassifiez le
raster de pente en appliquant les seuils et les valeurs de friction du
tableau ci-dessous. Nommez le raster obtenu `friction_pente.tif`.

Plus la valeur de friction est élevée, plus le pixel correspondant
représente un obstacle au déplacement de l'espèce ou du groupe d'espèces
considéré.

  ---------------------------------------------------------------------------
  Pente minimale (°) Pente maximale (°) Valeur de friction Interprétation
                                                           écologique /
                                                           déplacement
  ------------------ ------------------ ------------------ ------------------
                   0                 10                  1 Terrain plat à
                                                           faible pente :
                                                           progression aisée,
                                                           sans surcoût
                                                           énergétique.

                  10                 20                 20 Pente modérée :
                                                           effort
                                                           physiologique
                                                           accru, mais
                                                           déplacement
                                                           continu.

                  20                 30                 50 Pente forte :
                                                           ralentissement
                                                           significatif et
                                                           coût énergétique
                                                           élevé.

                  30                 40                 80 Pente très raide :
                                                           progression
                                                           difficile, terrain
                                                           accidenté.

                  40                 90                100 Pente extrême /
                                                           falaises
                                                           vosgiennes :
                                                           barrière
                                                           topographique
                                                           quasi
                                                           infranchissable.
  ---------------------------------------------------------------------------

Représentez ensuite le résultat spatialement à l'aide d'une palette
continue. Une fois la symbologie définie, exportez la légende sous le
nom `legende_friction`, depuis l'onglet **Style** situé en bas de la
fenêtre de symbologie.

#### 5.4. Attribuer des valeurs de friction à l'occupation du sol

Nous avons modélisé la contrainte que peut représenter la pente. Il
s'agit maintenant de convertir les différentes classes d'occupation du
sol en valeurs de friction.

Dans la couche vectorielle d'occupation du sol, créez un champ
`Friction_OCS` et attribuez les valeurs indiquées dans le tableau
ci-dessous, en vous appuyant sur la nomenclature de niveau 2 (`lib_n2`).

  ------------------------------------------------------------------------
  Valeur (`lib_n2`)        Friction (`Friction_OCS`) Justification
                                                     écologique
  --------------------- ---------------------------- ---------------------
  Forêts                                           1 Habitat optimal,
                                                     couvert protecteur
                                                     continu.

  Milieux humides                                  5 Végétation dense,
                                                     ressource en eau,
                                                     quiétude.

  Formations naturelles                           10 Milieu semi-naturel,
  herbacées ou                                       lisières favorables.
  arbustives                                         

  Espaces verts urbains                           15 Relais végétalisé
                                                     ponctuel, passage
                                                     nocturne.

  Espaces ouverts, sans                           20 Milieu ouvert,
  ou avec peu de                                     absence de couvert,
  végétation                                         mais progression
                                                     aisée.

  Autres zones                                    35 Prairies et pâtures,
  agricoles                                          absence d'abris
                                                     arborés.

  Cultures permanentes                            45 Vignobles et vergers,
                                                     dérangement et
                                                     traitements.

  Terres arables                                  50 Grandes cultures
                                                     ouvertes, labours,
                                                     machinisme.

  Espaces ouverts                                 65 Friches urbaines et
  urbains                                            chantiers, fort
                                                     dérangement.

  Espaces en mutation                             75 Décapages de sols,
                                                     travaux,
                                                     artificialisation.

  Habitat                                         85 Zones pavillonnaires,
                                                     clôtures, chiens
                                                     domestiques.

  Activités économiques                           90 Zones industrielles,
                                                     hangars, parkings
                                                     goudronnés.

  Équipements et                                  90 Enceintes closes
  infrastructures                                    imperméabilisées.
  collectives                                        

  Surfaces en eau                                 95 Plans d'eau et cours
                                                     d'eau profonds :
                                                     barrière physique.

  Infrastructures et                             100 Routes majeures et
  superstructures des                                voies ferrées :
  réseaux de transport                               risque de collision
                                                     létal.
  ------------------------------------------------------------------------

#### 5.5. Rasteriser la couche d'occupation du sol

Les valeurs de friction étant désormais associées à chaque classe
d'occupation du sol, nous allons convertir cette couche vectorielle en
raster. Cette opération permettra de rendre la couche directement
comparable, d'un point de vue géométrique, à la couche de friction issue
des pentes.

Utilisez l'outil de **conversion en raster** et renseignez correctement
les trois paramètres suivants : - le champ à rasteriser : `Friction_OCS`
; - la résolution spatiale, qui doit être identique à celle du MNT ; -
l'emprise, qui doit également être identique à celle du MNT.

Exportez le résultat sous le nom `friction_ocs.tif`.

Représentez ensuite spatialement ce raster en lui appliquant le style
précédemment exporté sous le nom `legende_friction`. En observant les
deux couches de friction --- celle issue de la pente et celle issue de
l'occupation du sol ---, identifiez les zones qui vous semblent les plus
problématiques pour le déplacement des espèces.

#### 5.6. Combiner les deux couches de friction

Cette étape vise à combiner les différentes couches de friction afin
d'obtenir un score de friction composite. Le modèle proposé repose sur
une pondération des deux composantes :

\[ C = w_1 `\times `{=tex}`\text{friction de la pente}`{=tex} + w_2
`\times `{=tex}`\text{friction de l’occupation du sol}`{=tex} \]

Nous allons tester deux hypothèses correspondant à deux profils
écologiques différents.

**Scénario 1 -- Ressource**

Avec (w_1 = 0{,}20) et (w_2 = 0{,}80), ce scénario donne davantage de
poids à l'occupation du sol qu'à la pente. Il vise à représenter les
déplacements de petits mammifères terrestres, tels que la martre des
pins, le chat forestier ou la belette d'Europe.

**Scénario 2 -- Énergie**

Avec (w_1 = 0{,}65) et (w_2 = 0{,}35), ce scénario donne davantage de
poids à la pente qu'à l'occupation du sol. Il vise à représenter les
déplacements de grands mammifères et d'ongulés, comme le cerf élaphe ou
le chevreuil.

À l'aide de la **Calculatrice raster**, calculez les deux nouvelles
couches de friction et nommez-les respectivement
`friction_ressource.tif` et `friction_energie.tif`.

Une fois les rasters exportés, réappliquez-leur la légende de friction
afin de visualiser spatialement les résultats et de comparer les deux
scénarios.

#### 5.7. Calculer les cartes de coût cumulé et de direction des déplacements

Notre objectif est désormais de tracer un chemin de déplacement entre
deux points, en tenant compte des scénarios de friction calculés
précédemment.

Une première étape consiste à calculer, pour chaque scénario, une carte
de coût cumulé et une carte de direction. La carte de coût cumulé stocke
des valeurs de coût qui augmentent à mesure que l'on s'éloigne du point
de départ. La valeur de chaque pixel représente le coût minimal total
nécessaire pour relier le point de départ à ce pixel.

La carte de direction, quant à elle, indique sous la forme d'une valeur
angulaire la direction du voisin correspondant au coût le plus faible.

Pour réaliser cette opération, utilisez l'outil **`r.cost`** : 1.
Choisissez comme couche d'entrée l'une des deux couches de friction
(`friction_ressource.tif` ou `friction_energie.tif`). 2. Définissez
uniquement un point de départ : il n'est pas nécessaire de renseigner un
point d'arrivée. Cliquez sur la position indiquée dans le GeoPackage
`point_deplacement`. 3. Exportez la carte de coût sous le nom
`carte_cout_ressource.tif` ou `carte_cout_energie.tif`. 4. Exportez la
carte de direction sous le nom `carte_direction_ressource.tif` ou
`carte_direction_energie.tif`.

Répétez l'opération pour les deux scénarios de friction et inspectez
spatialement les résultats.

**Question :** que met notamment en lumière la carte de direction ?

#### 5.8. Calculer les chemins de déplacement

La dernière étape consiste à générer les chemins de déplacement à partir
des cartes de coût et de direction. Il s'agit de déterminer quel chemin
de moindre coût peut être emprunté pour rejoindre le point A depuis le
point B.

Pour cela, utilisez l'outil **`r.drain`** : 1. En entrée, utilisez
d'abord votre carte de coût, puis votre carte de direction du mouvement.
2. Cette fois, le point de départ (*starting point*) est le point B, car
le chemin tracé « descendra » vers le point A. 3. Veillez à cocher
l'option **« La carte raster en entrée est une surface de coût »**. 4.
Exportez le résultat de l'algorithme (*Drain*) sous le nom
`corridor_ressource.gpkg` ou `corridor_energie.gpkg`.

Réalisez cette opération pour les deux scénarios.

#### 5.9. Réaliser la mise en carte

Ouvrez le **Composeur d'impression** depuis le menu **Projet \> Nouvelle
mise en page...**.

Organisez les couches dans le canevas selon l'ordre suivant :

1.  **Au sommet :** la couche `points_deplacements`, représentée par des
    ronds colorés distincts avec un contour blanc.
2.  **Juste en dessous :** les deux corridors :
    -   `corridor_ressource`, en violet magenta (`#e7298a`), avec une
        épaisseur de 1,2 mm ;
    -   `corridor_energie`, en orange vif (`#ff7f00`), avec une
        épaisseur de 1,2 mm.
3.  **À un niveau intermédiaire :** la couche d'occupation du sol,
    vectorielle ou rasterisée, avec une symbologie par catégories de
    niveau 1 et une opacité globale de 55 %.
4.  **En arrière-plan :** `ombrage_vosges.tif`, en niveaux de gris, afin
    de faire ressortir le relief du massif.

Ajoutez également les éléments cartographiques suivants :

-   **Carroyage :** coordonnées projetées en Lambert-93, avec un pas de
    10 000 m, des croix intérieures et les coordonnées indiquées en
    bordure.
-   **Titre obligatoire :** « Modélisation des corridors écologiques
    Vosges-Piémont : analyse comparative des coûts d'habitat et de
    relief ».
-   **Légende :** désactivez l'option **Mise à jour automatique** et
    retirez tous les rasters intermédiaires (`r.drain`, cartes de
    direction, `pente_degres`). Conservez uniquement les éléments
    suivants :
    -   Réservoirs d'habitats (source / destination) ;
    -   Corridor -- Scénario 1 (priorité au couvert végétal) ;
    -   Corridor -- Scénario 2 (contrainte énergie / pente) ;
    -   Grands types d'occupation du sol (forêts, prairies, cultures,
        bâti).
-   **Échelle :** ajoutez une barre d'échelle graphique subdivisée, avec
    des unités en kilomètres.
-   **Orientation :** ajoutez une flèche du nord discrète dans un angle
    supérieur.
-   **Cartouche de métadonnées :** indiquez les auteurs, la date, la
    projection (EPSG:2154 -- Lambert-93) et les sources de données (IGN
    BD ALTI®, OCS GE / CLC).

------------------------------------------------------------------------

     
     






