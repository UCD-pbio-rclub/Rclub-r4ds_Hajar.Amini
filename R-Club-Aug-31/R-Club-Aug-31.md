# R-Club-Aug-31
Hajar  
8/27/2017  



#20.4 Using atomic vectors

```r
x <- sample(20, 100, replace = TRUE)
y <- x > 10
sum(y)  # how many are greater than 10?
```

```
## [1] 52
```

```r
#> [1] 44
mean(y) # what proportion are greater than 10?
```

```
## [1] 0.52
```

```r
#> [1] 0.44
```


```r
if (length(x)) {
  # do something
}
```

```
## NULL
```


```r
typeof(c(TRUE, 1L))
```

```
## [1] "integer"
```

```r
#> [1] "integer"
typeof(c(1L, 1.5))
```

```
## [1] "double"
```

```r
#> [1] "double"
typeof(c(1.5, "a"))
```

```
## [1] "character"
```

```r
#> [1] "character"
```

#20.4.2 Test functions

#20.4.3 Scalars and recycling rules


```r
sample(10) + 100
```

```
##  [1] 107 102 105 106 109 108 103 110 101 104
```

```r
#>  [1] 109 108 104 102 103 110 106 107 105 101
runif(10) > 0.5
```

```
##  [1] FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE
```

```r
#>  [1]  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE
1:10 + 1:2
```

```
##  [1]  2  4  4  6  6  8  8 10 10 12
```


```r
library(tibble)
```

```
## Warning: package 'tibble' was built under R version 3.2.5
```

```r
#tibble(x = 1:4, y = 1:2)
tibble(x = 1:4, y = rep(1:2, 2))
```

```
## # A tibble: 4 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     2     2
## 3     3     1
## 4     4     2
```

```r
tibble(x = 1:4, y = rep(1:2, each = 2))
```

```
## # A tibble: 4 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     2     1
## 3     3     2
## 4     4     2
```

#20.4.4 Naming vectors


```r
c(x = 1, y = 2, z = 4)
```

```
## x y z 
## 1 2 4
```

```r
#set_names(1:3, c("a", "b", "c"))
```

#20.4.6 Exercises

1.What does mean(is.na(x)) tell you about a vector x? What about sum(!is.finite(x))?


```r
x <- c(1:10, NA, NaN, Inf, -Inf)
mean(is.na(x))
```

```
## [1] 0.1428571
```

```r
#The portion of missing value
mean(!is.infinite(x))
```

```
## [1] 0.8571429
```

```r
#The proportion of NA, NaN, or infinite.

mean(!is.finite(x))
```

```
## [1] 0.2857143
```
2.Carefully read the documentation of is.vector(). What does it actually test for? Why does is.atomic() not agree with the definition of atomic vectors above?


```r
?is.vector()
#he function is.vector only checks whether the object has no attributes other than names, so list is a vector
is.vector(list(a = 1, b = 2))
```

```
## [1] TRUE
```

```r
#but object with attributes rather than names is not vector

x <- 1:10
attr(x, "something") <- TRUE
is.vector(x)
```

```
## [1] FALSE
```

```r
#The function is.atomic checks whether an object is one of the atomic types (“logical”, “integer”, “numeric”, “complex”, “character”, and “raw”) or NULL.

is.atomic(1:10)
```

```
## [1] TRUE
```

```r
is.atomic(list(a = 1))
```

```
## [1] FALSE
```

3.Compare and contrast setNames() with purrr::set_names().


```r
setNames
```

```
## function (object = nm, nm) 
## {
##     names(object) <- nm
##     object
## }
## <bytecode: 0x7fb223e53d90>
## <environment: namespace:stats>
```

```r
purrr::set_names
```

```
## function (x, nm = x) 
## {
##     if (!is_vector(x)) {
##         stop("`x` must be a vector", call. = FALSE)
##     }
##     if (length(x) != length(nm)) {
##         stop("`x` and `nm` must be the same length", call. = FALSE)
##     }
##     names(x) <- nm
##     x
## }
## <environment: namespace:purrr>
```


4.Create functions that take a vector as input and returns:

