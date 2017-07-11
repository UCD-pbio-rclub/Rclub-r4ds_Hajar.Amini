# R-Club-Jul-12
Hajar  
7/11/2017  





```r
library("stringr")
```

```
## Warning: package 'stringr' was built under R version 3.2.5
```

```r
library("tidyverse")
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
#14.2 String basics

```r
string1 <- "This is a string"
string2 <- 'If I want to include a "quote" inside a string, I use single quotes'
double_quote <- "\""
single_quote <- '\''
x <- c("\"", "\\")
x
```

```
## [1] "\"" "\\"
```

```r
writeLines(x)
```

```
## "
## \
```
#14.2.1 String length

```r
str_length(c("a", "R for data science", NA))
```

```
## [1]  1 18 NA
```
#14.2.2 Combining strings


```r
str_c("x", "y")
```

```
## [1] "xy"
```

```r
str_c("x", "y", "z")
```

```
## [1] "xyz"
```

```r
str_c("x", "y", sep = ", ")
```

```
## [1] "x, y"
```

```r
x <- c("abc", NA)
str_c("|-", str_replace_na(x), "-|")
```

```
## [1] "|-abc-|" "|-NA-|"
```

```r
str_c("prefix-", c("a", "b", "c"), "-suffix")
```

```
## [1] "prefix-a-suffix" "prefix-b-suffix" "prefix-c-suffix"
```

```r
name <- "Hadley"
time_of_day <- "morning"
birthday <- FALSE

str_c(
  "Good ", time_of_day, " ", name,
  if (birthday) " and HAPPY BIRTHDAY",
  "."
)
```

```
## [1] "Good morning Hadley."
```

```r
str_c(c("x", "y", "z"), collapse = ", ")
```

```
## [1] "x, y, z"
```

#14.2.3 Subsetting strings


```r
x <- c("Apple", "Banana", "Pear")
str_sub(x, 1, 3)
```

```
## [1] "App" "Ban" "Pea"
```

```r
str_sub(x, -3, -1)
```

```
## [1] "ple" "ana" "ear"
```

```r
str_sub("a", 1, 5)
```

```
## [1] "a"
```

```r
str_sub(x, 1, 1) <- str_to_lower(str_sub(x, 1, 1))
```

#14.2.4 Locales

```r
str_to_upper(c("i", "ı"))
```

```
## [1] "I" "I"
```

```r
str_to_upper(c("i", "ı"), locale = "tr")
```

```
## [1] "İ" "I"
```

```r
x <- c("apple", "eggplant", "banana")
str_sort(x, locale = "en")  # English
```

```
## [1] "apple"    "banana"   "eggplant"
```

#14.2.5 Exercises

1.In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

The function paste seperates strings by spaces by default, while paste0 does not seperate strings with spaces by default


```r
paste("foo", "bar")
```

```
## [1] "foo bar"
```

```r
paste0("foo", "bar")
```

```
## [1] "foobar"
```

```r
#Since str_c does not seperate strings with spaces by default like paste0.

str_c("foo", "bar")
```

```
## [1] "foobar"
```

```r
#str_c() referse to NA value

str_c("foo", NA)
```

```
## [1] NA
```

```r
paste("foo", NA)
```

```
## [1] "foo NA"
```

```r
paste0("foo", NA)
```

```
## [1] "fooNA"
```

2.In your own words, describe the difference between the sep and collapse arguments to str_c().

The sep argument is the string inserted between argugments to str_c, while collapse is the string used to separate any elements of the character vector into a character vector of length one.

3.Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?


```r
x <- c("a", "abc", "abcd", "abcde", "abcdef")
L <- str_length(x)
m <- ceiling(L / 2)
str_sub(x, m, m)
```

```
## [1] "a" "b" "b" "c" "c"
```

4.What does str_wrap() do? When might you want to use it?

The function str_wrap wraps text so that it fits within a certain width. This is useful for wrapping long strings of text

5. What does str_trim() do? What’s the opposite of str_trim()?

The function str_trim trims the whitespace from a string.


```r
str_trim(" abc ")
```

```
## [1] "abc"
```

```r
str_trim(" abc ", side = "left")
```

```
## [1] "abc "
```

```r
str_trim(" abc ", side = "right")
```

```
## [1] " abc"
```

```r
#The opposite of str_trim is str_pad which adds characters to each side.

