Lab 02 - Plastic waste
================
David Marx-Lemieux

## Chargement des packages et des données

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

``` r
glimpse(plastic_waste)
```

    ## Rows: 240
    ## Columns: 10
    ## $ code                             <chr> "AFG", "ALB", "DZA", "ASM", "AND", "A…
    ## $ entity                           <chr> "Afghanistan", "Albania", "Algeria", …
    ## $ continent                        <chr> "Asia", "Europe", "Africa", "Oceania"…
    ## $ year                             <dbl> 2010, 2010, 2010, 2010, 2010, 2010, 2…
    ## $ gdp_per_cap                      <dbl> 1614.255, 9927.182, 12870.603, NA, NA…
    ## $ plastic_waste_per_cap            <dbl> NA, 0.069, 0.144, NA, NA, 0.062, 0.25…
    ## $ mismanaged_plastic_waste_per_cap <dbl> NA, 0.032, 0.086, NA, NA, 0.045, 0.01…
    ## $ mismanaged_plastic_waste         <dbl> NA, 29705, 520555, NA, NA, 62528, 52,…
    ## $ coastal_pop                      <dbl> NA, 2530533, 16556580, NA, NA, 379004…
    ## $ total_pop                        <dbl> 31411743, 3204284, 35468208, 68420, 8…

Commençons par filtrer les données pour retirer le point représenté par
Trinité et Tobago (TTO) qui est un outlier.

``` r
plastic_waste %>%
  filter(plastic_waste_per_cap < 3.5)
```

    ## # A tibble: 188 × 10
    ##    code  entity              continent    year gdp_per_cap plastic_waste_per_cap
    ##    <chr> <chr>               <chr>       <dbl>       <dbl>                 <dbl>
    ##  1 ALB   Albania             Europe       2010       9927.                 0.069
    ##  2 DZA   Algeria             Africa       2010      12871.                 0.144
    ##  3 AGO   Angola              Africa       2010       5898.                 0.062
    ##  4 AIA   Anguilla            North Amer…  2010         NA                  0.252
    ##  5 ATG   Antigua and Barbuda North Amer…  2010      19213.                 0.66 
    ##  6 ARG   Argentina           South Amer…  2010      18712.                 0.183
    ##  7 ABW   Aruba               North Amer…  2010         NA                  0.252
    ##  8 AUS   Australia           Oceania      2010      41464.                 0.112
    ##  9 BHS   Bahamas             North Amer…  2010      29222.                 0.39 
    ## 10 BHR   Bahrain             Asia         2010      40571.                 0.132
    ## # ℹ 178 more rows
    ## # ℹ 4 more variables: mismanaged_plastic_waste_per_cap <dbl>,
    ## #   mismanaged_plastic_waste <dbl>, coastal_pop <dbl>, total_pop <dbl>

# nombre de ligne

``` r
nrow(plastic_waste)
```

    ## [1] 240

# visualisation des données par un histogramme

``` r
ggplot(data = plastic_waste, aes(x = plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.2)
```

    ## Warning: Removed 51 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](lab-02_files/figure-gfm/distribution-des-donnees-1.png)<!-- -->

# identifier le pays “abberant”

``` r
plastic_waste %>%
  filter(plastic_waste_per_cap > 3.5)
```

    ## # A tibble: 1 × 10
    ##   code  entity              continent     year gdp_per_cap plastic_waste_per_cap
    ##   <chr> <chr>               <chr>        <dbl>       <dbl>                 <dbl>
    ## 1 TTO   Trinidad and Tobago North Ameri…  2010      31261.                   3.6
    ## # ℹ 4 more variables: mismanaged_plastic_waste_per_cap <dbl>,
    ## #   mismanaged_plastic_waste <dbl>, coastal_pop <dbl>, total_pop <dbl>

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste %>%
      filter(plastic_waste_per_cap < 3.5), aes(x = plastic_waste_per_cap))+
  geom_histogram(binwidth = 0.18)+
  facet_wrap(~ continent, ncol = 3)
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

La quantité de déchet par habitant est représentée par l’axe des X. De
ce fait, plus on s’éloigne de l’origine, plus les pays consomment des
déchets. L’axe des Y représente le nombre de pays. On constate que seul
quelques pays d’Amérique du Nord et d’Asie ont des moyennes de
consommations de déchets par habitant plus élevées que 0,7 kg/jour.
Quelques pays d’Europe et d’Amérique du Sud les suient de près.
L’Afrique, avec le continent avec le plus grand nombre de pays près de
l’origine, donc qui pollue peu par le plastique. L’Asie est le second
continent avec le plus de pays dans la tranche la plus faible pour la
quantité de déchet par habitant.

### Exercise 2

``` r
# insert code here
```

Réponse à la question…

### Exercise 3

Boxplot:

``` r
# insert code here
```

Violin plot:

``` r
# insert code here
```

Réponse à la question…

### Exercise 4

``` r
# insert code here
```

Réponse à la question…

### Exercise 5

``` r
# insert code here
```

``` r
# insert code here
```

Réponse à la question…

## Conclusion

Recréez la visualisation:

``` r
# insert code here
```
