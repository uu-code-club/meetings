Meeting notes
================
Martin Schobben
17 March, 2021

<!-- README.md is generated from README.Rmd. Please edit that file -->

``` r
project_path <- here("data-example/my-amazing-project")
data_path <- here("data-example/data/")
```

The goal of this meeting is to find optimal solutions to visualise your
data. This PPGU workshop is especially addressing the needs for good
figures for the up-and-coming NAC meeting.

# Reproducible workflow

## Structure

## Project setup

## Data management

Accessing your data in the organised structure. The use of explicit file
paths

``` r
project_path <- "...complete_path.../my-amazing-project"
data_path <- "...complete_path.../data"
```

The use of symlinks

``` r
file.symlink(data_path, file.path(project_path, "data"))
#> [1] TRUE
```

# Reading data

``` r
# install.packages("readr")
# library(readr) is already loaded with tidyverse
bonenburg_palyno <- read_csv(
  file.path("data-example/data/pollen_spores/Bonenburg_palyno.csv"), 
  col_types = "d--ddddddd-"
  )
```