str_pad("abc", 5, side = "both")
```

```
## [1] " abc "
```

```r
str_pad("abc", 4, side = "right")
```

```
## [1] "abc "
```

```r
str_pad("abc", 4, side = "left")
```

```
## [1] " abc"
```

6.Write a function that turns (e.g.) a vector c(“a”, “b”, “c”) into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.

I don't know

#14.3.5 Grouping and backreferences


```r
str_view(fruit, "(..)\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-a84f02541800018a14e7" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-a84f02541800018a14e7">{"x":{"html":"<ul>\n  <li>b<span class='match'>anan\u003c/span>a\u003c/li>\n  <li><span class='match'>coco\u003c/span>nut\u003c/li>\n  <li><span class='match'>cucu\u003c/span>mber\u003c/li>\n  <li><span class='match'>juju\u003c/span>be\u003c/li>\n  <li><span class='match'>papa\u003c/span>ya\u003c/li>\n  <li>s<span class='match'>alal\u003c/span> berry\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

#14.3.5.1 Exercises

1.Describe, in words, what these expressions will match:

(.)\1\1
The same character apearing three times in a row. E.g. “aaa”
"(.)(.)\\2\\1"
A pair of characters followed by the same pair of characters in reversed order. E.g. “abba”.
(..)\1
Any two characters repeated. E.g. “a1a1”.
(.).\\1.\\1
A character followed by any character, the original character, any other character, the original character again. E.g. “abaca”, “b8b.b”.
(.)(.)(.).*\\3\\2\\1
 Three characters followed by zero or more characters of any kind followed by the same three characters but in reverse order. E.g. “abcsgasgddsadgsdgcba” or “abccba” or “abc1cba”.

2.Construct regular expressions to match words that:

Start and end with the same character.


```r
str_view(words, "^(.).*\\1$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-34be404abcfa49b6a1c2" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-34be404abcfa49b6a1c2">{"x":{"html":"<ul>\n  <li><span class='match'>america\u003c/span>\u003c/li>\n  <li><span class='match'>area\u003c/span>\u003c/li>\n  <li><span class='match'>dad\u003c/span>\u003c/li>\n  <li><span class='match'>dead\u003c/span>\u003c/li>\n  <li><span class='match'>depend\u003c/span>\u003c/li>\n  <li><span class='match'>educate\u003c/span>\u003c/li>\n  <li><span class='match'>else\u003c/span>\u003c/li>\n  <li><span class='match'>encourage\u003c/span>\u003c/li>\n  <li><span class='match'>engine\u003c/span>\u003c/li>\n  <li><span class='match'>europe\u003c/span>\u003c/li>\n  <li><span class='match'>evidence\u003c/span>\u003c/li>\n  <li><span class='match'>example\u003c/span>\u003c/li>\n  <li><span class='match'>excuse\u003c/span>\u003c/li>\n  <li><span class='match'>exercise\u003c/span>\u003c/li>\n  <li><span class='match'>expense\u003c/span>\u003c/li>\n  <li><span class='match'>experience\u003c/span>\u003c/li>\n  <li><span class='match'>eye\u003c/span>\u003c/li>\n  <li><span class='match'>health\u003c/span>\u003c/li>\n  <li><span class='match'>high\u003c/span>\u003c/li>\n  <li><span class='match'>knock\u003c/span>\u003c/li>\n  <li><span class='match'>level\u003c/span>\u003c/li>\n  <li><span class='match'>local\u003c/span>\u003c/li>\n  <li><span class='match'>nation\u003c/span>\u003c/li>\n  <li><span class='match'>non\u003c/span>\u003c/li>\n  <li><span class='match'>rather\u003c/span>\u003c/li>\n  <li><span class='match'>refer\u003c/span>\u003c/li>\n  <li><span class='match'>remember\u003c/span>\u003c/li>\n  <li><span class='match'>serious\u003c/span>\u003c/li>\n  <li><span class='match'>stairs\u003c/span>\u003c/li>\n  <li><span class='match'>test\u003c/span>\u003c/li>\n  <li><span class='match'>tonight\u003c/span>\u003c/li>\n  <li><span class='match'>transport\u003c/span>\u003c/li>\n  <li><span class='match'>treat\u003c/span>\u003c/li>\n  <li><span class='match'>trust\u003c/span>\u003c/li>\n  <li><span class='match'>window\u003c/span>\u003c/li>\n  <li><span class='match'>yesterday\u003c/span>\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)


```r
# any two characters repeated
str_view(words, "(..).*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-f6a43de54df11012ca9e" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-f6a43de54df11012ca9e">{"x":{"html":"<ul>\n  <li>ap<span class='match'>propr\u003c/span>iate\u003c/li>\n  <li><span class='match'>church\u003c/span>\u003c/li>\n  <li>c<span class='match'>ondition\u003c/span>\u003c/li>\n  <li><span class='match'>decide\u003c/span>\u003c/li>\n  <li><span class='match'>environmen\u003c/span>t\u003c/li>\n  <li>l<span class='match'>ondon\u003c/span>\u003c/li>\n  <li>pa<span class='match'>ragra\u003c/span>ph\u003c/li>\n  <li>p<span class='match'>articular\u003c/span>\u003c/li>\n  <li><span class='match'>photograph\u003c/span>\u003c/li>\n  <li>p<span class='match'>repare\u003c/span>\u003c/li>\n  <li>p<span class='match'>ressure\u003c/span>\u003c/li>\n  <li>r<span class='match'>emem\u003c/span>ber\u003c/li>\n  <li><span class='match'>repre\u003c/span>sent\u003c/li>\n  <li><span class='match'>require\u003c/span>\u003c/li>\n  <li><span class='match'>sense\u003c/span>\u003c/li>\n  <li>the<span class='match'>refore\u003c/span>\u003c/li>\n  <li>u<span class='match'>nderstand\u003c/span>\u003c/li>\n  <li>w<span class='match'>hethe\u003c/span>r\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# more stringent, letters only, but also allowing for differences in capitalization
str_view(str_to_lower(words), "([a-z][a-z]).*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-82488a567a0a7054df17" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-82488a567a0a7054df17">{"x":{"html":"<ul>\n  <li>ap<span class='match'>propr\u003c/span>iate\u003c/li>\n  <li><span class='match'>church\u003c/span>\u003c/li>\n  <li>c<span class='match'>ondition\u003c/span>\u003c/li>\n  <li><span class='match'>decide\u003c/span>\u003c/li>\n  <li><span class='match'>environmen\u003c/span>t\u003c/li>\n  <li>l<span class='match'>ondon\u003c/span>\u003c/li>\n  <li>pa<span class='match'>ragra\u003c/span>ph\u003c/li>\n  <li>p<span class='match'>articular\u003c/span>\u003c/li>\n  <li><span class='match'>photograph\u003c/span>\u003c/li>\n  <li>p<span class='match'>repare\u003c/span>\u003c/li>\n  <li>p<span class='match'>ressure\u003c/span>\u003c/li>\n  <li>r<span class='match'>emem\u003c/span>ber\u003c/li>\n  <li><span class='match'>repre\u003c/span>sent\u003c/li>\n  <li><span class='match'>require\u003c/span>\u003c/li>\n  <li><span class='match'>sense\u003c/span>\u003c/li>\n  <li>the<span class='match'>refore\u003c/span>\u003c/li>\n  <li>u<span class='match'>nderstand\u003c/span>\u003c/li>\n  <li>w<span class='match'>hethe\u003c/span>r\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)


```r
str_view(words, "(.).*\\1.*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-0bd33a7fa870fb8113d0" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0bd33a7fa870fb8113d0">{"x":{"html":"<ul>\n  <li>a<span class='match'>pprop\u003c/span>riate\u003c/li>\n  <li><span class='match'>availa\u003c/span>ble\u003c/li>\n  <li>b<span class='match'>elieve\u003c/span>\u003c/li>\n  <li>b<span class='match'>etwee\u003c/span>n\u003c/li>\n  <li>bu<span class='match'>siness\u003c/span>\u003c/li>\n  <li>d<span class='match'>egree\u003c/span>\u003c/li>\n  <li>diff<span class='match'>erence\u003c/span>\u003c/li>\n  <li>di<span class='match'>scuss\u003c/span>\u003c/li>\n  <li><span class='match'>eleve\u003c/span>n\u003c/li>\n  <li>e<span class='match'>nvironmen\u003c/span>t\u003c/li>\n  <li><span class='match'>evidence\u003c/span>\u003c/li>\n  <li><span class='match'>exercise\u003c/span>\u003c/li>\n  <li><span class='match'>expense\u003c/span>\u003c/li>\n  <li><span class='match'>experience\u003c/span>\u003c/li>\n  <li><span class='match'>indivi\u003c/span>dual\u003c/li>\n  <li>p<span class='match'>aragra\u003c/span>ph\u003c/li>\n  <li>r<span class='match'>eceive\u003c/span>\u003c/li>\n  <li>r<span class='match'>emembe\u003c/span>r\u003c/li>\n  <li>r<span class='match'>eprese\u003c/span>nt\u003c/li>\n  <li>t<span class='match'>elephone\u003c/span>\u003c/li>\n  <li>th<span class='match'>erefore\u003c/span>\u003c/li>\n  <li>t<span class='match'>omorro\u003c/span>w\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
##exp tuturial
#one
1.
2. "123"
3. ...\.
4. [cmf]an
5. [^b]og
6. [A-C][n-p][a-c]
7. [w][a][z]{3,5}up
8. a+b*c+
9. \d+ files? found?
10. \d.\s+abc
11. ^Mission: successful$
12. ^(file.*).pdf$
13. (\w+ (\d+))
14. (\d+)x(\d+)
15. I love (cats|dogs)
16. .*

#decimal numbers

1. .*[^p]$
2. 1?[\s-]?\(?(\d{3})\)?[\s-]?\d{3}[\s-]?\d{4} (saw the solution)
3. ([^+@]+)
4. <(\w+)
5. (\w+).(jpg|png|gif)$
6. ^\s*(.*)$
7. need more time
8. ^(\w+)://([\w-.]+)(:(\d+))?

#Bigginer

1. HELP
2. BOBE
3. OOOO
4. **//
5. i don't konw

#Intermediate
1. ATOWEL
2. -
3. -
4. DONTPANIC
5. -

#Experienced

1. 

1. 
