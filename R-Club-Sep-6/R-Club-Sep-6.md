# R-Club-Sep-6
Hajar  
9/3/2017  




#21 Iteration


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

#21.2 For loops

```r
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)

median(df$a)
```

```
## [1] 0.1453245
```

```r
#> [1] -0.246
median(df$b)
```

```
## [1] 0.4589136
```

```r
#> [1] -0.287
median(df$c)
```

```
## [1] -0.2146978
```

```r
#> [1] -0.0567
median(df$d)
```

```
## [1] -0.5245556
```

```r
#using loop
output <- vector("double", ncol(df))  # 1. output
for (i in seq_along(df)) {            # 2. sequence
  output[[i]] <- median(df[[i]])      # 3. body
}
output
```

```
## [1]  0.1453245  0.4589136 -0.2146978 -0.5245556
```

```r
y <- vector("double", 0)
seq_along(y)
```

```
## integer(0)
```

```r
#> integer(0)
1:length(y) # not understand this function
```

```
## [1] 1 0
```

```r
#> [1] 1 0
```

#21.2.1 Exercises

1.Write for loops to:


```r
#1.Compute the mean of every column in mtcars
df<-mtcars
output <-  vector("double", ncol(df))
for (i in seq_along(df)) {            # 2. sequence
  output[[i]] <- median(df[[i]])      # 3. body
}
output
```

```
##  [1]  19.200   6.000 196.300 123.000   3.695   3.325  17.710   0.000
##  [9]   0.000   4.000   2.000
```

```r
colnames(mtcars)
```

```
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
```


```r
#2.Determine the type of each column in nycflights13::flights.
df <- nycflights13::flights
output <-  vector("list", ncol(df))
names(output) <- names(df)
for (i in seq_along(df)) {          
  output[[i]] <- class(df[[i]])      
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
#3.Compute the number of unique values in each column of iris.
df <- iris
output <- vector("double", ncol(df))
names(output) <- names(df)
for(i in seq_along(df)) {
  output[[i]] <- length(unique(df[[i]]))
}
output
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```


```r
#4.Generate 10 random normals for each of μ -10 0 10 100  
df <- c(-10, 0, 10, 100)
output <- vector("list" , length(df))
for( i in seq_along(df)) {
  output[[i]] <- mean(df[[i]])
}
output
```

```
## [[1]]
## [1] -10
## 
## [[2]]
## [1] 0
## 
## [[3]]
## [1] 10
## 
## [[4]]
## [1] 100
```

```r
#it is not complete
```

2.Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors


```r
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
out
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```

```r
stringr::str_c(letters, collapse = "")
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```


```r
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))
sd
```

```
## [1] 29.01149
```

```r
#just rename sd to sdd
x <- sample(100)
sdd <- 0
for (i in seq_along(x)) {
  sdd <- sdd + (x[i] - mean(x)) ^ 2
}
sdd <- sqrt(sdd / (length(x) - 1))
sdd
```

```
## [1] 29.01149
```

```r
sqrt(sum ((x-mean(x)) ^2) / (length(x) -1))
```

```
## [1] 29.01149
```


```r
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
out
```

```
##   [1]  0.6385916  1.4863248  2.2409041  2.6759896  3.5751748  3.7927612
##   [7]  4.2965629  5.2703517  6.1482241  6.8549569  7.3374799  8.2829320
##  [13]  8.6106190  9.3313546  9.9770779 10.5468406 10.9952501 11.0780489
##  [19] 11.8849634 12.3248137 12.3544474 12.9617310 13.3815782 13.5607681
##  [25] 13.7237187 13.8569161 14.2189422 14.7315767 15.2393459 15.9303410
##  [31] 16.0258637 16.6758447 17.0112967 17.0679702 17.9881873 18.1324993
##  [37] 18.7197303 19.6577755 19.7133907 20.6037562 21.0139844 21.8107188
##  [43] 22.1788704 22.9432663 23.1930618 24.1863573 24.6843754 25.0281046
##  [49] 25.5223021 26.0753195 26.6307930 26.9099459 27.7150848 27.9700809
##  [55] 28.3127743 28.7404283 29.1668313 29.8252113 30.4881821 31.2336481
##  [61] 32.1798207 32.7860919 33.4986413 34.2445319 34.9613863 35.1995447
##  [67] 35.7537695 36.0410963 36.1845625 36.9746161 37.1507952 37.3843124
##  [73] 38.1346427 39.0564687 39.9811423 40.4078881 40.6036588 40.8794132
##  [79] 41.7112659 41.9721367 42.6554753 42.9083488 43.5651664 44.5417089
##  [85] 44.7122049 45.1088144 45.4234055 46.1951116 46.9348070 47.1416937
##  [91] 47.8926004 48.0467131 48.6515074 49.5491145 50.1238164 50.4932663
##  [97] 50.9481449 51.4414824 51.9127707 52.8715496
```

