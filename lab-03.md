Lab 03 - Nobel laureates
================
Marq Schieber
2/14

### Load packages and data

``` r
library(tidyverse) 
```

``` r
nobel <- read_csv("data/nobel.csv")
```

## Exercises

### Exercise 1

``` r
count(nobel)
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   935

``` r
ncol(nobel)
```

    ## [1] 26

There are 935 observations and 26 variables. Each row represents one
observation.

There are some observations in this dataset that we will exclude from
our analysis to match the Buzzfeed results.

### Exercise 2

``` r
new_nobel <- nobel %>%
filter(country!='NA',
       gender!='org',
       is.na(died_date))
```

WHoops called the new data frame new_nobel.

### Exercise 3

``` r
new_nobel <- new_nobel %>%
  mutate(
    country_us = if_else(country == "USA", "USA", "Other")
  )
```

``` r
nobel_living_science <- new_nobel %>%
  filter(category %in% c("Physics", "Medicine", "Chemistry", "Economics"))
```

### Exercise 4

``` r
ggplot(data = nobel_living_science, aes(y = country_us)) +
  geom_bar() +
  facet_wrap(~ category)
```

![](lab-03_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Maybe I’m misunderstanding, but I don’t see how this visualization
relates to the buzzfeed article. This graph shows US winners vs. outside
foreign winners. The buzzfeed article was focused on solely US winners,
albeit those who were born in the US and those who immigrated here.

With that said, this article shows that a significant amount of
intellectual talent can be found outside of the United States. This adds
support to the article’s assertion that the US has much to gain (above
what it’s already gained) from importing researchers.

…

### Exercise 5

``` r
nobel_living_science <- nobel_living_science %>%
  mutate(
    born_country_us = if_else(born_country == "USA", "USA", "Other")
  )

nrow(filter(nobel_living_science, born_country_us == 'USA'))
```

    ## [1] 105

105 winners born in the US.

``` r
ggplot(data = nobel_living_science, aes(y = country_us, fill = born_country_us)) +
  geom_bar() +
  facet_wrap(~ category)
```

![](lab-03_files/figure-gfm/new%20bar%20chart-1.png)<!-- -->

The visualization shows that a large proportion of US nobel laureates
were born outside the United States. This suggests that immigration is
an important part of past US laureates and undoubtedbly future
laureates.

…

### Exercise 6

Germany and the UK are tied for most common.

``` r
filtered_nobel <- nobel_living_science %>%
filter(born_country_us=='Other',
       country_us=='USA')


arrange(count(filtered_nobel, born_country),desc(n))
```

    ## # A tibble: 21 × 2
    ##    born_country       n
    ##    <chr>          <int>
    ##  1 Germany            7
    ##  2 United Kingdom     7
    ##  3 China              5
    ##  4 Canada             4
    ##  5 Japan              3
    ##  6 Australia          2
    ##  7 Israel             2
    ##  8 Norway             2
    ##  9 Austria            1
    ## 10 Finland            1
    ## # … with 11 more rows

…
