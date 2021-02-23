---
title: "Midterm 2"
author: "Brian Rezende"
date: "2021-02-23"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 10 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Your plots should use consistent aesthetics throughout. Feel free to be creative- there are many possible solutions to these questions!  

This exam is due by **12:00p on Tuesday, February 23**.  

## Load the libraries

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.4     v dplyr   1.0.2
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(here)
```

```
## here() starts at C:/Users/brian/Desktop/BIS15W2021_brezende
```

```r
options(scipen=999) #disables scientific notation when printing
```

## Gapminder
For this assignment, we are going to use data from  [gapminder](https://www.gapminder.org/). Gapminder includes information about economics, population, social issues, and life expectancy from countries all over the world. We will use three data sets, so please load all three.  

One thing to note is that the data include years beyond 2021. These are projections based on modeling done by the gapminder organization. Start by importing the data.

```r
population <- readr::read_csv("data/population_total.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   country = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
population
```

```
## # A tibble: 195 x 302
##    country `1800` `1801` `1802` `1803` `1804` `1805` `1806` `1807` `1808` `1809`
##    <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Afghan~ 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6 3.28e6
##  2 Albania 4.00e5 4.02e5 4.04e5 4.05e5 4.07e5 4.09e5 4.11e5 4.13e5 4.14e5 4.16e5
##  3 Algeria 2.50e6 2.51e6 2.52e6 2.53e6 2.54e6 2.55e6 2.56e6 2.56e6 2.57e6 2.58e6
##  4 Andorra 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3 2.65e3
##  5 Angola  1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6 1.57e6
##  6 Antigu~ 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4 3.70e4
##  7 Argent~ 5.34e5 5.20e5 5.06e5 4.92e5 4.79e5 4.66e5 4.53e5 4.41e5 4.29e5 4.17e5
##  8 Armenia 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5 4.13e5
##  9 Austra~ 2.00e5 2.05e5 2.11e5 2.16e5 2.22e5 2.27e5 2.33e5 2.39e5 2.46e5 2.52e5
## 10 Austria 3.00e6 3.02e6 3.04e6 3.05e6 3.07e6 3.09e6 3.11e6 3.12e6 3.14e6 3.16e6
## # ... with 185 more rows, and 291 more variables: `1810` <dbl>, `1811` <dbl>,
## #   `1812` <dbl>, `1813` <dbl>, `1814` <dbl>, `1815` <dbl>, `1816` <dbl>,
## #   `1817` <dbl>, `1818` <dbl>, `1819` <dbl>, `1820` <dbl>, `1821` <dbl>,
## #   `1822` <dbl>, `1823` <dbl>, `1824` <dbl>, `1825` <dbl>, `1826` <dbl>,
## #   `1827` <dbl>, `1828` <dbl>, `1829` <dbl>, `1830` <dbl>, `1831` <dbl>,
## #   `1832` <dbl>, `1833` <dbl>, `1834` <dbl>, `1835` <dbl>, `1836` <dbl>,
## #   `1837` <dbl>, `1838` <dbl>, `1839` <dbl>, `1840` <dbl>, `1841` <dbl>,
## #   `1842` <dbl>, `1843` <dbl>, `1844` <dbl>, `1845` <dbl>, `1846` <dbl>,
## #   `1847` <dbl>, `1848` <dbl>, `1849` <dbl>, `1850` <dbl>, `1851` <dbl>,
## #   `1852` <dbl>, `1853` <dbl>, `1854` <dbl>, `1855` <dbl>, `1856` <dbl>,
## #   `1857` <dbl>, `1858` <dbl>, `1859` <dbl>, `1860` <dbl>, `1861` <dbl>,
## #   `1862` <dbl>, `1863` <dbl>, `1864` <dbl>, `1865` <dbl>, `1866` <dbl>,
## #   `1867` <dbl>, `1868` <dbl>, `1869` <dbl>, `1870` <dbl>, `1871` <dbl>,
## #   `1872` <dbl>, `1873` <dbl>, `1874` <dbl>, `1875` <dbl>, `1876` <dbl>,
## #   `1877` <dbl>, `1878` <dbl>, `1879` <dbl>, `1880` <dbl>, `1881` <dbl>,
## #   `1882` <dbl>, `1883` <dbl>, `1884` <dbl>, `1885` <dbl>, `1886` <dbl>,
## #   `1887` <dbl>, `1888` <dbl>, `1889` <dbl>, `1890` <dbl>, `1891` <dbl>,
## #   `1892` <dbl>, `1893` <dbl>, `1894` <dbl>, `1895` <dbl>, `1896` <dbl>,
## #   `1897` <dbl>, `1898` <dbl>, `1899` <dbl>, `1900` <dbl>, `1901` <dbl>,
## #   `1902` <dbl>, `1903` <dbl>, `1904` <dbl>, `1905` <dbl>, `1906` <dbl>,
## #   `1907` <dbl>, `1908` <dbl>, `1909` <dbl>, ...
```


```r
income <- readr::read_csv("data/income_per_person_gdppercapita_ppp_inflation_adjusted.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   country = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
income
```

```
## # A tibble: 193 x 242
##    country `1800` `1801` `1802` `1803` `1804` `1805` `1806` `1807` `1808` `1809`
##    <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Afghan~    603    603    603    603    603    603    603    603    603    603
##  2 Albania    667    667    667    667    667    668    668    668    668    668
##  3 Algeria    715    716    717    718    719    720    721    722    723    724
##  4 Andorra   1200   1200   1200   1200   1210   1210   1210   1210   1220   1220
##  5 Angola     618    620    623    626    628    631    634    637    640    642
##  6 Antigu~    757    757    757    757    757    757    757    758    758    758
##  7 Argent~   1640   1640   1650   1650   1660   1660   1670   1680   1680   1690
##  8 Armenia    514    514    514    514    514    514    514    514    514    514
##  9 Austra~    817    822    826    831    836    841    845    850    855    860
## 10 Austria   1850   1850   1860   1870   1880   1880   1890   1900   1910   1920
## # ... with 183 more rows, and 231 more variables: `1810` <dbl>, `1811` <dbl>,
## #   `1812` <dbl>, `1813` <dbl>, `1814` <dbl>, `1815` <dbl>, `1816` <dbl>,
## #   `1817` <dbl>, `1818` <dbl>, `1819` <dbl>, `1820` <dbl>, `1821` <dbl>,
## #   `1822` <dbl>, `1823` <dbl>, `1824` <dbl>, `1825` <dbl>, `1826` <dbl>,
## #   `1827` <dbl>, `1828` <dbl>, `1829` <dbl>, `1830` <dbl>, `1831` <dbl>,
## #   `1832` <dbl>, `1833` <dbl>, `1834` <dbl>, `1835` <dbl>, `1836` <dbl>,
## #   `1837` <dbl>, `1838` <dbl>, `1839` <dbl>, `1840` <dbl>, `1841` <dbl>,
## #   `1842` <dbl>, `1843` <dbl>, `1844` <dbl>, `1845` <dbl>, `1846` <dbl>,
## #   `1847` <dbl>, `1848` <dbl>, `1849` <dbl>, `1850` <dbl>, `1851` <dbl>,
## #   `1852` <dbl>, `1853` <dbl>, `1854` <dbl>, `1855` <dbl>, `1856` <dbl>,
## #   `1857` <dbl>, `1858` <dbl>, `1859` <dbl>, `1860` <dbl>, `1861` <dbl>,
## #   `1862` <dbl>, `1863` <dbl>, `1864` <dbl>, `1865` <dbl>, `1866` <dbl>,
## #   `1867` <dbl>, `1868` <dbl>, `1869` <dbl>, `1870` <dbl>, `1871` <dbl>,
## #   `1872` <dbl>, `1873` <dbl>, `1874` <dbl>, `1875` <dbl>, `1876` <dbl>,
## #   `1877` <dbl>, `1878` <dbl>, `1879` <dbl>, `1880` <dbl>, `1881` <dbl>,
## #   `1882` <dbl>, `1883` <dbl>, `1884` <dbl>, `1885` <dbl>, `1886` <dbl>,
## #   `1887` <dbl>, `1888` <dbl>, `1889` <dbl>, `1890` <dbl>, `1891` <dbl>,
## #   `1892` <dbl>, `1893` <dbl>, `1894` <dbl>, `1895` <dbl>, `1896` <dbl>,
## #   `1897` <dbl>, `1898` <dbl>, `1899` <dbl>, `1900` <dbl>, `1901` <dbl>,
## #   `1902` <dbl>, `1903` <dbl>, `1904` <dbl>, `1905` <dbl>, `1906` <dbl>,
## #   `1907` <dbl>, `1908` <dbl>, `1909` <dbl>, ...
```


```r
life_expectancy <- readr::read_csv("data/life_expectancy_years.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   country = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
life_expectancy
```

```
## # A tibble: 187 x 302
##    country `1800` `1801` `1802` `1803` `1804` `1805` `1806` `1807` `1808` `1809`
##    <chr>    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Afghan~   28.2   28.2   28.2   28.2   28.2   28.2   28.1   28.1   28.1   28.1
##  2 Albania   35.4   35.4   35.4   35.4   35.4   35.4   35.4   35.4   35.4   35.4
##  3 Algeria   28.8   28.8   28.8   28.8   28.8   28.8   28.8   28.8   28.8   28.8
##  4 Andorra   NA     NA     NA     NA     NA     NA     NA     NA     NA     NA  
##  5 Angola    27     27     27     27     27     27     27     27     27     27  
##  6 Antigu~   33.5   33.5   33.5   33.5   33.5   33.5   33.5   33.5   33.5   33.5
##  7 Argent~   33.2   33.2   33.2   33.2   33.2   33.2   33.2   33.2   33.2   33.2
##  8 Armenia   34     34     34     34     34     34     34     34     34     34  
##  9 Austra~   34     34     34     34     34     34     34     34     34     34  
## 10 Austria   34.4   34.4   34.4   34.4   34.4   34.4   34.4   34.4   34.4   34.4
## # ... with 177 more rows, and 291 more variables: `1810` <dbl>, `1811` <dbl>,
## #   `1812` <dbl>, `1813` <dbl>, `1814` <dbl>, `1815` <dbl>, `1816` <dbl>,
## #   `1817` <dbl>, `1818` <dbl>, `1819` <dbl>, `1820` <dbl>, `1821` <dbl>,
## #   `1822` <dbl>, `1823` <dbl>, `1824` <dbl>, `1825` <dbl>, `1826` <dbl>,
## #   `1827` <dbl>, `1828` <dbl>, `1829` <dbl>, `1830` <dbl>, `1831` <dbl>,
## #   `1832` <dbl>, `1833` <dbl>, `1834` <dbl>, `1835` <dbl>, `1836` <dbl>,
## #   `1837` <dbl>, `1838` <dbl>, `1839` <dbl>, `1840` <dbl>, `1841` <dbl>,
## #   `1842` <dbl>, `1843` <dbl>, `1844` <dbl>, `1845` <dbl>, `1846` <dbl>,
## #   `1847` <dbl>, `1848` <dbl>, `1849` <dbl>, `1850` <dbl>, `1851` <dbl>,
## #   `1852` <dbl>, `1853` <dbl>, `1854` <dbl>, `1855` <dbl>, `1856` <dbl>,
## #   `1857` <dbl>, `1858` <dbl>, `1859` <dbl>, `1860` <dbl>, `1861` <dbl>,
## #   `1862` <dbl>, `1863` <dbl>, `1864` <dbl>, `1865` <dbl>, `1866` <dbl>,
## #   `1867` <dbl>, `1868` <dbl>, `1869` <dbl>, `1870` <dbl>, `1871` <dbl>,
## #   `1872` <dbl>, `1873` <dbl>, `1874` <dbl>, `1875` <dbl>, `1876` <dbl>,
## #   `1877` <dbl>, `1878` <dbl>, `1879` <dbl>, `1880` <dbl>, `1881` <dbl>,
## #   `1882` <dbl>, `1883` <dbl>, `1884` <dbl>, `1885` <dbl>, `1886` <dbl>,
## #   `1887` <dbl>, `1888` <dbl>, `1889` <dbl>, `1890` <dbl>, `1891` <dbl>,
## #   `1892` <dbl>, `1893` <dbl>, `1894` <dbl>, `1895` <dbl>, `1896` <dbl>,
## #   `1897` <dbl>, `1898` <dbl>, `1899` <dbl>, `1900` <dbl>, `1901` <dbl>,
## #   `1902` <dbl>, `1903` <dbl>, `1904` <dbl>, `1905` <dbl>, `1906` <dbl>,
## #   `1907` <dbl>, `1908` <dbl>, `1909` <dbl>, ...
```

1. (3 points) Once you have an idea of the structure of the data, please make each data set tidy and store them as new objects. You will need both the original and tidy data!

```r
population_tidy <- janitor::clean_names(population)
population_tidy <- population_tidy %>% 
  pivot_longer(-country,
               names_to = "year",
               values_to = "pop") %>% 
  mutate(year=as.numeric(str_replace(year, 'x', '')))
glimpse(population_tidy)
```

```
## Rows: 58,695
## Columns: 3
## $ country <chr> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan",...
## $ year    <dbl> 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809,...
## $ pop     <dbl> 3280000, 3280000, 3280000, 3280000, 3280000, 3280000, 32800...
```


```r
income_tidy <- janitor::clean_names(income)
income_tidy <- income_tidy %>% 
  pivot_longer(-country,
               names_to = "year",
               values_to = "inc") %>% 
  mutate(year=as.numeric(str_replace(year, 'x', '')))
glimpse(income_tidy)
```

```
## Rows: 46,513
## Columns: 3
## $ country <chr> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan",...
## $ year    <dbl> 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809,...
## $ inc     <dbl> 603, 603, 603, 603, 603, 603, 603, 603, 603, 603, 604, 604,...
```


```r
life_expectancy_tidy <- janitor::clean_names(life_expectancy)
life_expectancy_tidy <- life_expectancy_tidy %>% 
  pivot_longer(-country,
               names_to = "year",
               values_to = "lifeExp") %>% 
  mutate(year=as.numeric(str_replace(year, 'x', '')))
glimpse(life_expectancy_tidy)
```

```
## Rows: 56,287
## Columns: 3
## $ country <chr> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan",...
## $ year    <dbl> 1800, 1801, 1802, 1803, 1804, 1805, 1806, 1807, 1808, 1809,...
## $ lifeExp <dbl> 28.2, 28.2, 28.2, 28.2, 28.2, 28.2, 28.1, 28.1, 28.1, 28.1,...
```

2. (1 point) How many different countries are represented in the data? Provide the total number and their names. Since each data set includes different numbers of countries, you will need to do this for each one.

```r
population_tidy %>% 
  group_by(country) %>% 
  count(country, sort = T) %>% 
  summarize(n_distinct(country),.groups = 'keep')
```

```
## # A tibble: 195 x 2
## # Groups:   country [195]
##    country             `n_distinct(country)`
##    <chr>                               <int>
##  1 Afghanistan                             1
##  2 Albania                                 1
##  3 Algeria                                 1
##  4 Andorra                                 1
##  5 Angola                                  1
##  6 Antigua and Barbuda                     1
##  7 Argentina                               1
##  8 Armenia                                 1
##  9 Australia                               1
## 10 Austria                                 1
## # ... with 185 more rows
```


```r
income_tidy %>% 
  group_by(country) %>% 
  count(country, sort = T) %>% 
  summarize(n_distinct(country),.groups = 'keep')
```

```
## # A tibble: 193 x 2
## # Groups:   country [193]
##    country             `n_distinct(country)`
##    <chr>                               <int>
##  1 Afghanistan                             1
##  2 Albania                                 1
##  3 Algeria                                 1
##  4 Andorra                                 1
##  5 Angola                                  1
##  6 Antigua and Barbuda                     1
##  7 Argentina                               1
##  8 Armenia                                 1
##  9 Australia                               1
## 10 Austria                                 1
## # ... with 183 more rows
```


```r
life_expectancy_tidy %>% 
  group_by(country) %>% 
  count(country, sort = T) %>% 
  summarize(n_distinct(country),.groups = 'keep')
```

```
## # A tibble: 187 x 2
## # Groups:   country [187]
##    country             `n_distinct(country)`
##    <chr>                               <int>
##  1 Afghanistan                             1
##  2 Albania                                 1
##  3 Algeria                                 1
##  4 Andorra                                 1
##  5 Angola                                  1
##  6 Antigua and Barbuda                     1
##  7 Argentina                               1
##  8 Armenia                                 1
##  9 Australia                               1
## 10 Austria                                 1
## # ... with 177 more rows
```

## Life Expectancy  

3. (2 points) Let's limit the data to the past 100 years (1920-2020). For these years, which country has the highest life expectancy? How about the lowest life expectancy?  

Highest

```r
life_expectancy_tidy %>% 
  group_by(country, year) %>% 
  filter(year>=1920, year<=2020) %>% 
  summarize(max_lifeExp=max(lifeExp),.groups = 'keep') %>% 
  arrange(desc(max_lifeExp)) %>% 
  head(n=1)
```

```
## # A tibble: 1 x 3
## # Groups:   country, year [1]
##   country    year max_lifeExp
##   <chr>     <dbl>       <dbl>
## 1 Singapore  2020        85.3
```

Lowest

```r
life_expectancy_tidy %>% 
  group_by(country, year) %>% 
  filter(year>=1920, year<=2020) %>% 
  summarize(low_lifeExp=min(lifeExp),.groups = 'keep') %>% 
  arrange(low_lifeExp) %>% 
  head(n=1)
```

```
## # A tibble: 1 x 3
## # Groups:   country, year [1]
##   country     year low_lifeExp
##   <chr>      <dbl>       <dbl>
## 1 Kazakhstan  1933        4.07
```

4. (3 points) Although we can see which country has the highest life expectancy for the past 100 years, we don't know which countries have changed the most. What are the top 5 countries that have experienced the biggest improvement in life expectancy between 1920-2020?

```r
life_expectancy_tidy %>% 
  group_by(country) %>% 
  filter(year>=1920, year <=2020) %>% 
  summarize(lifeExp_change=diff(lifeExp),.groups = 'keep') %>% 
  arrange(desc(lifeExp_change)) %>% 
  head(n=5)
```

```
## # A tibble: 5 x 2
## # Groups:   country [5]
##   country    lifeExp_change
##   <chr>               <dbl>
## 1 Rwanda               36.9
## 2 Kazakhstan           34.8
## 3 Lithuania            34  
## 4 Moldova              33.3
## 5 Germany              31.5
```

The top 5 countries that have experienced the biggest improvement in life expectancy between 1920-2020 include Rwanda, Kazakhstan, Lithuania, Moldova, and Germany. Below I display this improvement with a visual. 



```r
life_expectancy_tidy %>% 
  group_by(country) %>% 
  filter(year>=1920, year <=2020) %>%
  filter(country=="Rwanda" | country=="Kazakhstan" | country=="Lithuania" | country=="Moldova" | country=="Germany") %>%
  ggplot(aes(x=year, y=lifeExp)) +
  geom_line(aes(color=country))+
  labs(title = "Life Expectancy (1990-2020)",
       x="Year",
       y="Life Expectancy")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5, face = "bold"),
        axis.text = element_text(size = 11),
        axis.title = element_text(size = 14))
```

![](midterm_2_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

5. (3 points) Make a plot that shows the change over the past 100 years for the country with the biggest improvement in life expectancy. Be sure to add appropriate aesthetics to make the plot clean and clear. Once you have made the plot, do a little internet searching and see if you can discover what historical event may have contributed to this remarkable change.  


```r
life_expectancy_tidy %>% 
  group_by(country) %>% 
  filter(year>=1920, year <=2020) %>% 
  filter(country=="Rwanda") %>% 
  ggplot(aes(x=year, y=lifeExp))+
  geom_line() + geom_point(shape=1)+
  theme(plot.title=element_text(hjust=0.5), axis.text.x = element_text(angle = 45, hjust=1))+
  labs(title ="Life Expectancy Change for Rwanda Between 1920-2020",
       x="Year",
       y="Life Expectancy")
```

![](midterm_2_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

The steep drop in population is due to the 1994 genocide, which resulted ni deaths of up to one million people coupled with the displacement of millions more. Sources site that following the genocide in Rwanda, this country became one of the poorest countries in the world with the highest child mortality and lowest life expectancy at birth *anywhere*. The remarkable change following this tragic event could be attributed to the recovery of this country and improved vaccines, recovering economy, and support from governments and other alliances.  

## Population Growth
6. (3 points) Which 5 countries have had the highest population growth over the past 100 years (1920-2020)?

```r
population_tidy %>% 
  filter(year==1920 | year==2020) %>% 
  group_by(country) %>% 
  summarize(large_pop_growth=diff(pop),.groups = 'keep') %>% 
  arrange(desc(large_pop_growth)) %>% 
  head(n=5)
```

```
## # A tibble: 5 x 2
## # Groups:   country [5]
##   country       large_pop_growth
##   <chr>                    <dbl>
## 1 India               1063000000
## 2 China                968000000
## 3 Indonesia            226700000
## 4 United States        220000000
## 5 Pakistan             199300000
```

7. (4 points) Produce a plot that shows the 5 countries that have had the highest population growth over the past 100 years (1920-2020). Which countries appear to have had exponential growth?  

```r
population_tidy %>% 
  filter(year==1920 | year==2020) %>% 
  group_by(country) %>% 
  summarize(large_pop_growth=diff(pop),.groups = 'keep') %>% 
  arrange(desc(large_pop_growth)) %>% 
  head(n=5) %>% 
ggplot(aes(x=country, y=large_pop_growth, fill=country))+
  geom_col()+coord_flip()+
  scale_fill_brewer(palette = "Accent")+
  theme_light()+
  labs(title = "Top 5 Countries With Largest Population Growth 1920-2020",
       x = "Country",
       y = "Population Growth")
```

![](midterm_2_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

China and India appear to have had the greatest exponential growth compared to the other 3 countries. 

## Income
The units used for income are gross domestic product per person adjusted for differences in purchasing power in international dollars.

8. (4 points) As in the previous questions, which countries have experienced the biggest growth in per person GDP. Show this as a table and then plot the changes for the top 5 countries. With a bit of research, you should be able to explain the dramatic downturns of the wealthiest economies that occurred during the 1980's.

```r
income_tidy %>% 
  filter(year==1920 | year==2020) %>% 
  group_by(country) %>% 
  summarize(GDP_growth=diff(inc),.groups = 'keep') %>% 
  arrange(desc(GDP_growth)) %>% 
  head(n=5)
```

```
## # A tibble: 5 x 2
## # Groups:   country [5]
##   country    GDP_growth
##   <chr>           <dbl>
## 1 Qatar          113700
## 2 Luxembourg      89370
## 3 Singapore       88060
## 4 Brunei          72970
## 5 Ireland         68930
```


```r
income_tidy %>% 
  filter(between(year,1920,2020)) %>%
  filter(country=="Qatar" | country=="Luxembourg" | country=="Singapore" | country=="Brunei" | country=="Ireland") %>%
  ggplot(aes(x=year, y=inc)) +
  geom_line(aes(color=country))+
  labs(title = "GDP Growth (1920-2020)",
       x="Year",
       y="Per Person GDP")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5, face = "bold"),
        axis.text = element_text(size = 11),
        axis.title = element_text(size = 14))
