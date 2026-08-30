# Map exposure with a continuous scale (Deprecated)

This function still works, but will be removed in future versions of the
package. The same functionality is now included in
[`fire_exp_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map.md).
.

## Usage

``` r
fire_exp_map_cont(exposure, aoi, title = "Wildfire Exposure")
```

## Arguments

- exposure:

  SpatRaster from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)

- aoi:

  (Optional) SpatVector of an area of interest to mask the exposure

- title:

  (Optional) String. A custom title for the plot. The default is
  `"Wildfire Exposure"`

## Value

a map is returned as a ggplot object

## Details

This function returns a standardized map with basic cartographic
elements. The exposure values are mapped using a continuous scale. There
is no base map added with this function.

The plot is returned as a ggplot object which can be exported/saved to
multiple image file formats.

### Spatial Reference

The map will be drawn using the same CRS as the input data.

## See also

[`fire_exp_map_class()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map_class.md)

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# Compute exposure
exposure <- fire_exp(hazard)

fire_exp_map_cont(exposure)
#> Warning: 'fire_exp_map_cont' is deprecated.
#> Use 'fire_exp_map' instead.
#> See help("Deprecated")

```
