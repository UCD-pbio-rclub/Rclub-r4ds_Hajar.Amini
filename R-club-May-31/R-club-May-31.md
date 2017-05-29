# R-club-May-31
Hajar  
May 29, 2017  

#7.6 Patterns and models


```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.3.2
```

```r
ggplot(data = faithful) + 
  geom_point(mapping = aes(x = eruptions, y = waiting))
```

![](R-club-May-31_files/figure-html/unnamed-chunk-1-1.png)<!-- -->


```r
library(modelr)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
mod <- lm(log(price) ~ log(carat), data = diamonds)


diamonds2 <- diamonds %>% 
  add_residuals(mod) %>% 
  mutate(resid = exp(resid))

ggplot(data = diamonds2) + 
  geom_point(mapping = aes(x = carat, y = resid))
```

![](R-club-May-31_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


```r
ggplot(data = diamonds2) + 
  geom_boxplot(mapping = aes(x = cut, y = resid))
```

![](R-club-May-31_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

#7.7 ggplot2 calls


```r
ggplot(data = faithful, mapping = aes(x = eruptions)) + 
  geom_freqpoly(binwidth = 0.25)
```

![](R-club-May-31_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
ggplot(faithful, aes(eruptions)) + 
  geom_freqpoly(binwidth = 0.25)
```

![](R-club-May-31_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

```r
diamonds %>% 
  count(cut, clarity) %>% 
  ggplot(aes(clarity, cut, fill = n)) + 
    geom_tile()
```

![](R-club-May-31_files/figure-html/unnamed-chunk-4-3.png)<!-- -->
#7.8 Learning more

#10 Tibbles




