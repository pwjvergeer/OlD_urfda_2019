---
title: "Intro to RStudio and R"
author: "Marc A.T. Teunis"
date: "2019-04-04"
output:
  ioslides_presentation: 
    highlight: pygments
    widescreen: yes
---







## Getting to know your neighbour

Spent 4 minutes explaining to your neighbour:

 - What brings you here? What are your expectations?
 - What characterizes you regarding `R`:
 `Beginner`, `Intermediate`, `Advanced`, `Super Man`?
 - With what type of data do you normally work? 
 - What type of problems are you solving, or planning to solve?

## **Discuss with your neighbour:** {.smaller}

**What is wrong with this R code?**

 * Determine  what this code is meant to do
 * Find at leat 3 mistakes
 * What would you do to prevent these kind of mistakes?


```r
df$var_a[df$var_a ==  999] <- NA
df$var_b[df$var_b == -999] <- NA
df$var_h[df$var_h == -999] <- NA
df$var_i[df$var_j == -989] <- NA
df$var_j[df$var_j == -999] <- NA
df$var_o[df$var_n == -988] <- "NA"
## there are at least 5 errors in this code
```
Reference: Example (adapted from): Hadley Wickham, "Expressing yourself_ with R" Lecture  - https://richmedia.lse.ac.uk/methodologyinstitute/20170814_ExpressYourselfUsingR.mp4

## Functional programming

When working with R you can:

 - Solve each problem seperately (and spend a lot of time debugging and copying pasting)
 - Write small pieces of code (`functions`) that are generalisible to future problems
 
## Writing functions

```r
df <- tibble(a = c(1:9, -999), 
             b = c(1:5, -999, -999, 8:10),
             c = LETTERS[1:10])
df
```

```
## # A tibble: 10 x 3
##        a     b c    
##    <dbl> <dbl> <chr>
##  1     1     1 A    
##  2     2     2 B    
##  3     3     3 C    
##  4     4     4 D    
##  5     5     5 E    
##  6     6  -999 F    
##  7     7  -999 G    
##  8     8     8 H    
##  9     9     9 I    
## 10  -999    10 J
```

## Writing a function that replaces 
`a designated number or string` 
for `NA`


```r
replace_x_for_na <- function(x){
  
  x[x == -999] <- NA
  return(x)
}
```

## Apply this function to one column:

```r
replace_x_for_na(df$a)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 NA
```

## Apply our function to our whole df

```r
df <- purrr::modify_if(df, is.numeric, replace_x_for_na)
df
```

```
## # A tibble: 10 x 3
##        a     b c    
##    <dbl> <dbl> <chr>
##  1     1     1 A    
##  2     2     2 B    
##  3     3     3 C    
##  4     4     4 D    
##  5     5     5 E    
##  6     6    NA F    
##  7     7    NA G    
##  8     8     8 H    
##  9     9     9 I    
## 10    NA    10 J
```

## Good functions:

 * Do one thing good
 * Have no 'side-effects'
 * `class(input) = class(output)` (to make the function 'pipe-pable')
 
__You will hear about the 'pipe (`%>%`)' later__ 

__This workshop does not cover `writing functions` in dept__

We hope this small example got you inspired in writing more of your own functions

# Let's create some R objects
### Switch to RStudio RMarkdown document "part1_intro_to_rstudio_and_r.Rmd"

## _'numeric'_ vectors

```r
numbers <- c(1.1:10.5)
numbers
```

```
##  [1]  1.1  2.1  3.1  4.1  5.1  6.1  7.1  8.1  9.1 10.1
```

```r
sum(numbers)
```

```
## [1] 56
```

```r
integer_vector <- c(1:5)
integer_vector
```

```
## [1] 1 2 3 4 5
```

## What happens to a 'mixed' vector?

```r
vector <- c(1:10, NA, NA, NA,  3, "Python is great!", "R is greater!")
vector
```

```
##  [1] "1"                "2"                "3"               
##  [4] "4"                "5"                "6"               
##  [7] "7"                "8"                "9"               
## [10] "10"               NA                 NA                
## [13] NA                 "3"                "Python is great!"
## [16] "R is greater!"
```

