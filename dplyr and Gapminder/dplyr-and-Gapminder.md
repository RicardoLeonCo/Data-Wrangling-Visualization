---
title: "dplyr and Gapminder"
author: "Ricardo Leon Coronado"
date: "5/6/2021"
output:
  html_document:
    keep_md: TRUE
  
---

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```r
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
#We load the data.
#install.packages("gapminder")
library(gapminder)
```

```
## Warning: package 'gapminder' was built under R version 4.0.4
```



```r
#We apply a filter to get rid of Kuwait.

gapminder2 <- gapminder %>% filter(country != "Kuwait")

view(gapminder2)
```



```r
library(tidyverse)
```



```r
gapminder3 <- gapminder2 %>% 
  group_by(continent, year) %>%
  summarise(wtmean = weighted.mean(gdpPercap, pop),
            wtpop = sum(pop)/100000)
```

```
## `summarise()` has grouped output by 'continent'. You can override using the `.groups` argument.
```

```r
View(gapminder3)
```




```r
library(ggplot2)

plot1 <- ggplot(gapminder2, mapping = aes(x = year, y = gdpPercap))  +
  geom_point(aes(size = pop/100000, color = continent)) +
  geom_line(aes(group = country, color = continent)) +
  geom_line(data = gapminder3, aes(x = year, y = wtmean)) +
  geom_point(data = gapminder3, aes(x = year, y = wtmean, size = wtpop)) +
  labs(x = 'year', y ='GDP per capita', size = 'Population (100k)') +
  facet_wrap(~ continent, nrow = 1) +
  #scale_y_continuous(trans = "sqrt") +
  theme_bw()


ggsave("plot1.png", width = 15, units = "in")
```

```
## Saving 15 x 5 in image
```

```r
plot1
```

![](dplyr-and-Gapminder_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

