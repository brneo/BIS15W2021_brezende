---
title: "Lab 5 Homework"
author: "Brian Rezende"
date: "2021-01-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## i Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
superhero_info <- rename(superhero_info, gender = "Gender", eye_color = "Eye color", race = "Race", hair_color = "Hair color", height = "Height", publisher = "Publisher", skin_color = "Skin color", alignment = "Alignment", weight = "Weight")
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He~ `Lantern Power ~ `Dimensional Aw~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati~ FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, ...
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
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

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
superhero_info %>% 
  filter(alignment == "neutral")
```

```
## # A tibble: 24 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Biza~ Male   black     Biza~ Black         191 DC Comics white      neutral  
##  2 Blac~ Male   <NA>      God ~ <NA>           NA DC Comics <NA>       neutral  
##  3 Capt~ Male   brown     Human Brown          NA DC Comics <NA>       neutral  
##  4 Copy~ Female red       Muta~ White         183 Marvel C~ blue       neutral  
##  5 Dead~ Male   brown     Muta~ No Hair       188 Marvel C~ <NA>       neutral  
##  6 Deat~ Male   blue      Human White         193 DC Comics <NA>       neutral  
##  7 Etri~ Male   red       Demon No Hair       193 DC Comics yellow     neutral  
##  8 Gala~ Male   black     Cosm~ Black         876 Marvel C~ <NA>       neutral  
##  9 Glad~ Male   blue      Stro~ Blue          198 Marvel C~ purple     neutral  
## 10 Indi~ Female <NA>      Alien Purple         NA DC Comics <NA>       neutral  
## # ... with 14 more rows, and 1 more variable: weight <dbl>
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(name, alignment, race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>%
  filter(race!="Human")
```

```
## # A tibble: 222 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  2 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  3 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  4 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  7 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp~ <NA>           NA Dark Hor~ <NA>       good     
##  9 Ange~ Female yellow    Muta~ Black         165 Marvel C~ <NA>       good     
## 10 Anti~ Male   yellow    God ~ No Hair        61 DC Comics <NA>       bad      
## # ... with 212 more rows, and 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
superhero_info %>%
  filter(alignment == "good")
```

```
## # A tibble: 496 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Adam~ Male   blue      <NA>  Blond          NA NBC - He~ <NA>       good     
##  5 Adam~ Male   blue      Human Blond         185 DC Comics <NA>       good     
##  6 Agen~ Female blue      <NA>  Blond         173 Marvel C~ <NA>       good     
##  7 Agen~ Male   brown     Human Brown         178 Marvel C~ <NA>       good     
##  8 Agen~ Male   <NA>      <NA>  <NA>          191 Marvel C~ <NA>       good     
##  9 Alan~ Male   blue      <NA>  Blond         180 DC Comics <NA>       good     
## 10 Alex~ Male   <NA>      <NA>  <NA>           NA NBC - He~ <NA>       good     
## # ... with 486 more rows, and 1 more variable: weight <dbl>
```

```r
superhero_info %>% 
  filter(alignment == "bad")
```

```
## # A tibble: 207 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  2 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  3 Abso~ Male   blue      Human No Hair       193 Marvel C~ <NA>       bad      
##  4 Air-~ Male   blue      <NA>  White         188 Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alex~ Male   <NA>      Human <NA>           NA Wildstorm <NA>       bad      
##  7 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  8 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  9 Ammo  Male   brown     Human Black         188 Marvel C~ <NA>       bad      
## 10 Ange~ Female <NA>      <NA>  <NA>           NA Image Co~ <NA>       bad      
## # ... with 197 more rows, and 1 more variable: weight <dbl>
```


6. For the good guys, use the `tabyl` function to summarize their "race".

```r
superhero_info %>% 
  filter(alignment == "good") %>% 
  tabyl(race, show_na = F)
```

```
##               race   n     percent
##              Alien   3 0.010752688
##              Alpha   5 0.017921147
##             Amazon   2 0.007168459
##            Android   4 0.014336918
##             Animal   2 0.007168459
##          Asgardian   3 0.010752688
##          Atlantean   4 0.014336918
##         Bolovaxian   1 0.003584229
##              Clone   1 0.003584229
##             Cyborg   3 0.010752688
##           Demi-God   2 0.007168459
##              Demon   3 0.010752688
##            Eternal   1 0.003584229
##     Flora Colossus   1 0.003584229
##        Frost Giant   1 0.003584229
##      God / Eternal   6 0.021505376
##             Gungan   1 0.003584229
##              Human 148 0.530465950
##         Human-Kree   2 0.007168459
##      Human-Spartoi   1 0.003584229
##       Human-Vulcan   1 0.003584229
##    Human-Vuldarian   1 0.003584229
##    Human / Altered   2 0.007168459
##     Human / Cosmic   2 0.007168459
##  Human / Radiation   8 0.028673835
##      Icthyo Sapien   1 0.003584229
##            Inhuman   4 0.014336918
##    Kakarantharaian   1 0.003584229
##         Kryptonian   4 0.014336918
##            Martian   1 0.003584229
##          Metahuman   1 0.003584229
##             Mutant  46 0.164874552
##     Mutant / Clone   1 0.003584229
##             Planet   1 0.003584229
##             Saiyan   1 0.003584229
##           Symbiote   3 0.010752688
##           Talokite   1 0.003584229
##         Tamaranean   1 0.003584229
##            Ungaran   1 0.003584229
##            Vampire   2 0.007168459
##     Yoda's species   1 0.003584229
##      Zen-Whoberian   1 0.003584229
```

7. Among the good guys, Who are the Asgardians?

```r
superhero_info %>% 
  filter(alignment == "good") %>% 
  filter(race == "Asgardian")
```

```
## # A tibble: 3 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif   Female blue      Asga~ Black         188 Marvel C~ <NA>       good     
## 2 Thor  Male   blue      Asga~ Blond         198 Marvel C~ <NA>       good     
## 3 Thor~ Female blue      Asga~ Blond         175 Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
superhero_info %>% 
  filter(alignment == "bad") %>% 
  filter(race == "Human", gender == "Male") %>%
  filter(height >= 200)
```

```
## # A tibble: 5 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
## 2 Doct~ Male   brown     Human Brown         201 Marvel C~ <NA>       bad      
## 3 King~ Male   blue      Human No Hair       201 Marvel C~ <NA>       bad      
## 4 Liza~ Male   red       Human No Hair       203 Marvel C~ <NA>       bad      
## 5 Scor~ Male   brown     Human Brown         211 Marvel C~ <NA>       bad      
## # ... with 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
superhero_info %>% 
  select(name, alignment, hair_color) %>% 
  filter(alignment=="good", hair_color == "No Hair") %>% 
  tabyl(alignment, hair_color)
```

```
##  alignment No Hair
##       good      37
```

```r
superhero_info %>% 
  select(name, alignment, hair_color) %>% 
  filter(alignment == "bad", hair_color == "No Hair") %>% 
  tabyl(alignment, hair_color)
```

```
##  alignment No Hair
##        bad      35
```
There appears to be more good guys that are bald, but the number of bad guys that are bald are trailing right behind. 


10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
superhero_info %>% 
  select(name, race, alignment, height, weight) %>% 
  filter(height >= 200|weight >= 300) %>% 
  arrange(height, weight)
```

```
## # A tibble: 65 x 5
##    name               race              alignment height weight
##    <chr>              <chr>             <chr>      <dbl>  <dbl>
##  1 Giganta            <NA>              bad         62.5    630
##  2 Machine Man        <NA>              good       183      383
##  3 Destroyer          <NA>              bad        188      383
##  4 Drax the Destroyer Human / Altered   good       193      306
##  5 Rhino              Human / Radiation bad        196      320
##  6 Living Brain       <NA>              bad        198      360
##  7 Sinestro           Korugaran         neutral    201       92
##  8 Century            Alien             good       201       97
##  9 Steel              <NA>              good       201      131
## 10 Doctor Doom II     <NA>              bad        201      132
## # ... with 55 more rows
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>% 
  filter(height >= 300)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin ~ Male   red       Kaka~ No Hair      975  Marvel C~ green      good     
## 2 Gala~ Male   black     Cosm~ Black        876  Marvel C~ <NA>       neutral  
## 3 Groot Male   yellow    Flor~ <NA>         701  Marvel C~ <NA>       good     
## 4 MODOK Male   white     Cybo~ Brownn       366  Marvel C~ <NA>       bad      
## 5 Onsl~ Male   red       Muta~ No Hair      305  Marvel C~ <NA>       bad      
## 6 Sasq~ Male   red       <NA>  Orange       305  Marvel C~ <NA>       good     
## 7 Wolf~ Female green     <NA>  Auburn       366  Marvel C~ <NA>       good     
## 8 Ymir  Male   white     Fros~ No Hair      305. Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>% 
  filter(weight >= 450)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
## 2 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 3 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
## 5 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
## 6 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 7 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 8 Wolf~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

We would not have 16 rows in question 10 due to the use of the "|" operator. In question 10 I created a code that presented all superheros within a particular criteria because it produces an "or table". For instance, "Giganta" is a superhero that appears in question #12, but not in question #11. However, "Giganta" is the first result in question #10 since she fits in one of the two categories. The "|" operator outputs those who meet at least one of the weight/height requirements.

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
superhero_info %>% 
  mutate(height_weight_ratio=height/weight) %>% 
  select(name, height_weight_ratio) %>% 
  arrange(desc(height_weight_ratio))
```

```
## # A tibble: 734 x 2
##    name            height_weight_ratio
##    <chr>                         <dbl>
##  1 Groot                        175.  
##  2 Galactus                      54.8 
##  3 Fin Fang Foom                 54.2 
##  4 Longshot                       5.22
##  5 Jack-Jack                      5.07
##  6 Rocket Raccoon                 4.88
##  7 Dash                           4.52
##  8 Howard the Duck                4.39
##  9 Swarm                          4.17
## 10 Yoda                           3.88
## # ... with 724 more rows
```

Based on my calculations, it appears that **Groot**, **Galactus**, and **Fin Fang Foom** have the highest height to weight ratio. 

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
names(superhero_powers)
```

```
##   [1] "hero_names"                   "agility"                     
##   [3] "accelerated_healing"          "lantern_power_ring"          
##   [5] "dimensional_awareness"        "cold_resistance"             
##   [7] "durability"                   "stealth"                     
##   [9] "energy_absorption"            "flight"                      
##  [11] "danger_sense"                 "underwater_breathing"        
##  [13] "marksmanship"                 "weapons_master"              
##  [15] "power_augmentation"           "animal_attributes"           
##  [17] "longevity"                    "intelligence"                
##  [19] "super_strength"               "cryokinesis"                 
##  [21] "telepathy"                    "energy_armor"                
##  [23] "energy_blasts"                "duplication"                 
##  [25] "size_changing"                "density_control"             
##  [27] "stamina"                      "astral_travel"               
##  [29] "audio_control"                "dexterity"                   
##  [31] "omnitrix"                     "super_speed"                 
##  [33] "possession"                   "animal_oriented_powers"      
##  [35] "weapon_based_powers"          "electrokinesis"              
##  [37] "darkforce_manipulation"       "death_touch"                 
##  [39] "teleportation"                "enhanced_senses"             
##  [41] "telekinesis"                  "energy_beams"                
##  [43] "magic"                        "hyperkinesis"                
##  [45] "jump"                         "clairvoyance"                
##  [47] "dimensional_travel"           "power_sense"                 
##  [49] "shapeshifting"                "peak_human_condition"        
##  [51] "immortality"                  "camouflage"                  
##  [53] "element_control"              "phasing"                     
##  [55] "astral_projection"            "electrical_transport"        
##  [57] "fire_control"                 "projection"                  
##  [59] "summoning"                    "enhanced_memory"             
##  [61] "reflexes"                     "invulnerability"             
##  [63] "energy_constructs"            "force_fields"                
##  [65] "self_sustenance"              "anti_gravity"                
##  [67] "empathy"                      "power_nullifier"             
##  [69] "radiation_control"            "psionic_powers"              
##  [71] "elasticity"                   "substance_secretion"         
##  [73] "elemental_transmogrification" "technopath_cyberpath"        
##  [75] "photographic_reflexes"        "seismic_power"               
##  [77] "animation"                    "precognition"                
##  [79] "mind_control"                 "fire_resistance"             
##  [81] "power_absorption"             "enhanced_hearing"            
##  [83] "nova_force"                   "insanity"                    
##  [85] "hypnokinesis"                 "animal_control"              
##  [87] "natural_armor"                "intangibility"               
##  [89] "enhanced_sight"               "molecular_manipulation"      
##  [91] "heat_generation"              "adaptation"                  
##  [93] "gliding"                      "power_suit"                  
##  [95] "mind_blast"                   "probability_manipulation"    
##  [97] "gravity_control"              "regeneration"                
##  [99] "light_control"                "echolocation"                
## [101] "levitation"                   "toxin_and_disease_control"   
## [103] "banish"                       "energy_manipulation"         
## [105] "heat_resistance"              "natural_weapons"             
## [107] "time_travel"                  "enhanced_smell"              
## [109] "illusions"                    "thirstokinesis"              
## [111] "hair_manipulation"            "illumination"                
## [113] "omnipotent"                   "cloaking"                    
## [115] "changing_armor"               "power_cosmic"                
## [117] "biokinesis"                   "water_control"               
## [119] "radiation_immunity"           "vision_telescopic"           
## [121] "toxin_and_disease_resistance" "spatial_awareness"           
## [123] "energy_resistance"            "telepathy_resistance"        
## [125] "molecular_combustion"         "omnilingualism"              
## [127] "portal_creation"              "magnetism"                   
## [129] "mind_control_resistance"      "plant_control"               
## [131] "sonar"                        "sonic_scream"                
## [133] "time_manipulation"            "enhanced_touch"              
## [135] "magic_resistance"             "invisibility"                
## [137] "sub_mariner"                  "radiation_absorption"        
## [139] "intuitive_aptitude"           "vision_microscopic"          
## [141] "melting"                      "wind_control"                
## [143] "super_breath"                 "wallcrawling"                
## [145] "vision_night"                 "vision_infrared"             
## [147] "grim_reaping"                 "matter_absorption"           
## [149] "the_force"                    "resurrection"                
## [151] "terrakinesis"                 "vision_heat"                 
## [153] "vitakinesis"                  "radar_sense"                 
## [155] "qwardian_power_ring"          "weather_control"             
## [157] "vision_x_ray"                 "vision_thermal"              
## [159] "web_creation"                 "reality_warping"             
## [161] "odin_force"                   "symbiote_costume"            
## [163] "speed_force"                  "phoenix_force"               
## [165] "molecular_dissipation"        "vision_cryo"                 
## [167] "omnipresent"                  "omniscient"
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>% 
  select(hero_names, accelerated_healing, durability, super_strength) %>% 
  filter(accelerated_healing == "TRUE", durability == "TRUE", super_strength == "TRUE")
```

```
## # A tibble: 97 x 4
##    hero_names   accelerated_healing durability super_strength
##    <chr>        <lgl>               <lgl>      <lgl>         
##  1 A-Bomb       TRUE                TRUE       TRUE          
##  2 Abe Sapien   TRUE                TRUE       TRUE          
##  3 Angel        TRUE                TRUE       TRUE          
##  4 Anti-Monitor TRUE                TRUE       TRUE          
##  5 Anti-Venom   TRUE                TRUE       TRUE          
##  6 Aquaman      TRUE                TRUE       TRUE          
##  7 Arachne      TRUE                TRUE       TRUE          
##  8 Archangel    TRUE                TRUE       TRUE          
##  9 Ardina       TRUE                TRUE       TRUE          
## 10 Ares         TRUE                TRUE       TRUE          
## # ... with 87 more rows
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
superhero_powers %>% 
  select(ends_with("kinesis"))
```

```
## # A tibble: 667 x 9
##    cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 FALSE       FALSE          FALSE       FALSE        FALSE       
##  2 FALSE       FALSE          FALSE       FALSE        FALSE       
##  3 FALSE       FALSE          FALSE       FALSE        FALSE       
##  4 FALSE       FALSE          FALSE       FALSE        FALSE       
##  5 FALSE       FALSE          FALSE       FALSE        FALSE       
##  6 FALSE       FALSE          FALSE       FALSE        FALSE       
##  7 FALSE       FALSE          FALSE       FALSE        FALSE       
##  8 FALSE       FALSE          FALSE       FALSE        FALSE       
##  9 FALSE       FALSE          FALSE       FALSE        FALSE       
## 10 FALSE       FALSE          FALSE       FALSE        FALSE       
## # ... with 657 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

This data demonstrates all of the possible superpowers that use some type of kinesis. Though it is not specific enough since it only shows the kinesis names, even though some superheros may not use any kind of kinesis. 


```r
kinesis <- superhero_powers %>% 
  select(ends_with("kinesis")) %>% 
  filter_all(any_vars(.=="TRUE"))
kinesis
```

```
## # A tibble: 112 x 9
##    cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 FALSE       FALSE          FALSE       FALSE        TRUE        
##  2 TRUE        FALSE          FALSE       FALSE        FALSE       
##  3 FALSE       FALSE          TRUE        FALSE        FALSE       
##  4 TRUE        FALSE          FALSE       FALSE        FALSE       
##  5 FALSE       FALSE          TRUE        FALSE        FALSE       
##  6 TRUE        FALSE          FALSE       FALSE        TRUE        
##  7 FALSE       FALSE          TRUE        FALSE        FALSE       
##  8 FALSE       FALSE          TRUE        FALSE        FALSE       
##  9 FALSE       TRUE           FALSE       FALSE        FALSE       
## 10 FALSE       FALSE          FALSE       FALSE        TRUE        
## # ... with 102 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

In contrast, this data is much smaller. By scrolling through the results, we can see that in at least one of the rows, a superhero can use at least one type of kinesis. This removes all the other heros who lack kinesis abilities. 

16. Pick your favorite superhero and let's see their powers!


```r
drstrange <- superhero_powers %>% 
  filter(hero_names == "Doctor Strange") %>% 
  filter_all(any_vars(.=="TRUE"))
glimpse(drstrange)
```

```
## Rows: 1
## Columns: 168
## $ hero_names                   <chr> "Doctor Strange"
## $ agility                      <lgl> FALSE
## $ accelerated_healing          <lgl> FALSE
## $ lantern_power_ring           <lgl> FALSE
## $ dimensional_awareness        <lgl> TRUE
## $ cold_resistance              <lgl> FALSE
## $ durability                   <lgl> FALSE
## $ stealth                      <lgl> FALSE
## $ energy_absorption            <lgl> FALSE
## $ flight                       <lgl> TRUE
## $ danger_sense                 <lgl> FALSE
## $ underwater_breathing         <lgl> FALSE
## $ marksmanship                 <lgl> FALSE
## $ weapons_master               <lgl> FALSE
## $ power_augmentation           <lgl> FALSE
## $ animal_attributes            <lgl> FALSE
## $ longevity                    <lgl> TRUE
## $ intelligence                 <lgl> FALSE
## $ super_strength               <lgl> FALSE
## $ cryokinesis                  <lgl> FALSE
## $ telepathy                    <lgl> TRUE
## $ energy_armor                 <lgl> FALSE
## $ energy_blasts                <lgl> TRUE
## $ duplication                  <lgl> FALSE
## $ size_changing                <lgl> FALSE
## $ density_control              <lgl> FALSE
## $ stamina                      <lgl> FALSE
## $ astral_travel                <lgl> FALSE
## $ audio_control                <lgl> FALSE
## $ dexterity                    <lgl> FALSE
## $ omnitrix                     <lgl> FALSE
## $ super_speed                  <lgl> FALSE
## $ possession                   <lgl> FALSE
## $ animal_oriented_powers       <lgl> FALSE
## $ weapon_based_powers          <lgl> FALSE
## $ electrokinesis               <lgl> FALSE
## $ darkforce_manipulation       <lgl> FALSE
## $ death_touch                  <lgl> FALSE
## $ teleportation                <lgl> TRUE
## $ enhanced_senses              <lgl> FALSE
## $ telekinesis                  <lgl> TRUE
## $ energy_beams                 <lgl> FALSE
## $ magic                        <lgl> TRUE
## $ hyperkinesis                 <lgl> FALSE
## $ jump                         <lgl> FALSE
## $ clairvoyance                 <lgl> FALSE
## $ dimensional_travel           <lgl> TRUE
## $ power_sense                  <lgl> FALSE
## $ shapeshifting                <lgl> FALSE
## $ peak_human_condition         <lgl> FALSE
## $ immortality                  <lgl> FALSE
## $ camouflage                   <lgl> FALSE
## $ element_control              <lgl> FALSE
## $ phasing                      <lgl> TRUE
## $ astral_projection            <lgl> TRUE
## $ electrical_transport         <lgl> FALSE
## $ fire_control                 <lgl> FALSE
## $ projection                   <lgl> FALSE
## $ summoning                    <lgl> TRUE
## $ enhanced_memory              <lgl> FALSE
## $ reflexes                     <lgl> FALSE
## $ invulnerability              <lgl> FALSE
## $ energy_constructs            <lgl> FALSE
## $ force_fields                 <lgl> TRUE
## $ self_sustenance              <lgl> FALSE
## $ anti_gravity                 <lgl> FALSE
## $ empathy                      <lgl> FALSE
## $ power_nullifier              <lgl> FALSE
## $ radiation_control            <lgl> FALSE
## $ psionic_powers               <lgl> FALSE
## $ elasticity                   <lgl> FALSE
## $ substance_secretion          <lgl> FALSE
## $ elemental_transmogrification <lgl> FALSE
## $ technopath_cyberpath         <lgl> FALSE
## $ photographic_reflexes        <lgl> FALSE
## $ seismic_power                <lgl> FALSE
## $ animation                    <lgl> FALSE
## $ precognition                 <lgl> FALSE
## $ mind_control                 <lgl> FALSE
## $ fire_resistance              <lgl> FALSE
## $ power_absorption             <lgl> FALSE
## $ enhanced_hearing             <lgl> FALSE
## $ nova_force                   <lgl> FALSE
## $ insanity                     <lgl> FALSE
## $ hypnokinesis                 <lgl> TRUE
## $ animal_control               <lgl> FALSE
## $ natural_armor                <lgl> FALSE
## $ intangibility                <lgl> TRUE
## $ enhanced_sight               <lgl> FALSE
## $ molecular_manipulation       <lgl> FALSE
## $ heat_generation              <lgl> FALSE
## $ adaptation                   <lgl> FALSE
## $ gliding                      <lgl> FALSE
## $ power_suit                   <lgl> FALSE
## $ mind_blast                   <lgl> FALSE
## $ probability_manipulation     <lgl> FALSE
## $ gravity_control              <lgl> FALSE
## $ regeneration                 <lgl> FALSE
## $ light_control                <lgl> FALSE
## $ echolocation                 <lgl> FALSE
## $ levitation                   <lgl> FALSE
## $ toxin_and_disease_control    <lgl> FALSE
## $ banish                       <lgl> TRUE
## $ energy_manipulation          <lgl> FALSE
## $ heat_resistance              <lgl> FALSE
## $ natural_weapons              <lgl> FALSE
## $ time_travel                  <lgl> TRUE
## $ enhanced_smell               <lgl> FALSE
## $ illusions                    <lgl> TRUE
## $ thirstokinesis               <lgl> FALSE
## $ hair_manipulation            <lgl> FALSE
## $ illumination                 <lgl> FALSE
## $ omnipotent                   <lgl> FALSE
## $ cloaking                     <lgl> FALSE
## $ changing_armor               <lgl> FALSE
## $ power_cosmic                 <lgl> FALSE
## $ biokinesis                   <lgl> FALSE
## $ water_control                <lgl> FALSE
## $ radiation_immunity           <lgl> FALSE
## $ vision_telescopic            <lgl> FALSE
## $ toxin_and_disease_resistance <lgl> FALSE
## $ spatial_awareness            <lgl> FALSE
## $ energy_resistance            <lgl> FALSE
## $ telepathy_resistance         <lgl> FALSE
## $ molecular_combustion         <lgl> FALSE
## $ omnilingualism               <lgl> FALSE
## $ portal_creation              <lgl> FALSE
## $ magnetism                    <lgl> FALSE
## $ mind_control_resistance      <lgl> FALSE
## $ plant_control                <lgl> FALSE
## $ sonar                        <lgl> FALSE
## $ sonic_scream                 <lgl> FALSE
## $ time_manipulation            <lgl> TRUE
## $ enhanced_touch               <lgl> FALSE
## $ magic_resistance             <lgl> FALSE
## $ invisibility                 <lgl> TRUE
## $ sub_mariner                  <lgl> FALSE
## $ radiation_absorption         <lgl> FALSE
## $ intuitive_aptitude           <lgl> FALSE
## $ vision_microscopic           <lgl> FALSE
## $ melting                      <lgl> FALSE
## $ wind_control                 <lgl> FALSE
## $ super_breath                 <lgl> FALSE
## $ wallcrawling                 <lgl> FALSE
## $ vision_night                 <lgl> FALSE
## $ vision_infrared              <lgl> FALSE
## $ grim_reaping                 <lgl> FALSE
## $ matter_absorption            <lgl> FALSE
## $ the_force                    <lgl> FALSE
## $ resurrection                 <lgl> FALSE
## $ terrakinesis                 <lgl> FALSE
## $ vision_heat                  <lgl> FALSE
## $ vitakinesis                  <lgl> FALSE
## $ radar_sense                  <lgl> FALSE
## $ qwardian_power_ring          <lgl> FALSE
## $ weather_control              <lgl> FALSE
## $ vision_x_ray                 <lgl> FALSE
## $ vision_thermal               <lgl> FALSE
## $ web_creation                 <lgl> FALSE
## $ reality_warping              <lgl> FALSE
## $ odin_force                   <lgl> FALSE
## $ symbiote_costume             <lgl> FALSE
## $ speed_force                  <lgl> FALSE
## $ phoenix_force                <lgl> FALSE
## $ molecular_dissipation        <lgl> FALSE
## $ vision_cryo                  <lgl> FALSE
## $ omnipresent                  <lgl> FALSE
## $ omniscient                   <lgl> FALSE
```

In the end, I can safely assume that with dimensional awareness, flight, longevity, telepathy, energy blasts, telekinesis, *MAGIC*, dimensional travel, phasing, astral projection, summoning and **many** more powers, Dr. Strange can put up a good fight.
