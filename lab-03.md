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

…
