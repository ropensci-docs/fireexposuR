# Extract exposure values to features

`fire_exp_extract()` extracts the underlying exposure value for each
feature in the values layer.

## Usage

``` r
fire_exp_extract(exposure, values)
```

## Arguments

- exposure:

  SpatRaster (e.g. from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md))

- values:

  Spatvector of points or polygons

## Value

a SpatVector object with new attribute(s)

## Details

This function appends the underlying exposure value to the input feature
as a new attribute. The values input can be provided as either points or
polygons. The values should be singlepart features (i.e. the attribute
table has one row per value). If the values are polygon features both
the maximum and mean exposure is computed. Any values outside the extent
of the exposure raster will be returned with an exposure of NA.

Outputs from this function can be visualized with
[`fire_exp_extract_vis()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_vis.md)
or exported as a spatial feature.

### Spatial Reference

The inputs for the exposure and values layer must have the same
coordinate reference system (CRS) defined. The transects will be
returned in the same CRS as the inputs.

### Scale

The spatial resolution of the input exposure raster will effect the
output. The exposure value returned by this function are based on the
cell value underlying the feature in the values input. Note that if the
resolution of the exposure raster is coarse, there may be multiple
values within the same cell and the returned values will reflect this.
For polygon features, the maximum and mean value of all cells within the
boundary of the polygon are used. If the exposure raster is coarse,
there may not be more than one cell within the polygon which will result
in these values being the same.

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# read example area of interest
polygon_path <- system.file("extdata", "polygon.shp", package ="fireexposuR")
aoi <- terra::vect(polygon_path)

# generate random points within the aoi polygon
points <- terra::spatSample(aoi, 100)

# compute exposure
exposure <- fire_exp(hazard)

fire_exp_extract(exposure, points)
#> class       : SpatVector
#> geometry    : points
#> dimensions  : 100, 3  (geometries, attributes)
#> extent      : 343779.7, 344349.3, 5875150, 5876348  (xmin, xmax, ymin, ymax)
#> coord. ref. : NAD83 / Alberta 10-TM (Forest) (EPSG:3400)
#> names       : Shape_Leng Shape_Area exposure
#> type        :      <num>      <num>    <num>
#> values      :          0          0    0.525
#>                        0          0   0.3875
#>                        0          0    0.375
#>               ...
```
