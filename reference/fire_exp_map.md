# Visualize exposure in a map

`fire_exp_map()` produces a map with sensible defaults that can be
customized.

## Usage

``` r
fire_exp_map(
  exposure,
  aoi,
  classify,
  class_breaks,
  title = "Wildfire Exposure"
)
```

## Arguments

- exposure:

  SpatRaster (e.g. from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md))

- aoi:

  (Optional) SpatVector of an area of interest to mask exposure

- classify:

  character, either `"local"`, `"landscape"`, or `"custom"`, to specify
  classification scheme to use. The default is `"local"`. If set to
  `"custom"`: the parameter `class_breaks` must be used.

- class_breaks:

  vector of numeric values between 0-1 of the upper limits of each
  custom class. Ignored unless `classify = "custom"`. See details.

- title:

  (Optional) String. A custom title for the plot. The default is
  `"Wildfire exposure"`

## Value

a map is returned as a tmap object.

## Details

This function returns a map with basic cartographic elements and a
standardized colour scale. .

The plot is returned as a tmap object which can be further customized
using tmap commands or exported/saved to multiple image file formats.
See
[`tmap::tmap_save()`](https://r-tmap.github.io/tmap/reference/tmap_save.html)
for export details.

This function visualizes the outputs from
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md).
The map can be returned with a continuous scale or can be classified.

Classes can be chosen from the pre-set `"local"` and `"landscape"`
options, or customized. To use a custom classification scheme, it should
be defined with a list of numeric vectors defining the upper limits of
the breaks. A Nil class is added automatically for exposure values of
exactly zero.

Local classification breaks are predefined as `c(0.15, 0.3, 0.45, 1)`:

- Nil (0)

- 0 - 0.15

- 0.15 - 0.3

- 0.3 - 0.45

- 0.45 - 1

Landscape classification breaks are predefined as
`c(0.2, 0.4, 0.6, 0.8, 1)`:

- Nil (0)

- 0 - 0.2

- 0.2 - 0.4

- 0.4 - 0.6

- 0.6 - 0.8

- 0.8 - 1

### Spatial reference

This function dynamically pulls map tiles for a base map. The crs is set
automatically. See
[`tmap::tm_crs()`](https://r-tmap.github.io/tmap/reference/tm_crs.html)
for details.

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))


# compute exposure
exposure <- fire_exp(hazard)


fire_exp_map(exposure)

```
