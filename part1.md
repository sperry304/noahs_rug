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
phone_letters <- 
  tibble(
    letter = LETTERS,
    number = c(2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5, 6, 6, 6, 
                7, 7, 7, 7, 8, 8, 8, 9, 9, 9, 9)
  )

phone_letters
```

    ## # A tibble: 26 × 2
    ##    letter number
    ##    <chr>   <dbl>
    ##  1 A           2
    ##  2 B           2
    ##  3 C           2
    ##  4 D           3
    ##  5 E           3
    ##  6 F           3
    ##  7 G           4
    ##  8 H           4
    ##  9 I           4
    ## 10 J           5
    ## # ℹ 16 more rows

``` r
df <- 
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
df
```

    ## # A tibble: 8,260 × 9
    ##    customerid name   address citystatezip birthdate  phone timezone   lat   long
    ##         <dbl> <chr>  <chr>   <chr>        <date>     <chr> <chr>    <dbl>  <dbl>
    ##  1       1001 Jacqu… 105N E… Manhattan, … 1958-01-23 315-… America…  40.7  -74.0
    ##  2       1002 Julie… 185-1 … Brooklyn, N… 1956-12-03 680-… America…  40.7  -73.9
    ##  3       1003 Chris… 174-28… Jamaica, NY… 2001-09-20 315-… America…  40.7  -73.8
    ##  4       1004 Chris… 102 Mo… Bronx, NY 1… 1959-07-10 516-… America…  40.8  -73.9
    ##  5       1005 Jeffr… 17 St … Manhattan, … 1988-09-08 838-… America…  40.7  -74.0
    ##  6       1006 Emma … 86-34 … Ozone Park,… 1984-02-05 315-… America…  40.7  -73.9
    ##  7       1007 Chris… 1488 9… Baton Rouge… 1967-01-18 318-… America…  30.4  -91.1
    ##  8       1008 Chris… 2428 M… Ogden, UT 8… 1956-07-04 435-… America…  41.2 -112. 
    ##  9       1009 Mathe… 448 S … Brooklyn, N… 1976-04-18 914-… America…  40.7  -74.0
    ## 10       1010 Carol… 4370 N… Sully Squar… 1951-10-30 948-… America…  38.9  -77.4
    ## # ℹ 8,250 more rows

``` r
df |> 
  mutate(
    name = str_to_upper(str_remove_all(name, ".+ ")),
    phone = str_remove_all(phone, "-")
  ) |> 
  filter(str_length(name) == 10) |> 
  select(name, phone) |> 
  separate(name, into = letters[1:11], sep = "") |> 
  select(-a) |> 
  pivot_longer(-phone, names_to = "position", values_to = "letter") |> 
  left_join(phone_letters, by = "letter") |> 
  group_by(phone) |> 
  summarize(
    name = paste_c(letter),
    number = paste_c(number)
  ) |> 
  filter(phone == number)
```

    ## # A tibble: 1 × 3
    ##   phone      name       number    
    ##   <chr>      <chr>      <chr>     
    ## 1 8266362286 TANNENBAUM 8266362286
