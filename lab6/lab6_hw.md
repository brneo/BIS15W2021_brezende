---
title: "Lab 6 Homework"
author: "Brian Rezende"
date: "2021-01-27"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_character(),
##   `ISSCAAP group#` = col_double(),
##   `FAO major fishing area` = col_double()
## )
## i Use `spec()` for the full column specifications.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
glimpse(fisheries)
```

```
## Rows: 17,692
## Columns: 71
## $ Country                   <chr> "Albania", "Albania", "Albania", "Albania...
## $ `Common name`             <chr> "Angelsharks, sand devils nei", "Atlantic...
## $ `ISSCAAP group#`          <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 3...
## $ `ISSCAAP taxonomic group` <chr> "Sharks, rays, chimaeras", "Tunas, bonito...
## $ `ASFIS species#`          <chr> "10903XXXXX", "1750100101", "17710001XX",...
## $ `ASFIS species name`      <chr> "Squatinidae", "Sarda sarda", "Sphyraena ...
## $ `FAO major fishing area`  <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 3...
## $ Measure                   <chr> "Quantity (tonnes)", "Quantity (tonnes)",...
## $ `1950`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1951`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1952`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1953`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1954`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1955`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1956`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1957`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1958`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1959`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1960`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1961`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1962`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1963`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1964`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1965`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1966`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1967`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1968`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1969`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1970`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1971`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1972`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1973`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1974`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1975`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1976`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1977`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1978`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1979`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1980`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1981`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1982`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ `1983`                    <chr> NA, NA, NA, NA, NA, NA, "559", NA, NA, NA...
## $ `1984`                    <chr> NA, NA, NA, NA, NA, NA, "392", NA, NA, NA...
## $ `1985`                    <chr> NA, NA, NA, NA, NA, NA, "406", NA, NA, NA...
## $ `1986`                    <chr> NA, NA, NA, NA, NA, NA, "499", NA, NA, NA...
## $ `1987`                    <chr> NA, NA, NA, NA, NA, NA, "564", NA, NA, NA...
## $ `1988`                    <chr> NA, NA, NA, NA, NA, NA, "724", NA, NA, NA...
## $ `1989`                    <chr> NA, NA, NA, NA, NA, NA, "583", NA, NA, NA...
## $ `1990`                    <chr> NA, NA, NA, NA, NA, NA, "754", NA, NA, NA...
## $ `1991`                    <chr> NA, NA, NA, NA, NA, NA, "283", NA, NA, NA...
## $ `1992`                    <chr> NA, NA, NA, NA, NA, NA, "196", NA, NA, NA...
## $ `1993`                    <chr> NA, NA, NA, NA, NA, NA, "150 F", NA, NA, ...
## $ `1994`                    <chr> NA, NA, NA, NA, NA, NA, "100 F", NA, NA, ...
## $ `1995`                    <chr> "0 0", "1", NA, "0 0", "0 0", NA, "52", "...
## $ `1996`                    <chr> "53", "2", NA, "3", "2", NA, "104", "8", ...
## $ `1997`                    <chr> "20", "0 0", NA, "0 0", "0 0", NA, "65", ...
## $ `1998`                    <chr> "31", "12", NA, NA, NA, NA, "220", "18", ...
## $ `1999`                    <chr> "30", "30", NA, NA, NA, NA, "220", "18", ...
## $ `2000`                    <chr> "30", "25", "2", NA, NA, NA, "220", "20",...
## $ `2001`                    <chr> "16", "30", NA, NA, NA, NA, "120", "23", ...
## $ `2002`                    <chr> "79", "24", NA, "34", "6", NA, "150", "84...
## $ `2003`                    <chr> "1", "4", NA, "22", NA, NA, "84", "178", ...
## $ `2004`                    <chr> "4", "2", "2", "15", "1", "2", "76", "285...
## $ `2005`                    <chr> "68", "23", "4", "12", "5", "6", "68", "1...
## $ `2006`                    <chr> "55", "30", "7", "18", "8", "9", "86", "1...
## $ `2007`                    <chr> "12", "19", NA, NA, NA, NA, "132", "18", ...
## $ `2008`                    <chr> "23", "27", NA, NA, NA, NA, "132", "23", ...
## $ `2009`                    <chr> "14", "21", NA, NA, NA, NA, "154", "20", ...
## $ `2010`                    <chr> "78", "23", "7", NA, NA, NA, "80", "228",...
## $ `2011`                    <chr> "12", "12", NA, NA, NA, NA, "88", "9", NA...
## $ `2012`                    <chr> "5", "5", NA, NA, NA, NA, "129", "290", N...
```

```r
head(fisheries, n=10)
```

```
## # A tibble: 10 x 71
##    Country `Common name` `ISSCAAP group#` `ISSCAAP taxono~ `ASFIS species#`
##    <chr>   <chr>                    <dbl> <chr>            <chr>           
##  1 Albania Angelsharks,~               38 Sharks, rays, c~ 10903XXXXX      
##  2 Albania Atlantic bon~               36 Tunas, bonitos,~ 1750100101      
##  3 Albania Barracudas n~               37 Miscellaneous p~ 17710001XX      
##  4 Albania Blue and red~               45 Shrimps, prawns  2280203101      
##  5 Albania Blue whiting~               32 Cods, hakes, ha~ 1480403301      
##  6 Albania Bluefish                    37 Miscellaneous p~ 1702021301      
##  7 Albania Bogue                       33 Miscellaneous c~ 1703926101      
##  8 Albania Caramote pra~               45 Shrimps, prawns  2280100117      
##  9 Albania Catsharks, n~               38 Sharks, rays, c~ 10801003XX      
## 10 Albania Common cuttl~               57 Squids, cuttlef~ 3210200202      
## # ... with 66 more variables: `ASFIS species name` <chr>, `FAO major fishing
## #   area` <dbl>, Measure <chr>, `1950` <chr>, `1951` <chr>, `1952` <chr>,
## #   `1953` <chr>, `1954` <chr>, `1955` <chr>, `1956` <chr>, `1957` <chr>,
## #   `1958` <chr>, `1959` <chr>, `1960` <chr>, `1961` <chr>, `1962` <chr>,
## #   `1963` <chr>, `1964` <chr>, `1965` <chr>, `1966` <chr>, `1967` <chr>,
## #   `1968` <chr>, `1969` <chr>, `1970` <chr>, `1971` <chr>, `1972` <chr>,
## #   `1973` <chr>, `1974` <chr>, `1975` <chr>, `1976` <chr>, `1977` <chr>,
## #   `1978` <chr>, `1979` <chr>, `1980` <chr>, `1981` <chr>, `1982` <chr>,
## #   `1983` <chr>, `1984` <chr>, `1985` <chr>, `1986` <chr>, `1987` <chr>,
## #   `1988` <chr>, `1989` <chr>, `1990` <chr>, `1991` <chr>, `1992` <chr>,
## #   `1993` <chr>, `1994` <chr>, `1995` <chr>, `1996` <chr>, `1997` <chr>,
## #   `1998` <chr>, `1999` <chr>, `2000` <chr>, `2001` <chr>, `2002` <chr>,
## #   `2003` <chr>, `2004` <chr>, `2005` <chr>, `2006` <chr>, `2007` <chr>,
## #   `2008` <chr>, `2009` <chr>, `2010` <chr>, `2011` <chr>, `2012` <chr>
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
names(fisheries)
```

```
##  [1] "country"                 "common_name"            
##  [3] "isscaap_group_number"    "isscaap_taxonomic_group"
##  [5] "asfis_species_number"    "asfis_species_name"     
##  [7] "fao_major_fishing_area"  "measure"                
##  [9] "x1950"                   "x1951"                  
## [11] "x1952"                   "x1953"                  
## [13] "x1954"                   "x1955"                  
## [15] "x1956"                   "x1957"                  
## [17] "x1958"                   "x1959"                  
## [19] "x1960"                   "x1961"                  
## [21] "x1962"                   "x1963"                  
## [23] "x1964"                   "x1965"                  
## [25] "x1966"                   "x1967"                  
## [27] "x1968"                   "x1969"                  
## [29] "x1970"                   "x1971"                  
## [31] "x1972"                   "x1973"                  
## [33] "x1974"                   "x1975"                  
## [35] "x1976"                   "x1977"                  
## [37] "x1978"                   "x1979"                  
## [39] "x1980"                   "x1981"                  
## [41] "x1982"                   "x1983"                  
## [43] "x1984"                   "x1985"                  
## [45] "x1986"                   "x1987"                  
## [47] "x1988"                   "x1989"                  
## [49] "x1990"                   "x1991"                  
## [51] "x1992"                   "x1993"                  
## [53] "x1994"                   "x1995"                  
## [55] "x1996"                   "x1997"                  
## [57] "x1998"                   "x1999"                  
## [59] "x2000"                   "x2001"                  
## [61] "x2002"                   "x2003"                  
## [63] "x2004"                   "x2005"                  
## [65] "x2006"                   "x2007"                  
## [67] "x2008"                   "x2009"                  
## [69] "x2010"                   "x2011"                  
## [71] "x2012"
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy %>% 
  group_by(country) %>% 
  summarise_all(n_distinct) %>% 
  count(country, sort = T)
