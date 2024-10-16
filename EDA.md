EDA
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

``` r
weather_df = read_csv(file = "./data/weather_df.csv") %>%
  mutate(
    month = lubridate::floor_date(date, unit = "month")
  )
```

    ## Rows: 2190 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): name, id
    ## dbl  (3): prcp, tmax, tmin
    ## date (1): date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
weather_df
```

    ## # A tibble: 2,190 × 7
    ##    name           id          date        prcp  tmax  tmin month     
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>    
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01
    ## # ℹ 2,180 more rows

## ‘group_by’

``` r
weather_df %>%
  group_by(name, month)
```

    ## # A tibble: 2,190 × 7
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month     
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>    
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01
    ## # ℹ 2,180 more rows

## counting things

count month observations

``` r
weather_df %>%
  group_by (name, month) %>%
  summarize(n_obs = n())
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 72 × 3
    ## # Groups:   name [3]
    ##    name           month      n_obs
    ##    <chr>          <date>     <int>
    ##  1 CentralPark_NY 2021-01-01    31
    ##  2 CentralPark_NY 2021-02-01    28
    ##  3 CentralPark_NY 2021-03-01    31
    ##  4 CentralPark_NY 2021-04-01    30
    ##  5 CentralPark_NY 2021-05-01    31
    ##  6 CentralPark_NY 2021-06-01    30
    ##  7 CentralPark_NY 2021-07-01    31
    ##  8 CentralPark_NY 2021-08-01    31
    ##  9 CentralPark_NY 2021-09-01    30
    ## 10 CentralPark_NY 2021-10-01    31
    ## # ℹ 62 more rows

we can use ‘count()’

``` r
weather_df %>%
  count(name, month, name = "n_obs")
```

    ## # A tibble: 72 × 3
    ##    name           month      n_obs
    ##    <chr>          <date>     <int>
    ##  1 CentralPark_NY 2021-01-01    31
    ##  2 CentralPark_NY 2021-02-01    28
    ##  3 CentralPark_NY 2021-03-01    31
    ##  4 CentralPark_NY 2021-04-01    30
    ##  5 CentralPark_NY 2021-05-01    31
    ##  6 CentralPark_NY 2021-06-01    30
    ##  7 CentralPark_NY 2021-07-01    31
    ##  8 CentralPark_NY 2021-08-01    31
    ##  9 CentralPark_NY 2021-09-01    30
    ## 10 CentralPark_NY 2021-10-01    31
    ## # ℹ 62 more rows

**NEVER** use base R’s ‘table’

the professor hates this code and we should not use it

``` r
weather_df %>%
  pull(month) %>%
  table()
```

    ## .
    ## 2021-01-01 2021-02-01 2021-03-01 2021-04-01 2021-05-01 2021-06-01 2021-07-01 
    ##         93         84         93         90         93         90         93 
    ## 2021-08-01 2021-09-01 2021-10-01 2021-11-01 2021-12-01 2022-01-01 2022-02-01 
    ##         93         90         93         90         93         93         84 
    ## 2022-03-01 2022-04-01 2022-05-01 2022-06-01 2022-07-01 2022-08-01 2022-09-01 
    ##         93         90         93         90         93         93         90 
    ## 2022-10-01 2022-11-01 2022-12-01 
    ##         93         90         93

other helpful counters

``` r
weather_df%>%
  group_by(month)%>%
  summarize(
    n_obs=n(),
    n_days=n_distinct(date)
  )
```

    ## # A tibble: 24 × 3
    ##    month      n_obs n_days
    ##    <date>     <int>  <int>
    ##  1 2021-01-01    93     31
    ##  2 2021-02-01    84     28
    ##  3 2021-03-01    93     31
    ##  4 2021-04-01    90     30
    ##  5 2021-05-01    93     31
    ##  6 2021-06-01    90     30
    ##  7 2021-07-01    93     31
    ##  8 2021-08-01    93     31
    ##  9 2021-09-01    90     30
    ## 10 2021-10-01    93     31
    ## # ℹ 14 more rows

## a digression on 2x2 tables

``` r
weather_df %>%
  filter(name != "Molokai_HI") %>%
  mutate(
    cold=case_when(
     tmax <  5 ~ "cold",
      tmax >= 5 ~ "not_cold",
      TRUE      ~ ""
    )
  )%>%
  group_by(name, cold) %>%
  summarize(count = n())
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 5 × 3
    ## # Groups:   name [2]
    ##   name           cold       count
    ##   <chr>          <chr>      <int>
    ## 1 CentralPark_NY "cold"        96
    ## 2 CentralPark_NY "not_cold"   634
    ## 3 Waterhole_WA   ""            16
    ## 4 Waterhole_WA   "cold"       319
    ## 5 Waterhole_WA   "not_cold"   395

``` r
weather_df %>%
  filter(name != "Molokai_HI") %>%
  mutate(
    cold=case_when(
      tmax <  5 ~ "cold",
      tmax >= 5 ~ "not_cold",
      TRUE      ~ ""
    )
  )%>%
  janitor::tabyl(name, cold)
