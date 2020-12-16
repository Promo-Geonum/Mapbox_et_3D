[<img width="400" alt="Mapbox" src="https://raw.githubusercontent.com/mapbox/mapbox-gl-js-docs/publisher-production/docs/pages/assets/logo.png">](https://www.mapbox.com/)

### Exploration des possibilités de visualisation '3D' avec [©Mapbox](https://www.mapbox.com/)

L'idée de ce projet est de faire un tour d'horizon des possibilités proposées par mapbox en terme de visualisation et de gestion de données 3D. Vous y retrouverez: 
* un **tutoriel** simplifié pour visualiser et mettre en valeur des données du bâti lyonnais en 3D (2.5D avec extrusion), en se concentrant sur la thématique des toitures lyonnaises 
* une **bibliographie** des ressources sur le sujet

# Tutoriel

## En utilisant Mapbox GL JS

_Licence_: 

### Choix des données 

Toutes les données utilisées sont issues de [Data Grand Lyon](https://data.grandlyon.com/) en format flux WMS/WFS: 

* Bâtiments: https://data.grandlyon.com/jeux-de-donnees/batiments-metropole-lyon/donnees
* Arbres d'alignement: https://data.grandlyon.com/jeux-de-donnees/arbres-alignement-metropole-lyon/donnees

### Extrusion du bâti

### Personnalisation du style 

### Gestion de la caméra 

### Ciel (mon mari !)

### MNT

* Modèle Numérique de Terrain à 5mètres de précision: 

## En utilisant Mapbox Studio

_Licence_: 


# Bibliographie 
### Documentation générale

* Documentation mapbox: https://docs.mapbox.com/ 
* Prise en main générale de Mapbox: https://blog.ippon.fr/2020/09/11/introduction-a-mapbox/ 
* Article court d'introduction de géovisualisation avec Mapbox: https://digitalcommons.calpoly.edu/cpesp/137/

### Documentation technique 

#### Mapbox GL JS

Mapbox GL JS est une librairie JavaScript basée sur WebGL pour produire des rendus cartographiques interactifs à partir de tuiles vectorielles et/ ou de styles Mapbox.

* Présentation de Mabpox GL sur le Wiki OpenStreetMap: https://wiki.openstreetmap.org/wiki/Mapbox_GL
* Documentation Mapbox GL JS: https://docs.mapbox.com/mapbox-gl-js/api/
* Vidéo explicative - ajout de données 3D dans Mapbox: https://docs.mapbox.com/help/tutorials/add-3d-buildings-studio-data-driven-styling-video/ 
* Code pour extrusion des bâtiments dans Mapbox (2.5D): https://docs.mapbox.com/mapbox-gl-js/example/3d-buildings/
* Code d'ajout d'un MNT dans Mapbox: https://docs.mapbox.com/mapbox-gl-js/example/add-terrain/
* Indoor mapping: https://docs.mapbox.com/mapbox-gl-js/example/3d-extrusion-floorplan/

#### Mapbox Studio 

[©Mapbox Studio](https://www.mapbox.com/mapbox-studio/) est une application de Mapbox qui permet de gérer ses données géospatiales et de concevoir des styles de cartes personnalisés.

* Extrusion depuis Mapbox Studio : https://docs.mapbox.com/studio-manual/examples/3d-buildings/

### Références

* Exemples d'utilisation par Boris Mericskay: https://medium.com/@BorisMericskay/extrusion-3d-de-donn%C3%A9es-spatiales-9c67d76431b9