```

```
## # A tibble: 203 x 2
##    country                 n
##    <fct>               <int>
##  1 Albania                 1
##  2 Algeria                 1
##  3 American Samoa          1
##  4 Angola                  1
##  5 Anguilla                1
##  6 Antigua and Barbuda     1
##  7 Argentina               1
##  8 Aruba                   1
##  9 Australia               1
## 10 Bahamas                 1
## # ... with 193 more rows
```

There are 203 countries that are represented in this data. 

4. Refocus the data only to include only: country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
refined_fisheries_tidy <- fisheries_tidy %>% 
  select(c(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch))
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
refined_fisheries_tidy %>% 
  group_by(asfis_species_number) %>% 
  count(asfis_species_number, sort = T ) %>% 
  n_distinct("asfis_species_number")
```

```
## [1] 1551
```

There are 1,551 distinct fish species that were caught. 

6. Which country had the largest overall catch in the year 2000?

```r
refined_fisheries_tidy %>% 
  select(catch, country, year) %>% 
  filter(year == "2000") %>%
  group_by(country) %>%
  summarise(catch2000=sum(catch, na.rm = T),
            totaln=n(),.groups = 'keep') %>% 
  arrange(desc(catch2000)) %>% 
  head(catch2000, n =5)
```

```
## # A tibble: 5 x 3
## # Groups:   country [5]
##   country                  catch2000 totaln
##   <fct>                        <dbl>  <int>
## 1 China                        25899     93
## 2 Russian Federation           12181    192
## 3 United States of America     11762    438
## 4 Japan                         8510    241
## 5 Indonesia                     8341    140
```
China had a whopping catch number in 2000! 

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
refined_fisheries_tidy %>%
  select(country, asfis_species_name, catch, year) %>%
  filter(between(year, 1990,2000)) %>% 
  filter(asfis_species_name=="Sardina pilchardus") %>%
  group_by(country) %>% 
  summarise(total_sardines=sum(catch, na.rm = T),
            totaln=n(),.groups = 'keep') %>% 
  arrange(desc(total_sardines)) %>% 
  head(total_sardines, n=3)
