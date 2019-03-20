Lab 08 Replication Notebook
================
Christopher Prener, Ph.D.
(March 20, 2019)

## Introduction

This is the replication notebook for Lab-08 from the course SOC
4650/5650: Introduction to GISc.

## Load Dependencies

The following code loads the package dependencies for our analysis:

``` r
# tidyverse packages
library(readr)      # csv tools

# spatial packages
library(sf)         # spatial data tools
```

    ## Linking to GEOS 3.6.1, GDAL 2.1.3, PROJ 4.9.3

``` r
library(tidycensus) # data wrangling
library(tigris)     # data wrangling
```

    ## To enable 
    ## caching of data, set `options(tigris_use_cache = TRUE)` in your R script or .Rprofile.

    ## 
    ## Attaching package: 'tigris'

    ## The following object is masked from 'package:graphics':
    ## 
    ##     plot

``` r
# other packages
library(here)       # file path tools
```

    ## here() starts at /Users/prenercg/Dropbox/Professional/Teaching/SOC 5650 - GIS/2018-Spring/Lecture-08/Lab-07/lab-07-replication

## Part 1

### Question 1

First, we’ll download and preview the variables using the
`load_variables()` function from `tidycensus`.

``` r
acs <- load_variables(2017, "acs5", cache = TRUE)
```

The variable represents “PUBLIC ASSISTANCE INCOME OR FOOD STAMPS/SNAP IN
THE PAST 12 MONTHS FOR HOUSEHOLDS”.

### Question 2

First, we’ll download the relevant ACS data using `get_acs()`. We get
the data for all counties by specifying `"county"` as the
geography:

``` r
snapCounties <- get_acs(geography = "county", year = 2016, state = 29, variable = "B19058_002", survey = "acs5")
```

    ## Getting data from the 2012-2016 5-year ACS

With those downloaded and written to an object in our global
environment, we can use `readr` to write them to a `.csv` file:

``` r
write_csv(snapCounties, here("data", "MO_SNAP_HouseholdsByCounty.csv"))
```

We now have the raw data saved to our `data/` folder.

### Question 6

First, we’ll download the relevant ACS data using `get_acs()`. We get
the data for tracts in St. Louis County by specifying `"tract"` as the
geography and `189` for the
county:

``` r
snapTracts <- get_acs(geography = "tract", year = 2016, state = 29, county = 189, variable = "B19058_002", survey = "acs5")
```

    ## Getting data from the 2012-2016 5-year ACS

With those downloaded and written to an object in our global
environment, we can use `readr` to write them to a `.csv` file:

``` r
write_csv(snapCounties, here("data", "SLC_SNAP_HouseholdsByTract.csv"))
```

We now have the raw data saved to our `data/` folder.

## Part 3

### Question 7

Now we’ll download geometric data to go along with our tabular data.
We’ll start by downloading the county boundary data and converting it
to an sf object. The video shows you how to do this process in two
steps:

``` r
moCounties <- counties(state = 29, cb = FALSE)
moCounties <- st_as_sf(moCounties)
```

However, there is now an option within `tigris` to do it in a single
call:

``` r
moCounties <- counties(state = 29, cb = FALSE, class = "sf")
```

    ## 
      |                                                                       
      |                                                                 |   0%
      |                                                                       
      |                                                                 |   1%
      |                                                                       
      |=                                                                |   1%
      |                                                                       
      |=                                                                |   2%
      |                                                                       
      |==                                                               |   2%
      |                                                                       
      |==                                                               |   3%
      |                                                                       
      |==                                                               |   4%
      |                                                                       
      |===                                                              |   4%
      |                                                                       
      |===                                                              |   5%
      |                                                                       
      |====                                                             |   5%
      |                                                                       
      |====                                                             |   6%
      |                                                                       
      |====                                                             |   7%
      |                                                                       
      |=====                                                            |   7%
      |                                                                       
      |=====                                                            |   8%
      |                                                                       
      |======                                                           |   8%
      |                                                                       
      |======                                                           |   9%
      |                                                                       
      |======                                                           |  10%
      |                                                                       
      |=======                                                          |  10%
      |                                                                       
      |=======                                                          |  11%
      |                                                                       
      |=======                                                          |  12%
      |                                                                       
      |========                                                         |  12%
      |                                                                       
      |========                                                         |  13%
      |                                                                       
      |=========                                                        |  13%
      |                                                                       
      |=========                                                        |  14%
      |                                                                       
      |=========                                                        |  15%
      |                                                                       
      |==========                                                       |  15%
      |                                                                       
      |==========                                                       |  16%
      |                                                                       
      |===========                                                      |  16%
      |                                                                       
      |===========                                                      |  17%
      |                                                                       
      |===========                                                      |  18%
      |                                                                       
      |============                                                     |  18%
      |                                                                       
      |============                                                     |  19%
      |                                                                       
      |=============                                                    |  19%
      |                                                                       
      |=============                                                    |  20%
      |                                                                       
      |=============                                                    |  21%
      |                                                                       
      |==============                                                   |  21%
      |                                                                       
      |==============                                                   |  22%
      |                                                                       
      |===============                                                  |  22%
      |                                                                       
      |===============                                                  |  23%
      |                                                                       
      |===============                                                  |  24%
      |                                                                       
      |================                                                 |  24%
      |                                                                       
      |================                                                 |  25%
      |                                                                       
      |=================                                                |  25%
      |                                                                       
      |=================                                                |  26%
      |                                                                       
      |=================                                                |  27%
      |                                                                       
      |==================                                               |  27%
      |                                                                       
      |==================                                               |  28%
      |                                                                       
      |===================                                              |  28%
      |                                                                       
      |===================                                              |  29%
      |                                                                       
      |===================                                              |  30%
      |                                                                       
      |====================                                             |  30%
      |                                                                       
      |====================                                             |  31%
      |                                                                       
      |====================                                             |  32%
      |                                                                       
      |=====================                                            |  32%
      |                                                                       
      |=====================                                            |  33%
      |                                                                       
      |======================                                           |  33%
      |                                                                       
      |======================                                           |  34%
      |                                                                       
      |======================                                           |  35%
      |                                                                       
      |=======================                                          |  35%
      |                                                                       
      |=======================                                          |  36%
      |                                                                       
      |========================                                         |  36%
      |                                                                       
      |========================                                         |  37%
      |                                                                       
      |========================                                         |  38%
      |                                                                       
      |=========================                                        |  38%
      |                                                                       
      |=========================                                        |  39%
      |                                                                       
      |==========================                                       |  39%
      |                                                                       
      |==========================                                       |  40%
      |                                                                       
      |==========================                                       |  41%
      |                                                                       
      |===========================                                      |  41%
      |                                                                       
      |===========================                                      |  42%
      |                                                                       
      |============================                                     |  42%
      |                                                                       
      |============================                                     |  43%
      |                                                                       
      |============================                                     |  44%
      |                                                                       
      |=============================                                    |  44%
      |                                                                       
      |=============================                                    |  45%
      |                                                                       
      |==============================                                   |  45%
      |                                                                       
      |==============================                                   |  46%
      |                                                                       
      |==============================                                   |  47%
      |                                                                       
      |===============================                                  |  47%
      |                                                                       
      |===============================                                  |  48%
      |                                                                       
      |================================                                 |  48%
      |                                                                       
      |================================                                 |  49%
      |                                                                       
      |================================                                 |  50%
      |                                                                       
      |=================================                                |  50%
      |                                                                       
      |=================================                                |  51%
      |                                                                       
      |=================================                                |  52%
      |                                                                       
      |==================================                               |  52%
      |                                                                       
      |==================================                               |  53%
      |                                                                       
      |===================================                              |  53%
      |                                                                       
      |===================================                              |  54%
      |                                                                       
      |===================================                              |  55%
      |                                                                       
      |====================================                             |  55%
      |                                                                       
      |====================================                             |  56%
      |                                                                       
      |=====================================                            |  56%
      |                                                                       
      |=====================================                            |  57%
      |                                                                       
      |=====================================                            |  58%
      |                                                                       
      |======================================                           |  58%
      |                                                                       
      |======================================                           |  59%
      |                                                                       
      |=======================================                          |  59%
      |                                                                       
      |=======================================                          |  60%
      |                                                                       
      |=======================================                          |  61%
      |                                                                       
      |========================================                         |  61%
      |                                                                       
      |========================================                         |  62%
      |                                                                       
      |=========================================                        |  62%
      |                                                                       
      |=========================================                        |  63%
      |                                                                       
      |=========================================                        |  64%
      |                                                                       
      |==========================================                       |  64%
      |                                                                       
      |==========================================                       |  65%
      |                                                                       
      |===========================================                      |  65%
      |                                                                       
      |===========================================                      |  66%
      |                                                                       
      |===========================================                      |  67%
      |                                                                       
      |============================================                     |  67%
      |                                                                       
      |============================================                     |  68%
      |                                                                       
      |=============================================                    |  68%
      |                                                                       
      |=============================================                    |  69%
      |                                                                       
      |=============================================                    |  70%
      |                                                                       
      |==============================================                   |  70%
      |                                                                       
      |==============================================                   |  71%
      |                                                                       
      |==============================================                   |  72%
      |                                                                       
      |===============================================                  |  72%
      |                                                                       
      |===============================================                  |  73%
      |                                                                       
      |================================================                 |  73%
      |                                                                       
      |================================================                 |  74%
      |                                                                       
      |================================================                 |  75%
      |                                                                       
      |=================================================                |  75%
      |                                                                       
      |=================================================                |  76%
      |                                                                       
      |==================================================               |  76%
      |                                                                       
      |==================================================               |  77%
      |                                                                       
      |==================================================               |  78%
      |                                                                       
      |===================================================              |  78%
      |                                                                       
      |===================================================              |  79%
      |                                                                       
      |====================================================             |  79%
      |                                                                       
      |====================================================             |  80%
      |                                                                       
      |====================================================             |  81%
      |                                                                       
      |=====================================================            |  81%
      |                                                                       
      |=====================================================            |  82%
      |                                                                       
      |======================================================           |  82%
      |                                                                       
      |======================================================           |  83%
      |                                                                       
      |======================================================           |  84%
      |                                                                       
      |=======================================================          |  84%
      |                                                                       
      |=======================================================          |  85%
      |                                                                       
      |========================================================         |  85%
      |                                                                       
      |========================================================         |  86%
      |                                                                       
      |========================================================         |  87%
      |                                                                       
      |=========================================================        |  87%
      |                                                                       
      |=========================================================        |  88%
      |                                                                       
      |==========================================================       |  88%
      |                                                                       
      |==========================================================       |  89%
      |                                                                       
      |==========================================================       |  90%
      |                                                                       
      |===========================================================      |  90%
      |                                                                       
      |===========================================================      |  91%
      |                                                                       
      |===========================================================      |  92%
      |                                                                       
      |============================================================     |  92%
      |                                                                       
      |============================================================     |  93%
      |                                                                       
      |=============================================================    |  93%
      |                                                                       
      |=============================================================    |  94%
      |                                                                       
      |=============================================================    |  95%
      |                                                                       
      |==============================================================   |  95%
      |                                                                       
      |==============================================================   |  96%
      |                                                                       
      |===============================================================  |  96%
      |                                                                       
      |===============================================================  |  97%
      |                                                                       
      |===============================================================  |  98%
      |                                                                       
      |================================================================ |  98%
      |                                                                       
      |================================================================ |  99%
      |                                                                       
      |=================================================================|  99%
      |                                                                       
      |=================================================================| 100%

