ENC are Electronic Charts.

The data can be extracted using GDAL

The caveat here is to use `feature = layer.GetNextFeature()` instead of layer.GetFeature(i)


```
from osgeo import ogr


shapefiles = []
shapefiles.append(r"US5TX22M\US5TX22M.000")
shapefiles.append(r"US5TX27M.000")
driver = ogr.GetDriverByName("S57")

polys = dict()
pts  = dict()

for shapefile in shapefiles:

    dataSource = driver.Open(shapefile, 0)

    for i in range(dataSource.GetLayerCount()):
        layer = dataSource.GetLayer(i)

        name = layer.GetName()


        for j in range(layer.GetFeatureCount()):
            feature = layer.GetNextFeature()
            if feature:
                geom = feature.GetGeometryRef()
                if geom is not None:
                    points = geom.GetPoints()

                    if points:

                        if len(points)>1:
                            print(layer.GetName())

                            if name in polys:
                                data = polys[name]
                            else:
                                data = []
                            data.append(points)
                            polys[name] = data


                        elif len(points) == 1:

                            data = getattr(pts, name, [])
                            data.append(points)
                            pts[name] = data

import pickle

with open('points.pickle','wb') as f:
    pickle.dump(points, f)

with open('polys.pickle','wb') as f:
    pickle.dump(polys, f)
```

GDAL env:
`conda create -n gdal gdal -c conda-forge`
