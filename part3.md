Noah’s Rug, Part 3
================
Skip Perry
December 2024

``` r
options(dplyr.summarize.inform = FALSE)
library(tidyverse)
```

``` r
orders <- 
  "data/noahs-orders.csv" |> 
  vroom::vroom()

orders_items <- 
  "data/noahs-orders_items.csv" |> 
  vroom::vroom()

products <- 
  "data/noahs-products.csv" |> 
  vroom::vroom()

customers <- 
  "data/noahs-customers.csv" |> 
  vroom::vroom()
```

``` r
customers |> 
  filter(
    lubridate::year(birthdate) %in% seq(1915, 2011, by = 12),
    birthdate >= as.Date(paste(year(birthdate), 06, 22, sep = "-")),
    birthdate <= as.Date(paste(year(birthdate), 07, 22, sep = "-")),
    citystatezip == customers |> filter(phone == "332-274-4185") |> pull(citystatezip)
  ) |> 
  select(name, phone)
```

    ## # A tibble: 1 × 2
    ##   name          phone       
    ##   <chr>         <chr>       
    ## 1 Robert Morton 917-288-9635