Once downloaded, we’ll save the sf object as a `.shp`
    file:

``` r
st_write(moCounties, here("data", "MO_BOUNDARY_Counties.shp"))
```

    ## Writing layer `MO_BOUNDARY_Counties' to data source `/Users/prenercg/Dropbox/Professional/Teaching/SOC 5650 - GIS/2018-Spring/Lecture-08/Lab-07/lab-07-replication/data/MO_BOUNDARY_Counties.shp' using driver `ESRI Shapefile'
    ## features:       115
    ## fields:         17
    ## geometry type:  Multi Polygon

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1805075859 of field
    ## ALAND of feature 0 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1533067872 of field
    ## ALAND of feature 1 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1305376709 of field
    ## ALAND of feature 2 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1378706292 of field
    ## ALAND of feature 3 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2147833553 of field
    ## ALAND of feature 4 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1280463339 of field
    ## ALAND of feature 5 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1364869337 of field
    ## ALAND of feature 6 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1584391273 of field
    ## ALAND of feature 7 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2093947226 of field
    ## ALAND of feature 8 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1618104260 of field
    ## ALAND of feature 9 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1457243589 of field
    ## ALAND of feature 10 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1397247188 of field
    ## ALAND of feature 11 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1417462632 of field
    ## ALAND of feature 12 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1775580802 of field
    ## ALAND of feature 13 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1871276777 of field
    ## ALAND of feature 14 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1804141831 of field
    ## ALAND of feature 15 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2107295721 of field
    ## ALAND of feature 16 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1534730291 of field
    ## ALAND of feature 17 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1425177705 of field
    ## ALAND of feature 18 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1127383741 of field
    ## ALAND of feature 19 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1736428536 of field
    ## ALAND of feature 20 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1087765673 of field
    ## ALAND of feature 21 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1201848048 of field
    ## ALAND of feature 22 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 121059168 of field
    ## AWATER of feature 22 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1201192534 of field
    ## ALAND of feature 23 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1968056216 of field
    ## ALAND of feature 24 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1534813236 of field
    ## ALAND of feature 25 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1088302649 of field
    ## ALAND of feature 26 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1056782089 of field
    ## ALAND of feature 27 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1014013381 of field
    ## ALAND of feature 28 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1216727530 of field
    ## ALAND of feature 29 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1120696556 of field
    ## ALAND of feature 30 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1385765331 of field
    ## ALAND of feature 31 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1929467320 of field
    ## ALAND of feature 32 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1131627484 of field
    ## ALAND of feature 33 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1473200437 of field
    ## ALAND of feature 34 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1799185021 of field
    ## ALAND of feature 35 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1980627554 of field
    ## ALAND of feature 36 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1956716996 of field
    ## ALAND of feature 37 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2389734943 of field
    ## ALAND of feature 38 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1945539735 of field
    ## ALAND of feature 39 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1966257795 of field
    ## ALAND of feature 40 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1228905197 of field
    ## ALAND of feature 41 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1600366924 of field
    ## ALAND of feature 42 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1250217889 of field
    ## ALAND of feature 43 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1653653979 of field
    ## ALAND of feature 44 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2045561037 of field
    ## ALAND of feature 45 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1400578411 of field
    ## ALAND of feature 46 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1571017654 of field
    ## ALAND of feature 47 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1344215195 of field
    ## ALAND of feature 48 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1451493058 of field
    ## ALAND of feature 49 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1198369598 of field
    ## ALAND of feature 50 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1923108650 of field
    ## ALAND of feature 51 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1627640379 of field
    ## ALAND of feature 52 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1292800498 of field
    ## ALAND of feature 53 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1307147388 of field
    ## ALAND of feature 54 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 690523564 of field
    ## ALAND of feature 55 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1739898251 of field
    ## ALAND of feature 56 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1032907243 of field
    ## ALAND of feature 57 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2015659215 of field
    ## ALAND of feature 58 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1646005965 of field
    ## ALAND of feature 59 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1104350547 of field
    ## ALAND of feature 60 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1308052630 of field
    ## ALAND of feature 61 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1630498740 of field
    ## ALAND of feature 62 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1110070560 of field
    ## ALAND of feature 63 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1565835130 of field
    ## ALAND of feature 64 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2132125025 of field
    ## ALAND of feature 65 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1416985287 of field
    ## ALAND of feature 66 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1272776756 of field
    ## ALAND of feature 67 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1469362053 of field
    ## ALAND of feature 68 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1315122130 of field
    ## ALAND of feature 69 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1765780148 of field
    ## ALAND of feature 70 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1792880227 of field
    ## ALAND of feature 71 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1462738176 of field
    ## ALAND of feature 72 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1029881206 of field
    ## ALAND of feature 73 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1678264468 of field
    ## ALAND of feature 74 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2271326596 of field
    ## ALAND of feature 75 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1638012282 of field
    ## ALAND of feature 76 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1594303526 of field
    ## ALAND of feature 77 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1074911081 of field
    ## ALAND of feature 78 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2161546514 of field
    ## ALAND of feature 79 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1700722348 of field
    ## ALAND of feature 80 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1622776606 of field
    ## ALAND of feature 81 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 3049104544 of field
    ## ALAND of feature 82 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1547854691 of field
    ## ALAND of feature 83 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1749069385 of field
    ## ALAND of feature 84 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1314052192 of field
    ## ALAND of feature 85 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1498398267 of field
    ## ALAND of feature 86 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1065995413 of field
    ## ALAND of feature 87 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 795916406 of field
    ## ALAND of feature 88 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2401562305 of field
    ## ALAND of feature 89 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2075118128 of field
    ## ALAND of feature 90 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1458775344 of field
    ## ALAND of feature 91 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1297230230 of field
    ## ALAND of feature 92 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2599883711 of field
    ## ALAND of feature 93 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1228575201 of field
    ## ALAND of feature 94 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1269123308 of field
    ## ALAND of feature 95 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1130968300 of field
    ## ALAND of feature 96 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1339843854 of field
    ## ALAND of feature 97 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1170437420 of field
    ## ALAND of feature 98 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1085096688 of field
    ## ALAND of feature 99 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1799061775 of field
    ## ALAND of feature 100 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2140348948 of field
    ## ALAND of feature 101 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1823463548 of field
    ## ALAND of feature 102 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 125579915 of field
    ## AWATER of feature 102 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1949640578 of field
    ## ALAND of feature 103 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1748192193 of field
    ## ALAND of feature 104 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1175427726 of field
    ## ALAND of feature 105 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1766949639 of field
    ## ALAND of feature 106 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1091311254 of field
    ## ALAND of feature 107 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1401369327 of field
    ## ALAND of feature 108 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1677414414 of field
    ## ALAND of feature 109 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1275622233 of field
    ## ALAND of feature 110 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 160458044 of field
    ## ALAND of feature 111 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 2167013739 of field
    ## ALAND of feature 112 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1747828876 of field
    ## ALAND of feature 113 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 1699028737 of field
    ## ALAND of feature 114 not successfully written. Possibly due to too larger
    ## number with respect to field width

    ## Warning in CPL_write_ogr(obj, dsn, layer, driver,
    ## as.character(dataset_options), : GDAL Message 1: Value 136351823 of field
    ## AWATER of feature 114 not successfully written. Possibly due to too larger
    ## number with respect to field width

