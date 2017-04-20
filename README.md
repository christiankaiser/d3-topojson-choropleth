# Example choropleth map with D3 and TopoJSON

The resulting map can be viewed [here](https://cdn.rawgit.com/christiankaiser/d3-topojson-choropleth/9e1fa242720f04a66a81f0078e8d9e44052549e9/index.html).

This choropleth map shows the percentage of single young females (aged 20-35 years) across the Swiss municipalities in 2015.

It uses a TopoJSON file as source for the geometries, and a TSV file for the data. Jenks method is used for finding the optimal class limits.

The source for the geometries file is the Vector-200 municipalities file from [Swisstopo](https://www.swisstopo.admin.ch) for 2015. The data have been downloaded from [here](https://github.com/interactivethings/swiss-maps). The SRS of the geometries is CH1903/LV03 (EPSG:21781).

The source for the statistical data is the Swiss Federal Statistical Office, using the [STAT-TAB](https://www.bfs.admin.ch/bfs/en/home/services/recherche/stat-tab-online-data-search.html) app. The data source is [STATPOP](https://www.bfs.admin.ch/bfs/de/home/dienstleistungen/geostat/geodaten-bundesstatistik/gebaeude-wohnungen-haushalte-personen/ergebnisse-volkszaehlung-ab-2010.assetdetail.183280.html) 2015.



## Preparing the geometries

From the original data in ESRI Shapefile format, a GeoJSON file has been produced using QGIS. At the same time, unused attributes have been dropped.

Conversion from the GeoJSON files to TopoJSON has been done using the following command. Please note that the last part `topomerge` calculated the limits of the cantons and adds them as a separate layer to the TopoJSON:

```bash
geo2topo communes=vec200-communes-2015.geojson lacs=vec200-lakes.geojson | toposimplify -p 100 -f | topoquantize 1e4 | topomerge -k 'd.properties.kt' cantons=communes > vec200-topo.json
```

The resulting TopoJSON is less than 1 Mo, and can be compressed to roughly 270 Ko. However, it does not contain any data, even the name of the commune is stripped. All will be added with the TSV file.