```r
missing <- is.na(vector) %>% ## which is NA
  print() ## prints to the Console
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [12]  TRUE  TRUE FALSE FALSE FALSE
```

## Type of object

```r
class(numbers)
```

```
## [1] "numeric"
```

```r
class(missing) ## what is the `class`?
```

```
## [1] "logical"
```

```r
class(vector)
```

```
## [1] "character"
```

```r
class(integer_vector)
```

```
## [1] "integer"
```

## Getting amount of `NA`s

```r
missing_num <- as.numeric(missing) ## turn logical into numeric  
missing_num
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 0 0 0
```

```r
sum(missing_num) ## how many TRUEs
```

```
## [1] 3
```

```r
sum(missing) ## implicitly turning  logical into numeric
```

```
## [1] 3
```

```r
sum(is.na(vector)) ##  how many NAs
```

```
## [1] 3
```

## Mathematical operations on a `character` vector

```r
sum(vector) ## Warning sum on character
```

## Let's create some data 

```r
set.seed(1234)
normals <- rnorm(n = 100, mean = 5, sd = 1.3)
df <- normals %>% as_tibble()
## change number of observations to 10000 and rerun
```

## Plot histogram

```r
df %>% ## pipe `%>%` takes a value an puts it in a function
  ggplot(aes(x = value)) +
  geom_histogram(bins = 50)
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-1.png)

```r
## change the `bins` argument to see what happens
```

## **Discuss with your neighbour:**

 * How would you turn the above plot in a function that takes two arguments: `bins` and `df`?
 * Try it out using this backbone:
 
 ```
plot_histogram <- function(df, bins){

 ## write plot code here, use the example above
 
 plot <- df %>% ggplot(....)
 ...
 ...
 ...

 return(plot)

}
 
```


## R is case sensitive

*_Important note: R is case-sensitive, meaning that it will matter whether you use capital or lower case, keep this in mind if you get a cryptic warning or error!_*

## Programming (in R) can be frustrating {.build}

 * Writing (R) code can be challenging 
 * You will get mind-boggling errors. 
 * R is extremely picky about capitals and every opening `(`, `[` or `{` must be matched by a closing `)`, `]` or `}`. 

Mind that in R `.` is the default decimal placeholder!  

## Getting help {.build}

 * If you're stuck, try the help (`?` or `??` or `help()`) function. 
 * You can get help about any R function by running `?function_name` and `?package_name` 
 * or `??package_name`, skip down to the examples and look for code that matches what you're trying to do.
 * Another great tool is `Google` or `Stack Overflow`: trying googling the error message, as it's likely someone else has had the same problem, and has gotten help online.

## Statistics and R {.build}

 * This workshop does not cover statistical applications of R
 * All be it, we will look at an example of a linear regression later on
 * Nowadays R is a versatile language and can be used for various applications, but it was originally designed as a language for performing statistical analyses 
 * This is why very many statistical applications are available for R
 * To start learning about statisics and R, I can highly recommend the book "Discovering Statistics Using R" by Dr. Andy Field:
 
https://uk.sagepub.com/en-gb/eur/discovering-statistics-using-r/book236067%20

## For more on learning R, start e.g. with:

http://www.statsteachr.org/ or

https://www.coursera.org/learn/statistical-inference/home/welcome or

https://www.youtube.com/watch?v=ACWuV16tdhY&index=21&list=PLqzoL9-eJTNBDdKgJgJzaQcY6OXmsXAHU and

https://www.youtube.com/watch?v=kvmSAXhX9Hs&index=29&list=PLqzoL9-eJTNBDdKgJgJzaQcY6OXmsXAHU 

and the rest of Mike Marin's lectures, that are also a really great way to start learning R.
Or try datacamp.com

# Packages

## List of available CRAN packages 
http://cran.r-project.org/web/packages/available_packages_by_date.html

For this workshop we mainly use the `{tidyverse}` 
![plot of chunk unnamed-chunk-13](D:/r_projects/urfda_2019/images/tidyverse_sticker.png)

```r
# install.packages("tidyverse")
library(tidyverse)
```

## `{tidyverse}` {.build}
See: http://tidyverse.tidyverse.org/ for more info
The `{tidyverse}` package includes (among others):

 * Wrangling: `{dplyr}`, `{tidyr}` 
 * Reading data: `{readr}`, `{readxl}`
 * Functional prgramming: `{purrr}`
 * Models: `{broom}`
 * Strings and regular expressions: `{stringr}`
 * Factors: `{forcats}`
 * Time (series): `{lubridate}`

For all full list type `tidyverse_packages()` in the console, try it now!

## 'Biological' Packages
For 'Biological' application go to http://www.bioconductor.org

Installing Bioconductor packages is different from CRAN packages
example: `{affy}` 

Each Bioconductor package has a landing page:

https://www.bioconductor.org/packages/release/bioc/html/affy.html


```r
## installing `{affy}` package
BiocManager::install("affy")
biocLite("affy")

