binomen
=======



[![Build Status](https://api.travis-ci.org/ropensci/binomen.png)](https://travis-ci.org/ropensci/binomen)

`binomen` provides various taxonomic classes for defining a single taxon, multiple taxa, and a taxonomic data.frame

It is sort of a companion to [taxize](https://github.com/ropensci/taxize), where you can get taxonomic data on taxonomic names from the web.

## Installation


```r
install.packages("devtools")
devtools::install_github("ropensci/binomen")
```


```r
library('binomen')
```

## Make a taxon

Make a taxon object


```r
(obj <- make_taxon(genus="Poa", epithet="annua", authority="L.",
                   family='Poaceae', clazz='Poales', kingdom='Plantae', variety='annua'))
#> <taxon>
#>   binomial: Poa annua
#>   classification: 
#>     kingdom: Plantae
#>     clazz: Poales
#>     family: Poaceae
#>     genus: Poa
#>     species: Poa annua
#>     variety: annua
```

Index to various parts of the object

The binomial


```r
obj$binomial
#> <binomial>
#>   genus: Poa
#>   epithet: annua
#>   canonical: Poa annua
#>   species: Poa annua L.
#>   authority: L.
```

The authority


```r
obj$binomial$authority
#> [1] "L."
```

The classification


```r
obj$classification
#> <classification>
#>     kingdom: Plantae
#>     clazz: Poales
#>     family: Poaceae
#>     genus: Poa
#>     species: Poa annua
#>     variety: annua
```

The family


```r
obj$classification$family
#> <taxonref>
#>   rank: family
#>   name: Poaceae
#>   id: none
#>   uri: none
```

## Subset taxon objects

Get a single rank


```r
obj %>% select(family)
#> <taxonref>
#>   rank: family
#>   name: Poaceae
#>   id: none
#>   uri: none
```

Get a range of ranks via `span()`


```r
obj %>% span(kingdom, family)
#> $kingdom
#> <taxonref>
#>   rank: kingdom
#>   name: Plantae
#>   id: none
#>   uri: none
#> 
#> $clazz
#> <taxonref>
#>   rank: clazz
#>   name: Poales
#>   id: none
#>   uri: none
#> 
#> $family
#> <taxonref>
#>   rank: family
#>   name: Poaceae
#>   id: none
#>   uri: none
```

Extract classification as a `data.frame`


```r
gethier(obj)
#>      rank      name
#> 1 kingdom   Plantae
#> 2   clazz    Poales
#> 3  family   Poaceae
#> 4   genus       Poa
#> 5 species Poa annua
#> 6 variety     annua
```

## Taxonomic data.frame's

Make one


```r
df <- data.frame(order=c('Asterales','Asterales','Fagales','Poales','Poales','Poales'),
                family=c('Asteraceae','Asteraceae','Fagaceae','Poaceae','Poaceae','Poaceae'),
                genus=c('Helianthus','Helianthus','Quercus','Poa','Festuca','Holodiscus'),
                stringsAsFactors = FALSE)
(df2 <- taxon_df(df))
#>       order     family      genus
#> 1 Asterales Asteraceae Helianthus
#> 2 Asterales Asteraceae Helianthus
#> 3   Fagales   Fagaceae    Quercus
#> 4    Poales    Poaceae        Poa
#> 5    Poales    Poaceae    Festuca
#> 6    Poales    Poaceae Holodiscus
```

Parse - get rank order matching _Fagales_ via `select()`


```r
df2 %>% select(order, Fagales)
#>     order   family   genus
#> 3 Fagales Fagaceae Quercus
```

get rank family matching _Asteraceae_ via `select()`


```r
df2 %>% select(family, Asteraceae)
#>       order     family      genus
#> 1 Asterales Asteraceae Helianthus
#> 2 Asterales Asteraceae Helianthus
```

get rank genus matching _Poa_ via `select()`


```r
df2 %>% select(genus, Poa)
#>    order  family genus
#> 4 Poales Poaceae   Poa
```

get range of names via `span()`


```r
df2 %>% span(family, genus)
#>       family      genus
#> 1 Asteraceae Helianthus
#> 2 Asteraceae Helianthus
#> 3   Fagaceae    Quercus
#> 4    Poaceae        Poa
#> 5    Poaceae    Festuca
#> 6    Poaceae Holodiscus
```

Separate each row into a `taxon` class (many `taxon` objects are a `taxa` class)


```r
scatter(df2)
#> [[1]]
#> <taxon>
#>   binomial: Helianthus none
#>   classification: 
#>     order: Asterales
#>     family: Asteraceae
#>     genus: Helianthus
#>     species: Helianthus none
#> 
#> [[2]]
#> <taxon>
#>   binomial: Helianthus none
#>   classification: 
#>     order: Asterales
#>     family: Asteraceae
#>     genus: Helianthus
#>     species: Helianthus none
#> 
#> [[3]]
#> <taxon>
#>   binomial: Quercus none
#>   classification: 
#>     order: Fagales
#>     family: Fagaceae
#>     genus: Quercus
#>     species: Quercus none
#> 
#> [[4]]
#> <taxon>
#>   binomial: Poa none
#>   classification: 
#>     order: Poales
#>     family: Poaceae
#>     genus: Poa
#>     species: Poa none
#> 
#> [[5]]
#> <taxon>
#>   binomial: Festuca none
#>   classification: 
#>     order: Poales
#>     family: Poaceae
#>     genus: Festuca
#>     species: Festuca none
#> 
#> [[6]]
#> <taxon>
#>   binomial: Holodiscus none
#>   classification: 
#>     order: Poales
#>     family: Poaceae
#>     genus: Holodiscus
#>     species: Holodiscus none
```


## Meta

* Please [report any issues or bugs](https://github.com/ropensci/binomen/issues).
* License: MIT
* Get citation information for `binomen` in R doing `citation(package = 'binomen')`

[![ropensci](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
