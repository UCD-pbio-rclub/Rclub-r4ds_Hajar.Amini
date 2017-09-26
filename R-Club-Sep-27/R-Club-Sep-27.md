# R-Club-Sep-27



#23.3 Visualising models


```r
library(modelr)
grid <- sim1 %>% 
  data_grid(x)
```


```r
sim1_mod <- lm(y ~ x, data = sim1)

grid <- grid %>% 
  add_predictions(sim1_mod) 
grid
```

```
## # A tibble: 10 x 2
##        x      pred
##    <int>     <dbl>
##  1     1  6.272355
##  2     2  8.323888
##  3     3 10.375421
##  4     4 12.426954
##  5     5 14.478487
##  6     6 16.530020
##  7     7 18.581553
##  8     8 20.633087
##  9     9 22.684620
## 10    10 24.736153
```



```r
library(ggplot2)
ggplot(sim1, aes(x)) +
  geom_point(aes(y = y)) +
  geom_line(aes(y = pred), data = grid, colour = "red", size = 1)
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

#23.3.2 Residuals


```r
sim1 <- sim1 %>% 
  add_residuals(sim1_mod)
sim1
```

```
## # A tibble: 30 x 3
##        x         y        resid
##    <int>     <dbl>        <dbl>
##  1     1  4.199913 -2.072442018
##  2     1  7.510634  1.238279125
##  3     1  2.125473 -4.146882207
##  4     2  8.988857  0.664969362
##  5     2 10.243105  1.919217378
##  6     2 11.296823  2.972935148
##  7     3  7.356365 -3.019056466
##  8     3 10.505349  0.129928252
##  9     3 10.511601  0.136179642
## 10     4 12.434589  0.007634878
## # ... with 20 more rows
```


```r
ggplot(sim1, aes(resid)) + 
  geom_freqpoly(binwidth = 0.5)
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
ggplot(sim1, aes(x, resid)) + 
  geom_ref_line(h = 0) +
  geom_point()
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
#23.3.3 Exercises

1.Instead of using lm() to fit a straight line, you can use loess() to fit a smooth curve. Repeat the process of model fitting, grid generation, predictions, and visualisation on sim1 using loess() instead of lm(). How does the result compare to geom_smooth()?


```r
#First use add_predictions and add_residuals to add the predictions and residuals from a loess regression to the sim1 data.

sim1_loess <- loess(y ~ x, data = sim1)
grid_loess <- sim1 %>%
  add_predictions(sim1_loess)

sim1 <- sim1 %>%
  add_residuals(sim1_loess, var = "resid_loess") %>%
  add_predictions(sim1_loess, var = "pred_loess")
  
#This plots the loess predictions. The loess produces a nonlinear, but smooth line through the data.

plot_sim1_loess <- 
  ggplot(sim1, aes(x = x, y = y)) +
  geom_point() +
  geom_line(aes(x = x, y = pred), data = grid_loess, colour = "red")
plot_sim1_loess
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
#As we see the massege, The predictions of loess are the same as the default method for geom_smooth because geom_smooth() uses loess() by default.

plot_sim1_loess +
  geom_smooth(colour = "blue", se = FALSE, alpha = 0.20)
```

```
## `geom_smooth()` using method = 'loess'
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

```r
#plot the residuals (red), and compare them to the residuals from lm (black). In general, the loess model has smaller residuals within the sample 

#ggplot(sim1, aes(x = x)) +
 # geom_ref_line(h = 0) +
 # geom_point(aes(y = resid)) +
  #geom_point(aes(y = resid_loess), colour = "red") why Error in FUN(X[[i]], ...) : object 'resid_loess' not found?
```

2.add_predictions() is paired with gather_predictions() and spread_predictions(). How do these three functions differ?

The functions gather_predictions and spread_predictions allow for adding predictions from multiple models at once.

3.What does geom_ref_line() do? What package does it come from? Why is displaying a reference 
line in plots showing residuals useful and important?

The geom geom_ref_line() adds as reference line to a plot. Even though it alters a ggplot2 plot, it is in the modelr package. Putting a reference line at zero for residuals is important because good models should have residuals centered at zero, with approximately the same variance (or distribution) over the support of x, and no correlation. A zero reference line makes it easier to judge these characteristics visually.

4.Why might you want to look at a frequency polygon of absolute residuals? What are the pros and cons compared to looking at the raw residuals?

The frequency polygon makes it easier to judge whether the variance or absolute size of the residuals varies with respect to x. 

#23.4 Formulas and model families


```r
library(tibble)
df <- tribble(
  ~y, ~x1, ~x2,
  4, 2, 5,
  5, 1, 6
)
model_matrix(df, y ~ x1)
```

```
## # A tibble: 2 x 2
##   `(Intercept)`    x1
##           <dbl> <dbl>
## 1             1     2
## 2             1     1
```

```r
model_matrix(df, y ~ x2)
```

```
## # A tibble: 2 x 2
##   `(Intercept)`    x2
##           <dbl> <dbl>
## 1             1     5
## 2             1     6
```


```r
model_matrix(df, y ~ x1 - 1)
```

```
## # A tibble: 2 x 1
##      x1
##   <dbl>
## 1     2
## 2     1
```

#23.4.1 Categorical variables


```r
df <- tribble(
  ~ sex, ~ response,
  "male", 1,
  "female", 2,
  "male", 1
)
model_matrix(df, response ~ sex)
```

```
## # A tibble: 3 x 2
##   `(Intercept)` sexmale
##           <dbl>   <dbl>
## 1             1       1
## 2             1       0
## 3             1       1
```


```r
ggplot(sim2) + 
  geom_point(aes(x, y))
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


```r
mod2 <- lm(y ~ x, data = sim2)

grid <- sim2 %>% 
  data_grid(x) %>% 
  add_predictions(mod2)
grid
```

```
## # A tibble: 4 x 2
##       x     pred
##   <chr>    <dbl>
## 1     a 1.152166
## 2     b 8.116039
## 3     c 6.127191
## 4     d 1.910981
```


```r
ggplot(sim2, aes(x)) + 
  geom_point(aes(y = y)) +
  geom_point(data = grid, aes(y = pred), colour = "red", size = 4)
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


```r
#tibble(x = "e") %>% 
#  add_predictions(mod2)
#got error
```

#23.4.2 Interactions (continuous and categorical)

```r
ggplot(sim3, aes(x1, y)) + 
  geom_point(aes(colour = x2))
```

![](R-Club-Sep-27_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


```r
mod1 <- lm(y ~ x1 + x2, data = sim3)
mod2 <- lm(y ~ x1 * x2, data = sim3)
```


