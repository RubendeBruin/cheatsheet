Use PyProj (included with geopandas)

```python
from pyproj import CRS, Transformer

source = CRS.from_epsg(32066)
destination = CRS.from_epsg(4326)
transformer = Transformer.from_crs(crs_from=source, crs_to=destination)

lat, lon = transformer.transform(easting/0.3048, northing/0.3048)  # <-- may be in meters instead of survey feed
```

just print the source or destination objects for additional info:

```
<Derived Projected CRS: EPSG:32066>
Name: NAD27 / BLM 16N (ftUS)
Axis Info [cartesian]:
- X[east]: Easting (US survey foot)
- Y[north]: Northing (US survey foot)
Area of Use:
- name: United States (USA) - between 90°W and 84°W onshore and offshore - Alabama; Arkansas; Florida; Georgia; Indiana; Illinois; Kentucky; Louisiana; Michigan; Minnesota; Mississippi; Missouri; North Carolina; Ohio; Tennessee; Wisconsin; Gulf of Mexico outer continental shelf (GoM OCS) between approximately 90°W and 84°W - protraction areas Mobile; Viosca Knoll; Mississippi Canyon; Atwater Valley; Lund; Lund South; Pensacola; Destin Dome; De Soto Canyon; Lloyd Ridge; Henderson; Florida Plain; Campeche Escarpment; Apalachicola; Florida Middle Ground; The Elbow; Vernon Basin; Howell Hook; Rankin.
- bounds: (-90.01, 23.95, -83.91, 48.32)
Coordinate Operation:
- name: BLM zone 16N (US survey feet)
- method: Transverse Mercator
Datum: North American Datum 1927
- Ellipsoid: Clarke 1866
- Prime Meridian: Greenwich
```