```

    ##            name cold not_cold emptystring_
    ##  CentralPark_NY   96      634            0
    ##    Waterhole_WA  319      395           16

## general summaries

the na.rm helps us drop blank ones

you can do lots of summaries.

``` r
weather_df %>%
  group_by(name, month)%>%
  summarize(
    mean_tmax = mean(tmax, na.rm=TRUE),
    mean_prcp = mean(prcp, na.rm = TRUE),
    median_tmin = median(tmin, no.rm=TRUE)
  )
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 72 × 5
    ## # Groups:   name [3]
    ##    name           month      mean_tmax mean_prcp median_tmin
    ##    <chr>          <date>         <dbl>     <dbl>       <dbl>
    ##  1 CentralPark_NY 2021-01-01      4.27      18.9       -0.5 
    ##  2 CentralPark_NY 2021-02-01      3.87      46.6       -1.85
    ##  3 CentralPark_NY 2021-03-01     12.3       28.0        5   
    ##  4 CentralPark_NY 2021-04-01     17.6       22.8        8.05
    ##  5 CentralPark_NY 2021-05-01     22.1       35.7       11.1 
    ##  6 CentralPark_NY 2021-06-01     28.1       22.2       18.0 
    ##  7 CentralPark_NY 2021-07-01     28.4       90.9       21.1 
    ##  8 CentralPark_NY 2021-08-01     28.8       84.5       22.2 
    ##  9 CentralPark_NY 2021-09-01     24.8       84.9       17.5 
    ## 10 CentralPark_NY 2021-10-01     19.9       43.1       13.9 
    ## # ℹ 62 more rows

this is a dataframe!

``` r
weather_df %>%
  group_by(name, month)%>%
  summarize(
    mean_tmax = mean(tmax, na.rm=TRUE),
    mean_prcp = mean(prcp, na.rm = TRUE),
    median_tmin = median(tmin, no.rm=TRUE)
  ) %>%
  ggplot(aes(x=month, y=mean_tmax, color=name)) +
           geom_point()+
            geom_line()
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

<img src="EDA_files/figure-gfm/unnamed-chunk-10-1.png" width="90%" />

suppose you want to summarize many columns

``` r
weather_df%>%
  group_by(name, month)%>%
  summarize(across(prcp:tmin, mean))
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 72 × 5
    ## # Groups:   name [3]
    ##    name           month       prcp  tmax  tmin
    ##    <chr>          <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY 2021-01-01  18.9  4.27 -1.15
    ##  2 CentralPark_NY 2021-02-01  46.6  3.87 -1.39
    ##  3 CentralPark_NY 2021-03-01  28.0 12.3   3.1 
    ##  4 CentralPark_NY 2021-04-01  22.8 17.6   7.48
    ##  5 CentralPark_NY 2021-05-01  35.7 22.1  12.2 
    ##  6 CentralPark_NY 2021-06-01  22.2 28.1  18.9 
    ##  7 CentralPark_NY 2021-07-01  90.9 28.4  20.6 
    ##  8 CentralPark_NY 2021-08-01  84.5 28.8  21.8 
    ##  9 CentralPark_NY 2021-09-01  84.9 24.8  17.8 
    ## 10 CentralPark_NY 2021-10-01  43.1 19.9  13.4 
    ## # ℹ 62 more rows

reminder: sometimes your results are easier to read in another format

the knitr code below makes the output neater

``` r
weather_df %>%
  group_by(name, month)%>%
  summarize(mean_tmax = mean(tmax)) %>%
  pivot_wider(
    names_from=name,
    values_from=mean_tmax
  )%>%
knitr::kable(digits=1)
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

| month      | CentralPark_NY | Molokai_HI | Waterhole_WA |
|:-----------|---------------:|-----------:|-------------:|
| 2021-01-01 |            4.3 |       27.6 |          0.8 |
| 2021-02-01 |            3.9 |       26.4 |         -0.8 |
| 2021-03-01 |           12.3 |       25.9 |           NA |
| 2021-04-01 |           17.6 |       26.6 |          6.1 |
| 2021-05-01 |           22.1 |       28.6 |          8.2 |
| 2021-06-01 |           28.1 |       29.6 |         15.3 |
| 2021-07-01 |           28.4 |       30.0 |         17.3 |
| 2021-08-01 |           28.8 |       29.5 |         17.2 |
| 2021-09-01 |           24.8 |       29.7 |         12.6 |
| 2021-10-01 |           19.9 |       29.1 |          5.5 |
| 2021-11-01 |           11.5 |       28.8 |          3.5 |
| 2021-12-01 |            9.6 |       26.2 |           NA |
| 2022-01-01 |            2.9 |       26.6 |          3.6 |
| 2022-02-01 |            7.7 |       26.8 |           NA |
| 2022-03-01 |           12.0 |       27.7 |          3.4 |
| 2022-04-01 |           15.8 |       27.7 |          2.5 |
| 2022-05-01 |           22.3 |         NA |          5.8 |
| 2022-06-01 |           26.1 |       29.2 |         11.1 |
| 2022-07-01 |           30.7 |       29.5 |         15.9 |
| 2022-08-01 |           30.5 |       30.7 |           NA |
| 2022-09-01 |           24.9 |       30.4 |         15.2 |
| 2022-10-01 |           17.4 |       29.2 |         11.9 |
| 2022-11-01 |           14.0 |       28.0 |          2.1 |
| 2022-12-01 |            6.8 |       27.3 |           NA |

## group_by and mutate

this let me take the mean by group

``` r
weather_df%>%
  group_by(name) %>%
  mutate(
    mean_tmax = mean(tmax, no.rm=TRUE),
    centered_tmax = tmax - mean_tmax
  )%>%
  ggplot(aes(x=date, y=centered_tmax+tmax, color=name))+
  geom_point()
