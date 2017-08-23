# R-Club-Aug-23
Hajar  
8/20/2017  



#19.5 Function arguments

```r
# Compute confidence interval around mean using normal approximation
mean_ci <- function(x, conf = 0.95) {
  se <- sd(x) / sqrt(length(x))
  alpha <- 1 - conf
  mean(x) + se * qnorm(c(alpha / 2, 1 - alpha / 2))
}
x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4419410 0.5571712
```

```r
#> [1] 0.498 0.610
mean_ci(x, conf = 0.99)
```

```
## [1] 0.4238371 0.5752752
```

```r
#> [1] 0.480 0.628
```


```r
# Good
mean(1:10, na.rm = TRUE)
```

```
## [1] 5.5
```

```r
# Bad
mean(x = 1:10, , FALSE)
```

```
## [1] 5.5
```

```r
mean(, TRUE, x = c(1:10, NA))
```

```
## [1] 5.5
```


```r
# Good
#average <- mean(feet / 12 + inches, na.rm = TRUE)

# Bad
#average<-mean(feet/12+inches,na.rm=TRUE)
```

#19.5.1 Choosing names
#19.5.2 Checking values


```r
wt_mean <- function(x, w) {
  sum(x * w) / sum(w)
}
wt_var <- function(x, w) {
  mu <- wt_mean(x, w)
  sum(w * (x - mu) ^ 2) / sum(w)
}
wt_sd <- function(x, w) {
  sqrt(wt_var(x, w))
}
wt_mean(1:6, 1:3)
```

```
## [1] 7.666667
```


```r
wt_mean <- function(x, w) {
  if (length(x) != length(w)) {
    stop("`x` and `w` must be the same length", call. = FALSE)
  }
  sum(w * x) / sum(w)
}
```


```r
wt_mean <- function(x, w, na.rm = FALSE) {
  if (!is.logical(na.rm)) {
    stop("`na.rm` must be logical")
  }
  if (length(na.rm) != 1) {
    stop("`na.rm` must be length 1")
  }
  if (length(x) != length(w)) {
    stop("`x` and `w` must be the same length", call. = FALSE)
  }
  
  if (na.rm) {
    miss <- is.na(x) | is.na(w)
    x <- x[!miss]
    w <- w[!miss]
  }
  sum(w * x) / sum(w)
}
```


```r
wt_mean <- function(x, w, na.rm = FALSE) {
  stopifnot(is.logical(na.rm), length(na.rm) == 1)
  stopifnot(length(x) == length(w))
  
  if (na.rm) {
    miss <- is.na(x) | is.na(w)
    x <- x[!miss]
    w <- w[!miss]
  }
  sum(w * x) / sum(w)
}
#wt_mean(1:6, 6:1, na.rm = "foo")
```

#19.5.3 Dot-dot-dot (…)


```r
sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

```
## [1] 55
```

```r
#> [1] 55
stringr::str_c("a", "b", "c", "d", "e", "f")
```

```
## [1] "abcdef"
```


```r
commas <- function(...) stringr::str_c(..., collapse = ", ")
commas(letters[1:10])
```

```
## [1] "a, b, c, d, e, f, g, h, i, j"
```

```r
#> [1] "a, b, c, d, e, f, g, h, i, j"

rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}
rule("Important output")
```

```
## Important output ------------------------------------------------------
```

```r
x <- c(1, 2)
sum(x, na.mr = TRUE)
```

```
## [1] 4
```

#19.5.5 Exercises

1.What does commas(letters, collapse = "-") do? Why?


```r
#The argument collapse is passed to str_c as part of ...
commas <- function(...) stringr::str_c(..., collapse = "- ")
```

2.It’d be nice if you could supply multiple characters to the pad argument, e.g. rule("Title", pad = "-+"). Why doesn’t this currently work? How could you fix it?


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}

rule("Important output")
```

```
## Important output ------------------------------------------------------
```

```r
rule("Important output", pad = "-+")
```

```
## Important output -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

```r
#assuming that pad is only one character. 
#adjust the code to calculate the length of pad

rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  padchar <- nchar(pad)
  cat(title, " ",
      stringr::str_dup(pad, width %/% padchar),
      # if not multiple, fill in the remaining characters
      stringr::str_sub(pad, 1, width %% padchar),
      "\n", sep = "")
}

rule("Important output")
```

```
## Important output ------------------------------------------------------
```

```r
rule("Important output", pad = "-+")
```

```
## Important output -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

```r
rule("Important output", pad = "-+-")
```

```
## Important output -+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+-
```

3.What does the trim argument to mean() do? When might you use it?

The trim arguments trims a fraction of observations in the range) before calculating the mean. This is useful when outliers are availble

4.The default value for the method argument to cor() is c("pearson", "kendall", "spearman"). What does that mean? What value is used by default?
It means that the method argument can take one of those three values. The first value, "pearson", is used by default.

#19.6 Return values

#19.6.1 Explicit return statements


```r
complicated_function <- function(x, y, z) {
  if (length(x) == 0 || length(y) == 0) {
    return(0)
  }
    
  # Complicated code here
}

f <- function() {
  if (x) {
    # Do 
    # something
    # that
    # takes
    # many
    # lines
    # to
    # express
  } else {
    # return something short
  }
}

f <- function() {
  if (!x) {
    return(something_short)
  }
    # Do 
  # something
  # that
  # takes
  # many
  # lines
  # to
  # express
}
```

#19.6.2 Writing pipeable functions


