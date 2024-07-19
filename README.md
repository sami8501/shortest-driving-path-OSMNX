Voici un projet de fichier README pour le package OSMnx, axé sur le calcul des plus courts chemins, basé sur le notebook Python fourni :

# OSMnx - Calcul des plus courts chemins

OSMnx est une bibliothèque Python puissante pour travailler avec des données OpenStreetMap (OSM) et effectuer des analyses de réseau. Ce README se concentre sur l'utilisation d'OSMnx pour calculer les plus courts chemins entre des points d'intérêt dans une zone urbaine.

## Installation

```bash
pip install osmnx networkx geopandas matplotlib
```

## Fonctionnalités principales

### 1. Importation des bibliothèques nécessaires

```python
import pandas as pd
import geopandas as gpd
from shapely.geometry import LineString, Point
import osmnx as ox
import networkx as nx
import matplotlib.pyplot as plt
```

### 2. Définition des points d'origine et de destination

```python
origin = gpd.GeoDataFrame(columns=['name', 'geometry'], crs=4326, geometry='geometry')
origin.at[0, 'geometry'] = Point(43.304315, 5.394611)
origin.at[0, 'name'] = 'Palais Longchamp'
```

### 3. Récupération des données OSM

```python
place_name = 'Marseille, France'
graph = ox.graph_from_place(place_name)
```

### 4. Extraction des points d'intérêt (restaurants dans cet exemple)

```python
tags = {'amenity': 'restaurant'}
restaurants = ox.geometries_from_place(place_name, tags)
restaurants = restaurants[['name', 'geometry']]
restaurants['geometry'] = [geom.centroid for geom in restaurants['geometry']]
```

### 5. Calcul du plus court chemin

```python
def shortest_path(origin, destination, network='drive'):
    # ... (voir le code complet dans le notebook)
```

### 6. Visualisation des résultats

```python
routes = shortest_path(origin, restaurants, network='walk')
routes['distance'] = routes['geometry'].length
routes['distance'].hist(bins=50)
plt.show()
```

## Exemples d'utilisation

Le notebook fourni illustre :
- La récupération du réseau routier de Marseille
- L'extraction des restaurants de la zone
- Le calcul des plus courts chemins entre un point d'origine (Palais Longchamp) et tous les restaurants
- La visualisation des distances des chemins calculés

## Fonctions utilitaires supplémentaires

### Extraction du réseau de marche

```python
def osm_network(origin, destination, network='drive'):
    # ... (voir le code complet dans le notebook)

walking_network = osm_network(origin, restaurants, network='walk')
```

## Exportation des données

Les résultats peuvent être exportés en format GeoJSON pour une utilisation ultérieure ou une visualisation dans d'autres outils :

```python
routes_wgs.to_file(r'walking_routes.geojson', driver='GeoJSON')
restaurants.to_file(r'restaurants_marseille.geojson', driver='GeoJSON')
origin.to_file(r'palais_longchamp.geojson', driver='GeoJSON')
area.to_file(r'marseille_area.geojson', driver='GeoJSON')
walking_network.to_file(r'walk_network.geojson', driver='GeoJSON')
```

## Licence

OSMnx est distribué sous licence MIT.

## Contact

Pour plus d'informations, visitez la [documentation officielle d'OSMnx](https://osmnx.readthedocs.io/).