The last value. Should you use [ or [[?

The elements at even numbered positions.

Every element except the last value.

Only even numbers (and no missing values).


```r
last_value <- function(x) {
  # check for case with no length
  if (length(x)) {
    # Use [[ as suggested because it returns one element
    x[[length(x)]]  
  } else {
    x
  }
}
last_value(numeric())
```

```
## numeric(0)
```

```r
last_value(1)
```

```
## [1] 1
```

```r
last_value(1:10)
```

```
## [1] 10
```


5.Why is x[-which(x > 0)] not the same as x[x <= 0]?


```r
#They will treat missing values differently.

x <- c(-5:5, Inf, -Inf, NaN, NA)
x[-which(x > 0)]
```

```
## [1]   -5   -4   -3   -2   -1    0 -Inf  NaN   NA
```

```r
#> [1]   -5   -4   -3   -2   -1    0 -Inf  NaN   NA
-which(x > 0)
```

```
## [1]  -7  -8  -9 -10 -11 -12
```

```r
#> [1]  -7  -8  -9 -10 -11 -12
x[x <= 0]
```

```
## [1]   -5   -4   -3   -2   -1    0 -Inf   NA   NA
```

```r
#> [1]   -5   -4   -3   -2   -1    0 -Inf   NA   NA
x <= 0
```

```
##  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE  TRUE    NA    NA
```

```r
#-which(x > 0) which calculates the indexes for any value that is TRUE and ignores NA. Thus is keeps NA and NaN. x <= 0 works differently. If x <= 0 returns TRUE or FALSE it works the same way. if the comparison generates a NA, then it will always keep that entry, but set it to NA. This is why the last two values of x[x <= 0] are NA rather than c(NaN, NA).
```


6.What happens when you subset with a positive integer that’s bigger than the length of the vector? What happens when you subset with a name that doesn’t exist?


```r
(1:10)[11:12] # NA foe those integers larger than the length of the vector.
```

```
## [1] NA NA
```

```r
#c(a = 1, 2)[["b"]] got error becuse a name doesn't exist.
```
#20.5.4 Exercises

1.Draw the following lists as nested sets:

list(a, b, list(c, d), list(e, f))
list(list(list(list(list(list(a))))))
2.What happens if you subset a tibble as if you’re subsetting a list? What are the key differences between a list and a tibble?


```r
x <- tibble(a = 1:2, b = 3:4)
x[["a"]]
```

```
## [1] 1 2
```

```r
x["a"]
```

```
## # A tibble: 2 × 1
##       a
##   <int>
## 1     1
## 2     2
```

```r
x[1]
```

```
## # A tibble: 2 × 1
##       a
##   <int>
## 1     1
## 2     2
```

```r
x[1, ]
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     3
```


#20.7.4 Exercises

1.What does hms::hms(3600) return? How does it print? What primitive type is the augmented vector built on top of? What attributes does it use?

```r
x <- hms::hms(3600)
class(x)
```

```
## [1] "hms"      "difftime"
```

```r
x
```

```
## 01:00:00
```

```r
#> 01:00:00
#hms::hms returns an object of class, and prints the time in “%H:%M:%S” format.


typeof(x) # the type is double
```

```
## [1] "double"
```

```r
#> [1] "double"


attributes(x)
```

```
## $units
## [1] "secs"
## 
## $class
## [1] "hms"      "difftime"
```

```r
#The atttributes is uses are "units" and "class".
```


2.Try and make a tibble that has columns with different lengths. What happens?


```r
tibble(x = 1, y = 1:5)
```

```
## # A tibble: 5 × 2
##       x     y
##   <dbl> <int>
## 1     1     1
## 2     1     2
## 3     1     3
## 4     1     4
## 5     1     5
```

```r
#tibble(x = 1:3, y = 1:4)
#got error with different length
```


3.Based on the definition above, is it ok to have a list as a column of a tibble?


```r
tibble(x = 1:3, y = list("a", 1, list(1:3)))
```

```
## # A tibble: 3 × 2
##       x          y
##   <int>     <list>
## 1     1  <chr [1]>
## 2     2  <dbl [1]>
## 3     3 <list [1]>
```

