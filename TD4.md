# TD4 – Le pastoralisme dans le massif du Queyras : implantation des cabanes et contraintes environnementales

**Date :** 19-11-2026  
**Nom :** Herrault PA  
**Accès données :** *[Insérer le lien vers votre dépôt / Seafile]*

---

## 🎯 Objectifs
- Revoir les outils de sélection, découpage et jointure (spatiale et attributaire) sous QGIS.
- Mobiliser les algorithmes topographiques et microclimatiques de **GRASS GIS** (`r.slope.aspect`, `r.sun`, `r.horizon`).
- Extraire et croiser des statistiques zonales rasters et vectorielles autour d'entités ponctuelles.
- Construire une typologie multi-critères à l'aide de la calculatrice de champs et concevoir une carte de synthèse respectant les règles de sémiologie graphique.

---

## 🏔️ Contexte et problématique

Le pastoralisme dans les Hautes-Alpes est une pratique millénaire (attestée dès le Néolithique) qui a façonné les paysages d'altitude. Au-delà de sa fonction productive (élevage ovin et bovin pour le lait, le fromage ou la laine), cette activité joue un rôle écologique majeur : le pâturage estival permet de maintenir les milieux ouverts et de freiner la fermeture des paysages par les processus d'enfrichement et d'embroussaillement.

Cette pratique repose sur une gestion fine de l'espace par l'homme (bergers et éleveurs), qui doit conduire le troupeau tout en composant avec de multiples pressions :
- **des pressions biotiques**, notamment le retour des grands prédateurs (loup) nécessitant des dispositifs de protection (chiens de protection / patous, parcs de nuit) ;
- **des pressions anthropiques et touristiques**, liées à la fréquentation croissante des alpages en été (randonneurs, VTT), sources potentielles de dérangement ou de conflits d'usage.

Faire paître les troupeaux en haute montagne implique une connaissance approfondie du milieu afin d'offrir aux bêtes les meilleures conditions de **quiétude** et de **confort thermique** (ensoleillement, abri face aux vents dominants, topographie peu accidentée) tout en garantissant l'accès aux **ressources vitales** : des pelouses alpines riches (conditionnant la qualité du lait et la santé du troupeau) et des points d'eau accessibles.

**Problématique :** *L'objectif de ce TD est de comprendre les conditions environnementales qui régissent la distribution des troupeaux dans le massif du Queyras à travers l'implantation des cabanes pastorales. Grâce à une approche géomatique multi-critères, nous chercherons à déterminer dans quels environnements ces cabanes sont implantées, tant sur le plan topographique et microclimatique que vis-à-vis de l'accès aux ressources en eau et fourragères.*

---

### 0. Préparation de l’espace de travail

1. Créez un dossier principal `TD4_Pastoralisme_Queyras`.
2. Créez les sous-dossiers :
   - `Donnees`
   - `Resultats`
   - `Annexes`
