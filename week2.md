Week 2: Reading Data in R
================
written by Junvie Pailden

Matrices
--------

We often store numerical data in a rectangular format called matrices with two dimensions, rows and columns. Recall last week that we can create a matrix in R by binding two numerical vectors using the `cbind()` function. Another way is to use the `matrix()` function.

``` r
# store the vector (1, 2, 3, 4, 5, 6, 7, 8) into a 2x4 matrix named A
A <- matrix(c(1,2,3,4,5,6,7,8), nrow = 2, ncol = 4) # nrow = # of rows, ncol = # of columns
A
#      [,1] [,2] [,3] [,4]
# [1,]    1    3    5    7
# [2,]    2    4    6    8
```

If we wanted to fill the matrix in order of the rows first, we use the optional `byrow = TRUE` argument.

``` r
B <- matrix(1:8, nrow = 2, ncol = 4, byrow = TRUE) # 1:8 is another way of generating  the vector
B
#      [,1] [,2] [,3] [,4]
# [1,]    1    2    3    4
# [2,]    5    6    7    8
```

We can assign column and row names using `colnames()` and `rownames()`, respectively.

``` r
colnames(A) <- c('C1', 'C2', 'C3', 'C4')
rownames(A) <- c('R1', 'R2')
A
#    C1 C2 C3 C4
# R1  1  3  5  7
# R2  2  4  6  8
```

Accessing the element of a matrix can be done using `[row, col]` notation.

``` r
A[1, 3]   # row 1, col 3 entry
# [1] 5
A[1, 1:3] # row 1, col 1 to 3
# C1 C2 C3 
#  1  3  5
A[1, ]    # row 1
# C1 C2 C3 C4 
#  1  3  5  7
A[ , 3]   # col 3
# R1 R2 
#  5  6
```

Data Frames
-----------

Matrices can only store numerical values. We, however, often store values that are non-numeric. Data frames are generalized version of a matrix to include other types of data. Data frames are similar in concept to Excel spreadsheet where each column represents a variable or measurement and each row represents and observational unit. We can create a data frame using the function `data.frame()`.

We can check the structure of the data frame using the function `str()`. The output includes data information such as

-   dimension (number of observations and variables),
-   variable names
-   data type
    -   `logi`: logical, `TRUE` or `FALSE`
    -   `int`: integer
    -   `num`: numerical
    -   `chr`: character
    -   `Factor`: Factors are how R keeps track of categorical variables.

``` r
stark.kids <- data.frame(
  Name = c("Jon", "Sansa", "Arya", "Bran"),
  Age = c(24, 20, 18, 17)
)
str(stark.kids)
# 'data.frame': 4 obs. of  2 variables:
#  $ Name: Factor w/ 4 levels "Arya","Bran",..: 3 4 1 2
#  $ Age : num  24 20 18 17
stark.kids
#    Name Age
# 1   Jon  24
# 2 Sansa  20
# 3  Arya  18
# 4  Bran  17
```

We can also use the names of variables in a data frame to access the variable using the notation `data$name`.

``` r
stark.kids$Name
# [1] Jon   Sansa Arya  Bran 
# Levels: Arya Bran Jon Sansa
mean(stark.kids$Age) # average age
# [1] 19.75
```

Working with data frames included in R packages
-----------------------------------------------

Consider the data `chickwts` in the package `datasets` included with every R installation. The data was the result of an experiment conducted to measure and compare the effectiveness of various feed supplements on the growth rate of chickens.

``` r
str(chickwts) # check data structure
# 'data.frame': 71 obs. of  2 variables:
#  $ weight: num  179 160 136 227 217 168 108 124 143 140 ...
#  $ feed  : Factor w/ 6 levels "casein","horsebean",..: 2 2 2 2 2 2 2 2 2 2 ...
```

We can display selected rows using `head()` and `tail()` commands; or selected cell entries.

