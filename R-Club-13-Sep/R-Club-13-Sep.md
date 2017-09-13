# R-Club-13-Sep
Hajar  
9/11/2017  




```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 3.2.5
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Warning: package 'ggplot2' was built under R version 3.2.5
```

```
## Warning: package 'tibble' was built under R version 3.2.5
```

```
## Warning: package 'tidyr' was built under R version 3.2.5
```

```
## Warning: package 'purrr' was built under R version 3.2.5
```

```
## Warning: package 'dplyr' was built under R version 3.2.5
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
library(purrr)
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
map_dbl(df, mean)
```

```
##           a           b           c           d 
## -0.16249025 -0.12863519 -0.25105675 -0.07333331
```

```r
map_dbl(df, median)
```

```
##           a           b           c           d 
## -0.09793628 -0.31460568 -0.47337059 -0.09206005
```

```r
map_dbl(df, sd)
```

```
##         a         b         c         d 
## 0.9379572 1.0571549 1.1034270 1.5422229
```

```r
df %>% map_dbl(mean)
```

```
##           a           b           c           d 
## -0.16249025 -0.12863519 -0.25105675 -0.07333331
```

```r
df %>% map_dbl(median)
```

```
##           a           b           c           d 
## -0.09793628 -0.31460568 -0.47337059 -0.09206005
```

```r
df %>% map_dbl(sd)
```

```
##         a         b         c         d 
## 0.9379572 1.0571549 1.1034270 1.5422229
```



```r
map_dbl(df, mean, trim = 0.5)
```

```
##           a           b           c           d 
## -0.09793628 -0.31460568 -0.47337059 -0.09206005
```


```r
z <- list(x = 1:3, y = 4:5)
map_int(z, length)
```

```
## x y 
## 3 2
```
#21.5.1 Shortcuts


```r
models <- mtcars %>% 
  split(.$cyl) %>% 
  map(function(df) lm(mpg ~ wt, data = df))

models <- mtcars %>% 
  split(.$cyl) %>% 
  map(~lm(mpg ~ wt, data = .))

models %>% 
  map(summary) %>% 
  map_dbl(~.$r.squared)
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
#number of 4 6 8 is for cyl

models %>% 
  map(summary) %>% 
  map_dbl("r.squared")
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
#same result
```


```r
x <- list(list(1, 2, 3), list(4, 5, 6), list(7, 8, 9))
x %>% map_dbl(2)
```

```
## [1] 2 5 8
```

#21.5.2 Base R


```r
x1 <- list(
  c(0.27, 0.37, 0.57, 0.91, 0.20),
  c(0.90, 0.94, 0.66, 0.63, 0.06), 
  c(0.21, 0.18, 0.69, 0.38, 0.77)
)
x2 <- list(
  c(0.50, 0.72, 0.99, 0.38, 0.78), 
  c(0.93, 0.21, 0.65, 0.13, 0.27), 
  c(0.39, 0.01, 0.38, 0.87, 0.34)
)
threshold <- function(x, cutoff = 0.8) x[x > cutoff]
x1 %>% sapply(threshold) %>% str()
```

```
## List of 3
##  $ : num 0.91
##  $ : num [1:2] 0.9 0.94
##  $ : num(0)
```

```r
x2 %>% sapply(threshold) %>% str()
```

```
##  num [1:3] 0.99 0.93 0.87
```

#21.5.3 Exercises

1.Write code that uses one of the map functions to:
1.Compute the mean of every column in mtcars.


