---
title: "Midterm 1"
author: "Brian Rezende"
date: "2021-01-29"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  
All three of these components come together to work in unison, as R is the literal scripting language that is common among biostatisticians, coupled with being recognized in RStudio. RStudio is a GUI (graphical user interface) that allows programmers to input, store, and share code, which allows for converting data into clean and presentable markdown files. GitHub is home to users public and private repositories and it tracks all changes made in users programming projects. This platform is useful to programmers since it's open-source thus producing a massive community that can share code revisions, new ideas, and collaboration. The beauty of using RMmarkdown for all of this is to ensure reproducibility, since code chunks can be embedded into an .Rmd file. The output on an .Rmd file is clean, easy to read, and allows other programmers to get a sense of your analysis skills and the direction of your project. 

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**
The three types of data structures discussed so far include vector, data matrices, and data frames. Though each data structure varies, we utilize data frames in this course in order to practice data manipulation through working with a collection of variables and characters. Data frames allow us to streamline all the skills we learn and will continue to in this course in the context of biological, or out of this world (superhero), data. 


In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17...
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204....
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", ...
```

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- janitor::clean_names(elephants)
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

**5. (2 points) How many male and female elephants are represented in the data?**

```r
elephants %>% 
  group_by(sex) %>% 
  count(sex)
```

```
## # A tibble: 2 x 2
## # Groups:   sex [2]
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```
The data represents 150 females and 138 male elephants. 

**6. (2 points) What is the average age all elephants in the data?**

```r
mean(elephants$age)
```

```
## [1] 10.97132
```
For all elephants in this data set, the average age is 10.97132. 

**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>% 
  group_by(sex) %>%
  summarize(mean(age), mean(height),
            totaln=n(),.groups = 'keep')
```

```
## # A tibble: 2 x 4
## # Groups:   sex [2]
##   sex   `mean(age)` `mean(height)` totaln
##   <fct>       <dbl>          <dbl>  <int>
## 1 F           12.8            190.    150
## 2 M            8.95           185.    138
```
In females we observe an average age of 12.83 years and an average height of 190cm. 
In males we observe a younger average age of 8.94 years and an average height of 185 cm. 

**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>%
  filter(age>25)%>%
  group_by(sex) %>% 
  summarize(min_height=min(height), max_height=max(height), mean(height),
            total_elephants=n(),.groups = 'keep')
```

```
## # A tibble: 2 x 5
## # Groups:   sex [2]
##   sex   min_height max_height `mean(height)` total_elephants
##   <fct>      <dbl>      <dbl>          <dbl>           <int>
## 1 F           206.       278.           233.              25
## 2 M           237.       304.           273.               8
```
For elephants that are older than 25 years old, we see that their average height changes a bit. Now on average the older male elephants are taller than their female counterparts. Even in the min and max parameters, male elephants are dominant in the sense that they're taller. 


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
vertebrate_com <- readr::read_csv('data/IvindoData_DryadVersion.csv')
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
glimpse(vertebrate_com)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 1...
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24...
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None",...
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46,...
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", ...
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 1...
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 3...
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38...
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 5...
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4...
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2...
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, ...
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 4...
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0...
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 4...
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1...
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10...
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21,...
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0...
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2...
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11...
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0...
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1...
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 1...
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0...
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1...
```


```r
vertebrate_com$HuntCat <- as.factor(vertebrate_com$HuntCat)
class(vertebrate_com$HuntCat)
```

```
## [1] "factor"
```


```r
vertebrate_com$LandUse <- as.factor(vertebrate_com$LandUse)
class(vertebrate_com$LandUse)
```

```
## [1] "factor"
```

**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
vertebrate_com %>%
  group_by(HuntCat) %>%
  filter(HuntCat!="None") %>% 
  summarize(avg_bird_diversity=mean(Diversity_BirdSpecies), 
            avg_mammal_diversity=mean(Diversity_MammalSpecies),
            totaln=n(),.groups = 'keep')