```

    ## Warning: Removed 1460 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

<img src="EDA_files/figure-gfm/unnamed-chunk-13-1.png" width="90%" />

what about window functions?

we used this code to group by name then month, then rank within each
month the days according to tmax, then filtered by coldest day

``` r
weather_df%>%
  group_by(name,month)%>%
  mutate(temp_rank = min_rank(tmax))%>%
  filter(temp_rank == 1)
```

    ## # A tibble: 92 × 8
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month      temp_rank
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>         <int>
    ##  1 CentralPark_NY USW00094728 2021-01-29     0  -3.8  -9.9 2021-01-01         1
    ##  2 CentralPark_NY USW00094728 2021-02-08     0  -1.6  -8.2 2021-02-01         1
    ##  3 CentralPark_NY USW00094728 2021-03-02     0   0.6  -6   2021-03-01         1
    ##  4 CentralPark_NY USW00094728 2021-04-02     0   3.9  -2.1 2021-04-01         1
    ##  5 CentralPark_NY USW00094728 2021-05-29   117  10.6   8.3 2021-05-01         1
    ##  6 CentralPark_NY USW00094728 2021-05-30   226  10.6   8.3 2021-05-01         1
    ##  7 CentralPark_NY USW00094728 2021-06-11     0  20.6  16.7 2021-06-01         1
    ##  8 CentralPark_NY USW00094728 2021-06-12     0  20.6  16.7 2021-06-01         1
    ##  9 CentralPark_NY USW00094728 2021-07-03    86  18.9  15   2021-07-01         1
    ## 10 CentralPark_NY USW00094728 2021-08-04     0  24.4  19.4 2021-08-01         1
    ## # ℹ 82 more rows

using below to flip the order of the ranking

``` r
weather_df%>%
  group_by(name,month)%>%
  mutate(temp_rank = min_rank(desc(tmax)))
```

    ## # A tibble: 2,190 × 8
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month      temp_rank
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>         <int>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01        17
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01         1
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01        19
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01         9
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01        13
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01        14
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01        14
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01        20
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01        20
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01        14
    ## # ℹ 2,180 more rows

lag

copies the observation and shifts down by one

``` r
weather_df%>%
  group_by(name)%>%
  mutate(lag_temp = lag(tmax))
```

    ## # A tibble: 2,190 × 8
    ## # Groups:   name [3]
    ##    name           id          date        prcp  tmax  tmin month      lag_temp
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>        <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01     NA  
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01      4.4
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01     10.6
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01      3.3
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01      6.1
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01      5.6
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01      5  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01      5  
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01      2.8
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01      2.8
    ## # ℹ 2,180 more rows

``` r
weather_df%>%
  group_by(name)%>%
  mutate(lag_temp = lag(tmax,5))
```

    ## # A tibble: 2,190 × 8
    ## # Groups:   name [3]
    ##    name           id          date        prcp  tmax  tmin month      lag_temp
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>        <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01     NA  
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01     NA  
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01     NA  
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01     NA  
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01     NA  
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01      4.4
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01     10.6
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01      3.3
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01      6.1
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01      5.6
    ## # ℹ 2,180 more rows

lags are helpful when you’re trying to predict things like how much was
today different from yesterday’s temp?

in this instance i pulled the temp change from one day to the next, then
summarized to get the max temp change for each location

``` r
weather_df%>%
  group_by(name)%>%
  mutate(temp_change = tmax - lag(tmax))%>%
  summarize(
    temp_change_max = max(temp_change, na.rm = TRUE),
    temp_change_sd = sd(temp_change, na.rm=TRUE)
  )
```

    ## # A tibble: 3 × 3
    ##   name           temp_change_max temp_change_sd
    ##   <chr>                    <dbl>          <dbl>
    ## 1 CentralPark_NY            12.2           4.43
    ## 2 Molokai_HI                 5.6           1.24
    ## 3 Waterhole_WA              11.1           3.04
