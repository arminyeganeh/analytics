---
description: Lab 5, 150-250 lines, 1 hour to complete
---

# Transforming Data in R

### Reshaping data with `tidyr`

The concept of “tidy data” was established by Hadley Wickham and represents a standardized way to link the structure of a dataset (i.e., physical layout) with its semantics (i.e., meaning). The objective should always be to get a dataset into a tidy form which consists of:

* Each variable forms a column
* Each observation forms a row
* Each type of observational unit forms a table&#x20;

To create tidy data you need to be able to reshape your data, preferably via efficient and simple code. To help with this process Hadley created the package `tidyr`. This section covers the basics of `tidyr` to help you reshape your data as necessary.

### Making wide data long

There are times when our data is considered “wide” or “unstacked” and a common attribute/variable of concern is spread out across columns. To reformat the data such that these common attributes are gathered together as a single variable, the function `gather()` will take multiple columns and collapse them into key-value pairs, duplicating all other columns as needed. For example, let’s say we have the given data frame.

```r
wide<-data.frame(Group=c(1,1,1,1,2,2,2,2,3,3,3,3),
               Year=c(2006,2007,2008,2009,2006,2007,2008,2009,2006,2007,2008,2009),
               Qtr.1=c(15,12,22,10,12,16,13,23,11,13,17,14),
               Qtr.2=c(16,13,22,14,13,14,11,20,12,11,12,9),
               Qtr.3=c(19,27,24,20,25,21,29,26,22,27,23,31),
               Qtr.4=c(17,23,20,16,18,19,15,20,16,21,19,24))
```

This data is considered wide since the time variable (represented as quarters) is structured such that each quarter represents a variable. To re-structure the time component as an individual variable, we can gather each quarter within one column variable and also gather the values associated with each quarter in a second column variable.

```r
library (tidyr)
long <- wide %>% gather (Quarter, Revenue, Qtr.1:Qtr.4)
# note, for brevity, we only show the first 15 observations
head (long, 15)
## Source: local data frame [15 x 4]
##
## Group Year Quarter Revenue
##  (int) (int) (fctr) (int)
## 1    1  2006  Qtr.1    15
## 2    1  2007  Qtr.1    12
## 3    1  2008  Qtr.1    22
## 4    1  2009  Qtr.1    10
## 5    2  2006  Qtr.1    12
## 6    2  2007  Qtr.1    16
## 7    2  2008  Qtr.1    13
## 8    2  2009  Qtr.1    23
## 9    3  2006  Qtr.1    11
## 10   3  2007  Qtr.1    13
## 11   3  2008  Qtr.1    17
## 12   3  2009  Qtr.1    14
## 13   1  2006  Qtr.2    16
## 14   1  2007  Qtr.2    13
## 15   1  2008  Qtr.2    22
```

It’s important to note that there is fl exibility in how you specify the columns you would like to gather. These all produce the same results:

```r
wide %>% gather (Quarter, Revenue, Qtr.1:Qtr.4)
wide %>% gather (Quarter, Revenue, -Group, -Year)
wide %>% gather (Quarter, Revenue, 3:6)
wide %>% gather (Quarter, Revenue, Qtr.1, Qtr.2, Qtr.3, Qtr.4)
```

### Making long data wide

There are also times when we are required to turn long formatted data into wide formatted data. As a complement to `gather()` the function `spread()` spreads a key-value pair across multiple columns. So now let’s take our data frame `long` from above and turn the variable `Quarter` into column headings and spread the `Revenue` values across the quarters they are related to.

```r
back2wide <- long %>% spread (Quarter, Revenue)
back2wide

## Source: local data frame [12 x 6]
##
## Group Year Qtr.1 Qtr.2 Qtr.3 Qtr.4
##  (int) (int) (int) (int) (int) (int)
## 1    1  2006    15    16    19    17
## 2    1  2007    12    13    27    23
## 3    1  2008    22    22    24    20
## 4    1  2009    10    14    20    16
## 5    2  2006    12    13    25    18
## 6    2  2007    16    14    21    19
## 7    2  2008    13    11    29    15
## 8    2  2009    23    20    26    20
## 9    3  2006    11    12    22    16
## 10   3  2007    13    11    27    21
## 11   3  2008    17    12    23    19
## 12   3  2009    14     9    31    24
```

### Splitting a single column into multiple columns

