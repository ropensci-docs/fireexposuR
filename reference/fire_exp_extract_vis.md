# Visualize exposure to values in a summary table or map (deprecated)

This function still works, but please update your code to use the new
[`fire_exp_extract_summary()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_summary.md)
or
[`fire_exp_extract_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_map.md)
instead. This function will be removed in the future.

## Usage

``` r
fire_exp_extract_vis(
  values_ext,
  classify = c("local", "landscape", "custom"),
  class_breaks,
  method = c("max", "mean"),
  map = FALSE,
  zoom_level,
  title = "Classified Exposure to Values"
)
```

## Arguments

- values_ext:

  Spatvector of points or polygons from
  [`fire_exp_extract()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract.md)

- classify:

  character, either `"local"`, `"landscape"`, or `"custom"`, to specify
  classification scheme to use. The default is `"local"`. If set to
  `"custom"`: the parameter `class_breaks` must be used.

- class_breaks:

  vector of numeric values between 0-1. Ignored unless
  `classify = "custom"`. See details.

- method:

  character, either `"max"` or `"mean"`. If `values_ext` are polygons
  the default is `"max"`.This parameter is ignored when `values_ext` are
  point features.

- map:

  Boolean. When `TRUE`, a map is returned as a ggplot object. The
  default is `FALSE`.

- zoom_level:

  (Optional). Numeric. Ignored when `map = FALSE`. set the zoom level
  for the base map tile. See details. Defaults if:

  - `classify = "local"` or `"custom"` the zoom level default is `12`

  - `classify = "landscape"` the zoom level default is `7`

- title:

  (Optional) String. Ignored when `map = FALSE`. A custom title for the
  plot. The default is `"Classified Exposure to Values"`

## Value

a summary table is returned as a data frame object, Unless:
`map = TRUE`: a ggplot object

## Details

This function visualizes the outputs from
[`fire_exp_extract()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract.md)
with classes. Classes can be chosen from the pre-set `"local"` and
`"landscape"` options, or customized. To use a custom classification
scheme, it should be defined with a list of numeric vectors defining the
upper limits of the breaks. A Nil class is added automatically for
exposure values of exactly zero.

Local classification breaks are predefined as `c(0.15, 0.3, 0.45, 1)`:

- Nil (0)

- 0 - 0.15

- 0.15 - 0.3

- 0.3 - 0.45

- 0.45 - 1

\#' Landscape classification breaks are predefined as
`c(0.2, 0.4, 0.6, 0.8, 1)`:

- Nil (0)

- 0 - 0.2

- 0.2 - 0.4

- 0.4 - 0.6

- 0.6 - 0.8

- 0.8 - 1

### Spatial reference

This function dynamically pulls map tiles for a base map when
`map = TRUE`. The inputs are projected to WGS 84/Pseudo-Mercator
([EPSG:3857](https://epsg.io/3857)) to align them with the map tiles.

### Zoom level

The map tile zoom level may need to be adjusted. If the base map is
blurry, increase the zoom level. Higher zoom levels will slow down the
function, so only increase if necessary. Reference the [OpenStreetMap
Wiki](https://wiki.openstreetmap.org/wiki/Zoom_levels) for more
information on zoom levels.

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

values_exp <- fire_exp_extract(exposure, points)

# summarize example points in a table
fire_exp_extract_vis(values_exp, classify = "local")
#> Warning: 'fire_exp_extract_vis' is deprecated.
#> Use 'fire_exp_extract_summary' instead.
#> See help("Deprecated")
#>   class_range  n prop method
#> 1    0 - 0.15  8 0.08     NA
#> 2  0.15 - 0.3 19 0.19     NA
#> 3  0.3 - 0.45 52 0.52     NA
#> 4    0.45 - 1 21 0.21     NA

# visualize example points in standardized map
fire_exp_extract_vis(values_exp, map = TRUE)
#> Warning: 'fire_exp_extract_vis' is deprecated.
#> Use 'fire_exp_extract_map' instead.
#> See help("Deprecated")

```