```

```
## # A tibble: 2 x 4
## # Groups:   HuntCat [2]
##   HuntCat  avg_bird_diversity avg_mammal_diversity totaln
##   <fct>                 <dbl>                <dbl>  <int>
## 1 High                   1.66                 1.74      7
## 2 Moderate               1.62                 1.68      8
```
Between the averages of bird and mammal density, these two groups are fairly close. The average diverity is close, even despite the hunting category. 

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
vertebrate_com %>%
  select(TransectID, HuntCat, Distance, starts_with("RA_")) %>% 
  group_by(Distance) %>% 
  arrange(desc(Distance))
```

```
## # A tibble: 24 x 9
## # Groups:   Distance [23]
##    TransectID HuntCat Distance RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent
##         <dbl> <fct>      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>
##  1         24 None        26.8    4.91     31.6        0          54.1      1.29
##  2          6 None        24.1    3.78     42.7        1.11       46.2      3.1 
##  3          3 None        20.8   12.9      59.3        0.56       19.8      3.66
##  4          7 None        19.8    6.17     33.8        0.43       49.5      1.26
##  5         18 None        18.8    0.51     57.4        2.3        35.1      2.09
##  6          2 None        18.3    4.49     37.4        1.33       41.8      1.06
##  7          5 None        17.5    2.48     38.6        0          43.3      1.83
##  8          2 None        17.3    0        52.2        0.86       28.5      6.04
##  9          4 None        16.0    0        52.6        1          41.3      2.52
## 10         25 Modera~     15.0    1.28     62.0        0          23.9      3.97
## # ... with 14 more rows, and 1 more variable: RA_Ungulate <dbl>
```


```r
danger_zone <- vertebrate_com %>% 
  filter(Distance < 5) %>% 
  select(TransectID, Distance, HuntCat, starts_with("RA_")) %>% 
  summarize(across(starts_with("RA_"), mean))
danger_zone
```

```
## # A tibble: 1 x 6
##   RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    0.08     70.4      0.0967       24.1      3.66        1.59
```


```r
vertebrate_com %>% 
  filter(Distance > 20) %>% 
  select(TransectID, Distance, starts_with("RA_")) %>%
  summarize(across(contains("RA_"), mean))
```

```
## # A tibble: 1 x 6
##   RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    7.21     44.5       0.557       40.1      2.68        4.98
```
This analysis shows that the farther away from a village (20 km vs 5 km) there is a noticable difference among the relative abundance of these vertebrates. All categories, except for rodents, demonstrate a higher abundance the further away a transect is. 

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
vertebrate_com %>%
  group_by(NumHouseholds) %>% 
  summarize(across(c(Veg_Rich, Veg_liana, Veg_DBH, Veg_Canopy), mean),
            .groups = 'keep') %>% 
  arrange(desc(NumHouseholds))
```

```
## # A tibble: 11 x 5
## # Groups:   NumHouseholds [11]
##    NumHouseholds Veg_Rich Veg_liana Veg_DBH Veg_Canopy
##            <dbl>    <dbl>     <dbl>   <dbl>      <dbl>
##  1            73     17.4     12.9     52.4       3.32
##  2            56     15.8     14.1     52.1       3.57
##  3            54     15.1     10.4     49.4       3.67
##  4            46     11.6      8.12    49.2       3.32
##  5            36     14.6     11.9     38.1       3.75
##  6            29     15.7      9.84    43.4       3.57
##  7            25     12.6      5.13    45.2       3   
##  8            24     14.8     12.9     45.7       3.38
##  9            19     14.2     16.4     57.7       3.25
## 10            17     11.8     12.2     42.9       3.29
## 11            13     14.8     13.1     33.9       3.29
```

Since this is a paper on a defaunation gradient in Gabon, I was curious to see if there were any possible correlations with the number of households near a transect against the average vegetation abundance. I am wondering if population growth and industrialization of the Gabon landscape could possible influence the extinction of these vertebrates. Next, it would be interesting to combine both the "RA_" data with the table above to get a bigger picture of the environment. 
