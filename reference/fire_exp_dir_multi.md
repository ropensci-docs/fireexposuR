# Summarize directional load for multiple values

`fire_exp_dir_multi()` summarizes the directional vulnerability load for
multiple points in a study area in a table.

## Usage

``` r
fire_exp_dir_multi(exposure, values, ...)
```

## Arguments

- exposure:

  SpatRaster from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)

- values:

  Spatvector of value as a point or simplified polygon

- ...:

  arguments passed to
  [`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md).

## Value

a data.frame of the features with attributes: value featureID, degree,
seg1 (binary), seg2 (binary), seg3 (binary), full (binary), outer
(binary).

## Details

This function summarizes multiple directional vulnerability assessments
into a single table. For each degree, the frequency of input values with
a continuous pathway at that trajectory is found. This summary can be
useful in identifying trends in directional exposure to values within a
regional area of interest.

Continuous pathways are assessed for the full span of all three
directional assessment transect segments, or limited to the outer two
segments. If the values being assessed are variable sizes and being
represented as points, using the outer option is recommended. The
inner-most segment is sensitive to the size of the value when a point is
used. Adjusting the parameters for
[`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md)
is also supported. See details in
[`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md)
for more information.

## References

Beverly JL, Forbes AM (2023) Assessing directional vulnerability to
wildfire. *Natural Hazards* **117**, 831-849.
[doi:10.1007/s11069-023-05885-3](https://doi.org/10.1007/s11069-023-05885-3)

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# generate 10 random example points within the hazard extent
e <- terra::buffer(terra::vect(terra::ext(hazard), crs = hazard), -15500)
points <- terra::spatSample(e, 10)

# compute exposure metric
exposure <- fire_exp(hazard)

# directional load for multiple points
fire_exp_dir_multi(exposure, points, interval = 10)
#> # A tibble: 360 × 7
#>    featureID   deg  seg1  seg2  seg3  full outer
#>        <int> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1         1    10     0     0     1     0     0
#>  2         1    20     0     0     1     0     0
#>  3         1    30     0     1     0     0     0
#>  4         1    40     0     0     0     0     0
#>  5         1    50     0     1     0     0     0
#>  6         1    60     0     1     1     0     1
#>  7         1    70     1     1     0     0     0
#>  8         1    80     1     1     0     0     0
#>  9         1    90     1     1     0     0     0
#> 10         1   100     1     1     1     1     1
#> # ℹ 350 more rows
```