Many times a single column variable will capture multiple variables or even parts of a variable you just don’t care about. This is exemplified in the following data frame `messy_df`. Here, the variable `Grp_Ind` combines an individual variable `(a, b, c)` with the group variable `(1, 2, 3`. The variable `Yr_Mo` combines a year variable with a month variable, etc. In each case, there may be a purpose for separating parts of these columns into separate variables.

```r
messy_df
##   Grp_Ind    Yr_Mo       City_State Extra_variable
## 1     1.a 2006_Jan      Dayton (OH)   XX01person_1
## 2     1.b 2006_Feb Grand Forks (ND)   XX02person_2
## 3     1.c 2006_Mar       Fargo (ND)   XX03person_3
## 4     2.a 2007_Jan   Rochester (MN)   XX04person_4
```

This can be accomplished using the function `separate()` which turns a single character column into multiple columns. Additional arguments provide some flexibility with separating columns.

### Combining multiple columns into a single column

Similarly, there are times when we would like to combine the values of two variables. As a compliment to `separate()` the function `unite()` is a convenient function to paste together multiple variable values into one. Consider the following data frame that has separate date variables. To perform time series analysis or for visualization, we may desire to have a single date column.

```r
expenses <- data.frame(Year=c(2015,2015,2015,2015),
                       Month=c(01,02,02,03),
                       Day=c(01,05,22,10),
                       Expense=c(500,90,250,325))
```

To perform time series analysis or for visualizations, we may desire to have a single date column. We can accomplish this by uniting these columns into one variable with `unite()`.

```
# default separator when uniting is "_"
expenses %>% unite (col = "Date", c (Year, Month, Day))

##        Date Expense
## 1  2015_1_1     500
## 2  2015_2_5      90
## 3 2015_2_22     250
## 4 2015_3_10     325

# specify sep argument to change the separator
expenses %>% unite (col = "Date", c (Year, Month, Day), sep = "-")

##        Date Expense
## 1  2015-1-1     500
## 2  2015-2-5      90
## 3 2015-2-22     250
## 4 2015-3-10     325 


```

### Transforming data with `dplyr`

Transforming data can include filtering, summarizing, and ordering data by different means. This also includes combining disparate data sets, creating new variables, and many other manipulation tasks. Although many fundamental data transformation and manipulation functions exist in R, historically they have been a bit convoluted and lacked a consistent and cohesive code structure. Consequently, Hadley Wickham developed the popular package `dplyr` to make these data processing tasks more efficient along with a syntax that is consistent and easier to remember and read.

`dplyr` originates in the popular package `plyr` also produced by Hadley Wickham. `plyr` covers data transformation and manipulation for a range of data structures (data frames, lists, arrays) whereas `dplyr` is focused on the transformation and manipulation of data frames. `dplyr` offers far more functionality than we cover in this section. We cover the seven primary functions of `dplyr` for data transformation and manipulation. The full list of capabilities can be found in the highly recommended `dplyr` reference manual. Also, similar to `tidyr`, the package `dplyr` has the operator `%>%` baked into its functionality.

For most of these examples, we will use the following census data, which includes the K-12 public school expenditures by state. This data frame currently is 50 × 16 and includes expenditure data for 14 unique years (50 states and has data through the year 2011). Here I only show you a subset of the data.

```
##   Division      State   X1980    X1990    X2000    X2001    X2002   X2003
## 1        6    Alabama 1146713  2275233  4176082  4354794  4444390  4657643
## 2        9     Alaska  377947   828051  1183499  1229036  1284854  1326226
## 3        8    Arizona  949753  2258660  4288739  4846105  5395814  5892227
## 4        7   Arkansas  666949  1404545  2380331  2505179  2822877  2923401
## 5        9 California 9172158 21485782 38129479 42908787 46265544 47983402
## 6        8   Colorado 1243049  2451833  4401010  4758173  5151003  5551506
##      X2004     X2005    X2006    X2007    X2008    X2009    X2010    X2011
## 1  4812479   5164406  5699076  6245031  6832439  6683843  6670517  6592925
## 2  1354846   1442269  1529645  1634316  1918375  2007319  2084019  2201270
## 3  6071785   6579957  7130341  7815720  8403221  8726755  8482552  8340211
## 4  3109644   3546999  3808011  3997701  4156368  4240839  4459910  4578136
## 5 49215866  50918654 53436103 57352599 61570555 60080929 58248662 57526835
## 6  5666191   5994440  6368289  6579053  7338766  7187267  7429302  7409462
```