```r
output <- vector("double", ncol(mtcars))
names(output) <- names(mtcars)
for (i in names(mtcars)) {
  output[i] <- mean(mtcars[[i]])
}
output
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

```r
map_dbl(mtcars, mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

2.Determine the type of each column in nycflights13::flights.


```r
data("flights", package = "nycflights13")
output <- vector("list", ncol(flights))
names(output) <- names(flights)
for (i in names(flights)) {
  output[[i]] <- class(flights[[i]])
}
output
```

```
## $year
## [1] "integer"
## 
## $month
## [1] "integer"
## 
## $day
## [1] "integer"
## 
## $dep_time
## [1] "integer"
## 
## $dep_delay
## [1] "numeric"
## 
## $arr_time
## [1] "integer"
## 
## $arr_delay
## [1] "numeric"
## 
## $carrier
## [1] "character"
## 
## $tailnum
## [1] "character"
## 
## $flight
## [1] "integer"
## 
## $origin
## [1] "character"
## 
## $dest
## [1] "character"
## 
## $air_time
## [1] "numeric"
## 
## $distance
## [1] "numeric"
## 
## $hour
## [1] "numeric"
## 
## $minute
## [1] "numeric"
```

```r
map(nycflights13::flights, class)
```

```
## $year
## [1] "integer"
## 
## $month
## [1] "integer"
## 
## $day
## [1] "integer"
## 
## $dep_time
## [1] "integer"
## 
## $dep_delay
## [1] "numeric"
## 
## $arr_time
## [1] "integer"
## 
## $arr_delay
## [1] "numeric"
## 
## $carrier
## [1] "character"
## 
## $tailnum
## [1] "character"
## 
## $flight
## [1] "integer"
## 
## $origin
## [1] "character"
## 
## $dest
## [1] "character"
## 
## $air_time
## [1] "numeric"
## 
## $distance
## [1] "numeric"
## 
## $hour
## [1] "numeric"
## 
## $minute
## [1] "numeric"
```

```r
map_chr(nycflights13::flights, typeof)
```

```
##        year       month         day    dep_time   dep_delay    arr_time 
##   "integer"   "integer"   "integer"   "integer"    "double"   "integer" 
##   arr_delay     carrier     tailnum      flight      origin        dest 
##    "double" "character" "character"   "integer" "character" "character" 
##    air_time    distance        hour      minute 
##    "double"    "double"    "double"    "double"
```

3.Compute the number of unique values in each column of iris.


```r
data(iris)
iris_uniq <- vector("double", ncol(iris))
names(iris_uniq) <- names(iris)
for (i in names(iris)) {
  iris_uniq[i] <- length(unique(iris[[i]]))
}
iris_uniq
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

```r
map_int(iris, ~ length(unique(.)))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

4.Generate 10 random normals for each of 


```r
# number to draw
n <- 10
# values of the mean
mu <- c(-10, 0, 10, 100)
normals <- vector("list", length(mu))
for (i in seq_along(normals)) {
  normals[[i]] <- rnorm(n, mean = mu[i])
}
normals
```

```
## [[1]]
##  [1]  -9.385441 -10.873890 -10.969062 -10.971758  -8.499573  -9.851834
##  [7]  -9.320605 -10.662830  -8.826990  -9.970107
## 
## [[2]]
##  [1]  0.9638469 -0.3061373  1.8468079  1.4635091  0.4132593  0.2895560
##  [7] -1.2357425 -1.3078954  0.9062358 -0.7416870
## 
## [[3]]
##  [1] 10.026908  7.598107  9.038987 10.451486  8.872288  8.350142 10.833188
##  [8]  9.894786  9.054683 10.543261
## 
## [[4]]
##  [1]  99.59114  99.00749 100.60822 100.85445 100.91308 101.39552  99.76539
##  [8] 100.71289 101.52184 100.57670
```

```r
matrix(rnorm(n * length(mu), mean = mu), ncol = n)
```

```
##             [,1]       [,2]        [,3]        [,4]       [,5]        [,6]
## [1,] -11.1727206 -10.066937 -11.1287416  -9.2408619 -10.432697 -10.0786969
## [2,]  -0.7018147  -2.129748   0.2663856  -0.8157696  -0.615738   0.2262339
## [3,]  11.2378922  10.344120   9.7962819   9.4625535  10.451590   7.9568860
## [4,] 100.0161982  98.475142  98.7014116 100.0660261  99.370832 100.5714445
##             [,7]       [,8]       [,9]       [,10]
## [1,] -10.1965044  -9.925841  -9.967736 -10.4025223
## [2,]  -0.5659735   0.283248  -1.176082   0.7289359
## [3,]   8.9835914  10.781815   9.071624  10.4705387
## [4,]  99.5495601 100.039370 101.447475  99.6340139
```

```r
map(c(-10, 0, 10, 100), rnorm, n = 10)
```

```
## [[1]]
##  [1] -12.517198 -10.306458  -8.847054 -10.224266 -10.310417  -9.093335
##  [7] -10.335148  -9.492905 -10.970635  -9.851114
## 
## [[2]]
##  [1]  1.1183051  1.8615886 -0.2867723  0.3157808 -0.5290075 -1.3782453
##  [7] -0.3294514  0.9643799  0.1277047  0.2066829
## 
## [[3]]
##  [1] 10.300902 10.885596 11.148898 11.280405 10.568044  9.536446 10.852444
##  [8]  9.298483 10.056697  9.081297
## 
## [[4]]
##  [1] 101.90790 101.16181  98.48266  99.52711 100.40877 101.30241  99.40628
##  [8]  99.46486 100.56070 100.32456
```

2.How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?


```r
map_lgl(mtcars, is.factor)
```

```
##   mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb 
## FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

3.What happens when you use the map functions on vectors that aren’t lists? What does map(1:5, runif) do? Why?


```r
map(1:5, runif)
```

```
## [[1]]
## [1] 0.2254789
## 
## [[2]]
## [1] 0.8369535 0.7076248
## 
## [[3]]
## [1] 0.3049611 0.8377429 0.6710326
## 
## [[4]]
## [1] 0.7302970 0.6746073 0.1914397 0.9523159
## 
## [[5]]
## [1] 0.4639966 0.2093511 0.4572435 0.1962227 0.5899796
```

4.What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?


```r
#This takes samples of n = 5 from normal distributions of means -2, -1, 0, 1, and 2, and returns a list with each element a numeric vectors of length 5.

map(-2:2, rnorm, n = 5)
```

```
## [[1]]
## [1] -1.1445538 -0.5385322 -2.0731470 -0.9350673 -1.7206123
## 
## [[2]]
## [1] -1.4217710 -0.9637238 -0.8141767 -0.2160716 -1.5010421
## 
## [[3]]
## [1]  0.3147894  0.1821942 -0.5948736  3.1595567 -1.0827671
## 
## [[4]]
## [1] -0.7373660  1.0114921  2.8512000  0.1144313  1.7080900
## 
## [[5]]
## [1] 3.80692631 1.53400334 2.89243518 0.91244265 0.06168488
```

5.Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function.


```r
map(list(mtcars), ~ lm(mpg ~ wt, data = .))
```

```
## [[1]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      37.285       -5.344
```

#21.6 Dealing with failure

```r
safe_log <- safely(log)
str(safe_log(10))
```

```
## List of 2
##  $ result: num 2.3
##  $ error : NULL
```

```r
str(safe_log("a"))
```

```
## List of 2
##  $ result: NULL
##  $ error :List of 2
##   ..$ message: chr "non-numeric argument to mathematical function"
##   ..$ call   : language .f(...)
##   ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```


```r
x <- list(1, 10, "a")
y <- x %>% map(safely(log))
str(y)
```

```
## List of 3
##  $ :List of 2
##   ..$ result: num 0
##   ..$ error : NULL
##  $ :List of 2
##   ..$ result: num 2.3
##   ..$ error : NULL
##  $ :List of 2
##   ..$ result: NULL
##   ..$ error :List of 2
##   .. ..$ message: chr "non-numeric argument to mathematical function"
##   .. ..$ call   : language .f(...)
##   .. ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```


```r
y <- y %>% transpose()
str(y)
```

```
## List of 2
##  $ result:List of 3
##   ..$ : num 0
##   ..$ : num 2.3
##   ..$ : NULL
##  $ error :List of 3
##   ..$ : NULL
##   ..$ : NULL
##   ..$ :List of 2
##   .. ..$ message: chr "non-numeric argument to mathematical function"
##   .. ..$ call   : language .f(...)
##   .. ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```


```r
purrr::transpose
```

```
## function (.l) 
## {
##     .Call(transpose_impl, .l)
## }
## <environment: namespace:purrr>
```

```r
is_ok <- y$error %>% map_lgl(is_null)
#x[!is_ok]

#y$result[is_ok] %>% flatten_dbl()
```


```r
x <- list(1, 10, "a")
x %>% map_dbl(possibly(log, NA_real_))
```

```
## [1] 0.000000 2.302585       NA
```


```r
x <- list(1, -1)
x %>% map(quietly(log)) %>% str()
```

```
## List of 2
##  $ :List of 4
##   ..$ result  : num 0
##   ..$ output  : chr ""
##   ..$ warnings: chr(0) 
##   ..$ messages: chr(0) 
##  $ :List of 4
##   ..$ result  : num NaN
##   ..$ output  : chr ""
##   ..$ warnings: chr "NaNs produced"
##   ..$ messages: chr(0)
```

#21.7 Mapping over multiple arguments


```r
mu <- list(5, 10, -3)
mu %>% 
  map(rnorm, n = 5) %>% 
  str()
```

```
## List of 3
##  $ : num [1:5] 2.89 5.82 4.99 4.82 5.01
##  $ : num [1:5] 12.74 10.7 10.02 9.33 9.62
##  $ : num [1:5] -4.03 -1.24 -1.79 -3.38 -3.17
```


```r
sigma <- list(1, 5, 10)
seq_along(mu) %>% 
  map(~rnorm(5, mu[[.]], sigma[[.]])) %>% 
  str()
```

```
## List of 3
##  $ : num [1:5] 4.91 5.37 5.36 3.2 4.75
##  $ : num [1:5] 13.68 9.75 15.43 4.06 12.11
##  $ : num [1:5] -0.0373 -4.7266 -15.4574 22.9307 -12.9753
```

```r
# let me know more about .

map2(mu, sigma, rnorm, n = 5) %>% str()
```

```
## List of 3
##  $ : num [1:5] 4.44 5.34 4.54 4.15 4.75
##  $ : num [1:5] 25.73 15.25 5.74 3.06 10.47
##  $ : num [1:5] 9.97 -3.81 -16.89 -2.65 -9.52
```


```r
map2 <- function(x, y, f, ...) {
  out <- vector("list", length(x))
  for (i in seq_along(x)) {
    out[[i]] <- f(x[[i]], y[[i]], ...)
  }
  out
}
```


```r
n <- list(1, 3, 5)
args1 <- list(n, mu, sigma)
args1 %>%
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 5.47
##  $ : num [1:3] 10.7 11 10.1
##  $ : num [1:5] -7.6 -25.97 -26.85 1.22 18.14
```


```r
args2 <- list(mean = mu, sd = sigma, n = n)
args2 %>% 
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 5.5
##  $ : num [1:3] 7.34 15.31 11.11
##  $ : num [1:5] -2.91 -4.17 12.32 11.35 -25.14
```


```r
params <- tribble(
  ~mean, ~sd, ~n,
    5,     1,  1,
   10,     5,  3,
   -3,    10,  5
)
params %>% 
  pmap(rnorm)
```

```
## [[1]]
## [1] 5.670245
## 
## [[2]]
## [1]  9.766944  2.185485 12.232204
## 
## [[3]]
## [1]  14.475173 -18.050730 -21.142831  -3.166273   8.732465
```

#21.7.1 Invoking different functions


```r
f <- c("runif", "rnorm", "rpois")
param <- list(
  list(min = -1, max = 1), 
  list(sd = 5), 
  list(lambda = 10)
)
```


```r
f <- c("runif", "rnorm", "rpois")
param <- list(
  list(min = -1, max = 1), 
  list(sd = 5), 
  list(lambda = 10)
)
invoke_map(f, param, n = 5) %>% str()
```

```
## List of 3
##  $ : num [1:5] 0.917 -0.693 -0.716 -0.932 -0.196
##  $ : num [1:5] -1.64 2.92 2.87 2.34 -3.05
##  $ : int [1:5] 9 6 7 6 9
```


```r
sim <- tribble(
  ~f,      ~params,
  "runif", list(min = -1, max = 1),
  "rnorm", list(sd = 5),
  "rpois", list(lambda = 10)
)
#sim %>% 
 # mutate(sim = invoke_map(f, params, n = 10))
 # got error?????
```

#21.8 Walk


```r
x <- list(1, "a", 3)

x %>% 
  walk(print)
```

```
## [1] 1
## [1] "a"
## [1] 3
```


```r
library(ggplot2)
plots <- mtcars %>% 
  split(.$cyl) %>% 
  map(~ggplot(., aes(mpg, wt)) + geom_point())
paths <- stringr::str_c(names(plots), ".pdf")

pwalk(list(paths, plots), ggsave, path = tempdir())
```

```
## Saving 7 x 5 in image
## Saving 7 x 5 in image
## Saving 7 x 5 in image
```


#21.9.3 Exercises

1.Implement your own version of every() using a for loop. Compare it with purrr::every(). What does purrr’s version do that your version doesn’t?
..

2.Create an enhanced col_sum() that applies a summary function to every numeric column in a data frame.


```r
#I will use map to apply the function to all the columns, and keep to only select numeric columns.

col_sum2 <- function(df, f, ...) {
  map(keep(df, is.numeric), f, ...)
}
col_sum2(iris, mean)
```

```
## $Sepal.Length
## [1] 5.843333
## 
## $Sepal.Width
## [1] 3.057333
## 
## $Petal.Length
## [1] 3.758
## 
## $Petal.Width
## [1] 1.199333
```


3.A possible base R equivalent of col_sum() is:


```r
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]

  sapply(df_num, f)
}
#But it has a number of bugs as illustrated with the following inputs:

df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)
# OK
col_sum3(df, mean)
```

```
## x y 
## 2 2
```

```r
# Has problems: don't always return numeric vector
col_sum3(df[1:2], mean)
```

```
## x y 
## 2 2
```

```r
col_sum3(df[1], mean)
```

```
## x 
## 2
```

```r
#col_sum3(df[0], mean)

#What causes the bugs?
```

The problem is that sapply doesn’t always return numeric vectors. If no columns are selected, instead of exiting, it returns an empty list. This causes an error since we can’t use a list with [.


```r
sapply(df[0], is.numeric)
```

```
## named list()
```

```r
sapply(df[1], is.numeric)
```

```
##    x 
## TRUE
```

```r
sapply(df[1:2], is.numeric)
```

```
##    x    y 
## TRUE TRUE
```

