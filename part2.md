Noah’s Rug, Part 1
================
Skip Perry
December 2024

``` r
options(dplyr.summarize.inform = FALSE)
library(tidyverse)

paste_c <- function(t) { paste0(t, collapse = "") }
```

``` r
orders <- 
  "data/noahs-orders.csv" |> 
  vroom::vroom()
```

    ## Rows: 213232 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl  (3): orderid, customerid, total
    ## lgl  (1): items
    ## dttm (2): ordered, shipped
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
orders_items <- 
  "data/noahs-orders_items.csv" |> 
  vroom::vroom()
```

    ## Rows: 426541 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): sku
    ## dbl (3): orderid, qty, unit_price
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
products <- 
  "data/noahs-products.csv" |> 
  vroom::vroom()
```

    ## Rows: 1278 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): sku, desc, dims_cm
    ## dbl (1): wholesale_cost
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
customers <- 
  "data/noahs-customers.csv" |> 
  vroom::vroom()
```

    ## Rows: 8260 Columns: 9
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): name, address, citystatezip, phone, timezone
    ## dbl  (3): customerid, lat, long
    ## date (1): birthdate
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
rug_cleaner_orderids <- 
  orders_items |> 
  inner_join(products |> filter(str_detect(desc, "Rug Cleaner")), "sku") |> 
  select(orderid) |> 
  distinct()

rug_cleaner_orderids
```

    ## # A tibble: 355 × 1
    ##    orderid
    ##      <dbl>
    ##  1    1475
    ##  2    2030
    ##  3    3084
    ##  4    4042
    ##  5    5174
    ##  6    6018
    ##  7    6040
    ##  8    7345
    ##  9    7459
    ## 10    7783
    ## # ℹ 345 more rows

``` r
jp <- 
  customers |> 
    filter(
      str_count(name, " ") == 1,
      str_sub(name, start = 1L, end = 1L) == "J",
      str_sub(str_remove(name, ".+ "), start = 1L, end = 1L) == "P"
    )

jp
```

    ## # A tibble: 70 × 9
    ##    customerid name   address citystatezip birthdate  phone timezone   lat   long
    ##         <dbl> <chr>  <chr>   <chr>        <date>     <chr> <chr>    <dbl>  <dbl>
    ##  1       1166 John … 806A E… Bronx, NY 1… 1968-04-26 716-… America…  40.9  -73.9
    ##  2       1195 Julie… 462 Re… Brooklyn, N… 1990-01-25 332-… America…  40.7  -73.9
    ##  3       1264 Justi… 402-1 … Staten Isla… 1988-08-04 585-… America…  40.6  -74.2
    ##  4       1298 Jacqu… 3657 N… Houston, TX… 1969-09-07 430-… America…  29.8  -95.4
    ##  5       1312 Judy … 974 E … Bronx, NY 1… 1973-05-16 516-… America…  40.8  -73.9
    ##  6       1410 Julie… 473 Vi… Staten Isla… 1953-07-31 845-… America…  40.6  -74.1
    ##  7       1475 Joshu… 100-75… Jamaica, NY… 1947-02-05 332-… America…  40.7  -73.8
    ##  8       1623 Joshu… 4955 1… Pearl City,… 1998-08-17 808-… Pacific…  21.4 -158. 
    ##  9       1765 Jessi… 91-45 … Woodhaven, … 1964-02-04 716-… America…  40.7  -73.8
    ## 10       1825 Jesse… 320 Re… Brooklyn, N… 1961-06-12 716-… America…  40.7  -73.9
    ## # ℹ 60 more rows

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