``` r
head(chickwts, 4) # display first 4 rows
#   weight      feed
# 1    179 horsebean
# 2    160 horsebean
# 3    136 horsebean
# 4    227 horsebean
tail(chickwts, 2) # display last 2 rows
#    weight   feed
# 70    283 casein
# 71    332 casein
```

Reading CSV data in R
---------------------

Data analysis using R often involves importing or reading data at some point. While R can read other data types, comma separated files (.csv) are much easier to work with. Saving data files in .csv format is standard in practice. We use the command `read.csv` to read .csv data in R.

    read.csv(file, header = TRUE)

The arguments of the `read.csv` function includes (among others)

-   `file`: name or URL (Universal Resource Locator) of the data set

-   `header = TRUE`: if the file contains the names of the variables as its first line.

### A) Loading a data set from a webpage using its URL.

The article *Going Wireless (AARP Bulletin, June 2009)* reported the estimated percentage of house- holds with only wireless phone service (no land line) for the 50 U.S. states and the District of Columbia. In the accompanying data table, each state was also classified into one of three geographical regions—West (W), Middle states (M), and East (E).

The URL of the data set `Going Wireless` that we need to read the data is (<https://goo.gl/72BKSf>).

``` r
wireless.data <- read.csv("https://goo.gl/72BKSf", header = TRUE)
```

``` r
str(wireless.data) # check structure
# 'data.frame': 51 obs. of  3 variables:
#  $ Wireless: num  13.9 11.7 18.9 22.6 9 16.7 5.6 5.7 20 16.8 ...
#  $ Region  : Factor w/ 3 levels "E","M","W": 2 3 3 2 3 3 1 1 1 1 ...
#  $ State   : Factor w/ 51 levels "AK","AL","AR",..: 2 1 4 3 5 7 6 9 8 10 ...
```

``` r
head(wireless.data) # display first 6 rows by default
#   Wireless Region State
# 1     13.9      M    AL
# 2     11.7      W    AK
# 3     18.9      W    AZ
# 4     22.6      M    AR
# 5      9.0      W    CA
# 6     16.7      W    CO
```

### B) Loading a Data Set from the Working Directory.

R is always pointed at a directory/folder on your machine where it looks for data sets and source files. To check your current working directory, you can run the command `getwd()` in the RStudio console.

There are a number of ways to change the current working directory:

-   Use the [setwd](https://stat.ethz.ch/R-manual/R-devel/library/base/html/getwd.html) R function

-   Click `Session > Set Working Dir > Choose Directory` menu . This will also change directory location of the Files pane.

-   From within the `Files` panel (lower right), click `More > Set As Working Directory` menu. (Navigation within the Files pane alone will not change the working directory.)

> As good practice, save your `.Rmd` exercise file and your `.csv` data file in the same folder.

### Data on flight delays on the tarmac

Download `flight.delay.csv` from this [link](https://goo.gl/QjCxDz). Save this file into your local directory.

``` r
getwd() # no arguments needed
# [1] "C:/Users/Pailden/Google Drive/SIUE_Class/rstatlab/rstatlab"
delay <- read.csv("flight.delay.csv", header = TRUE)
str(delay)
# 'data.frame': 17 obs. of  3 variables:
#  $ Airline             : Factor w/ 17 levels "AirTran","American",..: 8 6 7 5 3 17 10 2 12 11 ...
#  $ Delays              : int  93 72 81 29 44 46 18 48 24 17 ...
#  $ Rate.per.10K.Flights: num  4.9 4.1 2.8 2.7 1.6 1.6 1.4 1.3 1.2 1.1 ...
head(delay)
#          Airline Delays Rate.per.10K.Flights
# 1     ExpressJet     93                  4.9
# 2    Continental     72                  4.1
# 3          Delta     81                  2.8
# 4         Comair     29                  2.7
# 5 American Eagle     44                  1.6
# 6     US Airways     46                  1.6
```

------------------------------------------------------------------------
