# Map directional exposure

`fire_exp_dir_map()` plots directional exposure transects onto a map.

## Usage

``` r
fire_exp_dir_map(transects, value, labels, title = "Directional Vulnerability")
```

## Arguments

- transects:

  SpatVector. Output from
  [`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md)

- value:

  (Optional) SpatVector. Adds the value to the map. Use the same value
  feature used to generate the transects with
  [`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md)

- labels:

  (Optional) a vector of three strings. Custom formatting for the
  distances in the legend. If left blank, the function will
  automatically label the distances in meters.

- title:

  (Optional) String. A custom title for the plot. The default is
  `"Directional Vulnerability"`

## Value

a map of directional exposure transects as a tmap object

## Details

This function returns a standardized map with basic cartographic
elements.

The plot is returned as a tmap object which can be further customized
using tmap commands or exported/saved to multiple image file formats.
See
[`tmap::tmap_save()`](https://r-tmap.github.io/tmap/reference/tmap_save.html)
for export details.

### Spatial reference

This function dynamically pulls map tiles for a base map. The crs is set
automatically. See
[`tmap::tm_crs()`](https://r-tmap.github.io/tmap/reference/tm_crs.html)
for details.

## Examples

``` r

# \donttest{
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# generate an example point
point_wkt <- "POINT (345000 5876000)"
point <- terra::vect(point_wkt, crs = hazard)

# compute exposure metric
exposure <- fire_exp(hazard)

# generate transects
transects <- fire_exp_dir(exposure, point, interval = 5)

fire_exp_dir_map(transects)
#> [basemaps] Tiles from "Esri.WorldImagery" will be projected so details (e.g.
#> text) could appear blurry
#> This message is displayed once per session.

# }
```