3. Placez dans `Donnees` les couches rasters et vectorielles fournies (toutes projetées en **RGF93 / Lambert-93 – EPSG:2154**) :
   - `mnt_queyras_20m.tif` (Couche raster : Modèle Numérique de Terrain à 20 m de résolution)
   - `troncons_hydro.gpkg` (Couche vectorielle linéaire : réseau hydrographique BD TOPO® IGN)
   - `carhab_habitats.gpkg` (Couche vectorielle surfacique : cartographie nationale des habitats naturels et semi-naturels CarHab)
   - `style_habitat_carhab.qml` (Fichier de style QGIS associé à la nomenclature CarHab)
   - `cabanes_pastorales.gpkg` (Couche vectorielle ponctuelle : localisation et attributs des cabanes pastorales)
   - `site_etude.gpkg` (Couche vectorielle surfacique : périmètre de la zone d'étude dans le Queyras)

---

### 1. Préparation et découpage des données sur le site d'étude

Afin de travailler avec des données précisément centrées sur le secteur étudié, commencez par extraire les entités comprises dans le périmètre de `site_etude.gpkg`.

- À l'aide de l'outil **Vecteur > Outils de géotraitement > Couper**, découpez les trois couches vectorielles sur l'emprise de `site_etude.gpkg` et enregistrez-les dans `Resultats` sous les noms suivants :
  - `troncons_hydro_se.gpkg`
  - `carhab_habitats_se.gpkg`

---

### 2. Exploration des cabanes pastorales et pression de prédation

Avant d'analyser les contraintes physiques du milieu, inspectez la table attributaire de `cabanes_pastorales.gpkg` afin de repérer les informations liées à la conduite des troupeaux (effectifs, présence de chiens de protection / patous, attaques ou prédation).

1. Affichez `mnt_queyras_20m.tif` en arrière-plan à l'aide d'un gradient continu d'altitude.
2. Représentez `cabanes_pastorales.gpkg` en **symboles proportionnels** (ou catégorisés selon le nombre/la présence de patous).
3. **Questions :**
   - Comment se répartissent les cabanes équipées de chiens de protection sur le territoire d'étude ?
   - Observe-t-on un lien entre l'altitude, l'ouverture des milieux (alpages d'altitude vs fonds de vallée) et la pression de prédation ?

---

### 3. Modélisation des variables topographiques et microclimatiques (GRASS GIS)

Nous allons maintenant calculer deux variables microclimatiques déterminantes pour le confort thermique des troupeaux et la phénologie de la ressource herbacée : **la durée d'ensoleillement estival** et **l'abri face aux vents dominants**. Nous mobiliserons pour cela les outils de **GRASS GIS** intégrés à la Boîte à outils de traitements de QGIS.

#### 3.1. Calcul de la pente et de l'exposition (`r.slope.aspect`)

Les modèles solaires de GRASS nécessitent en entrée des rasters de pente et d'exposition générés selon ses propres conventions géométriques (dans GRASS, l'exposition est mesurée en degrés dans le sens anti-horaire à partir de l'Est = $0^\circ$).

1. Ouvrez l'algorithme **GRASS > Raster > `r.slope.aspect`**.
2. Renseignez `mnt_se.tif` en couche d'élévation.
3. Vérifiez que le format de sortie de la pente (`Slope`) est réglé sur **`degrees`** (degrés).
4. Exportez les deux rasters générés dans `Resultats` sous les noms :
   - `pente.tif`
   - `exposition.tif`

#### 3.2. Calcul de la durée d'ensoleillement estival (`r.sun.insoltime`)

**Principe de l'outil :** L'algorithme **`r.sun`** (décliné sous `r.sun.insoltime` dans QGIS) est un modèle physique de rayonnement solaire. À partir du MNT, de la pente, de l'exposition et de la latitude du lieu, il calcule la trajectoire apparente du soleil dans le ciel pour un jour donné de l'année. Surtout, contrairement à un simple calcul local d'exposition, `r.sun` modélise les **ombres portées par le relief environnant** (crêtes et sommets voisins masquant le disque solaire à certaines heures de la journée), ce qui est indispensable en haute montagne.

1. Ouvrez **GRASS > Raster > `r.sun.insoltime`**.
2. Renseignez les paramètres d'entrée suivants :
   - **Elevation raster map :** `mnt_queyras_20m.tif`
   - **Slope raster map :** `pente.tif`
   - **Aspect raster map :** `exposition.tif`
   - **Day of the year (1-365) :** **`196`** (correspondant au **15 juillet**, date représentative du cœur de la saison d'estive en alpage).
   - **Time step when computing all-day radiation sums :** `0.5` (pas de temps de 30 minutes pour une précision fine de la course du soleil).
3. Exportez la couche de durée d'insolation (`Insolation time [h]`) dans `Resultats` sous le nom `insolation_time.tif`. *(Note : si certaines faces Nord très raides présentent des pixels `NoData` correspondant à $0\text{ h}$ d'ensoleillement direct, remplacez les valeurs nulles par `0` via l'outil `r.null` ou `Remplir les cellules NoData`).*
4. **Représentation et analyse :** Appliquez un gradient continu de couleurs sur `insolation_time.tif`. Quels secteurs du massif bénéficient des durées d'ensoleillement les plus longues (jusqu'à ~13–14 h) et où se situent les déficits radiatifs ?

#### 3.3. Calcul de l'abri topographique face aux vents dominants (`r.horizon`)

**Principe et paramétrage de l'outil :** Dans le massif du Queyras, le régime éolien est marqué par les flux dominants d'Ouest à Nord-Ouest (vents synoptiques et brises thermiques remontant les vallées). L'outil **`r.horizon`** calcule, pour chaque pixel du MNT et dans une direction donnée, **l'angle vertical (en radians) de l'obstacle topographique le plus haut qui bouche l'horizon**.
- Dans la convention angulaire de GRASS ($0^\circ$ = Est, $90^\circ$ = Nord, sens anti-horaire), le flux provenant de l'Ouest-Nord-Ouest ($300^\circ$ géographique) correspond à l'azimut **`150°`**.
- **Interprétation :** Plus la valeur du pixel est élevée, plus une crête ou un versant se dresse haut face au vent et joue le rôle de **bouclier naturel** (zone abritée sous le vent). À l'inverse, une valeur proche de `0` indique un terrain dégagé, directement exposé au flux (crêtes, cols ou fond de vallée aligné dans le couloir du vent).

1. Ouvrez **GRASS > Raster > `r.horizon`**.
2. Renseignez les paramètres suivants :
   - **Nom de la carte d'élévation raster source :** `mnt_queyras_20m.tif`
   - **Direction pour laquelle vous voulez connaître la hauteur de l'horizon :** **`150`**
   - **La distance maximale à prendre en compte :** **`5000`** (rayon de recherche des crêtes de 5 km)
   - *(Astuce QGIS : si l'interface exige un pas angulaire supérieur à 0, renseignez `360` en pas angulaire, `150` en angle de départ et `151` en angle de fin).*
3. Exportez le résultat dans `Resultats` sous le nom `abri_vent.tif`.

---

### 4. Analyse des ressources végétales (CarHab) et de l'accès à l'eau

#### 4.1. Caractérisation des milieux et de la ressource fourragère (`CarHab`)

**Qu'est-ce que la donnée CarHab ?** Le programme national **CarHab** (*Cartographie nationale des habitats naturels et semi-naturels*) modélise et cartographie les habitats végétaux (physionomie de la végétation, étages bioclimatiques, cortèges floristiques) à une échelle fine (1:25 000). Dans le cadre d'une étude pastorale, cette donnée est fondamentale car elle permet de distinguer les **milieux ouverts pâturables à forte valeur fourragère** (pelouses alpines et subalpines) des milieux contraignants ou non pâturables (forêts denses, landes fermées, éboulis, rochers).

1. Chargez la couche `carhab_habitats_se.gpkg` et appliquez-lui le style prédéfini (`Propriétés > Style > Charger le style...` > `style_habitat_carhab.qml`). Observez l'étagement altitudinal de la végétation sur le massif.
2. Calculez la superficie de chaque polygone en hectares dans la table attributaire (`$area / 10000`), puis utilisez l'outil **Statistiques par catégories** (en regroupant par type d'habitat, par exemple `nom_complet_hab` ou `nom_simplifie_hab`) pour établir le bilan des surfaces occupées par chaque milieu.
3. **Questions et ressources complémentaires :**
   - Quel est l'habitat le plus représenté sur votre zone d'étude du Queyras ?
   - Pour visualiser la physionomie réelle de ces habitats sur le terrain et consulter leurs fiches descriptives, référez-vous aux ressources officielles de l'INPN / PatriNat :
     - [Programme national CarHab (PatriNat / OFB)](https://www.patrinat.fr/fr/cartographie-nationale-des-habitats-carhab-6671)
     - [Cahiers d'habitats Flora Veg EU]([https://hal.science/hal-05456928v2/file/CarHab_Notice_2026.pdf](https://floraveg.eu/habitat/))
     - [Site du Parc naturel régional du Queyras](https://www.pnr-queyras.fr/)

#### 4.2. Calcul de l'éloignement aux ressources en eau

L'abreuvement des troupeaux et les besoins domestiques du berger imposent une proximité aux cours d'eau ou aux sources.

1. Ouvrez l'outil **Analyse vectorielle > Joindre les attributs par le plus proche** 
2. Configurez l'outil :
   - **Couche en entrée 1 :** `cabanes_pastorales_se.gpkg`
   - **Couche en entrée 2 :** `troncons_hydro_se.gpkg`
   - **Nombre maximum de plus proches voisins :** **`1`**
   - **Préfixe du champ joint :** `troncons_`
3. Exportez le résultat dans `Resultats` sous le nom `distance_eau.gpkg`.
4. **Questions :** À l'aide de l'outil *Afficher le résumé statistique* ($\Sigma$), quelle est la distance moyenne (en mètres) séparant les cabanes d'un cours d'eau ? Quelle est la cabane la plus éloignée de l'eau ?

---

### 5. Extraction multi-critères à l'échelle des cabanes

L'implantation d'une cabane ne se résume pas au seul pixel sur lequel repose son toit (qui peut être biaisé par un replat artificiel ou un talus) : il convient de caractériser **son environnement immédiat** (aire de repos du troupeau, parc de nuit, abords).

1. **Création des zones tampons :** À l'aide de l'outil **Vecteur > Outils de géotraitement > Tampon**, créez un buffer de **`100` mètres** autour de `cabanes_pastorales.gpkg` (en veillant à conserver l'identifiant unique de chaque cabane).
2. **Extraction des statistiques rasters :** Ouvrez l'outil **Analyse raster > Statistiques de zone**. Pour chacun des quatre rasters suivants, calculez la **Moyenne** (`mean`) à l'intérieur des tampons de 100 m en renseignant un préfixe explicite :
   - `mnt_se.tif` $\rightarrow$ préfixe `_altitude`
   - `pente.tif` $\rightarrow$ préfixe `_pente`
   - `insolation_time.tif` $\rightarrow$ préfixe `_insolation`
   - `abri_vent.tif` $\rightarrow$ préfixe `_abri_vent`
3. **Jointure attributaire :** À l'aide de l'onglet **Jointures** dans les propriétés de `cabanes_pastorales_se.gpkg` (ou via l'outil *Joindre les attributs par valeur de champ*), rapatriez ces quatre colonnes moyennes vers votre couche ponctuelle initiale `cabanes_pastorales_se.gpkg` à partir de l'identifiant unique des cabanes.

---

### 6. Association de l'habitat dominant (`CarHab`) aux cabanes

1. À partir des tampons de 100 m construits à l'étape précédente, croisez les buffers avec `carhab_habitats_se.gpkg` (via **Intersection** ou **Joindre les attributs par localisation (résumé)** en retenant l'habitat majoritaire / intersecté).
2. Joignez les attributs écologiques correspondants (`code_hab_carhab`, `nom_complet_hab`, `nom_simplifie_hab`, `etage_de_vegetation`) à votre couche `cabanes_pastorales_se.gpkg`.

---

### 7. Consolidation de la table attributaire finale

1. Par une dernière jointure attributaire (via l'identifiant unique de chaque cabane), rapatriez le champ de distance au cours d'eau (`troncons_distance`) issu de `distance_eau.gpkg` vers `cabanes_pastorales_se.gpkg`.
2. Exportez cette couche consolidée sous le nom `cabanes_synthese.gpkg`.
3. **Vérification :** Ouvrez la table attributaire de `cabanes_synthese.gpkg` et assurez-vous que chaque cabane dispose bien de l'ensemble des variables descriptives : altitude, pente, durée d'insolation, abri au vent, habitat CarHab, étage de végétation et distance à l'eau.

---

### 8. Élaboration d'une typologie multi-critères des cabanes

#### Philosophie de la typologie
L'analyse individuelle de chaque variable montre que les cabanes pastorales résultent d'un **compromis permanent** entre confort microclimatique, accès à l'eau et disponibilité de la ressource fourragère selon l'altitude. Construire une **typologie** permet de dépasser la simple juxtaposition de chiffres pour regrouper les cabanes selon des **profils fonctionnels homogènes** (cabanes de mi-versant abritées, cabanes d'estive haute en pelouse alpine, cabanes soumises à de fortes contraintes hydriques ou éoliennes). Cet outil d'aide à la décision permet notamment aux gestionnaires d'alpages d'identifier les équipements les plus vulnérables (stress hydrique estival, exposition aux intempéries) et de prioriser les aménagements (impluviums, abreuvoirs, rénovation du bâti).

#### Implémentation dans la calculatrice de champs
Ouvrez la table attributaire de `cabanes_synthese.gpkg`, lancez la **Calculatrice de champs** (`Ctrl+I`), cochez **Créer un nouveau champ** et exécutez les deux expressions suivantes *(adaptez les noms de colonnes entre guillemets si vos préfixes diffèrent)* :

1. **Création de la classe typologique (`typologie`, type `Texte`, longueur `60`) :**

```sql
CASE
  WHEN "troncons_distance" > 150 OR "_abri_ventmean" <= 0.06 THEN 'Type 3 - Contrainte hydrique et éolienne'
  WHEN "_altitudemean" >= 2450 THEN 'Type 2 - Estive haute (pelouse alpine)'
  ELSE 'Type 1 - Mi-versant abrité et proche eau'
END
```

2. **Calcul d'un indice synthétique de confort et de résilience sur 10 (`score_confort`, type `Nombre décimal (réel)`) :**

```sql
round(
  (
    -- Proximité de l'eau (max 3 pts si < 50 m, 0 pt si > 250 m)
    max(0, min(3, 3 * (1 - ("troncons_distance" - 50) / 200))) +
    -- Abri face aux vents dominants (max 3 pts si >= 0.30 rad)
    min(3, ("_abri_ventmean" / 0.30) * 3) +
    -- Ensoleillement estival (max 2 pts si >= 13 h)
    min(2, max(0, ("_insolationmean" - 10) / 3) * 2) +
    -- Faible pente aux abords (max 2 pts si <= 12°, 0 pt si >= 25°)
    max(0, min(2, 2 * (1 - ("_pentemean" - 12) / 13)))
  ), 1
)
```

---

### 9. Mise en page cartographique et règles de sémiologie graphique

Pour restituer les résultats de votre analyse spatiale de manière claire et hiérarchisée, ouvrez le **Composeur d'impression** (**Projet > Nouvelle mise en page...**) et appliquez les principes de sémiologie graphique suivants (Bertin) :

1. **Hiérarchie visuelle et superposition des plans (du sommet vers le fond) :**
   - **Premier plan (information thématique principale) :** La couche `cabanes_synthese.gpkg`. Appliquez une **double variable visuelle** :
     - la **couleur (variable qualitative)** selon le champ `typologie` (trois teintes contrastées et intuitives : vert/bleu pour le *Type 1 abrité*, orange/jaune pour le *Type 2 d'altitude*, rouge pour le *Type 3 sous contrainte*) ;
     - la **taille proportionnelle (variable quantitative)** selon le champ `score_confort` (ou l'effectif du troupeau), avec un fin contour blanc pour détacher les symboles du fond.
   - **Plan intermédiaire (contexte hydrographique et écologique) :**
     - le réseau hydrographique `troncons_hydro_se.gpkg` en bleu fin (`#1f78b4`), indispensable pour justifier visuellement la proximité à l'eau des cabanes ;
     - les grands types d'habitats `carhab_habitats_se.gpkg` (regroupés par grands milieux : pelouses, forêts, landes/rochers) avec une **opacité réduite à 40–50 %** afin de ne pas écraser l'information ponctuelle.
   - **Arrière-plan (support topographique) :** Un ombrage du relief (`hillshade` généré à partir de `mnt_se.tif`) en niveaux de gris pour donner la lecture immédiate des crêtes, des versants et des fonds de vallée.

2. **Habillage cartographique obligatoire :**
   - **Titre explicite :** *« Implantation des cabanes pastorales dans le massif du Queyras : typologie multi-critères des contraintes topographiques, microclimatiques et d'accès aux ressources »*.
   - **Légende structurée et nettoyée :** Désactivez la *Mise à jour automatique*, renommez tous les intitulés techniques en français clair (sans underscores ni noms de fichiers `.tif`/`.gpkg`), et regroupez les éléments par rubriques (*Typologie des cabanes*, *Réseau hydrographique*, *Grands milieux naturels CarHab*).
   - **Échelle graphique :** Barre d'échelle métrique/kilométrique simple à chiffres ronds.
   - **Orientation :** Flèche du Nord sobre placée dans un angle dégagé.
   - **Carroyage :** Coordonnées projetées en **RGF93 / Lambert-93 (EPSG:2154)** en bordure de cadre.
   - **Cartouche de métadonnées :** Auteur, date, système de projection (`EPSG:2154`), et sources des données (`IGN BD ALTI® / BD TOPO®`, `PatriNat / OFB CarHab`, `Observatoire du pastoralisme`).
