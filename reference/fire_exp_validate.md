# Validate exposure with observed fires

For advanced users. `fire_exp_validate()` compares the proportion of
exposure classes in a the study area to the proportion of exposure
classes within observed burned areas.

## Usage

``` r
fire_exp_validate(
  burnableexposure,
  fires,
  aoi,
  class_breaks = c(0.2, 0.4, 0.6, 0.8, 1),
  samplesize = 0.005
)
```

## Arguments

- burnableexposure:

  A SpatRaster of exposure, non-burnable cells should be removed using
  optional parameter `no_burn = `in
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md).

- fires:

  A SpatVector of observed fire perimeters

- aoi:

  (Optional) A SpatVector that delineates an area of interest

- class_breaks:

  (Optional) vector of numeric values between 0-1 of the upper limits of
  each class. The default is `c(0.2, 0.4, 0.6, 0.8, 1)`. See details.

- samplesize:

  Proportion of areas to sample. The default is `0.005` (0.5%)

## Value

a table of number of cells (n) and proportions (prop) of exposure
classes within a sampled area (Sample) and across the full extent
(Total).for the full extent of the exposure data (expected) and only
within the burned areas (observed).

## Details

This function automates a simple validation method to assess if fire
burns preferentially in areas with high exposure. The methods, and
figure produced with
[`fire_exp_validate_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate_plot.md),
are based on Beverly et al. (2021).

The function requires an exposure raster produced for a past point in
time. Cells that cannot burn, or do not represent natural land cover
should be removed by setting the `no_burn` parameter in
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
or
[`fire_exp_adjust()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_adjust.md).

The function also requires fire perimeter data. Currently, the function
takes the fires as a Vector of polygons because that is typically how
fire boundaries are stored in spatial databases. The fires input data
should include all of the burned area that has occurred following the
time period the input exposure layer was produced for. It is up to the
user to determine the appropriate amount of burned area required for a
meaningful assessment.

A random sample is taken to account for spatial autocorrelation, the
sampled location results can be used to test for significant
differences. The sample size can be adjusted. The sample size represents
a proportion of cells, the default is `0.005` (0.5%). It is the user's
responsibility to set an appropriate sample size.

The class breaks can be customized from the default of 0.2 intervals by
setting the `class_breaks` parameter. A class of Nil is automatically
added for values exactly equal to 0.

## References

Beverly JL, McLoughlin N, Chapman E (2021) A simple metric of landscape
fire exposure. *Landscape Ecology* **36**, 785-801.
[doi:10.1007/s10980-020-01173-8](https://doi.org/10.1007/s10980-020-01173-8)

## See also

[`fire_exp_validate_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate_plot.md)

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# generate example non-burnable cells data
polygon_path <- system.file("extdata", "polygon.shp", package ="fireexposuR")
polygon <- terra::vect(polygon_path)
no_burn <- terra::rasterize(polygon, hazard)

# generate example fire polygons by buffering random points
points <- terra::spatSample(terra::rescale(hazard, 0.8),
                            30, as.points = TRUE)
fires <- terra::buffer(points, 800)
# PLEASE NOTE THIS EXAMPLE DATA DOES NOT GENERATE MEANINGFUL RESULTS

# compute exposure and remove non-burnable cells
exposure <- fire_exp(hazard, no_burn = no_burn)

# validation table
fire_exp_validate(exposure, fires)
#>    exposure  exp_vals     of    group     n       prop
#> 1         0       Nil  Total Expected 12197 0.11994060
#> 2         1   0 - 0.2  Total Expected 12200 0.11997011
#> 3         2 0.2 - 0.4  Total Expected 10380 0.10207293
#> 4         3 0.4 - 0.6  Total Expected 12621 0.12411006
#> 5         4 0.6 - 0.8  Total Expected 16163 0.15894072
#> 6         5   0.8 - 1  Total Expected 38131 0.37496558
#> 7         0       Nil  Total Observed   577 0.09687710
#> 8         1   0 - 0.2  Total Observed   620 0.10409671
#> 9         2 0.2 - 0.4  Total Observed   770 0.12928140
#> 10        3 0.4 - 0.6  Total Observed   737 0.12374077
#> 11        4 0.6 - 0.8  Total Observed  1246 0.20920081
#> 12        5   0.8 - 1  Total Observed  2006 0.33680322
#> 13        0       Nil Sample Expected    42 0.08267717
#> 14        1   0 - 0.2 Sample Expected    74 0.14566929
#> 15        2 0.2 - 0.4 Sample Expected    57 0.11220472
#> 16        3 0.4 - 0.6 Sample Expected    52 0.10236220
#> 17        4 0.6 - 0.8 Sample Expected    96 0.18897638
#> 18        5   0.8 - 1 Sample Expected   187 0.36811024
#> 19        1   0 - 0.2 Sample Observed     5 0.16666667
#> 20        2 0.2 - 0.4 Sample Observed     5 0.16666667
#> 21        3 0.4 - 0.6 Sample Observed     3 0.10000000
#> 22        4 0.6 - 0.8 Sample Observed     3 0.10000000
#> 23        5   0.8 - 1 Sample Observed    14 0.46666667
```