## loading affy package and the vignettes pages of this package
# library(affy)
# browseVignettes("affy")
```

## Vignettes {.build}

 * Vignettes are long explanations and demos of a package
 * A vignette contains examples and a workflow that shows how the package can be used and which (research) questions can be adressed with the functions and datasets in the package
 * It usually is a good place to start for examples
 * It also shows the so-called dependencies of a package
 * It explains which other packages you would need and how the data should look to be able to work with the package the vignette belongs to

## Getting Help examples

```r
#install.packages("dplyr")
library(dplyr)
library(ggplot2)
?dplyr
?ggplot2
?mean
?mean  # goes to the page with functions related to '.mean.'
apropos("mean") # search on more options of or alternatives for a certain function
formals(dplyr::filter)
```

## Examples and demos on functions and packages

```r
example(mean) # to see a worked example
demo(graphics) # demonstration of R functions
```

## Functions; naming arguments

```r
seed <- c(1:4)
set.seed(seed = seed)
q <- rnorm(n = 10000, mean = 20, sd = 2)
hist(q)
```

## No naming

```r
set.seed(seed)
qq <- rnorm(10000, 20, 2)
all(q == qq)
```

__**Write code for humans AND computers, be explicit!**__

# R (Data) objects

## Data Structures: Lists

When using R for data analysis you will most likely work with data in a matrix, an array, a list or even more likely: a dataframe.

A matrix is a table with only numeric values. An array consists of multiple matices. A list is collection of R objects of different data type. A dataframe is a table with variable names in the first row and observations in the consecutive rows. The columns in a dataframe represnet different variables.  

The dataframe and the list are the most widely used datastructures when considering experimental data. 

## Create a List

```r
lst <-list(first_names = c(male = "Fred", female = "Mary"), no.children = 3, child_ages=c(4,7,16),
child_names = c("Suzy", "Marvin", "Jane"), address = c("Pandamonium Alley 114, Chaosville"),
marital_status = TRUE)
length(lst) # number of elements in the list
```

```
## [1] 6
```

```r
lst
```

```
## $first_names
##   male female 
## "Fred" "Mary" 
## 
## $no.children
## [1] 3
## 
## $child_ages
## [1]  4  7 16
## 
## $child_names
## [1] "Suzy"   "Marvin" "Jane"  
## 
## $address
## [1] "Pandamonium Alley 114, Chaosville"
## 
## $marital_status
## [1] TRUE
```

## Accessing items in a list

```r
lst[[1]] # 1st element of List
```

```
##   male female 
## "Fred" "Mary"
```

```r
lst[[3]][2] # second item of third element
```

```
## [1] 7
```

```r
names(lst) # named elements in this list
```

```
## [1] "first_names"    "no.children"    "child_ages"     "child_names"   
## [5] "address"        "marital_status"
```

```r
lst$child_names # pull "named" elements from a list using `$` operator
```

```
## [1] "Suzy"   "Marvin" "Jane"
```

## `str()` gives you the structure of an object

```r
str(lst) # display structure of lst
```

```
## List of 6
##  $ first_names   : Named chr [1:2] "Fred" "Mary"
##   ..- attr(*, "names")= chr [1:2] "male" "female"
##  $ no.children   : num 3
##  $ child_ages    : num [1:3] 4 7 16
##  $ child_names   : chr [1:3] "Suzy" "Marvin" "Jane"
##  $ address       : chr "Pandamonium Alley 114, Chaosville"
##  $ marital_status: logi TRUE
```
The new RStudio Interface also enables interactive exploration of R-objects (demo)

## Selecting single elements in a list
To select a single element from a variable in a list

```r
lst$child_ages[3] 
```

```
## [1] 16
```

```r
lst[[6]][2] # returns the value of the second element for your variable
```

```
## [1] NA
```

## Looping over lists

```r
purrr::map(lst, is.na)
```

```
## $first_names
##   male female 
##  FALSE  FALSE 
## 
## $no.children
## [1] FALSE
## 
## $child_ages
## [1] FALSE FALSE FALSE
## 
## $child_names
## [1] FALSE FALSE FALSE
## 
## $address
## [1] FALSE
## 
## $marital_status
## [1] FALSE
```

## The Dataframe
The dataframe is the most widely used data structure 
**Remember "Tidy data!"**   

## Creating a data frame from individual vectors

```r
people_df <- data.frame(age = c(24, 27, 19, 34),      
                        sex = c("F","F","M", "M"), 
                        weight = c(64, 55, 80, 70),
                        names = c("Christa", "Suzan", 
                                  "Matt", "John"))
