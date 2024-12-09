Noah’s Rug, Part 4
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
orders |> 
  filter(
    lubridate::hour(ordered) == 4, 
    lubridate::hour(shipped) == 4
  ) |> 
  left_join(orders_items, by = "orderid") |> 
  left_join(products, by = "sku") |> 
  left_join(customers |> select(name, phone, customerid), by = "customerid") |> 
  filter(!str_detect(desc, "Cat Food")) |> 
  group_by(orderid, name, phone) |> 
  summarize(items = paste(desc, collapse = ", ")) |> 
  knitr::kable()
```

    ## `summarise()` has grouped output by 'orderid', 'name'. You can override using
    ## the `.groups` argument.

| orderid | name | phone | items |
|---:|:---|:---|:---|
| 4099 | Sabrina Harper | 516-834-8964 | Electric Yoyo, Dilled Salmon Sandwich, Transformers Hebrew Blocks |
| 12206 | Michelle Green | 212-330-7079 | Lego Stickers |
| 45075 | Renee Harmon | 607-231-3605 | Sesame Twist |
| 45589 | Stacey Case | 607-943-8298 | Dilled Herring Salad |
| 47077 | Renee Harmon | 607-231-3605 | Poppyseed Linzer Cookie |
| 49639 | Renee Harmon | 607-231-3605 | Raspberry Babka |
| 108606 | James Eaton | 838-958-4372 | Tonka Stickers |
| 114923 | Ruben Mendoza | 516-444-0468 | Noah’s Poster (yellow) |
| 116028 | Renee Harmon | 607-231-3605 | Sesame Twist |
| 139378 | Renee Harmon | 607-231-3605 | Sesame Bagel |
| 160820 | Kara Debra Rocha | 838-829-5254 | Noah’s Jewelry (amber) |
| 169418 | Justin Swanson | 585-605-0799 | Marvel Airplane |
| 173066 | Jonathan Davis | 631-615-3495 | Pickled Salmon Salad |
| 190672 | Anthony Porter | 516-609-3061 | Star Wars Blocks |
| 193364 | Sabrina Harper | 516-834-8964 | Handmade Crayons |
