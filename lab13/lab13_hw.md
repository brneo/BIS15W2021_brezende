---
title: "Lab 13 Homework"
author: "Brian Rezende"
date: "2021-03-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.1.0     √ dplyr   1.0.4
## √ tidyr   1.1.3     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

```r
options(scipen = 999)
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.** 


```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", ~
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, ~
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"~
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano~
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35~
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15~
```
The glimpse function reveals a magnitude about this data set. The dimensions are 2,160 x 6, column names are shown in the code output coupled with their variable classes. 

```r
names(UC_admit)
```

```
## [1] "Campus"          "Academic_Yr"     "Category"        "Ethnicity"      
## [5] "Perc FR"         "FilteredCountFR"
```



```r
anyNA(UC_admit)
```

```
## [1] TRUE
```


```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```
The NA's in the dataset are being represented as "NA" as opposed to another variable such as "-999". It appears that both "Perc FR" and "FilteredCountFR" contain NA's. 


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "Diversity Among UC Campus Data"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Admission Detail", choices = c("Campus", "Academic_Yr", "Category"), selected = "Campus"),
  hr(),
      helpText("UC Information Center Reference: (https://www.universityofcalifornia.edu/infocenter)"),
  ), # close the first box
  box(title= "UC Data Focused on Ethnicity Distribution", width = 5,
  plotOutput("plot", width = "750px", height = "750px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$plot <- renderPlot({
    UC_admit %>% 
      filter(Ethnicity!="All") %>% 
  ggplot(aes_string(x = input$x, y = "FilteredCountFR", fill="Ethnicity"))+
  geom_col(position = "dodge", alpha=0.9, size=4)+
  theme_grey(base_size = 18)+ labs(x=NULL, y="Number of Individuals", fill="Fill Variable")+
  theme(axis.text.x = element_text(angle = 65, hjust = 1))
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "Diversity of Enrollment Across UC Campuses"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Enrollment Variable", choices = c("Campus", "Ethnicity", "Category"), selected = "Campus"),
  hr(),
      helpText("UC Information Center Reference: (https://www.universityofcalifornia.edu/infocenter)"),
  ), # close the first box
  box(title= "Enrollment Data Per UC Campus", width = 5,
  plotOutput("plot", width = "700px", height = "750px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$plot <- renderPlot({
    UC_admit %>% 
      filter(Ethnicity!="All") %>% 
  ggplot(aes_string(x ="Academic_Yr", y = "FilteredCountFR", fill=input$x))+
  geom_col(position = "dodge", alpha=0.9, size=4)+
  theme_light(base_size = 18)+ labs(x=NULL, y="Number of Admitted Students", fill="Fill Variable")
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