head(people_df)
```

```
##   age sex weight   names
## 1  24   F     64 Christa
## 2  27   F     55   Suzan
## 3  19   M     80    Matt
## 4  34   M     70    John
```

## Or the 'tidyverse' way

```r
people_tbl <- tibble::tibble(age = c(24, 27, 19, 34),      
                             sex = c("F","F","M", "M"), 
                             weight = c(64, 55, 80, 70),
                             names = c("Christa", "Suzan", 
                                       "Matt", "John"))
people_tbl
```

```
## # A tibble: 4 x 4
##     age sex   weight names  
##   <dbl> <chr>  <dbl> <chr>  
## 1    24 F         64 Christa
## 2    27 F         55 Suzan  
## 3    19 M         80 Matt   
## 4    34 M         70 John
```

## Viewing the contents of a dataframe

```r
summary(people_df)
table(people_df)

head(people_df) 			# gives the content of the data frame
names(people_df) 
str(people_df)

people_df$age # gives the content of the variable "age" from the data frame
```

## Using index on dataframes
Using the index "[]" on a dataframe is a bit tricky. The dataframe always consists of rows and columns. Indexing a dataframe goes like:

`dataframe[row number(s), column number(s)]`


```r
people_df$age[1] 	# first element of this vector
people_df[,2] 	  # content of 2nd variable (column) which is a character                      vector -> factor
people_df[1,] 	  # content of the 1st row
                  # multiple indices
people_df[2:3, c(1,3)] # remember to use c
```

## Import data into R

```r
library(tidyverse)
path_to_gender_age_data <- file.path(root, "data", "gender.txt")
gender_age <- read_delim(path_to_gender_age_data,
                         delim = "/")