```

![](midterm_2_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
The dramatic downturn of the wealthiest economies during the 1980's was due to the severe economic recession. 

9. (3 points) Create three new objects that restrict each data set (life expectancy, population, income) to the years 1920-2020. Hint: I suggest doing this with the long form of your data. Once this is done, merge all three data sets using the code I provide below. You may need to adjust the code depending on how you have named your objects. I called mine `life_expectancy_100`, `population_100`, and `income_100`. For some of you, learning these `joins` will be important for your project.  

life_expectancy_100

```r
life_expectancy_100 <- life_expectancy_tidy %>% 
  filter(year>=1920, year <=2020)
life_expectancy_100
```

```
## # A tibble: 18,887 x 3
##    country      year lifeExp
##    <chr>       <dbl>   <dbl>
##  1 Afghanistan  1920    30.6
##  2 Afghanistan  1921    30.7
##  3 Afghanistan  1922    30.8
##  4 Afghanistan  1923    30.8
##  5 Afghanistan  1924    30.9
##  6 Afghanistan  1925    31  
##  7 Afghanistan  1926    31  
##  8 Afghanistan  1927    31.1
##  9 Afghanistan  1928    31.1
## 10 Afghanistan  1929    31.2
## # ... with 18,877 more rows
```

population_100

```r
population_100 <- population_tidy %>% 
  filter(year>=1920, year<=2020)
