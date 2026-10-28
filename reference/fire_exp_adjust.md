# Compute the wildfire exposure metric with custom transmission distances (Deprecated)

This function still works, but will be removed in future versions of the
package. The same functionality is now included in
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md).
.

## Usage

``` r
fire_exp_adjust(hazard, tdist, no_burn)
```

## Arguments

- hazard:

  a SpatRaster that represents hazardous fuels for the transmission
  distance specified in `tdist`

- tdist:

  Numeric, transmission distance in meters

- no_burn:

  (Optional) SpatRaster that represents the non-burnable landscape. Any
  cells that cannot receive wildfire (e.g. open water, rock) and any
  cells that are not natural (e.g. built environment, irrigated
  agricultural areas) should be of value 1, all other cells should be
  NODATA. This parameter should be provided if preparing data for
  [`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)

## Value

SpatRaster object of exposure values

## Details

If the transmission distances from the wildfire exposure literature are
not representative of the wildland fuels in your area of interest, this
function can be used to change the transmission distance to a custom
distance. It is highly recommended that any exposure layers produced
with this function are validated with observed fire history using the
[`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)
function.

### Spatial Reference

The exposure raster will be returned in the same CRS as the input hazard
layer. A crs must be defined if the outputs will be used in other
functions in this package.

### Spatial resolution

For a specified transmission distance, the spatial resolution must be at
least 3 times finer. For example, for a transmission distance of 300
meters the input data should have a resolution of 100 meters or finer.

## See also

[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
for background information

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# compute long range exposure with custom disance of 800 m
exposure800 <- fire_exp_adjust(hazard, tdist = 800)
#> Warning: 'fire_exp_adjust' is deprecated.
#> Use 'fire_exp' instead.
#> See help("Deprecated")
```