```

## Inspecting the dataframe

```r
# getting the first few rows 
head(gender_age)
```

```
## # A tibble: 6 x 2
##   gender   age
##   <chr>  <dbl>
## 1 F         12
## 2 F         23
## 3 F         34
## 4 M         12
## 5 M         15
## 6 M         17
```

```r
# getting information on the variable, the dimensions
str(gender_age)
```

```
## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	17 obs. of  2 variables:
##  $ gender: chr  "F" "F" "F" "M" ...
##  $ age   : num  12 23 34 12 15 17 18 14 56 67 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   gender = col_character(),
##   ..   age = col_double()
##   .. )
```

```r
# selecting a variable
gender_age$gender
```

```
##  [1] "F" "F" "F" "M" "M" "M" "F" "F" "M" "F" "M" "M" "M" "F" "F" "F" "M"
```

```r
gender_age$age
```

```
##  [1] 12 23 34 12 15 17 18 14 56 67 89 34 45 12  3  5  4
```

```r
gender_age[1:6,2]
```

```
## # A tibble: 6 x 1
##     age
##   <dbl>
## 1    12
## 2    23
## 3    34
## 4    12
## 5    15
## 6    17
```

## read_csv
CSV is a format of a data file that uses commas or semicolons as separators for the columns.


```r
library(readr)
skin <- read_csv(paste0(root, "/data/skincolumns.csv")) 
skin %>% head(3)
```

```
## # A tibble: 3 x 2
##   `Genotype A` `Genotype B`
##          <dbl>        <dbl>
## 1         54.9         30.5
## 2         48.8         24.8
## 3         50.8         24.2
```

## Inspecting the skin dataframe

```r
head(skin)	 # content of the data frame
dim(skin)
attributes(skin)
summary(skin)
## ?read_csv 	 # help on the function
```

## `skin` dataset contains an NA, some functions do not work with NAs:

```r
mean(skin$`Genotype A`)
```

```
## [1] 48.22066
```

```r
mean(skin$`Genotype B`)
```

```
## [1] NA
```

```r
# to remove the NA, take care: consider leaving NAs in and use arguments like na.rm = TRUE
skin_noNA <- na.omit(skin)
mean(skin_noNA$`Genotype B`)
```

```
## [1] 25.72858
```

## Let's clean up the workspace

```r
rm(list=ls())
root <- find_root_file(criterion = is_rstudio_project)
## Note: never use this in code that is meant for others!!!
```

*_The above is an effective way to clear all the items in the Global Environment, but is is not very friendly to use this in code you share with others: can you think of why?_*

## Vectors {.build}
R is an object oriented language: meaning you can create and work with (manipulate/index/access) objects. Vectors are R's elementary objects and 
come in different flavours:

 - Nummeric vector: contains only numbers: decimal separator in R is "." (decimal point) and not "," (decimal comma) as is common in the English language. 
 - Character vector: contains only "words", but words can also be numbers: "23" or other items "100%" or "$2,000.00" 
 - An integer vector: an nummeric series: 1, 2, 3 is an integer of length 3.
 - Logical: logical vectors contain only two values: "TRUE" and/or "FALSE"
 - Mixed: Vectors do not need to be of one type. They can be mixed. They can only be of one class, so this operation will induce **_coercion_**.

## Numeric vectors

```r
c(2,8,5) 	# combines its arguments to form a vector
```

```
## [1] 2 8 5
```

```r
nv_1 <- c(2,8,5) # assignment statement
nv_1 # view the content of the object "nv_1"
```

```
## [1] 2 8 5
```

## Class tells you about the type of the variable

```r
nv_2 <- c(8.4,5.6,10.1,13.1,2.5,7.8,15.2,3.8,20.9)
nv_2
```

```
## [1]  8.4  5.6 10.1 13.1  2.5  7.8 15.2  3.8 20.9
```

```r
class(nv_2)
```

```
## [1] "numeric"
```

## **Discuss with your neighbour**

 * What happens to the vector values if you run ```nv_2_int <- as.integer(nv_2)``` 
 * Call ```class(nv_2_int)``` and ```nv_2_int``` to confirm your suspecion
 * What is the apparent difference between "nummeric" and "integer" vectors? 

# Number notations and rounding

## Scientific notations 

```r
big_numbers <- rnorm(10, mean = 10000000, sd = 2000000)
big_numbers %>% formatC(format = "e", digits = 5)
```

```
##  [1] "1.08290e+07" "9.05056e+06" "1.01320e+07" "8.99504e+06" "8.34800e+06"
##  [6] "1.03340e+07" "8.20747e+06" "1.03364e+07" "1.07099e+07" "9.89579e+06"
```

```r
many_digits <- c(2.55858868688584848)
round(many_digits, digits = 3)
```

```
## [1] 2.559
```

```r
sqrt(many_digits * 1000 /200 * 2^6)  
```

```
## [1] 28.61378
```

## Rounding numbers

```r
small_numbers <- runif(10, min = 0.001, max = 0.1) %>% print()
```

```
##  [1] 0.04281074 0.00410782 0.02655652 0.03414963 0.01422142 0.05045509
##  [7] 0.08041143 0.03437817 0.05138314 0.04994942
```

```r
small_numbers %>% round(digits = 2)
```

```
##  [1] 0.04 0.00 0.03 0.03 0.01 0.05 0.08 0.03 0.05 0.05
```

## Character vectors

```r
cv <- c("this is", "an", "example of", "1", "character", "vector", "with length:", "length(cv)")
cv
sum(cv)

