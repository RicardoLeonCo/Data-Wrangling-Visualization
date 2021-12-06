---
title: "Semester Project: Happiness Levels per Countries."
author: "Ricardo Leon Coronado"
date: "7/16/2021"
output:
  html_document:
    keep_md: TRUE
---

```r
#First we load the libraries that we will be needing for this project:
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.0.5
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.1.0     v dplyr   1.0.5
## v tidyr   1.1.3     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
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

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(mosaic)
```

```
## Warning: package 'mosaic' was built under R version 4.0.4
```

```
## Registered S3 method overwritten by 'mosaic':
##   method                           from   
##   fortify.SpatialPolygonsDataFrame ggplot2
```

```
## 
## The 'mosaic' package masks several functions from core packages in order to add 
## additional features.  The original behavior of these functions should not be affected by this.
```

```
## 
## Attaching package: 'mosaic'
```

```
## The following object is masked from 'package:Matrix':
## 
##     mean
```

```
## The following objects are masked from 'package:dplyr':
## 
##     count, do, tally
```

```
## The following object is masked from 'package:purrr':
## 
##     cross
```

```
## The following object is masked from 'package:ggplot2':
## 
##     stat
```

```
## The following objects are masked from 'package:stats':
## 
##     binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
##     quantile, sd, t.test, var
```

```
## The following objects are masked from 'package:base':
## 
##     max, mean, min, prod, range, sample, sum
```

```r
#install.packages("rgdal")
library(rgdal)
```

```
## Warning: package 'rgdal' was built under R version 4.0.5
```

```
## Loading required package: sp
```

```
## Warning: package 'sp' was built under R version 4.0.4
```

```
## rgdal: version: 1.5-23, (SVN revision 1121)
## Geospatial Data Abstraction Library extensions to R successfully loaded
## Loaded GDAL runtime: GDAL 3.2.1, released 2020/12/29
## Path to GDAL shared files: C:/Users/ricle/Documents/R/win-library/4.0/rgdal/gdal
## GDAL binary built with GEOS: TRUE 
## Loaded PROJ runtime: Rel. 7.2.1, January 1st, 2021, [PJ_VERSION: 721]
## Path to PROJ shared files: C:/Users/ricle/Documents/R/win-library/4.0/rgdal/proj
## PROJ CDN enabled: FALSE
## Linking to sp version:1.4-5
## To mute warnings of possible GDAL/OSR exportToProj4() degradation,
## use options("rgdal_show_exportToProj4_warnings"="none") before loading rgdal.
## Overwritten PROJ_LIB was C:/Users/ricle/Documents/R/win-library/4.0/rgdal/proj
```

```
## 
## Attaching package: 'rgdal'
```

```
## The following object is masked from 'package:mosaic':
## 
##     project
```

```r
library(ggplot2)
```

## Background

One of the trends in psychology that has gained a lot of traction recently is called "positive phsychology" which reflects a lot of the attitudes that our society has these days regarding the whole concept of "being happy". We all want to be happy and we all want to feel happy but what are some of the elements that constitute this genuine happiness and how do different countries interact with them. All the above motivated the research that follows.

## The Data

