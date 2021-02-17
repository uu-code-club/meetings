Meeting notes
================
Martin Schobben
17 February, 2021

``` r
# Default knitr options
knitr::opts_chunk$set(
   dpi = 300,
   digits = 2,
   results = 'asis'
   )
```

A minimal example of a workflow for plotting timeseries in geology with
`tidyverse`.

Load `tidyverse`, your swiss army knife\!

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.6     ✓ dplyr   1.0.4
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

# Reading csv files with readr

Here is an example of loading palynofloral dataset that I published some
time ago. This is in csv format and can be best loaded with the package
`readr` which is part of `tidyverse`. I defined here which columns I
want to load; discarding columns with `col_type = "-"`, and assigning
classes by `col_type = "d"`. The `"d"` stands for double precision
numeric. However, the `readr` package normally assigns classes quite
well, without any further help.

``` r
# install.packages("readr")
# library(readr) is already loaded in tidyverse
bonenburg <- read_csv(
   "https://raw.githubusercontent.com/MartinSchobben/bonenburg/master/supplement/data/palynomorphs/Bonenburg_palyno.csv", 
   col_types = "dddddddddd-"
   )
```

This data is now in a wide format.

| SampleID | terrestrial | aquatic |      Wood |   Cuticle | Plant Tissue |       AOM |    pollen |    spores | Botryococcus |
| -------: | ----------: | ------: | --------: | --------: | -----------: | --------: | --------: | --------: | -----------: |
|     3550 |        78.5 |    21.5 | 0.4681529 | 0.0000000 |    0.0000000 | 0.4617834 | 0.0254777 | 0.0191083 |    0.0000000 |
|     3400 |        88.2 |    11.8 | 0.9076923 | 0.0000000 |    0.0000000 | 0.0892308 | 0.0030769 | 0.0000000 |    0.0000000 |
|     3300 |        88.3 |    11.7 | 0.7436709 | 0.0063291 |    0.0348101 | 0.1708861 | 0.0000000 | 0.0031646 |    0.0000000 |
|     3200 |        81.0 |    19.0 | 0.6457680 | 0.0000000 |    0.0031348 | 0.2695925 | 0.0470219 | 0.0094044 |    0.0000000 |
|     3125 |        87.0 |    13.0 | 0.2101911 | 0.0000000 |    0.0000000 | 0.7420382 | 0.0286624 | 0.0000000 |    0.0095541 |
|     3100 |        93.5 |     6.5 | 0.4597015 | 0.0000000 |    0.0000000 | 0.4417910 | 0.0059701 | 0.0000000 |    0.0029851 |
|     3060 |        82.8 |    17.2 | 0.4766355 | 0.0000000 |    0.0031153 | 0.3208723 | 0.0591900 | 0.0186916 |    0.0000000 |
|     3040 |        90.6 |     9.4 | 0.1988131 | 0.0000000 |    0.0000000 | 0.6498516 | 0.0593472 | 0.0029674 |    0.0059347 |
|     3000 |        81.9 |    18.1 | 0.2424242 | 0.0090909 |    0.0060606 | 0.6303030 | 0.0181818 | 0.0030303 |    0.0000000 |
|     2970 |          NA |      NA | 0.6168224 | 0.0093458 |    0.0000000 | 0.3146417 | 0.0311526 | 0.0062305 |    0.0093458 |
|     2950 |        86.3 |    13.7 | 0.8286604 | 0.0809969 |    0.0000000 | 0.0591900 | 0.0062305 | 0.0186916 |    0.0062305 |
|     2850 |        78.0 |    22.0 | 0.9478528 | 0.0092025 |    0.0030675 | 0.0153374 | 0.0030675 | 0.0153374 |    0.0030675 |
|     2550 |        96.7 |     3.3 | 0.8513932 | 0.0061920 |    0.0000000 | 0.0557276 | 0.0371517 | 0.0433437 |    0.0061920 |
|     2450 |        91.5 |     8.5 | 0.8943894 | 0.0099010 |    0.0033003 | 0.0297030 | 0.0099010 | 0.0528053 |    0.0000000 |
|     2085 |        95.1 |     4.9 | 0.8498403 | 0.0191693 |    0.0031949 | 0.0511182 | 0.0223642 | 0.0543131 |    0.0000000 |
|     1800 |        96.1 |     3.9 | 0.8543046 | 0.0298013 |    0.0000000 | 0.0430464 | 0.0000000 | 0.0662252 |    0.0033113 |
|     1650 |        94.4 |     5.6 | 0.8794788 | 0.0097720 |    0.0032573 | 0.0749186 | 0.0065147 | 0.0260586 |    0.0000000 |
|     1450 |        95.7 |     4.3 | 0.8633540 | 0.0279503 |    0.0093168 | 0.0372671 | 0.0217391 | 0.0372671 |    0.0000000 |
|     1150 |        96.5 |     3.5 | 0.6766667 | 0.0700000 |    0.0100000 | 0.1333333 | 0.0233333 | 0.0766667 |    0.0066667 |
|     1100 |        48.8 |    51.2 | 0.4110032 | 0.0485437 |    0.0000000 | 0.1909385 | 0.0388350 | 0.1326861 |    0.1423948 |
|     1075 |        96.6 |     3.4 | 0.7614379 | 0.0196078 |    0.0032680 | 0.0620915 | 0.0261438 | 0.1111111 |    0.0000000 |
|     1051 |        59.3 |    40.7 | 0.7368421 | 0.0154799 |    0.0030960 | 0.0959752 | 0.0371517 | 0.0712074 |    0.0247678 |
|     1042 |        46.5 |    53.5 | 0.2484277 | 0.0125786 |    0.0062893 | 0.2295597 | 0.2484277 | 0.0031447 |    0.0000000 |
|     1036 |        60.7 |    39.3 | 0.1793313 | 0.0151976 |    0.0091185 | 0.5075988 | 0.1458967 | 0.0060790 |    0.0060790 |
|     1018 |        72.8 |    27.2 | 0.3147541 | 0.0098361 |    0.0032787 | 0.3409836 | 0.2131148 | 0.0032787 |    0.0065574 |
|     1012 |        81.7 |    18.3 | 0.3130699 | 0.0243161 |    0.0182371 | 0.4012158 | 0.1732523 | 0.0030395 |    0.0030395 |
|     1000 |        74.0 |    26.0 | 0.3304094 | 0.0146199 |    0.0029240 | 0.4181287 | 0.1315789 | 0.0029240 |    0.0058480 |
|      980 |        85.1 |    14.9 | 0.4304636 | 0.0099338 |    0.0066225 | 0.2913907 | 0.1887417 | 0.0165563 |    0.0132450 |
|      952 |        72.3 |    27.7 | 0.4342508 | 0.0244648 |    0.0030581 | 0.2018349 | 0.2018349 | 0.0214067 |    0.0152905 |
|      920 |        82.3 |    17.7 | 0.3876923 | 0.0123077 |    0.0061538 | 0.2184615 | 0.2676923 | 0.0000000 |    0.0184615 |
|      900 |        72.3 |    27.7 | 0.3750000 | 0.0159574 |    0.0079787 | 0.2047872 | 0.2872340 | 0.0026596 |    0.0292553 |
|      750 |        81.3 |    18.7 | 0.6717791 | 0.0245399 |    0.0030675 | 0.1257669 | 0.1349693 | 0.0122699 |    0.0214724 |
|      730 |          NA |      NA | 0.6288344 | 0.0030675 |    0.0153374 | 0.2300613 | 0.0460123 | 0.0061350 |    0.0153374 |
|      610 |        65.3 |    34.7 | 0.5693642 | 0.0028902 |    0.0028902 | 0.1387283 | 0.1502890 | 0.0086705 |    0.0028902 |
|      600 |          NA |      NA | 0.5228758 | 0.0000000 |    0.0098039 | 0.3921569 | 0.0163399 | 0.0098039 |    0.0000000 |
|      520 |          NA |      NA | 0.8018293 | 0.0030488 |    0.0182927 | 0.1006098 | 0.0335366 | 0.0000000 |    0.0091463 |
|      450 |        93.4 |     6.6 | 0.6770186 | 0.0155280 |    0.0062112 | 0.0559006 | 0.2204969 | 0.0217391 |    0.0031056 |
|      300 |        81.8 |    18.2 | 0.4460641 | 0.0087464 |    0.0087464 | 0.1807580 | 0.2536443 | 0.0058309 |    0.0087464 |
|      150 |        88.6 |    11.4 | 0.5000000 | 0.0246914 |    0.0216049 | 0.0740741 | 0.3333333 | 0.0092593 |    0.0061728 |
|        0 |        87.4 |    12.6 | 0.4625000 | 0.0093750 |    0.0062500 | 0.3750000 | 0.1000000 | 0.0031250 |    0.0062500 |