length(cv)
class(cv)
```

## Logical vectors

```r
lv <- c(TRUE, FALSE, TRUE, TRUE)
lv
```

```
## [1]  TRUE FALSE  TRUE  TRUE
```

```r
class(lv)
```

```
## [1] "logical"
```

```r
## logical vectors can also be converted to numeric vectors
nlv <- as.numeric(lv)
nlv
```

```
## [1] 1 0 1 1
```

```r
## note that coercion of a logical to a numeric vector changes the "TRUE" value to 1 and the "FALSE" value to 0
```

## Manipulating vectors
You can add/subtract/devide or use other arithmetic functions on numeric vectors

```r
a <- c(1,3,5,7,9)
b <- c(2,4,6,8)

z1 <- a - b
z1
```

```
## [1] -1 -1 -1 -1  7
```

```r
z2 <- b - a
z2
```

```
## [1]  1  1  1  1 -7
```

```r
z3 <- a / b
z3
```

```
## [1] 0.5000000 0.7500000 0.8333333 0.8750000 4.5000000
```

## Apply functions to a vector 

```r
z4 <- sum(a)
z4
```

```
## [1] 25
```

```r
z5 <- max(a) - max(b)
z5
```

```
## [1] 1
```

## Make sequences

```r
repetitions <- rep("Luke", times = 10)
saber <- rep("Sword", length.out = 20)
jedi <- c(repetitions, saber) %>% rep(each = 3)
```

## Indexing with [...]
You can get the individual items of a vector by using the index `[]`

```r
x <- c(8, 5, 10, 13, 2, 7, 15, 3, 20, 8)
x # create vector with 10 variables
```

```
##  [1]  8  5 10 13  2  7 15  3 20  8
```

```r
length(x)
```

```
## [1] 10
```

```r
mode(x) # information on data mode (numeric, character, logic)
```

```
## [1] "numeric"
```

## Using the index

```r
x
```

```
##  [1]  8  5 10 13  2  7 15  3 20  8
```

```r
x[3] ## creating a subset by indexing:
```

```
## [1] 10
```

```r
x[c(3, 4, 7)] # apply a simple function
```

```
## [1] 10 13 15
```

```r
mean(x[c(2,5)]) # example of a function
```

```
## [1] 3.5
```

## Using the vector index `[]` some more

```r
i <- 5
x[c(i, i+2)] # (i = 5 & i = 7), i-th element
```

```
## [1]  2 15
```

```r
x[-2] # all but the ith (second) element
```

```
## [1]  8 10 13  2  7 15  3 20  8
```

```r
x[3:5] # element 3 to 5
```

```
## [1] 10 13  2
```

```r
x[x > 9] # all greater than some value
```

```
## [1] 10 13 15 20
```

## Concatenate or `c()` command
The `c()` function can be used to __concatenate__, __combine__ or __create__ objects in R 

```r
pp <- c(x, 0, x)
length(pp) 
```

```
## [1] 21
```

```r
sum(c(pp, c(1:5), c(1,2, c(1:10)))) # sum of elements in the 'concatenated' vector
```

```
## [1] 255
```

## Generate R-code from this Rmd

```r
knitr::purl("hands_ons/part1_intro_to_rstudio_and_r.Rmd", output = "./hands_ons/part1_intro_to_rstudio_and_r.R")
```
