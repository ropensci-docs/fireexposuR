# Changelog

## fireexposuR 1.2.0 (September 29th, 2025)

CRAN release: 2025-10-01

- Improvement of documentation and vignettes
- use of
  [`fire_exp_adjust()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_adjust.md)
  has been deprecated. The functionality has been added to
  [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
  instead.
- [`fire_exp()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp.md)
  now takes a numeric value for the transmission distance (`t_dist`).
  - parameter `tdist` will still work, but return a message to update
    code
- use of
  [`fire_exp_map_cont()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map_cont.md)
  and
  [`fire_exp_map_class()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map_class.md)
  have been deprecated. The functionality of these has been merged into
  a new function:
  [`fire_exp_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map.md)
- split
  [`fire_exp_extract_vis()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_vis.md)
  into two separate functions:
  [`fire_exp_extract_summary()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_summary.md),
  [`fire_exp_dir_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_map.md)
  to be consistent with other function naming and expected outputs in
  the package
- split
  [`fire_exp_dir_multi()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_multi.md)
  into two separate functions:
  [`fire_exp_dir_multi()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_multi.md)
  and
  [`fire_exp_dir_multi_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_multi_plot.md)
  to be consistent with other function naming and expected outputs in
  the package
- example data is now for a real location, allowing for more
  interpretation of results in the vignettes
- maps are now built with tmap library to improve basemaps and
  components

## fireexposuR 1.1.0 (January 31, 2025)

CRAN release: 2025-05-27

### Major updates

- added parameter customization options to
  [`fire_exp_dir()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir.md):
  - customize transect lengths with `t_lengths` parameter
  - customize number of transects with `interval` parameter
  - customize high exposure threshold with `thresh_exp` parameter
  - customize viable pathway threshold with `thresh_viable` parameter
- added option to use custom classification breaks to
  [`fire_exp_summary()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_summary.md),
  [`fire_exp_map_class()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_map_class.md),
  [`fire_exp_extract_vis()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_extract_vis.md),
  [`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)
  - all functions that use classes now automatically add a ‘Nil’ class
    for values that are exactly 0
- split `fire_exp_dir_vis()` into two separate functions:
  [`fire_exp_dir_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_plot.md),
  [`fire_exp_dir_map()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_dir_map.md)
- removed plotting option from
  [`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)
- added new function
  [`fire_exp_validate_plot()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate_plot.md)
  to visualize results from
  [`fire_exp_validate()`](https://docs.ropensci.org/fireexposuR/reference/fire_exp_validate.md)
- added options to add custom titles to functions that return plots or
  maps
- significant updates and additions to the documentation

## fireexposuR 1.0.1 (October 2024)

- fixes for issues that came up during initial tests using different
  data
- fixes for many other things that came up as I learned about R package
  development

## fireexposuR 1.0 (September 2024)

- the initial release to make the WIP code publicly available
- submission to ROpenSci
