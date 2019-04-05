---
title: "Exercise - matrix / dataframe"
author: "Marc A.T. Teunis"
date: '2019-04-04'
output: html_document
---







__Write an Rmd file, containing code chunks (if applicable) and answer all the questions below. Store the file in a folder called: "./answers_exercises" in your course project folder. Give this file the same name as the title of the current Rmd file__

## Matrices

## Creating a basic matrix
During these exercises, we are going to work with a matrix of 8x10 that mostly contains (randomly generated) numerical values, interspersed with some missing values.

We do this in a number of steps.

1A) Create a numerical vector named `v` using the `runif()` function consisting of 80 values between -10 and 10.



1B) Assign missing values randomly to this vector using the following R code:


```r
v[sample(1:80, sample(10:20, 1))] <- NA
```

1C) How many missing values do you have?


1D) Create a matrix named m from this numerical vector that has 8 rows. Check to make sure the dimensions are OK!

- use the function `matrix()` to create the matrix



1E) Assign row and column names using the following R code:

 - number the rows `column_1`:`column_8` and the columns `column_1`:`column_10`
 - use the `paste()` function, set `sep = "_"`
 

```r
rownames(m) <- paste("row", 1:8, sep="_")
colnames(m) <- paste("column", 1:10, sep="_")
```

## Select statements
The next couple of exercises will take you through some of the basics of selecting data within a matrix given some criteria. These basics will come back in more real life examples during the remainder of the course.

2A) From row 1 select all values greater than 3. How many are there?

 - subsetting in dataframe or matrix also works with 

``` 
 df[rows, columns]
```



2B) What is the average of these numbers?


2C) Did you account for missing values? What happens when you have 
missing values within your data?
Probably not. These also get selected and also affect the outcome of certain functions.

2D) Do the same, but now also exclude missing values. How many values did you select now?


2E) What is the average of these numbers now?


## Storing intermediate products as index

Working with implicit logical vectors easily becomes daunting when the logics are more complicated. It is usually better to then first save this vector and then use this (or in combination with another logical vector). 

This also avoids mistakes during typing and reuses your code (both of which are good design principles). We are going to create a subselection of the data from rows 1, 4 and 8.

3A) Selecting subset of the matrix

To do this, first set up a vector for the rows that selects row 1, 4 and 8 and name this row_ind. Do this using a numerical vector and a character vector.



3B) Use this vector to create a new matrix that only contains these rows. Make sure you have a good look at the data structure to ensure you have selected the correct values.



3C) For the second row in this new matrix, select all values between -2.5 and 4, excluding missing values. How many are there and what is the average?


## Data frames

4A) Create a dataframe (or tibble) from the individual vectors below.


```r
nr <- c(1.8, 4.5, 10.1, 8.3, 7.5)
prime <- c(seq(1, 7, 2), 11)
name <- c("abc1", "foo2", "bar3", "app5")
valid <- c(TRUE, FALSE, FALSE, TRUE, FALSE)
name <- c("abc1", "foo2", "bar3", "app5", "bar1")
```

**ANSWER**


4B) Get the values from the "prime" column. Use two different ways.


4C) Select the second value of the "id" column. Use at least two different ways.


4D) Calculate the sum of the "prime" column. 


4E) Combine the "id" and "prime" columns (vectors) into a new vector.


## Ordering
The current data.frame has no particular ordering yet. The next few exercises, we are going to reorder the data.frame based on a particular column. We can order a dataframe by variable by using the `order()` function

An example to order the `mydata` dataframe by `name`

```r
mydata <- mydata[order(mydata$name), ]
mydata
```

```
##     id prime name valid
## 1  1.8     1 abc1  TRUE
## 4  8.3     7 app5  TRUE
## 5  7.5    11 bar1 FALSE
## 3 10.1     5 bar3 FALSE
## 2  4.5     3 foo2 FALSE
```
This statement can be read as "order all variables (columns) of the dataframe by the rows of he variable 'name' in decreasing order". In this case `name` is a character vector, and order will order the variable according alphabetical order.

You can also order dataframes (or so-called tibbles) with dplyr. We will see how this works with the `arrange()` function later in the course.

5A) Order the data.frame based on the "id" column in increasing order.


5B) Order the data.frame alphabetically using the "name" column.


5C) Order the data.frame based on the "prime" column in decreasing order.


5E) What happens if you use the "valid" column for ordering? Can you explain this?

FALSE can also be written as 0, TRUE as 1 in binary notation
