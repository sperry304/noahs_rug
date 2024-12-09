Noah’s Rug, Part 2
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
rug_cleaner_orderids <- 
  orders_items |> 
  inner_join(products |> filter(str_detect(desc, "Rug Cleaner")), "sku") |> 
  select(orderid) |> 
  distinct()
```

``` r
jp <- 
  customers |> 
    filter(
      str_count(name, " ") == 1,
      str_sub(name, start = 1L, end = 1L) == "J",
      str_sub(str_remove(name, ".+ "), start = 1L, end = 1L) == "P"
    )
```

``` r
orders |> 
  inner_join(jp, "customerid") |> 
  inner_join(rug_cleaner_orderids, "orderid") |> 
  filter(lubridate::year(ordered) == 2017) |> 
  select(name, phone) |> 
  distinct()
```

    ## # A tibble: 1 × 2
    ##   name            phone       
    ##   <chr>           <chr>       
    ## 1 Joshua Peterson 332-274-4185