The data presented in this project comes from Kaggle.com. [World Happiness Report 2021](https://www.kaggle.com/ajaypalsinghlo/world-happiness-report-2021). This very comprehensive dataset features multiple indicators that one would think are merely qualitative but are, in reality, quantifiable as well. The data does a really good job of giving a score to indicators such as generosity, perception of corruption and even freedom and assigning them values. This is the data we are working with:


```r
#Next step, we are going to load our data from this file that we downloaded from Kaggle. 
happy <- read.csv("world-happiness-report.csv")
View(happy)
```


```r
#Let's explore our data first...
summary(happy)
```

```
##  ï..Country.name         year       Life.Ladder    Log.GDP.per.capita
##  Length:1949        Min.   :2005   Min.   :2.375   Min.   : 6.635    
##  Class :character   1st Qu.:2010   1st Qu.:4.640   1st Qu.: 8.464    
##  Mode  :character   Median :2013   Median :5.386   Median : 9.460    
##                     Mean   :2013   Mean   :5.467   Mean   : 9.368    
##                     3rd Qu.:2017   3rd Qu.:6.283   3rd Qu.:10.353    
##                     Max.   :2020   Max.   :8.019   Max.   :11.648    
##                                                    NA's   :36        
##  Social.support   Healthy.life.expectancy.at.birth Freedom.to.make.life.choices
##  Min.   :0.2900   Min.   :32.30                    Min.   :0.2580              
##  1st Qu.:0.7498   1st Qu.:58.69                    1st Qu.:0.6470              
##  Median :0.8355   Median :65.20                    Median :0.7630              
##  Mean   :0.8126   Mean   :63.36                    Mean   :0.7426              
##  3rd Qu.:0.9050   3rd Qu.:68.59                    3rd Qu.:0.8560              
##  Max.   :0.9870   Max.   :77.10                    Max.   :0.9850              
##  NA's   :13       NA's   :55                       NA's   :32                  
##    Generosity      Perceptions.of.corruption Positive.affect  Negative.affect 
##  Min.   :-0.3350   Min.   :0.0350            Min.   :0.3220   Min.   :0.0830  
##  1st Qu.:-0.1130   1st Qu.:0.6900            1st Qu.:0.6255   1st Qu.:0.2060  
##  Median :-0.0255   Median :0.8020            Median :0.7220   Median :0.2580  
##  Mean   : 0.0001   Mean   :0.7471            Mean   :0.7100   Mean   :0.2685  
##  3rd Qu.: 0.0910   3rd Qu.:0.8720            3rd Qu.:0.7990   3rd Qu.:0.3200  
##  Max.   : 0.6980   Max.   :0.9830            Max.   :0.9440   Max.   :0.7050  
##  NA's   :89        NA's   :110               NA's   :22       NA's   :16
```

```r
#We need to work on the names of the columns for this dataset because they are a little confusing.
names(happy)
```

```
##  [1] "ï..Country.name"                  "year"                            
##  [3] "Life.Ladder"                      "Log.GDP.per.capita"              
##  [5] "Social.support"                   "Healthy.life.expectancy.at.birth"
##  [7] "Freedom.to.make.life.choices"     "Generosity"                      
##  [9] "Perceptions.of.corruption"        "Positive.affect"                 
## [11] "Negative.affect"
```


```r
#We wrangle our data to change the names of the columns and make them easier to read and interpret.
#In this case we'll give them the following new names:

names(happy) <- c("country", "year", "life ladder", "gdp", "social support", "life expectancy", "freedom", "generosity", "corruption", "positive affect", "negative affect")
names(happy)
```

```
##  [1] "country"         "year"            "life ladder"     "gdp"            
##  [5] "social support"  "life expectancy" "freedom"         "generosity"     
##  [9] "corruption"      "positive affect" "negative affect"
```


```r
#We continue with our data wrangling and now we also need to make sure the dataset is in correct shape by deleting all NAs. We will use the function "na.omit" to make this happen.
happy <- na.omit(happy)

#This is the final product:
View(happy)
```

## GDP

To further the study I will work now with two subsets of data. One of them will become my "Developed Countries" subset and the other one will represent "Developing Countries". Both of them are randomly picked but they each have countries that are either economically estable and even regarded as "wealthy" whereas the other group represent countries who economies are not necessarily entirely developed but are in the process of.


```r
#This represents my first group. A selection of "Developed" Countries. Only five of them assorted from different continents.
countries <- filter(happy, country %in% c("United States", "Canada", "France", "New Zealand", "Germany"))
```

Investopedia defines GDP as: "The total monetary or market value of all the finished goods and services produced within a country’s borders in a specific time period. As a broad measure of overall domestic production, **it functions as a comprehensive scorecard of a given country’s economic health."**

That being said, it was only natural that we wanted to analyze the GDP of the assortment of countries that we picked from the developed countries. Many of these countries would also be regarded as "first-world countries".


```r
#Here is the code for our graph where we can see the changes in gdp over the years for this group of countries. Notice something interesting?

ggplot(data = countries, 
       mapping = aes(x = year, 
                     y = gdp, 
                     color = country)) +
  geom_line() +
  geom_point()
```

![](Semester-Project_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
Something interesting to notice is that up until 2020, all the aforementioned countries had gdp that were steadily rising. Their economies were successfully building up. At the top of all the other countries in terms of GDP we find the United States. Unfortunately 2020 was a rough year for all the world economies. The reason? COVID.

## Freedom

Another attribute that we can focus some attention on is the perceived Freedom of each one of these countries. What was the overall perception of freedom in the first group of countries, aka the "Developed Countries"?


```r
ggplot(countries, aes(x=year, y=freedom, color=country)) +
  geom_col() +
  facet_wrap(~ country) +
  theme(axis.text.x=element_text(angle=45,hjust=1,vjust=1)) +
  labs(x= "Year",
       y= "Freedom",
       title= "Perception of Freedom by Country",
       subtitle = "Progression over Time in Developed Countries")
```

![](Semester-Project_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
#Now I want to see how has generosity increased or decreased over the last few year. In developing countries is this something that has augmented or has it been the opposite case?
countries2 <- filter(happy, country %in% c("El Salvador", "Guatemala", "Kenya", "Lebanon", "Mozambique"))
```

What about our second group? The Developing Countries. We have some years were it's blank and we also notice many slopes. Meaning that some countries start with lower perceptions of freedom to later get higher scores in the more recent years, with a few notable exceptions such as Kenya and Lebanon.


```r
ggplot(countries2, aes(x=year, y=freedom, color=country)) +
  geom_col() +
  facet_wrap(~ country) +
  theme(axis.text.x=element_text(angle=45,hjust=1,vjust=1)) +
  labs(x= "Year",
       y= "Freedom",
       title= "Perception of Freedom by Country",
       subtitle = "Progression over Time in Developing Countries")
```

![](Semester-Project_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
## Generosity

Generosity is a very interesting attribute to measure. The dataset does not give us further details as to how they calculated this metric but we have all this data regarding the generosity levels of different countries. What is the difference in the generosity levels of both group of countries that are developing? Some would say that wealthy countries could be more generous because they have more resources to share than their less wealthy counterparts.

## Generosity in First-world Countries


```r
boxplot(generosity ~ year, data=countries,
        xlab="Year",
        ylab="Generosity",
        main="Generosity Score in First-World Countries
        ",
        col="blue")
```

![](Semester-Project_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

It's interesting to notice that the generosity score of the wealthy countries normally reaches higher levels but in the most recent years it starts going downhill. To be completely honest, the last few years hasn't been easy for any economy and the COVID crisis made things even harder, so we can whyat least in the recent years, the average generosity of wealthy countries has decreased.

## Generosity in Developing Countries


```r
boxplot(generosity ~ year, data=countries2,
        xlab="Year",
        ylab="Generosity",
        main="Generosity Score in Developing Countries
        ",
        col="red")
```

![](Semester-Project_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

For the poor countries however, they have reached levels that are initially not as high as rich countries in average, but over the years, we notice something interesting, we see that the quartiles increase and also it stays somewhat stable.
 
What happens now if we analyze the relationship between Generosity and the Perception of Freedom? Are countries that feel themselves as freer in a better position to share their resources with others?

## Generosity & Freedom
 

```r
plot(freedom ~ generosity, 
     data=happy, 
     xlab="Generosity Levels",  
     ylab="Freedom Score",  
     main="Is Generosity Influenced by Perception of Freedom?
       A comparison of both attributes
     ",
     col="purple",  
     pch=16 )
```

![](Semester-Project_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

It's interesting what we can observe in this scatterplot. We can see a cluster of countries that score higher in the perception of freedom that also find themselves scoring very low in generosity levels. Interesting... From this graph we cannot make the conclusion that bigger wealth makes for a bigger heart.

## Generosity & Corruption


```r
plot(generosity ~ corruption, 
     data=happy, 
     xlab="Corruption Levels",  
     ylab="Generosity Score",  
     main="What is the relationship between Generosity & Corruption?
       A comparison of both attributes
     ",
     col="yellow",  
     pch=17 )
```

![](Semester-Project_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

For this other graph, we see a cluster and a concentration the higher the level of corruptions. When the levels of corruption are higher the generosity score of different countries also dropped drastically, hinting at the fact that corruption in a country and generosity levels seem to be intertwined.

Now, let's sort through our data to find the most generous countries and see if we can recognize any:

## Most Generous Countries


```r
#What are the most generous countries? Do we recognize any?
generous <- happy %>% 
  group_by(country) %>% 
  summarise(avg_gen = mean(generosity, na.rm = TRUE)) %>% 
  slice_max(avg_gen, n = 15)

generous
```

```
## # A tibble: 15 x 2
##    country        avg_gen
##    <chr>            <dbl>
##  1 Myanmar          0.609
##  2 Indonesia        0.397
##  3 Thailand         0.381
##  4 Gambia           0.320
##  5 United Kingdom   0.311
##  6 Haiti            0.308
##  7 Bhutan           0.299
##  8 Netherlands      0.276
##  9 Laos             0.276
## 10 Australia        0.274
## 11 Malta            0.269
## 12 Iceland          0.258
## 13 New Zealand      0.256
## 14 Ireland          0.240
## 15 Qatar            0.235
```

Myanmar scored the highest generosity levels of all the countries. Myanmar is a country in Southeast Asia where the predominant religion is Buddhism. There are other countries in Europe that are also featured in this list as well as Australia.

What about the countries with the lowest scores?

## Lowest Scoring Countries in Generosity


```r
#What are the least generous countries? Do we recognize any?
lowgenerous <- happy %>% 
  group_by(country) %>% 
  summarise(avg_gen = mean(generosity, na.rm = TRUE)) %>% 
  slice_min(avg_gen, n = 15)

lowgenerous
```

```
## # A tibble: 15 x 2
##    country    avg_gen
##    <chr>        <dbl>
##  1 Greece      -0.281
##  2 Georgia     -0.244
##  3 Lithuania   -0.243
##  4 Russia      -0.229
##  5 Morocco     -0.223
##  6 Armenia     -0.200
##  7 Gabon       -0.197
##  8 Tunisia     -0.196
##  9 Botswana    -0.194
## 10 Portugal    -0.178
## 11 Estonia     -0.172
## 12 Azerbaijan  -0.170
## 13 Belarus     -0.165
## 14 Venezuela   -0.164
## 15 Japan       -0.162
```

Negative numbers are no bueno! Some of the members in this list are smaller countries in either Europe or Asia. Russia is a big country though! One that scored particularly low on the list.

## Conclusion

In conclusion, Happiness levels are tied to multiple different factors, some of them that we didn't know we could make quantifiable, but we can. I feel like we should take all these into account and realize that perception of happiness is closely tied to other factors that go beyond monetary-economical, such as generosity and the perception of corruption in a country, and, above all, the perception of freedom.
