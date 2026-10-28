# Summarize exposure by class

`fire_exp_summary()` creates a summary table of area and proportions of
exposure in predetermined or custom exposure classes.

## Usage

``` r
fire_exp_summary(
  exposure,
  aoi,
  classify = c("landscape", "local", "custom"),
  class_breaks
)
```

## Arguments

- exposure:

  SpatRaster from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)

- aoi:

  (optional) SpatVector of an area of interest to mask exposure for
  summary

- classify:

  character, either `"local"`, `"landscape"`, or `"custom"`, to specify
  classification scheme to use. The default is `"local"`. If set to
  `"custom"`: the parameter `class_breaks` must be used.

- class_breaks:

  vector of numeric values between 0-1 of the upper limits of each
  custom class. Ignored unless `classify = "custom"`. See details.

## Value

a summary table as a data frame object

## Details

This function summarizes the outputs from
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
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

Landscape classification breaks are predefined as
`c(0.2, 0.4, 0.6, 0.8, 1)`:

- Nil (0)

- 0 - 0.2

- 0.2 - 0.4

- 0.4 - 0.6

- 0.6 - 0.8

- 0.8 - 1

The table reports the number of pixels, the proportion, and area in
hectares and meters squared in each class.

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# read example area of interest
polygon_path <- system.file("extdata", "polygon.shp", package ="fireexposuR")
aoi <- terra::vect(polygon_path)

# Compute exposure
exposure <- fire_exp(hazard)

# Summary for full extent of data
fire_exp_summary(exposure, classify = "landscape")
#>   class_range npixels   prop    aream2 areaha
#> 1         Nil   12197 0.1199 121970000  12197
#> 2     0 - 0.2   12203 0.1199 122030000  12203
#> 3   0.2 - 0.4   10412 0.1023 104120000  10412
#> 4   0.4 - 0.6   12639 0.1242 126390000  12639
#> 5   0.6 - 0.8   16163 0.1589 161630000  16163
#> 6     0.8 - 1   38131 0.3748 381310000  38131

# Summary masked to an area of interest
fire_exp_summary(exposure, aoi, classify = "landscape")
#>   class_range npixels   prop aream2 areaha
#> 1     0 - 0.2       6 0.0822  60000      6
#> 2   0.2 - 0.4      39 0.5342 390000     39
#> 3   0.4 - 0.6      28 0.3836 280000     28
```
