Viz part 1
================
Juliet Rowe
2023-09-28

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggridges)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/Juliet/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-09-28 10:19:41.395166 (8.524)

    ## file min/max dates: 1869-01-01 / 2023-09-30

    ## using cached file: /Users/Juliet/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-09-28 10:19:47.549166 (3.83)

    ## file min/max dates: 1949-10-01 / 2023-09-30

    ## using cached file: /Users/Juliet/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-09-28 10:19:49.918441 (0.994)

    ## file min/max dates: 1999-09-01 / 2023-09-30

Let’s make a plot!

``` r
ggp_nyc_weather =
  weather_df |>
  filter(name=="CentralPark_NY") |>
  ggplot(aes(x=tmin, y=tmax))+geom_point()
```

# Fancy plot

``` r
ggplot(weather_df, aes(x=tmin, y=tmax, color=name)) +
  geom_point(aes(color=name), alpha = 0.3) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

Plot with facets

``` r
ggplot(weather_df, aes(x=tmin, y=tmax, color=name)) +
  geom_point(alpha=0.3)+
  geom_smooth() +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

let’s try a different plot. temps are boring

``` r
ggplot(weather_df, aes(x=date, y=tmax, color=name))+
  geom_point(aes(size=prcp), alph=0.3) +
  geom_smooth() + 
  facet_grid(. ~ name)
```

    ## Warning in geom_point(aes(size = prcp), alph = 0.3): Ignoring unknown
    ## parameters: `alph`

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values (`geom_point()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-6-1.png" width="90%" />

try assigning a specific color

``` r
weather_df |>
  filter(name== "CentralPark_NY") |>
  ggplot(aes(x=date, y=tmax)) +
  geom_point(color="blue")
```

<img src="viz_part1_files/figure-gfm/unnamed-chunk-7-1.png" width="90%" />

``` r
weather_df |>
  ggplot(aes(x=tmin, y=tmax))+
  geom_hex()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_binhex()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-8-1.png" width="90%" />

``` r
weather_df |>
  filter(name=="Molokai_HI") |>
  ggplot(aes(x=date, y=tmax))+
  geom_line(alpha=0.5)+
  geom_point(size=0.5)
```

<img src="viz_part1_files/figure-gfm/unnamed-chunk-9-1.png" width="90%" />

\##Univariate plotting

histogram

``` r
ggplot(weather_df, aes(x=tmax))+
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite values (`stat_bin()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-10-1.png" width="90%" />

let’s use a density plot

``` r
ggplot(weather_df, aes(x=tmax, fill=name)) +
  geom_density(alpha=0.3, adjust=0.75)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_density()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-11-1.png" width="90%" />

using boxplots

``` r
ggplot(weather_df, aes(y=tmax, x=name))+
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_boxplot()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-12-1.png" width="90%" />

violin plots?

``` r
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_violin(aes(fill = name), alpha = .5) + 
  stat_summary(fun = "median", color = "blue")
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_ydensity()`).

    ## Warning: Removed 17 rows containing non-finite values (`stat_summary()`).

    ## Warning: Removed 3 rows containing missing values (`geom_segment()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-13-1.png" width="90%" />

ridge plot

``` r
ggplot(weather_df, aes(x = tmax, y = name)) + 
  geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 1.54

    ## Warning: Removed 17 rows containing non-finite values
    ## (`stat_density_ridges()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-14-1.png" width="90%" />

## Saving plots

``` r
ggp_weather = 
  ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) 

ggsave("ggp_weather.pdf", ggp_weather, width = 8, height = 5)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

``` r
ggp_weather
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="viz_part1_files/figure-gfm/unnamed-chunk-16-1.png" width="90%" />
