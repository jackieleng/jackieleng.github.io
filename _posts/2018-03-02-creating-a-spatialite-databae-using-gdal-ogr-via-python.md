---
layout: post
title: "Creating a Spatialite database using GDAL/OGR via Python"
date: 2018-03-02 16:07
categories: update
---

A seemingly simple task, creating a Spatialite database using GDAL/OGR via
Python, has been a lot more complicated than I expected it to be.

After studying the Python GDAL/OGR cookbook for a bit it seemed simple enough:
get the desired vector layer driver using OGR (for Spatialite you have to use
the 'SQLite' driver), create a data source, create a layer using the data
source, create fields (your columns), and then you can start creating features
with geometries, and put them into the layer. The example in the cookbook was
for shapefile, but making it work for Spatialite shouldn't be too hard, right?
Just switch drivers and you're done. Something like this:

```python
# NOTE: this code doesn't work correctly
from osgeo import ogr, osr

# get Spatialite/SQLite driver
driver = ogr.GetDriverByName('SQLite')

# create data source
data_source = driver.CreateDataSource("cities.sqlite")
srs = osr.SpatialReference()
srs.ImportFromEPSG(4326)

# create layer
layer = data_source.CreateLayer("cities", srs, geom_type=ogr.wkbPoint)

# create fields
field_city = ogr.FieldDefn("city", ogr.OFTString)
field_city.SetWidth(24)
layer.CreateField(field_city)

# create feature
feature = ogr.Feature(layer.GetLayerDefn())
feature.SetField("city", "Paris")

# set feature geometry
point = ogr.Geometry(ogr.wkbPoint)
point.AddPoint(1, 2)
feature.SetGeometry(point)

# create feature in layer
layer.CreateFeature(feature)
feature = None

# close data source
data_source = None
```

Unfortunately, when I tried opening the Spatialite file in QGIS I got this
error: `Failure getting table metadata ... is this really a Spatialite database?`
When I tried opening the file as a regular vector layer in QGIS it worked
however. I found out that you have to explicitly enable the Spatialite
extension when creating the data source. Otherwise, you're still using the
OGR data provider, but with an SQLite container. These driver specific options
can be specified using the `options` argument in `CreateDataSource`:

```python
data_source = driver.CreateDataSource("cities.sqlite", options=['SPATIALITE=YES'])
```

GDAL/OGR does have documentation available for its Python API which is
relatively detailed: [http://gdal.org/python/osgeo.ogr.Driver-class.html#CreateDataSource](http://gdal.org/python/osgeo.ogr.Driver-class.html#CreateDataSource).
The documentation also links to a page where you can find all the driver
options and information: [http://www.gdal.org/ogr_formats.html](http://www.gdal.org/ogr_formats.html)

However, after enabling Spatialite extension, the features were not showing up
in QGIS anymore. The database was apparently empty.

After searching on the internet for a while I found a [SE answer](https://gis.stackexchange.com/questions/97311/no-viewable-feature-when-creating-line-with-ogr-in-python)
that suggests that changing the geometry type to a '2.5D' type (e.g.:
`ogr.wkbPoint25D`) would help. It didn't make sense to me how this would have
any impact, but since I couldn't find a lot of other people with this problem I
tried it anyway. To my surprise, it did fix the problem. My features
were finally showing up in QGIS. For completeness, I changed the layer creation
to:

```python
layer = data_source.CreateLayer("cities", srs, geom_type=ogr.wkbPoint25D)
```

and kept the feature geometries the same (`ogr.wkbPoint`).

However, this seemed like a hack to me, so tried reproducing this problem again
on another computer. This time Python gave me some errors:

```
ERROR 1: sqlite3_step() failed:
  cities.GEOMETRY violates Geometry constraint [geom-type or SRID not allowed] (19)
```

I was working on Ubuntu 14.04 when doing this, which has an old GDAL version
that unfortunately didn't give any errors or warning. On the other computer
a newer GDAL version was installed where you can actually see that there is a
problem when creating the feature. However, it is *even better* to set
`ogr.UseExceptions()`so that things don't silently fail.

The error message gives a hint to what the real problem was with creating our
features: the feature geometries were not compatible with the geometry type
that we specified. This problem is caused by the `AddPoint` method.
The geometry is correctly created using `ogr.Geometry(ogr.wkbPoint)`, but after
calling `AddPoint` the geometry type changes to `ogr.wkbPoint25D`:

```python
>>> point = ogr.Geometry(ogr.wkbPoint)
>>> point.GetGeometryType()
1  # ogr.wkbPoint
>>> point.AddPoint(1, 2)
>>> point.GetGeometryType()
-2147483647  # ogr.wkbPoint2D
```

Apparently `AddPoint` also adds a Z coordinate to the geometry, which defaults
to 0 if you leave it out. There is also an `ogr.AddPoint_2D` method which only
does 2D coordinates.

```python
>>> point = ogr.Geometry(ogr.wkbPoint)
>>> point.AddPoint_2D(1, 2)
>>> point.GetGeometryType()
1
```

If you want to be safe, the best way is probably to create the geometry from
WKT yourself:

```python
wkt = "POINT(%f %f)" %  (1, 2)
point = ogr.CreateGeometryFromWkt(wkt)
```

GDAL/OGR made it seem to me that writing to a different file format would just
be matter of switching to another driver. However, it isn't always that simple
and some drivers do have their own specific quirks you have to be aware of.
And as I've found out some problems only happen in specific drivers but are
silenty ignored or 'remedied' by other drivers. An instance of
['leaky abstractions'](https://en.wikipedia.org/wiki/Leaky_abstraction) I
suppose.