# Reading xlsx workbook

You can also just load a xlsx workbook, if that is your preferred way of
working with data. For this you can use the package: `readxl`, which is
not loaded with the `tidyverse` package.

``` r
library(readxl) 
tb_xlsx <- read_xlsx(readxl_example("clippy.xlsx"))
```

``` r
knitr::kable(tb_xlsx)
```

| name                 | value     |
| :------------------- | :-------- |
| Name                 | Clippy    |
| Species              | paperclip |
| Approx date of death | 39083     |
| Weight in grams      | 0.9       |

# Long format

After loading the data, first adjust your dataframe to a long format,
where every observation is just one variable with a label identifying
what type of data is associated with it. This can be done with the
function `pivot_longer()`.

``` r
# long format
long_format <- bonenburg %>% pivot_longer(-SampleID)
```

``` r
long_format %>% knitr::kable()
```

| SampleID | name         |      value |
| -------: | :----------- | ---------: |
|     3550 | terrestrial  | 78.5000000 |
|     3550 | aquatic      | 21.5000000 |
|     3550 | Wood         |  0.4681529 |
|     3550 | Cuticle      |  0.0000000 |
|     3550 | Plant Tissue |  0.0000000 |
|     3550 | AOM          |  0.4617834 |
|     3550 | pollen       |  0.0254777 |
|     3550 | spores       |  0.0191083 |
|     3550 | Botryococcus |  0.0000000 |
|     3400 | terrestrial  | 88.2000000 |
|     3400 | aquatic      | 11.8000000 |
|     3400 | Wood         |  0.9076923 |
|     3400 | Cuticle      |  0.0000000 |
|     3400 | Plant Tissue |  0.0000000 |
|     3400 | AOM          |  0.0892308 |
|     3400 | pollen       |  0.0030769 |
|     3400 | spores       |  0.0000000 |
|     3400 | Botryococcus |  0.0000000 |
|     3300 | terrestrial  | 88.3000000 |
|     3300 | aquatic      | 11.7000000 |
|     3300 | Wood         |  0.7436709 |
|     3300 | Cuticle      |  0.0063291 |
|     3300 | Plant Tissue |  0.0348101 |
|     3300 | AOM          |  0.1708861 |
|     3300 | pollen       |  0.0000000 |
|     3300 | spores       |  0.0031646 |
|     3300 | Botryococcus |  0.0000000 |
|     3200 | terrestrial  | 81.0000000 |
|     3200 | aquatic      | 19.0000000 |
|     3200 | Wood         |  0.6457680 |
|     3200 | Cuticle      |  0.0000000 |
|     3200 | Plant Tissue |  0.0031348 |
|     3200 | AOM          |  0.2695925 |
|     3200 | pollen       |  0.0470219 |
|     3200 | spores       |  0.0094044 |
|     3200 | Botryococcus |  0.0000000 |
|     3125 | terrestrial  | 87.0000000 |
|     3125 | aquatic      | 13.0000000 |
|     3125 | Wood         |  0.2101911 |
|     3125 | Cuticle      |  0.0000000 |
|     3125 | Plant Tissue |  0.0000000 |
|     3125 | AOM          |  0.7420382 |
|     3125 | pollen       |  0.0286624 |
|     3125 | spores       |  0.0000000 |
|     3125 | Botryococcus |  0.0095541 |
|     3100 | terrestrial  | 93.5000000 |
|     3100 | aquatic      |  6.5000000 |
|     3100 | Wood         |  0.4597015 |
|     3100 | Cuticle      |  0.0000000 |
|     3100 | Plant Tissue |  0.0000000 |
|     3100 | AOM          |  0.4417910 |
|     3100 | pollen       |  0.0059701 |
|     3100 | spores       |  0.0000000 |
|     3100 | Botryococcus |  0.0029851 |
|     3060 | terrestrial  | 82.8000000 |
|     3060 | aquatic      | 17.2000000 |
|     3060 | Wood         |  0.4766355 |
|     3060 | Cuticle      |  0.0000000 |
|     3060 | Plant Tissue |  0.0031153 |
|     3060 | AOM          |  0.3208723 |
|     3060 | pollen       |  0.0591900 |
|     3060 | spores       |  0.0186916 |
|     3060 | Botryococcus |  0.0000000 |
|     3040 | terrestrial  | 90.6000000 |
|     3040 | aquatic      |  9.4000000 |
|     3040 | Wood         |  0.1988131 |
|     3040 | Cuticle      |  0.0000000 |
|     3040 | Plant Tissue |  0.0000000 |
|     3040 | AOM          |  0.6498516 |
|     3040 | pollen       |  0.0593472 |
|     3040 | spores       |  0.0029674 |
|     3040 | Botryococcus |  0.0059347 |
|     3000 | terrestrial  | 81.9000000 |
|     3000 | aquatic      | 18.1000000 |
|     3000 | Wood         |  0.2424242 |
|     3000 | Cuticle      |  0.0090909 |
|     3000 | Plant Tissue |  0.0060606 |
|     3000 | AOM          |  0.6303030 |
|     3000 | pollen       |  0.0181818 |
|     3000 | spores       |  0.0030303 |
|     3000 | Botryococcus |  0.0000000 |
|     2970 | terrestrial  |         NA |
|     2970 | aquatic      |         NA |
|     2970 | Wood         |  0.6168224 |
|     2970 | Cuticle      |  0.0093458 |
|     2970 | Plant Tissue |  0.0000000 |
|     2970 | AOM          |  0.3146417 |
|     2970 | pollen       |  0.0311526 |
|     2970 | spores       |  0.0062305 |
|     2970 | Botryococcus |  0.0093458 |
|     2950 | terrestrial  | 86.3000000 |
|     2950 | aquatic      | 13.7000000 |
|     2950 | Wood         |  0.8286604 |
|     2950 | Cuticle      |  0.0809969 |
|     2950 | Plant Tissue |  0.0000000 |
|     2950 | AOM          |  0.0591900 |
|     2950 | pollen       |  0.0062305 |
|     2950 | spores       |  0.0186916 |
|     2950 | Botryococcus |  0.0062305 |
|     2850 | terrestrial  | 78.0000000 |
|     2850 | aquatic      | 22.0000000 |
|     2850 | Wood         |  0.9478528 |
|     2850 | Cuticle      |  0.0092025 |
|     2850 | Plant Tissue |  0.0030675 |
|     2850 | AOM          |  0.0153374 |
|     2850 | pollen       |  0.0030675 |
|     2850 | spores       |  0.0153374 |
|     2850 | Botryococcus |  0.0030675 |
|     2550 | terrestrial  | 96.7000000 |
|     2550 | aquatic      |  3.3000000 |
|     2550 | Wood         |  0.8513932 |
|     2550 | Cuticle      |  0.0061920 |
|     2550 | Plant Tissue |  0.0000000 |
|     2550 | AOM          |  0.0557276 |
|     2550 | pollen       |  0.0371517 |
|     2550 | spores       |  0.0433437 |
|     2550 | Botryococcus |  0.0061920 |
|     2450 | terrestrial  | 91.5000000 |
|     2450 | aquatic      |  8.5000000 |
|     2450 | Wood         |  0.8943894 |
|     2450 | Cuticle      |  0.0099010 |
|     2450 | Plant Tissue |  0.0033003 |
|     2450 | AOM          |  0.0297030 |
|     2450 | pollen       |  0.0099010 |
|     2450 | spores       |  0.0528053 |
|     2450 | Botryococcus |  0.0000000 |
|     2085 | terrestrial  | 95.1000000 |
|     2085 | aquatic      |  4.9000000 |
|     2085 | Wood         |  0.8498403 |
|     2085 | Cuticle      |  0.0191693 |
|     2085 | Plant Tissue |  0.0031949 |
|     2085 | AOM          |  0.0511182 |
|     2085 | pollen       |  0.0223642 |
|     2085 | spores       |  0.0543131 |
|     2085 | Botryococcus |  0.0000000 |
|     1800 | terrestrial  | 96.1000000 |
|     1800 | aquatic      |  3.9000000 |
|     1800 | Wood         |  0.8543046 |
|     1800 | Cuticle      |  0.0298013 |
|     1800 | Plant Tissue |  0.0000000 |
|     1800 | AOM          |  0.0430464 |
|     1800 | pollen       |  0.0000000 |
|     1800 | spores       |  0.0662252 |
|     1800 | Botryococcus |  0.0033113 |
|     1650 | terrestrial  | 94.4000000 |
|     1650 | aquatic      |  5.6000000 |
|     1650 | Wood         |  0.8794788 |
|     1650 | Cuticle      |  0.0097720 |
|     1650 | Plant Tissue |  0.0032573 |
|     1650 | AOM          |  0.0749186 |
|     1650 | pollen       |  0.0065147 |
|     1650 | spores       |  0.0260586 |
|     1650 | Botryococcus |  0.0000000 |
|     1450 | terrestrial  | 95.7000000 |
|     1450 | aquatic      |  4.3000000 |
|     1450 | Wood         |  0.8633540 |
|     1450 | Cuticle      |  0.0279503 |
|     1450 | Plant Tissue |  0.0093168 |
|     1450 | AOM          |  0.0372671 |
|     1450 | pollen       |  0.0217391 |
|     1450 | spores       |  0.0372671 |
|     1450 | Botryococcus |  0.0000000 |
|     1150 | terrestrial  | 96.5000000 |
|     1150 | aquatic      |  3.5000000 |
|     1150 | Wood         |  0.6766667 |
|     1150 | Cuticle      |  0.0700000 |
|     1150 | Plant Tissue |  0.0100000 |
|     1150 | AOM          |  0.1333333 |
|     1150 | pollen       |  0.0233333 |
|     1150 | spores       |  0.0766667 |
|     1150 | Botryococcus |  0.0066667 |
|     1100 | terrestrial  | 48.8000000 |
|     1100 | aquatic      | 51.2000000 |
|     1100 | Wood         |  0.4110032 |
|     1100 | Cuticle      |  0.0485437 |
|     1100 | Plant Tissue |  0.0000000 |
|     1100 | AOM          |  0.1909385 |
|     1100 | pollen       |  0.0388350 |
|     1100 | spores       |  0.1326861 |
|     1100 | Botryococcus |  0.1423948 |
|     1075 | terrestrial  | 96.6000000 |
|     1075 | aquatic      |  3.4000000 |
|     1075 | Wood         |  0.7614379 |
|     1075 | Cuticle      |  0.0196078 |
|     1075 | Plant Tissue |  0.0032680 |
|     1075 | AOM          |  0.0620915 |
|     1075 | pollen       |  0.0261438 |
|     1075 | spores       |  0.1111111 |
|     1075 | Botryococcus |  0.0000000 |
|     1051 | terrestrial  | 59.3000000 |
|     1051 | aquatic      | 40.7000000 |
|     1051 | Wood         |  0.7368421 |
|     1051 | Cuticle      |  0.0154799 |
|     1051 | Plant Tissue |  0.0030960 |
|     1051 | AOM          |  0.0959752 |
|     1051 | pollen       |  0.0371517 |
|     1051 | spores       |  0.0712074 |
|     1051 | Botryococcus |  0.0247678 |
|     1042 | terrestrial  | 46.5000000 |
|     1042 | aquatic      | 53.5000000 |
|     1042 | Wood         |  0.2484277 |
|     1042 | Cuticle      |  0.0125786 |
|     1042 | Plant Tissue |  0.0062893 |
|     1042 | AOM          |  0.2295597 |
|     1042 | pollen       |  0.2484277 |
|     1042 | spores       |  0.0031447 |
|     1042 | Botryococcus |  0.0000000 |
|     1036 | terrestrial  | 60.7000000 |
|     1036 | aquatic      | 39.3000000 |
|     1036 | Wood         |  0.1793313 |
|     1036 | Cuticle      |  0.0151976 |
|     1036 | Plant Tissue |  0.0091185 |
|     1036 | AOM          |  0.5075988 |
|     1036 | pollen       |  0.1458967 |
|     1036 | spores       |  0.0060790 |
|     1036 | Botryococcus |  0.0060790 |
|     1018 | terrestrial  | 72.8000000 |
|     1018 | aquatic      | 27.2000000 |
|     1018 | Wood         |  0.3147541 |
|     1018 | Cuticle      |  0.0098361 |
|     1018 | Plant Tissue |  0.0032787 |
|     1018 | AOM          |  0.3409836 |
|     1018 | pollen       |  0.2131148 |
|     1018 | spores       |  0.0032787 |
|     1018 | Botryococcus |  0.0065574 |
|     1012 | terrestrial  | 81.7000000 |
|     1012 | aquatic      | 18.3000000 |
|     1012 | Wood         |  0.3130699 |
|     1012 | Cuticle      |  0.0243161 |
|     1012 | Plant Tissue |  0.0182371 |
|     1012 | AOM          |  0.4012158 |
|     1012 | pollen       |  0.1732523 |
|     1012 | spores       |  0.0030395 |
|     1012 | Botryococcus |  0.0030395 |
|     1000 | terrestrial  | 74.0000000 |
|     1000 | aquatic      | 26.0000000 |
|     1000 | Wood         |  0.3304094 |
|     1000 | Cuticle      |  0.0146199 |
|     1000 | Plant Tissue |  0.0029240 |
|     1000 | AOM          |  0.4181287 |
|     1000 | pollen       |  0.1315789 |
|     1000 | spores       |  0.0029240 |
|     1000 | Botryococcus |  0.0058480 |
|      980 | terrestrial  | 85.1000000 |
|      980 | aquatic      | 14.9000000 |
|      980 | Wood         |  0.4304636 |
|      980 | Cuticle      |  0.0099338 |
|      980 | Plant Tissue |  0.0066225 |
|      980 | AOM          |  0.2913907 |
|      980 | pollen       |  0.1887417 |
|      980 | spores       |  0.0165563 |
|      980 | Botryococcus |  0.0132450 |
|      952 | terrestrial  | 72.3000000 |
|      952 | aquatic      | 27.7000000 |
|      952 | Wood         |  0.4342508 |
|      952 | Cuticle      |  0.0244648 |
|      952 | Plant Tissue |  0.0030581 |
|      952 | AOM          |  0.2018349 |
|      952 | pollen       |  0.2018349 |
|      952 | spores       |  0.0214067 |
|      952 | Botryococcus |  0.0152905 |
|      920 | terrestrial  | 82.3000000 |
|      920 | aquatic      | 17.7000000 |
|      920 | Wood         |  0.3876923 |
|      920 | Cuticle      |  0.0123077 |
|      920 | Plant Tissue |  0.0061538 |
|      920 | AOM          |  0.2184615 |
|      920 | pollen       |  0.2676923 |
|      920 | spores       |  0.0000000 |
|      920 | Botryococcus |  0.0184615 |
|      900 | terrestrial  | 72.3000000 |
|      900 | aquatic      | 27.7000000 |
|      900 | Wood         |  0.3750000 |
|      900 | Cuticle      |  0.0159574 |
|      900 | Plant Tissue |  0.0079787 |
|      900 | AOM          |  0.2047872 |
|      900 | pollen       |  0.2872340 |
|      900 | spores       |  0.0026596 |
|      900 | Botryococcus |  0.0292553 |
|      750 | terrestrial  | 81.3000000 |
|      750 | aquatic      | 18.7000000 |
|      750 | Wood         |  0.6717791 |
|      750 | Cuticle      |  0.0245399 |
|      750 | Plant Tissue |  0.0030675 |
|      750 | AOM          |  0.1257669 |
|      750 | pollen       |  0.1349693 |
|      750 | spores       |  0.0122699 |
|      750 | Botryococcus |  0.0214724 |
|      730 | terrestrial  |         NA |
|      730 | aquatic      |         NA |
|      730 | Wood         |  0.6288344 |
|      730 | Cuticle      |  0.0030675 |
|      730 | Plant Tissue |  0.0153374 |
|      730 | AOM          |  0.2300613 |
|      730 | pollen       |  0.0460123 |
|      730 | spores       |  0.0061350 |
|      730 | Botryococcus |  0.0153374 |
|      610 | terrestrial  | 65.3000000 |
|      610 | aquatic      | 34.7000000 |
|      610 | Wood         |  0.5693642 |
|      610 | Cuticle      |  0.0028902 |
|      610 | Plant Tissue |  0.0028902 |
|      610 | AOM          |  0.1387283 |
|      610 | pollen       |  0.1502890 |
|      610 | spores       |  0.0086705 |
|      610 | Botryococcus |  0.0028902 |
|      600 | terrestrial  |         NA |
|      600 | aquatic      |         NA |
|      600 | Wood         |  0.5228758 |
|      600 | Cuticle      |  0.0000000 |
|      600 | Plant Tissue |  0.0098039 |
|      600 | AOM          |  0.3921569 |
|      600 | pollen       |  0.0163399 |
|      600 | spores       |  0.0098039 |
|      600 | Botryococcus |  0.0000000 |
|      520 | terrestrial  |         NA |
|      520 | aquatic      |         NA |
|      520 | Wood         |  0.8018293 |
|      520 | Cuticle      |  0.0030488 |
|      520 | Plant Tissue |  0.0182927 |
|      520 | AOM          |  0.1006098 |
|      520 | pollen       |  0.0335366 |
|      520 | spores       |  0.0000000 |
|      520 | Botryococcus |  0.0091463 |
|      450 | terrestrial  | 93.4000000 |
|      450 | aquatic      |  6.6000000 |
|      450 | Wood         |  0.6770186 |
|      450 | Cuticle      |  0.0155280 |
|      450 | Plant Tissue |  0.0062112 |
|      450 | AOM          |  0.0559006 |
|      450 | pollen       |  0.2204969 |
|      450 | spores       |  0.0217391 |
|      450 | Botryococcus |  0.0031056 |
|      300 | terrestrial  | 81.8000000 |
|      300 | aquatic      | 18.2000000 |
|      300 | Wood         |  0.4460641 |
|      300 | Cuticle      |  0.0087464 |
|      300 | Plant Tissue |  0.0087464 |
|      300 | AOM          |  0.1807580 |
|      300 | pollen       |  0.2536443 |
|      300 | spores       |  0.0058309 |
|      300 | Botryococcus |  0.0087464 |
|      150 | terrestrial  | 88.6000000 |
|      150 | aquatic      | 11.4000000 |
|      150 | Wood         |  0.5000000 |
|      150 | Cuticle      |  0.0246914 |
|      150 | Plant Tissue |  0.0216049 |
|      150 | AOM          |  0.0740741 |
|      150 | pollen       |  0.3333333 |
|      150 | spores       |  0.0092593 |
|      150 | Botryococcus |  0.0061728 |
|        0 | terrestrial  | 87.4000000 |
|        0 | aquatic      | 12.6000000 |
|        0 | Wood         |  0.4625000 |
|        0 | Cuticle      |  0.0093750 |
|        0 | Plant Tissue |  0.0062500 |
|        0 | AOM          |  0.3750000 |
|        0 | pollen       |  0.1000000 |
|        0 | spores       |  0.0031250 |
|        0 | Botryococcus |  0.0062500 |

# Plotting data for exploration with ggplot2

Finally, we can plot this data with `ggplot2` and display all the
variable by making use of `facet_grid()`.

``` r
ggplot(long_format, aes(y = SampleID , x = value)) +
   geom_point() +
   facet_grid(cols = vars(name))
```

    ## Warning: Removed 8 rows containing missing values (geom_point).

![](README_files/figure-gfm/ggplot-1.png)<!-- -->
