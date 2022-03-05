Lab 2-2
================
Ollie L. Schilling
(March 05, 2022)

``` r
knitr::opts_chunk$set(cache = FALSE)
```

## Introduction

This notebook creates an sf object describing SNAP and Medicaid benefit
usage in Missouri.

## Dependencies

This notebook requires…

``` r
# tidyverse packages
library(dplyr)       # data wrangling
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# spatial packages
library(mapview)     # preview geometric data
```

    ## Warning: multiple methods tables found for 'direction'

    ## Warning: multiple methods tables found for 'gridDistance'

``` r
library(sf)          # spatial tools
```

    ## Linking to GEOS 3.8.1, GDAL 3.2.1, PROJ 7.2.1; sf_use_s2() is TRUE

``` r
library(tidycensus)  # demographic data
library(tigris)      # tiger/line data
```

    ## To enable 
    ## caching of data, set `options(tigris_use_cache = TRUE)` in your R script or .Rprofile.

    ## 
    ## Attaching package: 'tigris'

    ## The following object is masked from 'package:tidycensus':
    ## 
    ##     fips_codes

``` r
# other packages
library(here)       # file path tools
```

    ## here() starts at /Users/ollieschilling/Documents/GitHub/module-2-combine-sources

## Part 1

### Question 1

This notebook requires both SNAP data and Medicaid data, to be joined.

``` r
acs <- load_variables(2019, "acs5", cache = TRUE) 
```

## Variables to map are:

1.  PUBLIC ASSISTANCE INCOME OR FOOD STAMPS/SNAP IN THE PAST 12 MONTHS
    FOR HOUSEHOLDS
2.  MEDICAID/MEANS-TESTED PUBLIC COVERAGE BY SEX BY AGE

### Question 2

This code will download the SNAP necessary data for the specified
variables “B19058_001” and “B19058_002”. The data will include all
counties in Missouri.

``` r
snapCounties <- get_acs(geography = "county", year = 2019, state = 29, variables = c("B19058_001", "B19058_002"), output = "wide", geometry = TRUE )
```

    ## Getting data from the 2015-2019 5-year ACS

    ## Downloading feature geometry from the Census website.  To cache shapefiles for use in future sessions, set `options(tigris_use_cache = TRUE)`.

    ##   |                                                                              |                                                                      |   0%  |                                                                              |                                                                      |   1%  |                                                                              |=                                                                     |   1%  |                                                                              |=                                                                     |   2%  |                                                                              |==                                                                    |   3%  |                                                                              |===                                                                   |   4%  |                                                                              |====                                                                  |   5%  |                                                                              |====                                                                  |   6%  |                                                                              |=====                                                                 |   7%  |                                                                              |======                                                                |   9%  |                                                                              |=======                                                               |  10%  |                                                                              |========                                                              |  12%  |                                                                              |=========                                                             |  12%  |                                                                              |==========                                                            |  14%  |                                                                              |==========                                                            |  15%  |                                                                              |============                                                          |  17%  |                                                                              |=============                                                         |  19%  |                                                                              |==============                                                        |  20%  |                                                                              |==============                                                        |  21%  |                                                                              |===============                                                       |  21%  |                                                                              |===================                                                   |  27%  |                                                                              |=====================                                                 |  30%  |                                                                              |=====================                                                 |  31%  |                                                                              |======================                                                |  31%  |                                                                              |======================                                                |  32%  |                                                                              |=======================                                               |  33%  |                                                                              |========================                                              |  35%  |                                                                              |=========================                                             |  36%  |                                                                              |=============================                                         |  42%  |                                                                              |==============================                                        |  42%  |                                                                              |==============================                                        |  43%  |                                                                              |===============================                                       |  45%  |                                                                              |================================                                      |  45%  |                                                                              |================================                                      |  46%  |                                                                              |=================================                                     |  47%  |                                                                              |==================================                                    |  48%  |                                                                              |==================================                                    |  49%  |                                                                              |===================================                                   |  50%  |                                                                              |===================================                                   |  51%  |                                                                              |====================================                                  |  51%  |                                                                              |====================================                                  |  52%  |                                                                              |=====================================                                 |  53%  |                                                                              |=======================================                               |  55%  |                                                                              |=======================================                               |  56%  |                                                                              |=========================================                             |  58%  |                                                                              |=========================================                             |  59%  |                                                                              |==========================================                            |  59%  |                                                                              |==========================================                            |  60%  |                                                                              |==========================================                            |  61%  |                                                                              |===========================================                           |  61%  |                                                                              |===========================================                           |  62%  |                                                                              |=============================================                         |  64%  |                                                                              |=============================================                         |  65%  |                                                                              |==================================================                    |  72%  |                                                                              |====================================================                  |  74%  |                                                                              |====================================================                  |  75%  |                                                                              |=====================================================                 |  75%  |                                                                              |=====================================================                 |  76%  |                                                                              |==========================================================            |  83%  |                                                                              |==========================================================            |  84%  |                                                                              |============================================================          |  85%  |                                                                              |==============================================================        |  89%  |                                                                              |===============================================================       |  90%  |                                                                              |================================================================      |  91%  |                                                                              |================================================================      |  92%  |                                                                              |==================================================================    |  95%  |                                                                              |===================================================================   |  95%  |                                                                              |===================================================================   |  96%  |                                                                              |====================================================================  |  97%  |                                                                              |===================================================================== |  99%  |                                                                              |======================================================================| 100%

``` r
mapview(snapCounties)
```

![](Lab-2-2_files/figure-gfm/preview-snap-counties-1.png)<!-- --> ###
Question 3 This code will clean the snapCounties data.

``` r
snapCounties %>%
  rename(total_pop = B19058_001E,
    total_pop_mo = B19058_001M,
    snap = B19058_002E,
    snap_mo = B19058_002M) -> snapCounties
```

## Part 2

### Question 4

This code will download the necessary Medicaid data for the specified
variables “C27007_002” and “C27007_012”.

``` r
medicaidCounties <- get_acs(geography = "county", year = 2019, state = 29, variables = c("C27007_002", "C27007_012"), output = "wide", geometry = FALSE)
```

    ## Getting data from the 2015-2019 5-year ACS

### Question 5

This code will clean the medicaid data.

``` r
medicaidCounties %>%
  rename(
    medicaid_male = C27007_002E,
    medicaid_male_mo = C27007_002M,
    medicaid_female = C27007_012E,
    medicaid_female_mo = C27007_012M) %>%
  mutate(medicaid = medicaid_male + medicaid_female) %>% 
  select(-NAME) -> medicaidCounties
```

## Part 3

### Question 6

This code will join data sets together.

``` r
services <- left_join(snapCounties, medicaidCounties, by = "GEOID")
```

#Preview the data

``` r
mapview(services, zcol = "medicaid")
```

![](Lab-2-2_files/figure-gfm/preview-counties-1.png)<!-- --> A
description of the results.
