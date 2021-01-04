[<img width="400" alt="Mapbox" src="https://raw.githubusercontent.com/mapbox/mapbox-gl-js-docs/publisher-production/docs/pages/assets/logo.png">](https://www.mapbox.com/)

### Exploration des possibilités de visualisation 3D avec [©Mapbox](https://www.mapbox.com/)

L'idée de ce projet est de faire un tour d'horizon des possibilités proposées par mapbox en terme de visualisation et de gestion de données 3D. Vous y retrouverez: 
* un **tutoriel** simplifié pour visualiser et mettre en valeur des données du bâti lyonnais en 3D (2.5D avec extrusion).
* une **bibliographie** des ressources sur le sujet

# Tutoriel

## Quelques informations de cadrage

1. Mapbox GL JS :

**Mapbox GL JS** est une librairie JavaScript basée sur WebGL pour produire des rendus cartographiques interactifs à partir de tuiles vectorielles et/ou de styles Mapbox.

2. Mapbox Studio :

3. Licence :

Les cartes qui mobilisent les fonds de plan, jeux de données ou librairies de Mapbox doivent déclarer une **attribution**. Cette déclaration est automatique dans le cas de l'emploi de Mapbox GL JS. 
D'après les [termes d'utilisation](https://www.mapbox.com/legal/tos), l'utilisateur qui se crée un compte se voit attribuer une licence non-exclusive et non-transférable (et des clés d'accès), ainsi que le droit à :
* user des services Mapbox pour développer des services en ligne, bureau et des applications mobiles ;
* rendre les services disponibles à des utilisateurs finaux.
Pour le reste, Mapbox (site et services) est propriété de Mapbox, Inc.

Chaque utilisateur conserve la propriété du contenu mobilisé via les services Mapbox, mais n'est pas propriétaire du contenu extrait de Mapbox. À l'inverse, Mapbox s'accorde le droit de publier, distribuer, modifier et stocker notre contenu. 

Mapbox peut être employé gratuitement **dans la limite de certains volumes** d'utilisation et de stockage de données. Mapbox tarife son utilisation d'après la [grille de prix suivante](https://www.mapbox.com/pricing/). 

Un exemple des fonctionnalités proposées par Mapbox GL JS est disponible sur ce dépôt, à ce [lien](https://github.com/Promo-Geonum/Mapbox_et_3D/blob/main/index3d.html). En plus des annotations insérées au code, vous trouverez ci-contre les bouts de code associés à certaines fonctionnalités. Mapbox propose quelques fonctions ***en lien avec la 3D (ou 2,5D)*** - extrusion du bâti, application d'un relief, etc. - que nous avons explorées dans une carte web dynamique. Celle-ci explore le **bâti lyonnais placé dans son relief topographique**.

4. Utilisation : 

Nous recommandons d'employer la librairie GL JS et Mapbox Studio **de façon conjointe**. Les deux fonctionnent bien ensemble, et permettent la création de cartes personnalisées assez simplement. La 3D est possible aussi bien dans GL JS que dans Mapbox Studio. Elle est cependant plus flexible en manipulant du code source.

### Mise en place de la carte

Comme pour Leaflet ou Open Layers, il faut, en premier lieu, faire appel à la librairie Mapbox (style et JS), si l'on veut constituer et enrichir sa carte. En l'occurence, la librairie n'a pas besoin d'être téléchargée en local :

```
<html>
<head>
<meta charset="utf-8" />
<title>Ma carte Mapbox</title>
<meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
<script src="https://api.mapbox.com/mapbox-gl-js/v2.0.0/mapbox-gl.js"></script> <!-- lien vers le script Mapbox -->
<link href="https://api.mapbox.com/mapbox-gl-js/v2.0.0/mapbox-gl.css" rel="stylesheet" /> <!-- lien vers le style par défaut de Mapbox -->
```
Dans le script : il faut déclarer une carte, un ***access token*** (ie clé d'accès personnelle associée au compte Mapbox, disponible depuis son profil utilisateur), et les paramètres essentiels de la carte :
```
mapboxgl.accessToken = 'id_AccessToken';
var map = new mapboxgl.Map({
container: 'map', // container id
style: 'mapbox://styles/profilutilisateur/iddestyle', // style URL
center: [lng, lat], // starting position [lng, lat]
zoom: 9, // starting zoom
pitch: 58 // inclinaison
});
```
On peut ici attribuer un style à la carte. Mapbox donne l'accès à de nombreux styles par défaut, dont on récupère l'id sur Mapbox Studio. Depuis son compte utilisateur, on peut également en créer ou en charger. Pour cela :
- Aller dans "Styles", "New style"
- Sélectionner un style, le personnaliser si envie, et enfin le charger
- Pour une utilisation publique, veiller à rendre le style public (depuis la page "styles", et les 3 petits points à droite du style en question)
- Récupérer l'URL et l'ajouter au code

Enfin, il faut déclarer la carte dans le HTML :
```
<div id="map"></div>
```
Et définir un style générique à l'objet map :
```
<style>
	body { margin: 0; padding: 0; }
	#map { position: absolute; top: 0; bottom: 0; width: 100%; }
```

### Choix des données 

Mapbox permet l'utilisation de :
* flux de données WMS/WFS
* tilesets
* données en local (intégrées par du geojson par exemple)

Nous avons exploré les ***tilesets***, originalité de Mapbox. L'intégration de flux de données ou de données locales ne diffère pas des codes classiques.

À l'inverse de librairies comme Leaflet ou Open Layers, Mapbox Studio propose un service d'hébergement de données, transformées au format *tileset*. Un *tileset* est une collection de données matricielles ou vectorielles converties en une grille uniforme à laquelle sont associés des niveaux de zoom. Le format de ces données fluidifie la navigation cartographique.

L'ajout de données personnelles au format tileset nécessite plusieurs étapes :
1. Aller sur Mapbox Studio - "Tilesets" - "New tileset"
2. Glisser des données au format shp zippé, geojson, csv, etc.
3. Ajout, dans le code, d'une **source** et de la **couche** (exemple de *données ponctuelles au format vecteur*) :
```
map.on('load', function () {
	map.addSource('nom_source', {
		type: 'vector',
		url: 'mapbox://profilutilisateur.IDtileset' //ID du tileset à copier depuis mapbox
});

map.addLayer({
	'id': 'nom_source',
	'type': 'circle', //type associé aux entités ponctuelles
	'source': 'nom_source',
	'source-layer': 'nom_tileset', //nom du tileset à récupérer depuis Mapbox (Mapbox modifie légèrement le nom du jeu de données initial, en lui ajoutant quelques caractères aléatoires)
	'paint': {
		'circle-color': 'orange', 'circle-radius': 5
	}
});
```
Dans Mapbox, les différents "types" d'entités vectorielles sont :
* Circle (point)
* Symbol (point avec pictogramme)
* Line (ligne)
* Fill (polygone)
* **Fill-extrusion (3D)**

### Extrusion du bâti

Comme indiqué plus haut, il existe un format spécifique aux données 3D : c'est le format **fill-extrusion**. Ce format est géré de façon semblable dans le code. Il requiert seulement la définition de quelques paramètres spécifiques. Dans notre cas, nous avons créer un tileset à partir de la couche bâtie de la **BD Topo de Lyon**, défini une source (cf supra), puis ajouter la couche :
```
map.addLayer({
	'id': 'topo',
	'type': 'fill-extrusion', //format de données spécifiques à la 2,5 D
	'source': 'topo',
	'source-layer': 'Bati_Lyon_2019-7537lw', //nom du tileset à copier depuis mapbox
	'paint': {
		'fill-extrusion-color': 'white',
		'fill-extrusion-opacity': 0.8,
		'fill-extrusion-height': {'type': 'identity', 'property': 'HAUTEUR'}, //champ renseignant la hauteur des bâtiments dans la BD Topo
		'fill-extrusion-base': 0
	}
});
});
```
Ce code fait apparaître les bâtiments de la BD Topo en blanc, et procède à l'extrusion du bâti. Évidemment, celle-ci n'est visible qu'en inclinant la carte (paramètre ***pitch***). Le champ "HAUTEUR" est celui qui contient l'information de hauteur des bâtiments : il est donc renseigné dans le paramètre de style 'fill-extrusion-height'.

### MNT

La librairie GL JS rend possible l'ajout et la modélisation 3D d'un **modèle numérique de terrain**, via des fonctions dédiées. Il est possible :
* d'ajouter un MNT personnel (en créant un tileset, par exemple)
* de mobiliser des MNT publics, fournis par Mapbox

C'est ce point que nous explorons dans notre code. Pour ce faire, il faut, comme pour les données vectorielles, **ajouter une source** correspondant au MNT et enfin ajouter la source comme une **couche de type "terrain"**.
```
//Ajout d'une source correspondant au MNT
map.on('load', function () {
	map.addSource('mapbox-dem', {
		'type': 'raster-dem',
		'url': 'mapbox://mapbox.mapbox-terrain-dem-v1',
		'tileSize': 512,
		'maxzoom': 14
});

//Ajouter la source MNT comme une couche "terrain" avec une hauteur exagérée
map.setTerrain({ 'source': 'mapbox-dem', 'exaggeration': 1.5 });
 
});
```
Les 3 types génériques lus par Mapbox sont : *vector, raster, raster-dem*, ce dernier étant dédié aux MNT. Pour rendre compte du relief, il s'agit ensuite de définir une valeur d'exagération, fixée ici à 1,5.

### Gestion de la caméra

### Ciel ! (mon mari)

### Interactivité avec la carte, personnalisation

Nous renseignons ici quelques fonctionnalités bien utiles (voire requises) en cartographie web :

1. Ajout de l'échelle
```
map.addControl(
	new mapboxgl.ScaleControl({
		maxWidth: 120,
		unit: 'metric'
	}));
```

2. Ajout d'un bouton navigation
```
var nav= new mapboxgl.NavigationControl();
map.addControl(nav, 'top-left');
```

3. Ajout d'un menu de couches

En trois étapes :
* Configurer le style du menu en CSS
```
#menu {
width: 20%; margin-right: auto; margin-left: auto;
z-index: 1; top: 10px; right: 10px; position: absolute;
border-color: #FFFFFF; background-color: #808080 ;
font-size: 12px; font-family: 'Helvetica Neue', Arial, Helvetica, sans-serif; }

	#menu a {
		display: block; color: #FFFFFF; padding: 8px 16px;
		text-align: center; font-weight: bold;
		border-style: solid; border-color: #000000;}

	#menu a.active { background-color: #CC6600;
	color: #FFFFFF; }

	#menu a:hover:not(.active) {
		background-color: #CC6600;
		color: #FFFFFF;
	}
 ```

* Ajout du menu dans le HTML
```
<div id="menu"></div> 
```

* Ajout du code JS
```
var toggleableLayerIds= ['couche1', 'couche2'];

for (var i = 0; i < toggleableLayerIds.length; i++) {var id = toggleableLayerIds[i];

	var link= document.createElement('a');
	link.href= '#';link.className= 'inactive';
	link.textContent= id;

	link.onclick= function(e) {var clickedLayer= this.textContent;
		e.preventDefault();
		e.stopPropagation();
		var visibility= map.getLayoutProperty(clickedLayer, 'visibility');
		if (visibility=== 'visible') 
			{map.setLayoutProperty(clickedLayer, 'visibility', 'none');this.className= '';} else{this.className= 'active';map.setLayoutProperty(clickedLayer, 'visibility', 'visible');} };

	var layers= document.getElementById('menu');  layers.appendChild(link); }
 ```
 
4. Ajout d'un bouton qui recentre la carte sur des coordonnées spécifiées
En trois étapes également :
* CSS :
```
#fit {
display: block;
position: relative;
margin: 0px auto;
width: 50%;
height: 40px;
padding: 10px;
border: none;
border-radius: 3px;
font-size: 12px;
text-align: center;
color: #fff;
background: #ee8a65;
}
```

* HTML :
```
<button id="fit">Centrer sur X</button> <!-- Bouton qui permettra à l'usager de centrer la carte sur X -->
```

* JS :
```
//Fonction associée au bouton "fit" qui recentre la carte sur les coordonnées GPS indiquées (SW,NE)
document.getElementById('fit').addEventListener('click', function () {
map.fitBounds([
[LngSW,LatSW],
[LngNE,LatNE]
]);
});
```

5. Ajout d'une fenêtre popup au click
Cette fonction permet l'affichage d'un attribut sémantique (champ "titre") au click sur une entité de la couche "layer" de la carte.
```
map.on('click', 'layer', function (e) {
var coordinates = e.features[0].geometry.coordinates.slice();
var titre = e.features[0].properties.titre;

while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
}
 
new mapboxgl.Popup()
.setLngLat(coordinates)
.setHTML(titre)
.addTo(map);
});

// Changer le curseur quand la souris passe au-dessus de la couche "layer"
map.on('mouseenter', 'layer', function () {
map.getCanvas().style.cursor = 'pointer';
});
 
// Le changer à nouveau quand il quitte la couche "layer"
map.on('mouseleave', 'layer', function () {
map.getCanvas().style.cursor = '';
});
```

## En utilisant Mapbox Studio

[©Mapbox Studio](https://www.mapbox.com/mapbox-studio/) est une application de Mapbox qui permet de gérer ses données géospatiales et de concevoir des styles de cartes personnalisés.

Pour accéder à Mapbox Studio il est nécessaire de créer un compte, plusieurs modalités existent: 
* une version payante avec un tarif à la consommation
* une version gratuite pour les étudiant.e.s notamment mais avec un nombre d'actions limitées (**200 000** requêtes de l'API Mapbox Static Tiles i.e. des tuiles matricielles statiques générées à partir d'un style basé sur Mapbox GL.)

À la création du compte, un jetons d'accès [`access token`](https://docs.mapbox.com/help/how-mapbox-works/access-tokens/#how-access-tokens-work) sera créé afin de donner accès aux produits Mapbox en associant l'API de requêtes Mapbox à votre compte. C'est ce jeton d'accès sous la forme `pk.codepersonnalisé` qu'il faudra préciser dans le code de la `map` pour accéder aux ressource de l'API Mapbox.

_Licence_: 

## Ce que propose Mapbox Studio 

Certianes données appelées _Components_ sont disponibles de base dans le Studio, par exemple: 
* limites administratives 
* **bâtiments** 
* utilisations du sol et l'eau 
* élements naturels 
* toponymes
* points d'intérêt (avec le nom) 
* réseau routier
* imageries satellites
* topographie (_Terrain_)
* transports (aérien, ferroviaire, routier, cyclable)
* multimodal: marche, vélo, etc

### Connecter à QGis

On peut ajouter un tileset à QGis comme une **couche WMTS**. Dans QGis :
1. Add WMTS Layer
2. Coller l'URL suivant en modifier username, URL du style, access token :
https://api.mapbox.com/styles/v1/username/styleURL/wmts?access_token=id_accesstoken


## Utilisez vos propres données !
#### Intégration de données 

(image du processus upload - dataset - tileset etc)

* `dataset`: collection de données brutes au format GeoJSON, éditables (pour des données trop lourdes il faut passer par [l'API Datasets](https://docs.mapbox.com/api/maps/uploads/)) 
* [`tileset`](https://docs.mapbox.com/studio-manual/reference/tilesets/): collection de données (datasets) découpées en une grille uniforme de tuiles de données de même taille pour être gérées par Mapbox (avec une limite de 20 uploads / mois et 300 MB / upload)

Si vous n'avez pas besoin de modifier vos données, téléchargez-les directement en dans un `tileset`

### Personnaliser votre tileset dans Studio 

Ajouter votre tileset: 
> Add new layer > Select data > **Source** > Sélectionner votre tileset (ici le bâti ajouté précédemment)

**Géométrie** de votre tileset (ici extrusion 2.5D):  
> Add new layer > Select data > **Type** > Fill extrusion 


**Style** de la couche bâti (tileset ici): 
> Sélectionner votre couche > **Style**, plusieurs options s'offrent à vous: 
  * Height : définir la hauetur des bâtiments 
  * Color: modifier la couleur de vos bâtiments 

Tous ces paramètres peuvent tenir compte d'expression en fonction de vos données  
(EX: si vous avez des données de hauteur pour chaque bâtiment vous pouvez préciser cela dans _Style with data condition_) 


# Bibliographie 
### Documentation générale

* Documentation mapbox: https://docs.mapbox.com/ 
* Prise en main générale de Mapbox: https://blog.ippon.fr/2020/09/11/introduction-a-mapbox/ 
* Article court d'introduction de géovisualisation avec Mapbox: https://digitalcommons.calpoly.edu/cpesp/137/

### Documentation technique 

#### Mapbox GL JS

* Présentation de Mabpox GL sur le Wiki OpenStreetMap: https://wiki.openstreetmap.org/wiki/Mapbox_GL
* Documentation Mapbox GL JS: https://docs.mapbox.com/mapbox-gl-js/api/
* Code pour extrusion des bâtiments dans Mapbox (2.5D): https://docs.mapbox.com/mapbox-gl-js/example/3d-buildings/
* Code d'ajout d'un MNT dans Mapbox: https://docs.mapbox.com/mapbox-gl-js/example/add-terrain/
* Indoor mapping: https://docs.mapbox.com/mapbox-gl-js/example/3d-extrusion-floorplan/

#### Mapbox Studio 

* Extrusion depuis Mapbox Studio : https://docs.mapbox.com/studio-manual/examples/3d-buildings/
* Vidéo explicative - ajout de données 3D dans Mapbox avec ©Studio: https://docs.mapbox.com/help/tutorials/add-3d-buildings-studio-data-driven-styling-video/ 
* Documentation sur l'extrusion dans Studio: https://docs.mapbox.com/mapbox-gl-js/style-spec/layers/#fill-extrusion

### Références

* Exemples d'utilisation par Boris Mericskay: https://medium.com/@BorisMericskay/extrusion-3d-de-donn%C3%A9es-spatiales-9c67d76431b9
