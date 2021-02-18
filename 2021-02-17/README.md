Meeting notes
================
Martin Schobben
17 February, 2021

A minimal example of a workflow for plotting timeseries in geology with
`tidyverse`.

Load `tidyverse`, your swiss army knife\!

``` r
library(tidyverse)
```

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
# library(readr) is already loaded with tidyverse
bonenburg <- read_csv(
   "https://raw.githubusercontent.com/MartinSchobben/bonenburg/master/supplement/data/palynomorphs/Bonenburg_palyno.csv", 
   col_types = "d--ddddddd-"
   )
```

This data is now in a wide format.

| SampleID |      Wood |   Cuticle | Plant Tissue |       AOM |    pollen |    spores | Botryococcus |
| -------: | --------: | --------: | -----------: | --------: | --------: | --------: | -----------: |
|     3550 | 0.4681529 | 0.0000000 |    0.0000000 | 0.4617834 | 0.0254777 | 0.0191083 |    0.0000000 |
|     3400 | 0.9076923 | 0.0000000 |    0.0000000 | 0.0892308 | 0.0030769 | 0.0000000 |    0.0000000 |
|     3300 | 0.7436709 | 0.0063291 |    0.0348101 | 0.1708861 | 0.0000000 | 0.0031646 |    0.0000000 |
|     3200 | 0.6457680 | 0.0000000 |    0.0031348 | 0.2695925 | 0.0470219 | 0.0094044 |    0.0000000 |
|     3125 | 0.2101911 | 0.0000000 |    0.0000000 | 0.7420382 | 0.0286624 | 0.0000000 |    0.0095541 |
|     3100 | 0.4597015 | 0.0000000 |    0.0000000 | 0.4417910 | 0.0059701 | 0.0000000 |    0.0029851 |

# Reading xlsx workbook

You can also just load a xlsx workbook, if that is your preferred way of
working with data. For this you can use the package: `readxl`, which is
not loaded with the `tidyverse` package.

``` r
library(readxl) 
tb_xlsx <- read_xlsx(readxl_example("clippy.xlsx"))
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
long_format <- bonenburg %>% 
   pivot_longer(-SampleID)
```

| SampleID | name         |     value |
| -------: | :----------- | --------: |
|     3550 | Wood         | 0.4681529 |
|     3550 | Cuticle      | 0.0000000 |
|     3550 | Plant Tissue | 0.0000000 |
|     3550 | AOM          | 0.4617834 |
|     3550 | pollen       | 0.0254777 |
|     3550 | spores       | 0.0191083 |

# Plotting data for exploration with ggplot2

Finally, we can plot this data with `ggplot2` and display all the
variable by making use of `facet_grid()`.

``` r
ggplot(long_format, aes(y = SampleID , x = value)) +
   geom_point() +
   facet_grid(cols = vars(name)) +
   theme_classic()
```

![](README_files/figure-gfm/ggplot-1.png)<!-- -->

<img src="bonenburg.png" width="680" />
