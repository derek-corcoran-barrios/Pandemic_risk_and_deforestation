
- <a href="#1-pandemic_risk_and_deforestation"
  id="toc-1-pandemic_risk_and_deforestation">1
  Pandemic_risk_and_deforestation</a>
  - <a href="#11-load-necessary-packages"
    id="toc-11-load-necessary-packages">1.1 load necessary packages:</a>
  - <a
    href="#12-mammal-species-richness-abundance-and-number-of-infected-individuals"
    id="toc-12-mammal-species-richness-abundance-and-number-of-infected-individuals">1.2
    Mammal species richness, abundance and number of infected
    individuals</a>
- <a href="#2-used-packages-and-versions"
  id="toc-2-used-packages-and-versions">2 Used Packages and Versions</a>
- <a href="#3-references" id="toc-3-references">3 References</a>

<!-- README.md is generated from README.Rmd. Please edit that file -->

# 1 Pandemic_risk_and_deforestation

<!-- badges: start -->
<!-- badges: end -->

The goal of the *Pandemic_risk_and_deforestation* is to document the
code and datasets used for the mansucript *Pandemic risk and
deforestation policies in the world tropics* this repo goes section by
section with the code

## 1.1 load necessary packages:

first we load the necessary packages

``` r
library(terra)
library(dplyr)
library(broom)
```

## 1.2 Mammal species richness, abundance and number of infected individuals

We used IUCN range maps for mammalian species in the Orders Chiroptera,
Primate, Rodentia and Ungulata gathered from the IUCN red list
(<https://www.iucnredlist.org/resources/spatial-data-download>) in
raster format to calculate species richness ($S$) for each order within
each cell. To transform number of species into number of individuals, we
used models fitted to empirical data obtained from a world database of
species richness and abundance in local communities throughout the world
(The Ecological Register (Alroy 2015).

``` r
Data <- readRDS("Alroy_Data.rds") |>  
    dplyr::filter(Goods_U > 0.9, count > 100) |>  
    dplyr::mutate(LogS = log(Richness), LogA = log(Fishers_Alpha))
```

From these relationships (for values of Goods_U \> 0.9 and counts of
more of a 100), we then calculated the expected abundance for each cell
$S$

``` r
powerlaw.model <- nls(Fishers_Alpha~a*Richness^y, start= list(y=0, a = 1), data =Data)
```

see results in table <a href="#tab:tableNLS">1.1</a>

| term | estimate | std.error | statistic | p.value |
|:-----|---------:|----------:|----------:|--------:|
| y    |    1.056 |     0.008 |   138.742 |       0 |
| a    |    0.198 |     0.007 |    28.702 |       0 |

<span id="tab:tableNLS"></span>Table 1.1: Parameters from the non linear
model for the power law of fishers alpha

# 2 Used Packages and Versions

- **knitr**: 1.45
- **broom**: 1.0.5
- **dplyr**: 1.1.4
- **terra**: 1.7-73

In this code, `sessionInfo()$otherPkgs` is used to extract the loaded
packages during the rendering of the R Markdown document. We then
extract the package names and versions from this information and format
it into a Markdown list, excluding base and recommended packages.

# 3 References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-alroy2015shape" class="csl-entry">

Alroy, John. 2015. “The Shape of Terrestrial Abundance Distributions.”
*Science Advances* 1 (8): e1500082.

</div>

</div>
