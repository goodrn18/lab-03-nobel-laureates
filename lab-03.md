Lab 03 - Nobel laureates
================
Rachel Good
January 22, 2022

### Load packages and data

``` r
library(tidyverse) 
```

``` r
nobel <- read_csv("data/nobel.csv")
```

``` r
library(skimr)
skim(nobel)
```

|                                                  |       |
|:-------------------------------------------------|:------|
| Name                                             | nobel |
| Number of rows                                   | 935   |
| Number of columns                                | 26    |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |       |
| Column type frequency:                           |       |
| character                                        | 21    |
| Date                                             | 2     |
| numeric                                          | 3     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |       |
| Group variables                                  | None  |

Data summary

**Variable type: character**

| skim\_variable          | n\_missing | complete\_rate | min | max | empty | n\_unique | whitespace |
|:------------------------|-----------:|---------------:|----:|----:|------:|----------:|-----------:|
| firstname               |          0 |           1.00 |   2 |  59 |     0 |       720 |          0 |
| surname                 |         29 |           0.97 |   2 |  26 |     0 |       851 |          0 |
| category                |          0 |           1.00 |   5 |  10 |     0 |         6 |          0 |
| affiliation             |        250 |           0.73 |   4 | 110 |     0 |       303 |          0 |
| city                    |        255 |           0.73 |   4 |  27 |     0 |       185 |          0 |
| country                 |        254 |           0.73 |   3 |  14 |     0 |        27 |          0 |
| gender                  |          0 |           1.00 |   3 |   6 |     0 |         3 |          0 |
| born\_city              |         28 |           0.97 |   3 |  29 |     0 |       613 |          0 |
| born\_country           |         28 |           0.97 |   3 |  28 |     0 |        80 |          0 |
| born\_country\_code     |         28 |           0.97 |   2 |   2 |     0 |        77 |          0 |
| died\_city              |        327 |           0.65 |   4 |  29 |     0 |       303 |          0 |
| died\_country           |        321 |           0.66 |   3 |  16 |     0 |        48 |          0 |
| died\_country\_code     |        321 |           0.66 |   2 |   2 |     0 |        46 |          0 |
| overall\_motivation     |        918 |           0.02 |  55 | 114 |     0 |         7 |          0 |
| motivation              |          0 |           1.00 |  24 | 337 |     0 |       656 |          0 |
| born\_country\_original |         28 |           0.97 |   3 |  52 |     0 |       122 |          0 |
| born\_city\_original    |         28 |           0.97 |   3 |  36 |     0 |       616 |          0 |
| died\_country\_original |        321 |           0.66 |   3 |  35 |     0 |        52 |          0 |
| died\_city\_original    |        327 |           0.65 |   4 |  29 |     0 |       303 |          0 |
| city\_original          |        255 |           0.73 |   4 |  27 |     0 |       185 |          0 |
| country\_original       |        254 |           0.73 |   3 |  35 |     0 |        29 |          0 |

**Variable type: Date**

| skim\_variable | n\_missing | complete\_rate | min        | max        | median     | n\_unique |
|:---------------|-----------:|---------------:|:-----------|:-----------|:-----------|----------:|
| born\_date     |         33 |           0.96 | 1817-11-30 | 1997-07-12 | 1916-06-28 |       885 |
| died\_date     |        308 |           0.67 | 1903-11-01 | 2019-08-07 | 1983-03-09 |       616 |

**Variable type: numeric**

| skim\_variable | n\_missing | complete\_rate |    mean |     sd |   p0 |    p25 |  p50 |    p75 | p100 | hist  |
|:---------------|-----------:|---------------:|--------:|-------:|-----:|-------:|-----:|-------:|-----:|:------|
| id             |          0 |              1 |  475.12 | 277.83 |    1 |  234.5 |  470 |  716.5 |  969 | ▇▇▇▇▇ |
| year           |          0 |              1 | 1970.44 |  33.30 | 1901 | 1947.0 | 1976 | 1999.0 | 2018 | ▃▃▅▆▇ |
| share          |          0 |              1 |    1.99 |   0.94 |    1 |    1.0 |    2 |    3.0 |    4 | ▇▇▁▅▂ |