```r
#The code above is calculating a cumulative sum

all_equal(cumsum(x),out)
```

```
## [1] TRUE
```

3.Combine your function writing and for loop skills:

1.Write a for loop that prints() the lyrics to the children’s song “Alice the camel”.
I dont know what is lyrics ...

2.Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.


```r
#The lyrics for Ten in the Bed:

numbers <- c("ten", "nine", "eight", "seven", "six", "five",
             "four", "three", "two", "one")
#for (i in numbers) {
  #cat(str_c("There were ", i, " in the bed\n"))
  #cat("and the little one said\n")
 # if (i == "one") {
  #  cat("I'm lonely...")
 # } else {
  #  cat("Roll over, roll over\n")
  #  cat("So they all rolled over and one fell out.\n")
 # }
#  cat("\n")
#}
# got error
```

3.Convert the song “99 bottles of beer on the wall” to a function. Generalise to any number of any vessel containing any liquid on any surface.
i don't konw

4.It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:


```r
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```

```
##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [71] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```

#21.3 For loop variations
#21.3.1 Modifying an existing object


```r
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
rescale01 <- function(x) {
  rng <- range(x, na.rm = TRUE)
  (x - rng[1]) / (rng[2] - rng[1])
}

df$a <- rescale01(df$a)
df$b <- rescale01(df$b)
df$c <- rescale01(df$c)
df$d <- rescale01(df$d)

 for(i in seq_along(df)) {
 df[[i]] <- rescale01(df[[i]])
}
```

#21.3.2 Looping patterns


```r
results <- vector("list", length(x))
names(results) <- names(x)
```


```r
for (i in seq_along(x)) {
  name <- names(x)[[i]]
  value <- x[[i]]
}
```

#21.3.3 Unknown output length


```r
means <- c(0, 1, 2)

output <- double()
for (i in seq_along(means)) {
  n <- sample(100, 1)
  output <- c(output, rnorm(n, means[[i]]))
}
str(output)
```

```
##  num [1:149] -1.838 -1.877 0.814 -1.136 -0.107 ...
```


```r
out <- vector("list", length(means))
for (i in seq_along(means)) {
  n <- sample(100, 1)
  out[[i]] <- rnorm(n, means[[i]])
}
str(out)
```

```
## List of 3
##  $ : num [1:15] 0.108 0.995 -0.832 -0.215 -0.455 ...
##  $ : num [1:67] -0.986 0.324 1.874 1.717 0.81 ...
##  $ : num [1:55] 1.84 2.13 3.15 3.85 2.29 ...
```

```r
str(unlist(out))
```

```
##  num [1:137] 0.108 0.995 -0.832 -0.215 -0.455 ...
```

#21.3.4 Unknown sequence length


```r
#for (i in seq_along(x)) {
  # body
#}

# Equivalent to
#i <- 1
#while (i <= length(x)) {
  # body
  #i <- i + 1 
#}i
```


```r
flip <- function() sample(c("T", "H"), 1)

flips <- 0
nheads <- 0

while (nheads < 3) {
  if (flip() == "H") {
    nheads <- nheads + 1
  } else {
    nheads <- 0
  }
  flips <- flips + 1
}
flips
```

```
## [1] 10
```

#21.3.5 Exercises

1.Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, files <- dir("data/", pattern = "\\.csv$", full.names = TRUE), and now want to read each one with read_csv(). Write the for loop that will load them into a single data frame.


