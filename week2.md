Week 2: Reading Data in R
================
written by Junvie Pailden

### Data Set Structure

A data set usually includes variables (columns) and observations (rows). We can check the structure of the data using the function `str()`. The output includes data information such as

-   dimension (number of observations and variables),
-   variable names
-   data type
    -   `logi`: logical, `TRUE` or `FALSE`
    -   `int`: integer
    -   `num`: numerical
    -   `chr`: character
    -   `Factor`: categorical with defined levels

1) Working with data provided by R packages
-------------------------------------------

Lets consider the data `chickwts` in the package `datasets` included with every R installation. The data was the result of an experiment conducted to measure and compare the effectiveness of various feed supplements on the growth rate of chickens.

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
chickwts[1, 1] # display 1st row and 1st column entry
# [1] 179
chickwts[, 2] # display column 2, or use chickwts$feed
#  [1] horsebean horsebean horsebean horsebean horsebean horsebean horsebean
#  [8] horsebean horsebean horsebean linseed   linseed   linseed   linseed  
# [15] linseed   linseed   linseed   linseed   linseed   linseed   linseed  
# [22] linseed   soybean   soybean   soybean   soybean   soybean   soybean  
# [29] soybean   soybean   soybean   soybean   soybean   soybean   soybean  
# [36] soybean   sunflower sunflower sunflower sunflower sunflower sunflower
# [43] sunflower sunflower sunflower sunflower sunflower sunflower meatmeal 
# [50] meatmeal  meatmeal  meatmeal  meatmeal  meatmeal  meatmeal  meatmeal 
# [57] meatmeal  meatmeal  meatmeal  casein    casein    casein    casein   
# [64] casein    casein    casein    casein    casein    casein    casein   
# [71] casein   
# Levels: casein horsebean linseed meatmeal soybean sunflower
```

2) Reading CSV data in R
------------------------

Data analysis using R often involves importing or reading data at some point. While R can read other data types, comma separated files (.csv) are much easier to work with. Saving data files in .csv format is standard in practice. We use the command `read.csv` to read .csv data in R.

    read.csv(file, header = TRUE)

The arguments of the `read.csv` function includes (among others)

-   `file`: name of the data set

-   `header = TRUE`: if the file contains the names of the variables as its first line.

### A) Loading a data set from a webpage using its URL (Universal Resource Locator).

The article *Going Wireless (AARP Bulletin, June 2009)* reported the estimated percentage of house- holds with only wireless phone service (no land line) for the 50 U.S. states and the District of Columbia. In the accompanying data table, each state was also classified into one of three geographical regions—West (W), Middle states (M), and East (E).

``` r
wireless.data <- read.csv("http://siue.edu/~jpailde/data/s244/Ex0125.csv", header = TRUE)
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

-   Use the `Tools | Change Working Dir...` menu (`Session | Set Working Directory` on a mac). This will also change directory location of the Files pane.

-   From within the Files pane, use the More | Set As Working Directory menu. (Navigation within the Files pane alone will not change the working directory.)

### Data on flight delays on the tarmac

Download `Ex0127.csv` from this link (<http://siue.edu/~jpailde/data/s244/Ex0125.csv>). Save this file into your local directory.

``` r
getwd() # no arguments needed
# [1] "/Users/JPMac/Dropbox/rstatlab/rstatlab"
delay <- read.csv("Ex0127.csv", header = TRUE)
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

Laboratory Exercise for Week 2 (10 points)
==========================================

*Directions*:

-   Download this week's exercise file and open the file using RStudio. Right click the link and "Save as..." to your desktop. (&lt;&gt;)
-   This file type is called RMarkdown and is used widely to share and collaborate R outputs. More information is found on this link (<http://rmarkdown.rstudio.com/articles_docx.html>).
-   All R codes should be written inside code chunks. Check (<http://rmarkdown.rstudio.com/authoring_rcodechunks.html>).
-   Submit your completed laboratory exercise using Blackboard's Turnitin feature. Your Turnitin upload link is found on your Blackboard Course shell under the Laboratory folder.

1.  The `RailTrail` dataset within the `mosaic` package includes the counts of crossings of a rail trail in Northampton, Massachusetts for 90 days in 2005. City officials are interested in understanding usage of the trail network, and how it changes as a function of temperature and day of the week.
    1.  Check the structure of the`RailTrail`.
    2.  How many variables and observations are in the data set?
    3.  Which variables are `integer` type?
    4.  Display the first 4 rows of the `RailTrail`.

\#\# Code chunk

``` r
# Insert your code for this question after this line
```