## Exercises

### Exercise 1

There are 935 observations and 26 variables in the nobel laureates
dataset.

``` r
spec(nobel)
```

    ## cols(
    ##   id = col_double(),
    ##   firstname = col_character(),
    ##   surname = col_character(),
    ##   year = col_double(),
    ##   category = col_character(),
    ##   affiliation = col_character(),
    ##   city = col_character(),
    ##   country = col_character(),
    ##   born_date = col_date(format = ""),
    ##   died_date = col_date(format = ""),
    ##   gender = col_character(),
    ##   born_city = col_character(),
    ##   born_country = col_character(),
    ##   born_country_code = col_character(),
    ##   died_city = col_character(),
    ##   died_country = col_character(),
    ##   died_country_code = col_character(),
    ##   overall_motivation = col_character(),
    ##   share = col_double(),
    ##   motivation = col_character(),
    ##   born_country_original = col_character(),
    ##   born_city_original = col_character(),
    ##   died_country_original = col_character(),
    ##   died_city_original = col_character(),
    ##   city_original = col_character(),
    ##   country_original = col_character()
    ## )

``` r
count(nobel)
```

    ## # A tibble: 1 x 1
    ##       n
    ##   <int>
    ## 1   935

``` r
nrow(nobel)
```

    ## [1] 935

``` r
ncol(nobel)
```

    ## [1] 26

### Exercise 2

``` r
nobel %>%
  mutate(nobel_living = country, gender, died_date) %>%
  filter(gender != "org",)
```

    ## # A tibble: 908 x 27
    ##       id firstname  surname  year category affiliation  city  country born_date 
    ##    <dbl> <chr>      <chr>   <dbl> <chr>    <chr>        <chr> <chr>   <date>    
    ##  1     1 Wilhelm C~ Röntgen  1901 Physics  Munich Univ~ Muni~ Germany 1845-03-27
    ##  2     2 Hendrik A. Lorentz  1902 Physics  Leiden Univ~ Leid~ Nether~ 1853-07-18
    ##  3     3 Pieter     Zeeman   1902 Physics  Amsterdam U~ Amst~ Nether~ 1865-05-25
    ##  4     4 Henri      Becque~  1903 Physics  École Polyt~ Paris France  1852-12-15
    ##  5     5 Pierre     Curie    1903 Physics  École munic~ Paris France  1859-05-15
    ##  6     6 Marie      Curie    1903 Physics  <NA>         <NA>  <NA>    1867-11-07
    ##  7     6 Marie      Curie    1911 Chemist~ Sorbonne Un~ Paris France  1867-11-07
    ##  8     8 Lord       Raylei~  1904 Physics  Royal Insti~ Lond~ United~ 1842-11-12
    ##  9     9 Philipp    Lenard   1905 Physics  Kiel Univer~ Kiel  Germany 1862-06-07
    ## 10    10 J.J.       Thomson  1906 Physics  University ~ Camb~ United~ 1856-12-18
    ## # ... with 898 more rows, and 18 more variables: died_date <date>,
    ## #   gender <chr>, born_city <chr>, born_country <chr>, born_country_code <chr>,
    ## #   died_city <chr>, died_country <chr>, died_country_code <chr>,
    ## #   overall_motivation <chr>, share <dbl>, motivation <chr>,
    ## #   born_country_original <chr>, born_city_original <chr>,
    ## #   died_country_original <chr>, died_city_original <chr>, city_original <chr>,
    ## #   country_original <chr>, nobel_living <chr>

### Exercise 3

Remove this text, and add your answer for Exercise 1 here. Add code
chunks as needed. Don’t forget to label your code chunk. Do not use
spaces in code chunk labels.

### Exercise 4

…

### Exercise 5

…

### Exercise 6

…