population_100
```

```
## # A tibble: 19,695 x 3
##    country      year      pop
##    <chr>       <dbl>    <dbl>
##  1 Afghanistan  1920 10600000
##  2 Afghanistan  1921 10500000
##  3 Afghanistan  1922 10300000
##  4 Afghanistan  1923  9710000
##  5 Afghanistan  1924  9200000
##  6 Afghanistan  1925  8720000
##  7 Afghanistan  1926  8260000
##  8 Afghanistan  1927  7830000
##  9 Afghanistan  1928  7420000
## 10 Afghanistan  1929  7100000
## # ... with 19,685 more rows
```

income_100

```r
income_100 <- income_tidy %>% 
  filter(year>=1920, year<=2020)
income_100
```

```
## # A tibble: 19,493 x 3
##    country      year   inc
##    <chr>       <dbl> <dbl>
##  1 Afghanistan  1920  1490
##  2 Afghanistan  1921  1520
##  3 Afghanistan  1922  1550
##  4 Afghanistan  1923  1570
##  5 Afghanistan  1924  1600
##  6 Afghanistan  1925  1630
##  7 Afghanistan  1926  1650
##  8 Afghanistan  1927  1680
##  9 Afghanistan  1928  1710
## 10 Afghanistan  1929  1740
## # ... with 19,483 more rows
```


```r
gapminder_join <- inner_join(life_expectancy_100, population_100, by= c("country", "year"))
gapminder_join <- inner_join(gapminder_join, income_100, by= c("country", "year"))
gapminder_join
```

```
## # A tibble: 18,887 x 5
##    country      year lifeExp      pop   inc
##    <chr>       <dbl>   <dbl>    <dbl> <dbl>
##  1 Afghanistan  1920    30.6 10600000  1490
##  2 Afghanistan  1921    30.7 10500000  1520
##  3 Afghanistan  1922    30.8 10300000  1550
##  4 Afghanistan  1923    30.8  9710000  1570
##  5 Afghanistan  1924    30.9  9200000  1600
##  6 Afghanistan  1925    31    8720000  1630
##  7 Afghanistan  1926    31    8260000  1650
##  8 Afghanistan  1927    31.1  7830000  1680
##  9 Afghanistan  1928    31.1  7420000  1710
## 10 Afghanistan  1929    31.2  7100000  1740
## # ... with 18,877 more rows
```

10. (4 points) Use the joined data to perform an analysis of your choice. The analysis should include a comparison between two or more of the variables `life_expectancy`, `population`, or `income.`

```r
gapminder_join %>% 
  filter(country=="Pakistan" | country=="India"| country=="China"| country=="Indonesia") %>% 
  ggplot(aes(x=lifeExp, y=inc, color=country, shape=country))+
  geom_point(alpha=1, size=0.75)+
  labs(title="Life Expectancy vs GDP (1920-2020)",
       x = "Life Expectancy (Years)",
       y =  "GDP")
```

![](midterm_2_files/figure-html/unnamed-chunk-24-1.png)<!-- -->