```r
show_missings <- function(df) {
  n <- sum(is.na(df))
  cat("Missing values: ", n, "\n", sep = "")
  
  invisible(df)
}
```


```r
show_missings(mtcars)
```

```
## Missing values: 0
```

```r
x <- show_missings(mtcars) 
```

```
## Missing values: 0
```

```r
class(x)
```

```
## [1] "data.frame"
```

```r
dim(x)
```

```
## [1] 32 11
```


```r
library(tidyr)
```

```
## Warning: package 'tidyr' was built under R version 3.2.5
```

```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.2.5
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
 mtcars %>% 
  show_missings() %>% 
  mutate(mpg = ifelse(mpg < 20, NA, mpg)) %>% 
  show_missings()
```

```
## Missing values: 0
## Missing values: 18
```

#19.7 Environment


```r
f <- function(x) {
  x + y
} 
```


```r
y <- 100
f(10)
```

```
## [1] 110
```


```r
`+` <- function(x, y) {
  if (runif(1) < 0.1) {
    sum(x, y)
  } else {
    sum(x, y) * 1.1
  }
}
table(replicate(1000, 1 + 2))
```

```
## 
##   3 3.3 
## 105 895
```

```r
#> 
#>   3 3.3 
#> 100 900
rm(`+`)
```

#20 Vectors

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 3.2.5
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: readr
## Loading tidyverse: purrr
```

```
## Warning: package 'ggplot2' was built under R version 3.2.5
```

```
## Warning: package 'tibble' was built under R version 3.2.5
```

```
## Warning: package 'purrr' was built under R version 3.2.5
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
typeof(letters)
```

```
## [1] "character"
```

```r
typeof(1:10)
```

```
## [1] "integer"
```

```r
x <- list("a", "b", 1:10)
length(x)
```

```
## [1] 3
```

#20.3.1 Logical


```r
1:10 %% 3 == 0
```

```
##  [1] FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE
```

```r
c(TRUE, TRUE, FALSE, NA)
```

```
## [1]  TRUE  TRUE FALSE    NA
```

#20.3.2 Numeric


```r
typeof(1)
```

```
## [1] "double"
```

```r
typeof(1L)
```

```
## [1] "integer"
```

```r
1.5L
```

```
## [1] 1.5
```

#20.3.3 Character

```r
x <- "This is a reasonably long string."
pryr::object_size(x)
```

```
## 136 B
```

```r
#> 136 B

y <- rep(x, 1000)
pryr::object_size(y)
```

```
## 8.13 kB
```

#20.3.4 Missing values


```r
NA            # logical
```

```
## [1] NA
```

```r
#> [1] NA
NA_integer_   # integer
```

```
## [1] NA
```

```r
#> [1] NA
NA_real_      # double
```

```
## [1] NA
```

```r
#> [1] NA
NA_character_ # character
```

```
## [1] NA
```

#20.3.5 Exercises

1.Describe the difference between is.finite(x) and !is.infinite(x).


```r
x <- c(0, NA, NaN, Inf, -Inf)
is.finite(x)
```

```
## [1]  TRUE FALSE FALSE FALSE FALSE
```

```r
!is.infinite(x)
```

```
## [1]  TRUE  TRUE  TRUE FALSE FALSE
```

```r
#is.finite considers only a number to be finite, and considers missing (NA), not a number (NaN), and positive and negative infinity to be not finite. However, since is.infinite only considers Inf and -Inf to be inifinite, 
```


2.Read the source code for dplyr::near() (Hint: to see the source code, drop the ()). How does it work?


```r
dplyr::near
```

```
## function (x, y, tol = .Machine$double.eps^0.5) 
## {
##     abs(x - y) < tol
## }
## <environment: namespace:dplyr>
```

```r
#Instead of checking for exact equality, it checks that two numbers are within a certain tolerance, tol. 
```

3.A logical vector can take 3 possible values. How many possible values can an integer vector take? How many possible values can a double take? Use google to do some research.
The help for .Machine describes some of this:

As all current implementations of R use 32-bit integers and usne IEC 60559 floating-point (double precision) arithmetic,

The IEC 60559 or IEEE 754 format uses a 64 bit vector

4.Brainstorm at least four functions that allow you to convert a double to an integer. How do they differ? Be precise.

5.What functions from the readr package allow you to turn a string into logical, integer, and double vector?


```r
#The functions parse_logical, parse_integer, and parse_number.

parse_logical(c("TRUE", "FALSE", "1", "0", "true", "t", "NA"))
```

```
## Warning: 4 parsing failures.
## row col       expected actual
##   3  -- T/F/TRUE/FALSE   1   
##   4  -- T/F/TRUE/FALSE   0   
##   5  -- T/F/TRUE/FALSE   true
##   6  -- T/F/TRUE/FALSE   t
```

```
## [1]  TRUE FALSE    NA    NA    NA    NA    NA
## attr(,"problems")
## # A tibble: 4 × 4
##     row   col       expected actual
##   <int> <int>          <chr>  <chr>
## 1     3    NA T/F/TRUE/FALSE      1
## 2     4    NA T/F/TRUE/FALSE      0
## 3     5    NA T/F/TRUE/FALSE   true
## 4     6    NA T/F/TRUE/FALSE      t
```

```r
parse_integer(c("1235", "0134", "NA"))
```

```
## [1] 1235  134   NA
```

```r
parse_number(c("1.0", "3.5", "1,000", "NA"))
```

```
## [1]    1.0    3.5 1000.0     NA
```
