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
nobel_living <- nobel %>%
  filter(gender != "org",
         !is.na(country),
         is.na(died_date))

nrow(nobel_living)
```

    ## [1] 228

### Exercise 3

Excluding the category of economics, a large portion of nobel laureates
originate outside of the United States, supporting the Buzzfeed article
headline about the importance of immigration to American Science.

``` r
nobel_living <- nobel_living %>%
  mutate(
    country_us = if_else(country == "USA", "USA", "Other")
  )
```

``` r
nobel_living_science <- nobel_living %>%
  filter(category %in% c("Physics", "Medicine", "Chemistry", "Economics"))
```

``` r
nobel_living_science %>% 
  ggplot(aes(x = country_us)) +
  geom_bar() +
  facet_wrap(~category) +
  coord_flip() +
  labs(title = "Nobel Laureates by Category", subtitle = "A look at country origin",
       x = "Country of Origin", y = "Number of Laureates")
```

![](lab-03_files/figure-gfm/exercise3-1.png)<!-- -->

### Exercise 4

``` r
nobel_living <- nobel_living %>% 
  mutate(
    born_country_us = if_else(born_country == "USA", "USA", "Other")
)
```

…

### Exercise 5

``` r
nobel_living %>% 
  ggplot(aes(x = born_country_us)) +
  geom_bar() +
  facet_wrap(~category) +
  coord_flip() +
  labs(title = "Nobel Laureates by Category", subtitle = "A look at country origin",
       x = "Country of Origin", y = "Number of Laureates")
```

![](lab-03_files/figure-gfm/exercise%205-1.png)<!-- --> …

### Exercise 6

``` r
nobel_living %>% 
  filter(
    country == "USA", 
    born_country != "USA") 
```

    ## # A tibble: 45 x 28
    ##       id firstname surname   year category affiliation  city  country born_date 
    ##    <dbl> <chr>     <chr>    <dbl> <chr>    <chr>        <chr> <chr>   <date>    
    ##  1    68 Chen Ning Yang      1957 Physics  Institute f~ Prin~ USA     1922-09-22
    ##  2    69 Tsung-Dao Lee       1957 Physics  Columbia Un~ New ~ USA     1926-11-24
    ##  3    97 Leo       Esaki     1973 Physics  IBM Thomas ~ York~ USA     1925-03-12
    ##  4    98 Ivar      Giaever   1973 Physics  General Ele~ Sche~ USA     1929-04-05
    ##  5   111 Arno      Penzias   1978 Physics  Bell Labora~ Holm~ USA     1933-04-26
    ##  6   156 Horst L.  Störmer   1998 Physics  Columbia Un~ New ~ USA     1949-04-06
    ##  7   157 Daniel C. Tsui      1998 Physics  Princeton U~ Prin~ USA     1939-02-28
    ##  8   258 Roald     Hoffmann  1981 Chemist~ Cornell Uni~ Itha~ USA     1937-07-18
    ##  9   265 Yuan T.   Lee       1986 Chemist~ University ~ Berk~ USA     1936-11-19
    ## 10   270 Johann    Deisenh~  1988 Chemist~ University ~ Dall~ USA     1943-09-30
    ## # ... with 35 more rows, and 19 more variables: died_date <date>, gender <chr>,
    ## #   born_city <chr>, born_country <chr>, born_country_code <chr>,
    ## #   died_city <chr>, died_country <chr>, died_country_code <chr>,
    ## #   overall_motivation <chr>, share <dbl>, motivation <chr>,
    ## #   born_country_original <chr>, born_city_original <chr>,
    ## #   died_country_original <chr>, died_city_original <chr>, city_original <chr>,
    ## #   country_original <chr>, country_us <chr>, born_country_us <chr>

``` r
nobel_living %>%   
  count(born_country) %>% 
    arrange(n)
```

    ## # A tibble: 34 x 2
    ##    born_country     n
    ##    <chr>        <int>
    ##  1 Algeria          1
    ##  2 Belgium          1
    ##  3 Cyprus           1
    ##  4 Finland          1
    ##  5 Hungary          1
    ##  6 Ireland          1
    ##  7 Lithuania        1
    ##  8 Luxembourg       1
    ##  9 Mexico           1
    ## 10 Morocco          1
    ## # ... with 24 more rows

…