```r
files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)
df <- vector("list", length(files))
for (fname in seq_along(files)) {
  df[[i]] <- read_csv(files[[i]])
}
df <- bind_rows(df)
```

2.What happens if you use for (nm in names(x)) and x has no names? What if only some of the elements are named? What if the names are not unique?


```r
x <- 1:3
print(names(x))
```

```
## NULL
```

```r
#> NULL
for (nm in names(x)) {
  print(nm)
  print(x[[nm]])
}

length(NULL)
```

```
## [1] 0
```

```r
x <- c(a = 1, 2, c = 3)
names(x)
```

```
## [1] "a" ""  "c"
```

```r
#for (nm in names(x)) {
  #print(nm)
  #print(x[[nm]])
#}

#> Error in x[[nm]]: subscript out of bounds
#Finally, if there are duplicate names, then x[[nm]] will give the first element with that name. There is no way to access duplicately named elements by name.

x <- c(a = 1, a = 2, c = 3)
names(x)
```

```
## [1] "a" "a" "c"
```

```r
for (nm in names(x)) {
  print(nm)
  print(x[[nm]])
}
```

```
## [1] "a"
## [1] 1
## [1] "a"
## [1] 1
## [1] "c"
## [1] 3
```

3.Write a function that prints the mean of each numeric column in a data frame, along with its name. For example, show_mean(iris) would print:


```r
#show_mean(iris)
#no idea
```

4.What does this code do? How does it work?


```r
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}
```
This code mutates the disp and am columns:

disp is multiplied by 0.0163871
am is replaced by a factor variable.
then with forloop calls the named function in the list on the column of mtcars with the same name, and replaces the values of that column.


```r
trans[["disp"]]
```

```
## function(x) x * 0.0163871
```

```r
#This applies the function to the column of mtcars with the same name

trans[["disp"]](mtcars[["disp"]])
```

```
##  [1] 0.04296593 0.04296593 0.02900200 0.06928256 0.09667334 0.06042084
##  [7] 0.09667334 0.03939438 0.03781002 0.04500681 0.04500681 0.07406252
## [13] 0.07406252 0.07406252 0.12674949 0.12352704 0.11815630 0.02113387
## [19] 0.02032825 0.01909298 0.03225130 0.08539478 0.08163526 0.09398797
## [25] 0.10741482 0.02121443 0.03230501 0.02553787 0.09425650 0.03893787
## [31] 0.08082965 0.03249298
```

#21.4 For loops vs. functionals


```r
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
```


```r
col_mean <- function(df) {
  output <- vector("double", length(df))
  for (i in seq_along(df)) {
    output[i] <- mean(df[[i]])
  }
  output
}
```


```r
col_median <- function(df) {
  output <- vector("double", length(df))
  for (i in seq_along(df)) {
    output[i] <- median(df[[i]])
  }
  output
}
col_sd <- function(df) {
  output <- vector("double", length(df))
  for (i in seq_along(df)) {
    output[i] <- sd(df[[i]])
  }
  output
}
```


```r
f1 <- function(x) abs(x - mean(x)) ^ 1
f2 <- function(x) abs(x - mean(x)) ^ 2
f3 <- function(x) abs(x - mean(x)) ^ 3
```


```r
f <- function(x, i) abs(x - mean(x)) ^ i
```


```r
col_summary <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}
col_summary(df, median)
```

```
## [1] -0.3794399  0.5887994 -0.7589252 -0.1278297
```

```r
#> [1]  0.237 -0.218  0.254 -0.133
col_summary(df, mean)
```

```
## [1] -0.2169643  0.3237729 -0.6484704  0.0824593
```

```r
#> [1]  0.2026 -0.2068  0.1275 -0.0917
```

#21.4.1 Exercises

1.Read the documentation for apply(). In the 2d case, what two for loops does it generalise?
t generalises looping over the rows or columns of a matrix or data-frame.

2.Adapt col_summary() so that it only applies to numeric columns You might want to start with an is_numeric() function that returns a logical vector that has a TRUE corresponding to each numeric column.