We now have the raw data saved to our `data/` folder.

### Question 8

Now we’ll download geometric data to go along with our tabular data.
We’ll start by downloading the tract boundary data for St. Louis
County and converting it to an sf object:

``` r
slcTracts <- tracts(state = 29, county = 189, cb = FALSE)
```

    ## 
      |                                                                       
      |                                                                 |   0%
      |                                                                       
      |                                                                 |   1%
      |                                                                       
      |=                                                                |   1%
      |                                                                       
      |=                                                                |   2%
      |                                                                       
      |==                                                               |   2%
      |                                                                       
      |==                                                               |   3%
      |                                                                       
      |==                                                               |   4%
      |                                                                       
      |===                                                              |   4%
      |                                                                       
      |===                                                              |   5%
      |                                                                       
      |====                                                             |   5%
      |                                                                       
      |====                                                             |   6%
      |                                                                       
      |====                                                             |   7%
      |                                                                       
      |=====                                                            |   7%
      |                                                                       
      |=====                                                            |   8%
      |                                                                       
      |======                                                           |   9%
      |                                                                       
      |======                                                           |  10%
      |                                                                       
      |=======                                                          |  10%
      |                                                                       
      |=======                                                          |  11%
      |                                                                       
      |========                                                         |  12%
      |                                                                       
      |========                                                         |  13%
      |                                                                       
      |=========                                                        |  13%
      |                                                                       
      |=========                                                        |  14%
      |                                                                       
      |=========                                                        |  15%
      |                                                                       
      |==========                                                       |  15%
      |                                                                       
      |==========                                                       |  16%
      |                                                                       
      |===========                                                      |  16%
      |                                                                       
      |===========                                                      |  17%
      |                                                                       
      |===========                                                      |  18%
      |                                                                       
      |============                                                     |  18%
      |                                                                       
      |============                                                     |  19%
      |                                                                       
      |=============                                                    |  19%
      |                                                                       
      |=============                                                    |  20%
      |                                                                       
      |=============                                                    |  21%
      |                                                                       
      |==============                                                   |  21%
      |                                                                       
      |==============                                                   |  22%
      |                                                                       
      |===============                                                  |  22%
      |                                                                       
      |===============                                                  |  23%
      |                                                                       
      |===============                                                  |  24%
      |                                                                       
      |================                                                 |  24%
      |                                                                       
      |================                                                 |  25%
      |                                                                       
      |=================                                                |  25%
      |                                                                       
      |=================                                                |  26%
      |                                                                       
      |=================                                                |  27%
      |                                                                       
      |==================                                               |  27%
      |                                                                       
      |==================                                               |  28%
      |                                                                       
      |===================                                              |  29%
      |                                                                       
      |===================                                              |  30%
      |                                                                       
      |====================                                             |  30%
      |                                                                       
      |====================                                             |  31%
      |                                                                       
      |=====================                                            |  32%
      |                                                                       
      |=====================                                            |  33%
      |                                                                       
      |======================                                           |  33%
      |                                                                       
      |======================                                           |  34%
      |                                                                       
      |======================                                           |  35%
      |                                                                       
      |=======================                                          |  35%
      |                                                                       
      |=======================                                          |  36%
      |                                                                       
      |========================                                         |  36%
      |                                                                       
      |========================                                         |  37%
      |                                                                       
      |========================                                         |  38%
      |                                                                       
      |=========================                                        |  38%
      |                                                                       
      |=========================                                        |  39%
      |                                                                       
      |==========================                                       |  39%
      |                                                                       
      |==========================                                       |  40%
      |                                                                       
      |==========================                                       |  41%
      |                                                                       
      |===========================                                      |  41%
      |                                                                       
      |===========================                                      |  42%
      |                                                                       
      |============================                                     |  42%
      |                                                                       
      |============================                                     |  43%
      |                                                                       
      |============================                                     |  44%
      |                                                                       
      |=============================                                    |  44%
      |                                                                       
      |=============================                                    |  45%
      |                                                                       
      |==============================                                   |  46%
      |                                                                       
      |==============================                                   |  47%
      |                                                                       
      |===============================                                  |  47%
      |                                                                       
      |===============================                                  |  48%
      |                                                                       
      |================================                                 |  48%
      |                                                                       
      |================================                                 |  49%
      |                                                                       
      |================================                                 |  50%
      |                                                                       
      |=================================                                |  50%
      |                                                                       
      |=================================                                |  51%
      |                                                                       
      |==================================                               |  52%
      |                                                                       
      |==================================                               |  53%
      |                                                                       
      |===================================                              |  53%
      |                                                                       
      |===================================                              |  54%
      |                                                                       
      |===================================                              |  55%
      |                                                                       
      |====================================                             |  55%
      |                                                                       
      |====================================                             |  56%
      |                                                                       
      |=====================================                            |  56%
      |                                                                       
      |=====================================                            |  57%
      |                                                                       
      |=====================================                            |  58%
      |                                                                       
      |======================================                           |  58%
      |                                                                       
      |======================================                           |  59%
      |                                                                       
      |=======================================                          |  59%
      |                                                                       
      |=======================================                          |  60%
      |                                                                       
      |=======================================                          |  61%
      |                                                                       
      |========================================                         |  61%
      |                                                                       
      |========================================                         |  62%
      |                                                                       
      |=========================================                        |  62%
      |                                                                       
      |=========================================                        |  63%
      |                                                                       
      |=========================================                        |  64%
      |                                                                       
      |==========================================                       |  64%
      |                                                                       
      |==========================================                       |  65%
      |                                                                       
      |===========================================                      |  65%
      |                                                                       
      |===========================================                      |  66%
      |                                                                       
      |===========================================                      |  67%
      |                                                                       
      |============================================                     |  67%
      |                                                                       
      |============================================                     |  68%
      |                                                                       
      |=============================================                    |  69%
      |                                                                       
      |=============================================                    |  70%
      |                                                                       
      |==============================================                   |  70%
      |                                                                       
      |==============================================                   |  71%
      |                                                                       
      |===============================================                  |  72%
      |                                                                       
      |===============================================                  |  73%
      |                                                                       
      |================================================                 |  73%
      |                                                                       
      |================================================                 |  74%
      |                                                                       
      |================================================                 |  75%
      |                                                                       
      |=================================================                |  75%
      |                                                                       
      |=================================================                |  76%
      |                                                                       
      |==================================================               |  76%
      |                                                                       
      |==================================================               |  77%
      |                                                                       
      |==================================================               |  78%
      |                                                                       
      |===================================================              |  78%
      |                                                                       
      |===================================================              |  79%
      |                                                                       
      |====================================================             |  79%
      |                                                                       
      |====================================================             |  80%
      |                                                                       
      |====================================================             |  81%
      |                                                                       
      |=====================================================            |  81%
      |                                                                       
      |=====================================================            |  82%
      |                                                                       
      |======================================================           |  82%
      |                                                                       
      |======================================================           |  83%
      |                                                                       
      |======================================================           |  84%
      |                                                                       
      |=======================================================          |  84%
      |                                                                       
      |=======================================================          |  85%
      |                                                                       
      |========================================================         |  86%
      |                                                                       
      |========================================================         |  87%
      |                                                                       
      |=========================================================        |  87%
      |                                                                       
      |=========================================================        |  88%
      |                                                                       
      |==========================================================       |  88%
      |                                                                       
      |==========================================================       |  89%
      |                                                                       
      |==========================================================       |  90%
      |                                                                       
      |===========================================================      |  90%
      |                                                                       
      |===========================================================      |  91%
      |                                                                       
      |============================================================     |  92%
      |                                                                       
      |============================================================     |  93%
      |                                                                       
      |=============================================================    |  93%
      |                                                                       
      |=============================================================    |  94%
      |                                                                       
      |==============================================================   |  95%
      |                                                                       
      |==============================================================   |  96%
      |                                                                       
      |===============================================================  |  96%
      |                                                                       
      |===============================================================  |  97%
      |                                                                       
      |===============================================================  |  98%
      |                                                                       
      |================================================================ |  98%
      |                                                                       
      |================================================================ |  99%
      |                                                                       
      |=================================================================|  99%
      |                                                                       
      |=================================================================| 100%

``` r
slcTracts <- st_as_sf(slcTracts)
```

Once downloaded, we’ll save the sf object as a `.shp`
    file:

``` r
st_write(slcTracts, here("data", "SLC_DEMOS_Tracts.shp"))
```

    ## Writing layer `SLC_DEMOS_Tracts' to data source `/Users/prenercg/Dropbox/Professional/Teaching/SOC 5650 - GIS/2018-Spring/Lecture-08/Lab-07/lab-07-replication/data/SLC_DEMOS_Tracts.shp' using driver `ESRI Shapefile'
    ## features:       199
    ## fields:         12
    ## geometry type:  Polygon

We now have the raw data saved to our `data/` folder.
