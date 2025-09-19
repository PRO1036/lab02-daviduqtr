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
  facet_wrap(~ continent, ncol = 3)+
  labs(title = "Nombre de pays en fonction de la quantité de déchet par habitant (Kg/jour), selon le continent")
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
ggplot(plastic_waste %>%
         filter(plastic_waste_per_cap < 3.5), 
       aes(x = plastic_waste_per_cap,
           colour = continent, fill = continent))+
        geom_density(alpha = 0.6)+
      labs(title = "Densité du nombre de pays qui fonction de la quantité de déchet par habitant (Kg/jour), selon le continent")
```

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

La couleur et le remplissage (fill) se trouvent dans l’aes (estéthique)
alors que la transparence (alpha) se retrouve dans geom_density, car la
couleur et le remplissage sont des propriétés esthétiques, alors que la
transparence sert à mettre en relief les différentes fonctions
géométriques. La fonction “transparence” ne sert donc pas seulement
l’esthétisme, mais aussi la géométrie elle-même.

### Exercise 3

Boxplot:

``` r
ggplot(plastic_waste %>%
         filter(plastic_waste_per_cap < 3.5), 
       aes(x = continent, y = plastic_waste_per_cap,
           colour = continent, fill = continent))+
        geom_boxplot(alpha = 0.6)+
      labs(title = "Génération de déchet (kg/jour) par habitant en fonction du continent")
```

![](lab-02_files/figure-gfm/plastic-waste-boxplot-1.png)<!-- -->

Violin plot:

``` r
ggplot(plastic_waste %>%
      filter(plastic_waste_per_cap < 3.5), 
      aes(x = continent, y = plastic_waste_per_cap,
      colour = continent, fill = continent))+
    geom_violin(alpha = 0.6)+
    labs(title = "Génération de déchet (kg/jour) par habitant en fonction du continent")
```

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

Le diagramme en violon permet une meilleure précision dans la
visualisation de la répartition des données dans un échantillons. Ici,
par exemple, on voit qu’en Océanie, qu’il y a deux types d’échantillon.
Les pays qui polluent moins et les pays qui polluent plus. Mais que dans
tous les cas, le nombre de pays qui sont de faibles pollueurs, polluent
plus que la majorité des pays d’Afrique et d’Asie. Les résultats sont
beaucoup plus visuels et parlant que dans les boîtes à moustaches où
l’on pourrait simplement dire, grâce à l’écart-type, qu’il y a plus de
pays faiblement pollueur que très polueur.

### Exercise 4

``` r
ggplot(plastic_waste %>% filter(plastic_waste_per_cap < 3.5),
       aes(x = plastic_waste_per_cap, y = mismanaged_plastic_waste_per_cap, 
           colour = continent))+
      geom_point()+
      geom_smooth(method=lm,
                  se=FALSE)+
      labs(title = "Quantité de déchets non gérés par les pays en fonction de la quantité de déchets produit par les habitant (kg/jour), à chaque jour")
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

source : <http://www.cookbook-r.com/Graphs/Scatterplots_(ggplot2)/>

À part les observations précédemment dit, comme le fait que les pays
africains et asiatiques sont généralement les pays qui pollue le moins
par habitant, on peut aussi remarqué que, généralement, ce sont ces
derniers qui ont le plus de difficultés à gérer les déchets. On remarque
aussi que les grands pollueurs sont ceux qui gèrent le mieux leurs
déchets.

### Exercise 5

``` r
ggplot(plastic_waste %>% filter(plastic_waste_per_cap < 3.5, total_pop < 1000000000),
      aes(x = total_pop, y = plastic_waste_per_cap))+
      geom_point()+
  geom_smooth(method=lm,
                  se=FALSE)+
      labs(title = "Quantité de déchets générés par les habitants d'un pays (kg/jour) en fonction de la population totale du pays")
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(plastic_waste %>% filter(plastic_waste_per_cap < 3.5),
       aes(x = coastal_pop, y = plastic_waste_per_cap))+
      geom_point()+
  geom_smooth(method=lm,
                  se=FALSE)+

      labs(title = "Quantité de déchets générés par les habitants d'un pays (kg/jour) en fonction de la population vivant sur les côtes")
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

Il ne semble pas avoir de forte corrélation, ni entre la population
total et la quantité de polution générée, ni entre le nombre d’habitant
côtiers et la quantité de déchets générées.

## Conclusion

Recréez la visualisation:

``` r
plastic_waste <- plastic_waste %>% 
  mutate(coastal_pop_prop = coastal_pop / total_pop)
```

``` r
print(plastic_waste)
```

    ## # A tibble: 240 × 11
    ##    code  entity              continent    year gdp_per_cap plastic_waste_per_cap
    ##    <chr> <chr>               <chr>       <dbl>       <dbl>                 <dbl>
    ##  1 AFG   Afghanistan         Asia         2010       1614.                NA    
    ##  2 ALB   Albania             Europe       2010       9927.                 0.069
    ##  3 DZA   Algeria             Africa       2010      12871.                 0.144
    ##  4 ASM   American Samoa      Oceania      2010         NA                 NA    
    ##  5 AND   Andorra             Europe       2010         NA                 NA    
    ##  6 AGO   Angola              Africa       2010       5898.                 0.062
    ##  7 AIA   Anguilla            North Amer…  2010         NA                  0.252
    ##  8 ATG   Antigua and Barbuda North Amer…  2010      19213.                 0.66 
    ##  9 ARG   Argentina           South Amer…  2010      18712.                 0.183
    ## 10 ARM   Armenia             Europe       2010       6703.                NA    
    ## # ℹ 230 more rows
    ## # ℹ 5 more variables: mismanaged_plastic_waste_per_cap <dbl>,
    ## #   mismanaged_plastic_waste <dbl>, coastal_pop <dbl>, total_pop <dbl>,
    ## #   coastal_pop_prop <dbl>

``` r
ggplot(plastic_waste %>% filter(plastic_waste_per_cap < 3.5),
       aes(x = coastal_pop_prop, y = plastic_waste_per_cap))+
      geom_point(aes(color = continent))+
      geom_smooth(color = "black")+
      labs(title = "Quantité de déchets plastiques vs Proportion de la population côtière",
          subtitle = "Selon le continent",
           x = "Proportion de la population côtière (Coastal / total population)", y = "Nombre de déchêts plastiques par habitants",
           colour = "Continent")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- --> source :
<https://www.statology.org/ratios-in-r/>
<https://stackoverflow.com/questions/40600824/how-to-apply-geom-smooth-for-every-group>

## calcul médiane

``` r
mediane_dechets_par_continent <- plastic_waste %>%
  group_by(continent)%>%
    summarise(mediane_dechets = median(plastic_waste_per_cap, na.rm = TRUE))
```

``` r
print(mediane_dechets_par_continent)
```

    ## # A tibble: 6 × 2
    ##   continent     mediane_dechets
    ##   <chr>                   <dbl>
    ## 1 Africa                  0.071
    ## 2 Asia                    0.144
    ## 3 Europe                  0.196
    ## 4 North America           0.252
    ## 5 Oceania                 0.144
    ## 6 South America           0.164
