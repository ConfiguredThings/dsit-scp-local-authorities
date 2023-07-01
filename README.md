# DSIT Secure Connected Places Local Authority Maps

This repository relates to a request from DSIT to produce a map of the local authorities that have taken part in the Secure Connected Places research project related to the [Secure Connected Places Playbook](https://www.gov.uk/guidance/secure-connected-places-playbook). 

Original maps were sourced from the Office for National Statistics' [Geoportal](https://geoportal.statistics.gov.uk), however unfortunately they utilise the [OSGB36](https://en.wikipedia.org/wiki/Ordnance_Survey_National_Grid) coordinate reference system. These required mapping to the more widely supported [WGS84](https://en.wikipedia.org/wiki/World_Geodetic_System) system, to do so [GDAL](https://gdal.org)'s `ogr2ogr` tool was utilised with the folllowing syntax:
```
ogr2ogr -f "GeoJSON" <output-file> <input-file> -t_srs EPSG:4326 -s_srs EPSG:27700
```

As the [South London Partnership](https://southlondonpartnership.co.uk) is not a formal administrative organisation, its boundary was formed by creating a GeoJSON [FeatureCollection](https://datatracker.ietf.org/doc/html/rfc7946#section-3.3) of [Features](https://datatracker.ietf.org/doc/html/rfc7946#section-3.2) and utilising [TurfJS](https://turfjs.org)'s [dissolve](https://turfjs.org/docs/#dissolve) function, creating a GeometryCollection object. Finally TurfJS's [flatten](https://turfjs.org/docs/#flatten) function is utilised to convert the GeometryCollection back to a simple FeatureCollection.

The generated GeoJSON files then required conversion to KML for import into Google Maps. Again `ogr2ogr` was utilised, this time with the following syntax:
```
ogr2ogr -f "KML" <output-file> <input-file>
```

The resulting KML files were imported into Google [My Maps](https://www.google.com/mymaps), and the resulting [map](https://www.google.com/maps/d/edit?mid=1nDhFbcEtHwt1-HFj5LZgOytrRdl8PsE&usp=sharing) shared with the client.
