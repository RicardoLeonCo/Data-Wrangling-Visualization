---
title: "Exploratory Data Analysis"
author: "Ricardo Leon Coronado"
date: "December 05, 2021"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---



## Reading Notes

Replace this text with your reading notes.

## EDA Example

The code below is an example of the EDA process using the `starwars` data from the `tidyverse` package. (Make sure you have the `tidyverse` package installed!)

Run the code line-by-line and look at the output. Add a comment to each line of code that explains what it does/what insights it provides.


```r
#This loads the library that we want to use.
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.0.5
```

```
## Warning: package 'ggplot2' was built under R version 4.0.5
```

```
## Warning: package 'tibble' was built under R version 4.0.4
```

```
## Warning: package 'tidyr' was built under R version 4.0.4
```

```
## Warning: package 'readr' was built under R version 4.0.5
```

```
## Warning: package 'dplyr' was built under R version 4.0.5
```

```
## Warning: package 'forcats' was built under R version 4.0.4
```

```r
#It gives us the dimensions of the dataframe, number of rows and number of columns.
dim(starwars)
```

```
## [1] 87 14
```

```r
#This gives us the name of the all the different columns in the database.
colnames(starwars)
```

```
##  [1] "name"       "height"     "mass"       "hair_color" "skin_color"
##  [6] "eye_color"  "birth_year" "sex"        "gender"     "homeworld" 
## [11] "species"    "films"      "vehicles"   "starships"
```

```r
#Head should give us the first few rows of the tables.
head(starwars)
```

```
## # A tibble: 6 x 14
##   name     height  mass hair_color  skin_color eye_color birth_year sex   gender
##   <chr>     <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> <chr> 
## 1 Luke Sk~    172    77 blond       fair       blue            19   male  mascu~
## 2 C-3PO       167    75 <NA>        gold       yellow         112   none  mascu~
## 3 R2-D2        96    32 <NA>        white, bl~ red             33   none  mascu~
## 4 Darth V~    202   136 none        white      yellow          41.9 male  mascu~
## 5 Leia Or~    150    49 brown       light      brown           19   fema~ femin~
## 6 Owen La~    178   120 brown, grey light      blue            52   male  mascu~
## # ... with 5 more variables: homeworld <chr>, species <chr>, films <list>,
## #   vehicles <list>, starships <list>
```

```r
#Glimpse should give us a more comprehensive view of the data that's in every column but not quite all of it,
glimpse(starwars)
```

```
## Rows: 87
## Columns: 14
## $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or~
## $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2~
## $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.~
## $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N~
## $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "~
## $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",~
## $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, ~
## $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",~
## $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini~
## $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T~
## $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma~
## $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return~
## $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp~
## $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",~
```

```r
#When you type a question mark before the name of a method or a dataset, you get more information or "help" on such data set or such method, and that shows up in your view window to the right.
?starwars

#The pipe operator is used to "pipe" in R. Upon research I learned that it basically takes the output of one function and passes it into another function as an argument.
starwars %>% count(species)
```

```
## # A tibble: 38 x 2
##    species       n
##    <chr>     <int>
##  1 Aleena        1
##  2 Besalisk      1
##  3 Cerean        1
##  4 Chagrian      1
##  5 Clawdite      1
##  6 Droid         6
##  7 Dug           1
##  8 Ewok          1
##  9 Geonosian     1
## 10 Gungan        3
## # ... with 28 more rows
```

```r
#This allows us to get the means of the values of one entire column, the one after the dollar sign. So, in other words, the "height" column.
mean(starwars$height)
```

```
## [1] NA
```

```r
#In this case, it's the na.rm what allows rows in the column that have NA to exist.
mean(starwars$height, na.rm = TRUE)
```

```
## [1] 174.358
```

```r
#The summary gives us the values of the Min, Median, Mean, Max, and the 1st and 3rd quartile altogether.
summary(starwars$height)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    66.0   167.0   180.0   174.4   191.0   264.0       6
```

```r
#This is a function to turn centimeters to feet and it's taking the measurement in centimeters and dividing it by 30.48 and returning the value and storing it inside the variable "ft".
cm_to_ft <- function(cm){
  ft = cm / 30.48
  return(ft)
}

#
starwars_2 <- starwars %>% mutate(height_ft = cm_to_ft(height)) 

#This will give us the dimensions of the new dataframe, starwars_2. Number of columns by number of rows.
dim(starwars_2)
```

```
## [1] 87 15
```

```r
#This will give us the all the different column names for the dataframe (starwars_2)
colnames(starwars_2)
```

```
##  [1] "name"       "height"     "mass"       "hair_color" "skin_color"
##  [6] "eye_color"  "birth_year" "sex"        "gender"     "homeworld" 
## [11] "species"    "films"      "vehicles"   "starships"  "height_ft"
```

```r
#Similar to a previous summary, this command tell the computer that we want a summary of the values located specifically in the "height_ft" column in the dataframe.
summary(starwars_2$height_ft)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   2.165   5.479   5.906   5.720   6.266   8.661       6
```

```r
#This command produced a histogram with the numbers and values of the dataframe.
ggplot(starwars_2, aes(height_ft)) + 
  geom_histogram()
```

```
## Warning: Removed 6 rows containing non-finite values (stat_bin).
```

![](Exploratory-Data-Analysis-Assignment_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
#I am not 100% sure on this one but I think it is giving us the value with the highest height in all our dataset.
starwars_2 %>% filter(height_ft == max(height_ft))
```

```
## # A tibble: 0 x 15
## # ... with 15 variables: name <chr>, height <int>, mass <dbl>,
## #   hair_color <chr>, skin_color <chr>, eye_color <chr>, birth_year <dbl>,
## #   sex <chr>, gender <chr>, homeworld <chr>, species <chr>, films <list>,
## #   vehicles <list>, starships <list>, height_ft <dbl>
```

```r
starwars_2 %>% filter(height_ft == max(height_ft, na.rm = TRUE))
```

```
## # A tibble: 1 x 15
##   name     height  mass hair_color skin_color eye_color birth_year sex   gender 
##   <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr>  
## 1 Yarael ~    264    NA none       white      yellow            NA male  mascul~
## # ... with 6 more variables: homeworld <chr>, species <chr>, films <list>,
## #   vehicles <list>, starships <list>, height_ft <dbl>
```

```r
# https://starwars.fandom.com/wiki/Yarael_Poof
```

## EDA Practice

Continue exploring the `starwars` data to gain additional insights, using [R4DS: Chapter 7](https://r4ds.had.co.nz/exploratory-data-analysis.html) as a guide.

It is ok if you don't understand the all code in Chapter 7. (That is what we'll be learning the next two weeks!) If writing your own code is a struggle, try the "copy, paste, and tweak" method.


```r
# your code goes here
```
