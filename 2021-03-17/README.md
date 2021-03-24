Meeting notes
================
Martin Schobben
17 March, 2021

<!-- README.md is generated from README.Rmd. Please edit that file -->

The goal of this meeting is to find optimal solutions to visualise your
data. This PPGU workshop is especially addressing the needs for good
figures for the up-and-coming NAC meeting.

# Reproducible workflow

## Structure

It is good practice to separate the raw data from the project files
where you perform your data-analysis, create figures and write reports.
This ensures unnecessary loss of data by accidental deletion. It is also
good to store raw data as read-only.

## Project setup

R projects (integrated in R studio) are a good way to keep R scripts for
a specific research question together in one organised project. The path
are in an R project are always relative to the project. For the
experienced user it also offers other benefits like version-control and
package building.

## Data management

Accessing your data in the organised structure with separate directories
for raw-data can be a bit of challenge. The use of explicit file paths
with `file.path()` which is platform independent is one option.

``` r
project_path <- "...complete_path.../my-amazing-project"
data_path <- "...complete_path.../data"
```

The more elegant solution uses symlinks which creates a pointer to the
data in your R project.

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