```

```
## # A tibble: 3 x 3
## # Groups:   country [3]
##   country            total_sardines totaln
##   <fct>                       <dbl>  <int>
## 1 Morocco                      7470     22
## 2 Spain                        3507     33
## 3 Russian Federation           1639     11
```
Top country who found sardine harvest to be most bountiful during this time was Morocco. 

8. Which five countries caught the most cephalopods between 2008-2012?

```r
refined_fisheries_tidy %>% 
  select(!asfis_species_number) %>% 
  filter(isscaap_taxonomic_group=="Squids, cuttlefishes, octopuses") %>%
  filter(asfis_species_name=="Cephalopoda") %>% 
  filter(year>=2008, year<=2012) %>% 
  group_by(country) %>% 
  summarise(most_cephalopods=sum(catch, na.rm = T),
                                 totaln=n(),.groups = 'keep') %>% 
  arrange(desc(most_cephalopods))
```

```
## # A tibble: 16 x 3
## # Groups:   country [16]
##    country                  most_cephalopods totaln
##    <fct>                               <dbl>  <int>
##  1 India                                 570     10
##  2 China                                 257      5
##  3 Spain                                 198     10
##  4 Algeria                               162      5
##  5 France                                101      7
##  6 Mauritania                             90      5
##  7 TimorLeste                             76      5
##  8 Italy                                  66      1
##  9 Mozambique                             16      5
## 10 Cambodia                               15      5
## 11 Taiwan Province of China               13      3
## 12 Madagascar                             11      5
## 13 Croatia                                 7      3
## 14 Israel                                  0      1
## 15 Somalia                                 0      5
## 16 Viet Nam                                0      5
```

Wow! In the end the 5 countries with the greates luck in catching cephalopods include India, Spain, China, Alegria, and France. 

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)


```r
refined_fisheries_tidy %>% 
  filter(between(year, 2008, 2012)) %>% 
  group_by(asfis_species_name) %>% 
  summarize(highest_species_catch=sum(catch, na.rm = T),
            totaln=n(),.groups = 'keep') %>% 
  arrange(desc(highest_species_catch)) %>% 
  head(n=2)
```

```
## # A tibble: 2 x 3
## # Groups:   asfis_species_name [2]
##   asfis_species_name    highest_species_catch totaln
##   <chr>                                 <dbl>  <int>
## 1 Osteichthyes                         107808   1460
## 2 Theragra chalcogramma                 41075     35
```
Though our data shows Osteichthyes as the highest catch total, our hint helps us disqualify them. So, theragra chalcogramma is the species with the highest catch total. This species of fish is also known as the Alaskan pollock! 


10. Use the data to do at least one analysis of your choice.

```r
refined_fisheries_tidy %>%
  filter(between(year, 2003,2012)) %>% 
  filter(country=="Brazil") %>% 
  group_by(asfis_species_name) %>% 
  summarize(brazil_catch_total=sum(catch, na.rm = T),
            totaln=n(),.groups = 'keep') %>% 
  arrange(desc(brazil_catch_total)) %>% 
  head(n=10)
```

```
## # A tibble: 10 x 3
## # Groups:   asfis_species_name [10]
##    asfis_species_name      brazil_catch_total totaln
##    <chr>                                <dbl>  <int>
##  1 Xiphopenaeus kroyeri                   662     10
##  2 Clupeoidei                             579     10
##  3 Kyphosidae                             532     10
##  4 Osteichthyes                           531     10
##  5 Micropogonias furnieri                 527     10
##  6 Tetrapturus albidus                    518     10
##  7 Sardinella brasiliensis                486     10
##  8 Katsuwonus pelamis                     474     10
##  9 Isopisthus parvipinnis                 460     10
## 10 Umbrina canosai                        447     10
```

I was curious to see what species had the highest catch total when I would spend my formative years in Brazil with my extended family. To no surprise prawns, Xiphopenaeus kroyeri, were some of the most caught! This made me miss my family and the amazing dishes they served! 

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
