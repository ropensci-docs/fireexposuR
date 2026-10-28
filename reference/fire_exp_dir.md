# Conduct a directional exposure assessment

`fire_exp_dir()` returns a SpatVector of linear transects toward a
value. Transects are assessed as viable wildfire pathways by
intersecting them with areas of high exposure.

## Usage

``` r
fire_exp_dir(
  exposure,
  value,
  t_lengths = c(5000, 5000, 5000),
  interval = 1,
  thresh_exp = 0.6,
  thresh_viable = 0.8,
  table = FALSE
)
```

## Arguments

- exposure:

  SpatRaster (e.g. from
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md))

- value:

  Spatvector of value as a point or simplified polygon

- t_lengths:

  (Optional) A vector of three numeric values. The length of each
  transect starting from the value in meters. The default is
  `c(5000, 5000, 5000)`.

- interval:

  (Optional) Numeric. The degree interval at which to draw the transects
  for analysis. Can be one of 0.25, 0.5, 1, 2, 3, 4, 5, 6, 8, or 10
  (factors of 360, ensures even spacing). The default is `1`.

- thresh_exp:

  (optional) Numeric. The exposure value to use to define high exposure
  as a proportion. Must be between 0-1. The default is `0.6`.

- thresh_viable:

  (optional) Numeric. The minimum intersection of a transect with a high
  exposure patch to be defined as a viable pathway as a proportion. Must
  be between 0-1. The default is `0.8`.

- table:

  Boolean, when `TRUE`: returns a table instead of a feature. The
  default is `FALSE`.

## Value

a SpatVector of the transects with the attributes: 'deg' = degree, 'seg'
= segment, and 'viable'. The transects will be returned with the same
CRS as the input features.

If `table = TRUE`: a data frame is returned instead with an additional
attribute 'wkt', which is a Well Known Text (WKT) string of transect
geometries (coordinate reference system: WGS84).

## Details

`fire_exp_dir()` automates the directional vulnerability assessment
methods documented in Beverly and Forbes (2023). This analysis is used
to assess linear wildfire vulnerability on the landscape in a systematic
radial sampling pattern. This is a landscape scale process, so the
exposure raster used should also be landscape scale. Use `tdist = "l"`
when preparing data with
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
for use with this function. See
[`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
details for more information.

The output line features will have the attribute 'viable' which can be
used to visualize the pathways. Outputs can be visualized with
[`fire_exp_dir_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_plot.md),
[`fire_exp_dir_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_map.md),
or exported as a spatial feature.

### Spatial Reference

The inputs for the exposure and value layer must have the same
coordinate reference system (CRS) defined. The transects will be
returned in the same CRS as the inputs.

This function draws the transects by calculating the end point of a
transect by finding the shortest path along the WGS84
([EPSG:4326](https://epsg.io/5326)) ellipsoid at a given bearing and
distance. The `value` input is reprojected from the input CRS to
latitude and longitude using WGS84 for the calculations. After the
transects are created, they are projected to match the CRS of the input
exposure and value layer. The lengths of the projected transects will be
effected by the scale factor of the input CRS; however, the geodesic
lengths are maintained.

### Value feature

The value feature can be provided as a point or a simplified polygon.

If using a point feature the analysis can be sensitive to the placement
of a point. For example, if using a point to represent a large town a
point placed at the centroid will likely have different outputs than a
point placed at the edge of the community due to the arrangement of
lower exposure values typical of a built environment.

An option to use a simplified polygon has been added to this function
for values that may be too large to represent with a single point. The
polygon should be drawn with the consideration of the following or the
function will not be able to run. The polygon must be a single part
polygon with no holes. The polygon should have a smooth and simple
shape, ideally circular or ellipsoidal. The polygon should also have an
approximate radius of less than 5000 meters from the center. If the area
of interest is larger than this then consider using multiple
assessments.

### Default Values

The default values are based on the methods used and validated in
Alberta, Canada by Beverly and Forbes (2023). Options have been added to
the function to allow these values to be adjusted by the user if
required. Adjusting these values can lead to unexpected and uncertain
results.

### Adjusting the transects

The drawing of the transects can be customized by varying the intervals
and segment lengths if desired. Adjustments to the `interval` and
`t_lengths` parameters will effect how much of the exposure data is
being captured by the transects. If both parameters are being adjusted
some trigonometry might be required to find the optimal combination of
the parameters to ensure distances between the transects make sense for
the intended application. The resolution of the exposure raster may also
be a factor in these decisions.

#### Interval

The interval parameter defines how many transects are drawn. The default
of `1` draws a transect at every whole degree from 1-360. This outputs a
total of 1080 transects, 360 for each segment. Increasing the interval
will output less transects and process faster. When the interval is
increased, the distance between the ends of the transects will also be
increased. For example: the terminus of 15000 meter transects (the
default) drawn every 1 degree (the default) will be approximately 250
meters apart, but if drawn at 10 degree intervals will be approximately
2500 meters apart. Larger intervals will increase speed and processing
time, but might not capture potential pathways between transects farther
from the value.

#### Lengths

The t_lengths parameter allows a custom distance to be defined for the
three transect segments in meters. Lengths can be increased or
decreased. The segments can also be different lengths if desired.

### Adjusting the thresholds

Threshold adjustments should only be considered if validation within the
area of interest have been done. The function
[`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)
has been included with this package to make the process easier, but
still requires significant time, data, and understanding.

#### High exposure

The `thresh_exp` parameter can be adjusted to define the minimum
exposure value to be considered for the directional assessment. The
default value of `0.6` is based on the findings of Beverly et al. (2021)
who showed that observed fires burned preferentially in areas where
wildfire exposure values exceed 60%. Adjusting this value is only
recommended after a validation of wildfire exposure has been conducted
in the area of interest.

#### Viability

The `thresh_viable` parameter defines the minimum intersection with high
exposure areas to be considered a viable pathway. The default value of
`0.8` was determined by Beverly and Forbes (2023) by drawing continuous
linear transects within burned areas to represent observed pathways. It
was found that the average intersection with patches of pre-fire high
exposure was 80%. This methodology could be repeated in the users area
of interest.

## References

Beverly JL, Forbes AM (2023) Assessing directional vulnerability to
wildfire. *Natural Hazards* **117**, 831-849.
[doi:10.1007/s11069-023-05885-3](https://doi.org/10.1007/s11069-023-05885-3)

Beverly JL, McLoughlin N, Chapman E (2021) A simple metric of landscape
fire exposure. *Landscape Ecology* **36**, 785-801.
[doi:10.1007/s10980-020-01173-8](https://doi.org/10.1007/s10980-020-01173-8)

## Examples

``` r
# read example hazard data
hazard_file_path <- "extdata/hazard.tif"
hazard <- terra::rast(system.file(hazard_file_path, package = "fireexposuR"))

# generate an example point
point_wkt <- "POINT (345000 5876000)"
point <- terra::vect(point_wkt, crs = hazard)

# compute exposure metric
exposure <- fire_exp(hazard)

# assess directional exposure
fire_exp_dir(exposure, point)
#> class       : SpatVector
#> geometry    : lines
#> dimensions  : 1080, 3  (geometries, attributes)
#> extent      : 330007.1, 359991.9, 5861008, 5890992  (xmin, xmax, ymin, ymax)
#> coord. ref. : NAD_1983_Transverse_Mercator
#> names       :   deg   seg viable
#> type        : <num> <chr>  <num>
#> values      :     1  seg1      0
#>                   1  seg2      1
#>                   1  seg3      1
#>               ...
```
