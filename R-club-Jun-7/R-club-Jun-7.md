# R-club-Jun-7
Hajar  
June 5, 2017  
#11 Data import
11.1 Introduction


```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 3.3.2
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
## Warning: package 'ggplot2' was built under R version 3.3.2
```

```
## Warning: package 'tidyr' was built under R version 3.3.2
```

```
## Warning: package 'readr' was built under R version 3.3.2
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

#11.2 Getting started


```r
read_csv("1,2,3\n4,5,6", col_names = c("x", "y", "z"))
```

```
## # A tibble: 2 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
read_csv("a,b,c\n1,2,.", na = ".")
```

```
## # A tibble: 1 × 3
##       a     b     c
##   <int> <int> <chr>
## 1     1     2  <NA>
```

#11.2.1 Compared to base R

11.2.2 Exercises

1.What function would you use to read a file where fields were separated with
“|”?
I’d use read_delim with delim="|"
read_delim(file,delim="|")

2.Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?
-col_names and col_types are used to specify the column names and how to parse the columns
-locale is important for determining things like the enecoding and whether “.” or “,” is used as a decimal mark.
-na and quoted_na control which strings are treated as missing values 
-trim_ws trims whitespace 
-n_max sets how many rows to read
-guess_max
-progress determines whether a progress bar is shown

3.What are the most important arguments to read_fwf()?
The most important argument to read_fwf which reads “fixed-width formats”, is col_positions which tells the function where data columns begin and end.

4.Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?


```r
x <- "x,y\n1,'a,b'"
read_delim(x, ",", quote = "'")
```

```
## # A tibble: 1 × 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

5.Identify what is wrong with each of the following inline CSV files. What happens when you run the code?


```r
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 2 columns 3 columns literal data
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

```r
#Only two columns are specified in the header “a” and “b”, but the rows have three columns, so the last column is dropped
```


```r
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 3 columns 2 columns literal data
##   2  -- 3 columns 4 columns literal data
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
# In row one, there are only two values, so column c is set to missing. In row two, there is an extra value, and that value is dropped
```


```r
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row col                     expected    actual         file
##   1  a  closing quote at end of file           literal data
##   1  -- 2 columns                    1 columns literal data
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

```r
#The opening quote \\"1 is dropped because it is not closed
```


```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
#Both “a” and “b” are treated as character vectors since they contain non-numeric strings
```


```r
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 × 1
##   `a;b`
##   <chr>
## 1   1;3
```

```r
#The values are separated by “;” rather than “,”. Use read_csv2 instead:

read_csv2("a;b\n1;3")
```

```
## Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     3
```

#11.3 Parsing a vector


```r
str(parse_logical(c("TRUE", "FALSE", "NA")))
```

```
##  logi [1:3] TRUE FALSE NA
```

```r
str(parse_integer(c("1", "2", "3")))
```

```
##  int [1:3] 1 2 3
```

```r
str(parse_date(c("2010-01-01", "1979-10-14")))
```

```
##  Date[1:2], format: "2010-01-01" "1979-10-14"
```

```r
parse_integer(c("1", "231", ".", "456"), na = ".")
```

```
## [1]   1 231  NA 456
```

```r
x <- parse_integer(c("123", "345", "abc", "123.45"))
```

```
## Warning: 2 parsing failures.
## row col               expected actual
##   3  -- an integer                abc
##   4  -- no trailing characters    .45
```

```r
problems(x)
```

```
## # A tibble: 2 × 4
##     row   col               expected actual
##   <int> <int>                  <chr>  <chr>
## 1     3    NA             an integer    abc
## 2     4    NA no trailing characters    .45
```

#11.3.2 Strings

#11.3.5 Exercises

1.What are the most important arguments to locale()?
The locale controls
date and time formats: date_names, date_format, and time_format
time_zone: tz
numbers: decimal_mark, grouping_mark
encoding: encoding

2.What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?


```r
#If the decimal and grouping marks are set to the same character, locale throws an error:

#locale(decimal_mark = ".", grouping_mark = ".")

#If the decimal_mark is set to the comma “,", then the grouping mark is set to the period ".":
#If the grouping mark is set to a period, then the decimal mark is set to a comma
```

3. I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.

Examples from the readr vignette of parsing French dates



```r
locale()
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
parse_date("1 janvier 2015", "%d %B %Y", locale = locale("fr"))
```

```
## [1] "2015-01-01"
```

```r
parse_date("14 oct. 1979", "%d %b %Y", locale = locale("fr"))
```

```
## [1] "1979-10-14"
```

```r
#Useful for listing the timezones for different dates 
```

4. If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.


```r
locale()
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

5.What’s the difference between read_csv() and read_csv2()?

The delimiter. The function read_csv uses a comma, while read_csv2 uses a semi-colon (;). Using a semi-colon is useful when commas are used as the decimal point (as in Europe).

6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.

Western European Latin script languages: ISO-8859-1, Windows-1250 (also CP-1250 for code-point)
Eastern European Latin script languages: ISO-8859-2, Windows-1252
Greek: ISO-8859-7
Turkish: ISO-8859-9, Windows-1254
Hebrew: ISO-8859-8, IBM424, Windows 1255
Russian: Windows 1251
Japanese: Shift JIS, ISO-2022-JP, EUC-JP
Korean: ISO-2022-KR, EUC-KR
Chinese: GB18030, ISO-2022-CN (Simplified), Big5 (Traditional)
Arabic: ISO-8859-6, IBM420, Windows 1256

7.Generate the correct format string to parse each of the following dates and times:


```r
d1 <- "January 1, 2010"
d2 <- "2015-Mar-07"
d3 <- "06-Jun-2017"
d4 <- c("August 19 (2015)", "July 1 (2015)")
d5 <- "12/30/14" # Dec 30, 2014
t1 <- "1705"
t2 <- "11:15:10.12 PM"
d1 <- "January 1, 2010"
parse_date(d1, "%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2 <- "2015-Mar-07"
parse_date(d2, format="%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d %.%Y%.")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5, "%m/%d/%y")
```

```
## [1] "2014-12-30"
```

```r
t1 <- "1705"
parse_time(t1, "%H%M")
```

```
## 17:05:00
```

```r
t2 <- "11:15:10.12 PM"
parse_time(t2, "%I:%M:%OS %p")
```

```
## 23:15:10.12
```

