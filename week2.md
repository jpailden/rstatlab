Week 2: Reading Data and Initial Exploration
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

### Mosaic package

The `mosaic` package was written to simplify the use of R for introductory statistics courses. A short summary of the R needed to teach introductory statistics can be found in the mosaic package vignettes (<http://cran.r-project.org/web/packages/mosaic>).

``` r
install.packages(`mosaic`)
library(mosaic)
```

### Numerical Summaries

We can compute the mean of the `weight` variable in the data `chickwts`.

``` r
mean(~ weight, data = chickwts)
# [1] 261
```

The `mean` function in the `mosaic` package supports formula interface common to regression and anova models (more on this later). The same output can be obtained using `$` notation.

``` r
mean(chickwts$weight)
# [1] 261
```

We can also tally the count or frequency of various feed supplements.

``` r
tally(~ feed, data = chickwts)
# feed
#    casein horsebean   linseed  meatmeal   soybean sunflower 
#        12        10        12        11        14        12
```

The formula interface allows us to compute the mean weight for every feed supplements.

``` r
mean(weight ~ feed, data = chickwts)
#    casein horsebean   linseed  meatmeal   soybean sunflower 
#       324       160       219       277       246       329
```

You can also compute other numerical summaries such as the `median()`, variance `var()`, standard deviation `sd()`, etc.

Another handy function in the `mosaic` package is `favstats` which outputs the

-   five-number summary
-   mean
-   standard deviation
-   count
-   number missing values

``` r
favstats(~ weight, data = chickwts)
#  min  Q1 median  Q3 max mean   sd  n missing
#  108 204    258 324 423  261 78.1 71       0
favstats(weight ~ feed, data = chickwts)
#        feed min  Q1 median  Q3 max mean   sd  n missing
# 1    casein 216 277    342 371 404  324 64.4 12       0
# 2 horsebean 108 137    152 176 227  160 38.6 10       0
# 3   linseed 141 178    221 258 309  219 52.2 12       0
# 4  meatmeal 153 250    263 320 380  277 64.9 11       0
# 5   soybean 158 207    248 270 329  246 54.1 14       0
# 6 sunflower 226 313    328 340 423  329 48.8 12       0
```

### Graphical Summaries

The `mosaic` package also includes commands for common graphical summaries.

Bargraph for Categorical Variables

``` r
bargraph(~ feed, data = chickwts)
```

<img src="figures/11-wk02-1.png" style="display: block; margin: auto;" /> Dot Plots are used often to describe small size numerical data sets.

``` r
dotPlot(~ weight, data = chickwts)
```

<img src="figures/12-wk02-1.png" style="display: block; margin: auto;" />

Histograms are used often to describe moderate to large size numerical data sets.

``` r
histogram(~ weight, data = chickwts)
```

<img src="figures/13-wk02-1.png" style="display: block; margin: auto;" />

### Formula Interface

The formula interface syntax is used for graphical summaries, numerical summaries, and inference procedures.

> `goal(y ~ x | z,  data = ..., groups = ...)`

For plots,

-   `y`: y-axis variable

-   `x`: x-axis variable

-   `z`: z-axis variable

-   `groups`: conditioning variable (overlaid graphs)

Dotplots for `weight` across different `feed` panels.

``` r
dotPlot(~ weight | feed, data = chickwts, cex = 0.8) # reduce the size of the dots by 20%
```

<img src="figures/14-wk02-1.png" style="display: block; margin: auto;" /> Boxplots for `weights` by different `feeds` in the same panel.

``` r
bwplot(weight ~ feed, data = chickwts)
```

<img src="figures/15-wk02-1.png" style="display: block; margin: auto;" />

2) Reading CSV data in R
------------------------

Data analysis using R often involves importing or reading data at some point. While R can read other data types, comma separated files (.csv) are much easier to work with. Saving data files in .csv format is standard in practice. We use the command `read.csv` to read .csv data in R.

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

Descriptive summaries for the Going Wireless data

``` r
favstats(~ Wireless, data = wireless.data)
#  min   Q1 median Q3  max mean   sd  n missing
#  5.1 10.8   15.2 19 25.5 14.8 5.34 51       0
favstats(Wireless ~ Region, data = wireless.data)
#   Region min    Q1 median   Q3  max mean   sd  n missing
# 1      E 5.1  8.65   11.4 15.2 20.6 11.9 4.59 19       0
# 2      M 6.4 15.10   16.9 21.1 23.2 17.4 4.55 19       0
# 3      W 8.0 10.80   16.3 18.9 25.5 15.3 5.66 13       0
```

Graphical summaries for the Going Wireless data

``` r
histogram(~ Wireless | Region, data = wireless.data, width = 3) # histogram bin width = 3
```

<img src="figures/20-wk02-1.png" style="display: block; margin: auto;" />

``` r
bwplot(Wireless ~ Region, data = wireless.data) # boxplots
```

<img src="figures/20-wk02-2.png" style="display: block; margin: auto;" />

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
favstats(~ Rate.per.10K.Flights, data = delay)
#  min  Q1 median  Q3 max mean  sd  n missing
#  0.1 0.8    1.2 1.6 4.9 1.61 1.3 17       0
histogram(~ Rate.per.10K.Flights, data = delay)
```

<img src="figures/21-wk02-1.png" style="display: block; margin: auto;" />

Scatterplot with `Delays` on the horizontal axis and `Rate.per.10K.Flights` on the vertical axis.

``` r
xyplot(Rate.per.10K.Flights ~ Delays, data = delay)
```

<img src="figures/22-wk02-1.png" style="display: block; margin: auto;" />

------------------------------------------------------------------------

Laboratory Exercise for Week 2 (10 points)
==========================================

*Directions*:

-   Download this week's exercise file and open the file using RStudio. Right click the link and "Save as..." to your desktop. (&lt;&gt;)
-   This file type is called RMarkdown and is used widely to share and collaborate R outputs. More information is found on this link (<http://rmarkdown.rstudio.com/articles_docx.html>).
-   All R codes should be written inside code chunks. Check (<http://rmarkdown.rstudio.com/authoring_rcodechunks.html>).
-   Submit your completed laboratory exercise using Blackboard's Turnitin feature. Your Turnitin upload link is found on your Blackboard Course shell under the Laboratory folder.
