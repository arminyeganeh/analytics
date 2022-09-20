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

### Selecting variables of interest

When working with a sizable data frame, often we desire to only assess specific variables. The function `select()`  allows you to select and/or rename variables. Let’s say our goal is to only assess the five most recent years worth of expenditure data. Applying the function `select()` we can select only the variables of concern.

```
sub_exp <- expenditures %>% select (Division, State, X2007:X2011) 

# for brevity only display the first 6 rows
head (sub_exp)
##     Division      State    X2007    X2008    X2009    X2010    X2011
## 1          6    Alabama  6245031  6832439  6683843  6670517  6592925
## 2          9     Alaska  1634316  1918375  2007319  2084019  2201270
## 3          8    Arizona  7815720  8403221  8726755  8482552  8340211
## 4          7   Arkansas  3997701  4156368  4240839  4459910  4578136
## 5          9 California 57352599 61570555 60080929 58248662 57526835
## 6          8   Colorado  6579053  7338766  7187267  7429302  7409462 
```

We can also apply some of the special functions within `select()`. For instance, we can select all variables that start with ‘X’ (The command `?select` shows available functions):

```
expenditures %>%
select (starts_with ("X")) %>%
head()
 
## X1980 X1990 X2000 X2001 X2002 X2003 X2004 X2005
## 1 1146713 2275233 4176082 4354794 4444390 4657643 4812479 5164406
## 2 377947 828051 1183499 1229036 1284854 1326226 1354846 1442269
## 3 949753 2258660 4288739 4846105 5395814 5892227 6071785 6579957
## 4 666949 1404545 2380331 2505179 2822877 2923401 3109644 3546999
## 5 9172158 21485782 38129479 42908787 46265544 47983402 49215866 50918654
## 6 1243049 2451833 4401010 4758173 5151003 5551506 5666191 5994440
## X2006 X2007 X2008 X2009 X2010 X2011
## 1 5699076 6245031 6832439 6683843 6670517 6592925
## 2 1529645 1634316 1918375 2007319 2084019 2201270
## 3 7130341 7815720 8403221 8726755 8482552 8340211
## 4 3808011 3997701 4156368 4240839 4459910 4578136
## 5 53436103 57352599 61570555 60080929 58248662 57526835
## 6 6368289 6579053 7338766 7187267 7429302 7409462
```

You can also deselect variables by using “-” prior to their name or function. The following produces the inverse of the functions above:

```
expenditures %>% select (-X1980:-X2006)
expenditures %>% select (- starts_with ("X")) 
```

And for convenience, you can rename the selected variables with two options:

```
# select and rename a single column
expenditures %>% select (Yr_1980 = X1980)

# Select and rename the multiple variables with an "X" prefix:
expenditures %>% select (Yr_ = starts_with ("X"))

# keep all variables and rename a single variable
expenditures %>% rename (`2011` = X2011) 
```

### Filtering rows

Filtering data is a common task to identify/select observations in which a particular variable matches a specific value/condition. The function `filter()` provides this capability. Continuing with our data `sub_exp` which includes only the recent 5 years' worth of expenditures, we can filter by `Division`:

```r
sub_exp %>% filter (Division == 3)

##     Division     State    X2007    X2008    X2009    X2010    X2011
## 1          3  Illinois 20326591 21874484 23495271 24695773 24554467
## 2          3   Indiana  9497077  9281709  9680895  9921243  9687949
## 3          3  Michigan 17013259 17053521 17217584 17227515 16786444
## 4          3      Ohio 18251361 18892374 19387318 19801670 19988921
## 5          3 Wisconsin  9029660  9366134  9696228  9966244 10333016
```

We can apply multiple logic rules in the function `filter()` such as:

```
< Less than                    != Not equal to
> Greater than                 %in% Group membership
== Equal to                    is.na is NA
<= Less than or equal to       !is.na is not NA
>= Greater than or equal to    &,|,! Boolean operators
```

For instance, we can filter for Division 3 and expenditures in 2011 that were greater than $10B. This results in Indiana being excluded since it falls within division 3 and its expenditures were < $10B (FYI—the raw census data are reported in units of $1000)

```r
# Raw census data are in units of $1000
sub_exp %>% filter (Division == 3, X2011 > 10000000)

##   Division     State    X2007    X2008    X2009    X2010    X2011
## 1        3  Illinois 20326591 21874484 23495271 24695773 24554467
## 2        3  Michigan 17013259 17053521 17217584 17227515 16786444
## 3        3      Ohio 18251361 18892374 19387318 19801670 19988921
## 4        3 Wisconsin  9029660  9366134  9696228  9966244 10333016
```

There are additional filtering and subsetting functions that are quite useful:

```r
# remove duplicate rows
sub_exp %>% distinct ()

# random sample, 50% sample size without replacement
sub_exp %>% sample_frac (size = 0.5, replace = FALSE)

# random sample of 10 rows with replacement
sub_exp %>% sample_n (size = 10, replace = TRUE)

# select rows 3-5
sub_exp %>% slice (3:5)

# select top n entries - in this case ranks variable X2011 and selects

# the rows with the top 5 values
sub_exp %>% top_n (n = 5, wt = X2011) 
```

Grouping data by categorical variables

Often, observations are nested within groups or categories and our goal is to perform statistical analysis both at the observation level and also at the group level. The function `group_by()` allows us to create these categorical groupings.&#x20;

The function `group_by()` is a silent function in which no observable manipulation of the data is performed as a result of applying the function. Rather, the only change you’ll notice is when you print the data frame you will notice underneath the source information and prior to the actual data frame, an indicator of what variable the data is grouped by will be provided. In the example that follows you’ll notice that we grouped by `Division` and there are nine categories for this variable. The real magic of the function `group_by()` comes when we perform summary statistics which we will cover shortly.

```r
group.exp <- sub_exp %>% group_by (Division) 

group.exp
## Source: local data frame [50 x 7]
## Groups: Division [9]
##
## Division       State    X2007    X2008    X2009    X2010    X2011
##    (int)       (chr)    (int)    (int)    (int)    (int)    (int)
## 1      6     Alabama  6245031  6832439  6683843  6670517  6592925
## 2      9      Alaska  1634316  1918375  2007319  2084019  2201270
## 3      8     Arizona  7815720  8403221  8726755  8482552  8340211
## 4      7    Arkansas  3997701  4156368  4240839  4459910  4578136
## 5      9  California 57352599 61570555 60080929 58248662 57526835
## 6      8    Colorado  6579053  7338766  7187267  7429302  7409462
## 7      1 Connecticut  7855459  8336789  8708294  8853337  9094036
## 8      5    Delaware  1437707  1489594  1518786  1549812  1613304
## 9      5     Florida 22887024 24224114 23328028 23349314 23870090
## 10     5     Georgia 14828715 16030039 15976945 15730409 15527907
## ..     …           …        …        …        …        …        … 

# we can ungroup our data with
ungroup (group.exp)
## Source: local data frame [50 x 7]
##
## Division       State    X2007    X2008    X2009    X2010    X2011
##    (int)       (chr)    (int)    (int)    (int)    (int)    (int)
## 1      6     Alabama  6245031  6832439  6683843  6670517  6592925
## 2      9      Alaska  1634316  1918375  2007319  2084019  2201270
## 3      8     Arizona  7815720  8403221  8726755  8482552  8340211
## 4      7    Arkansas  3997701  4156368  4240839  4459910  4578136
## 5      9  California 57352599 61570555 60080929 58248662 57526835
## 6      8    Colorado  6579053  7338766  7187267  7429302  7409462
## 7      1 Connecticut  7855459  8336789  8708294  8853337  9094036
## 8      5    Delaware  1437707  1489594  1518786  1549812  1613304
## 9      5     Florida 22887024 24224114 23328028 23349314 23870090
## 10     5     Georgia 14828715 16030039 15976945 15730409 15527907
## ..     …           …        …        …        …        …        … 


```

### Performing summary statistics on variables

Obviously, the goal of all this data wrangling is to be able to perform statistical analysis on our data. The function `summarise()` allows us to perform the majority of summary statistics when performing exploratory data analysis. Let’s get the mean expenditure value across all states in 2011:

```
sub_exp %>% summarise (Mean_2011 = mean (X2011))
## Mean_2011
## 1 10513678
```

Not too bad, let’s get some more summary stats:

```
sub_exp %>% summarise (Min = min (X2011, na.rm = TRUE),
                       Median = median (X2011, na.rm = TRUE),
                       Mean = mean (X2011, na.rm = TRUE),
                       Var = var (X2011, na.rm = TRUE),
                       SD = sd (X2011, na.rm = TRUE),
                       Max = max (X2011, na.rm = TRUE),
                       N = n ())
 
##    Min     Median  Mean     Var         SD       Max      N
## 1  1049772 6527404 10513678 1.48619e+14 12190938 57526835 50
```

This information is useful, but being able to compare summary statistics at multiple levels is when you really start to gather some insights. This is where the function `group_by()` comes in. First, let’s group by `Division` and see how the different regions compare across years 2010 and 2011.

```r
sub_exp %>%
  group_by (Division)%>%
  summarise (Mean_2010 = mean (X2010, na.rm = TRUE),
             Mean_2011 = mean (X2011, na.rm = TRUE))
             
## Source: local data frame [9 x 3]
##
## Division Mean_2010 Mean_2011
##    (int)     (dbl)     (dbl)
## 1      1   5121003   5222277
## 2      2  32415457  32877923
## 3      3  16322489  16270159
## 4      4   4672332   4672687
## 5      5  10975194  11023526
## 6      6   6161967   6267490
## 7      7  14916843  15000139
## 8      8   3894003   3882159
## 9      9  15540681  15468173
```

Now we’re starting to see some differences pop out. How about we compare states within a Division? We can start to apply multiple functions we’ve learned so far to get the 5 year average for each state within Division 3.

```r
library (tidyr)

sub_exp %>%
  gather (Year, Expenditure, X2007:X2011) %>% # turn wide data to long
  filter (Division == 3) %>%                  # only assess Division 3
  group_by (State) %>%                        # summarize data by state
  summarise (Mean = mean (Expenditure),       # calculate mean & SD
             SD = sd (Expenditure))

## Source: local data frame [5 x 3]
##
## State Mean SD
## (chr) (dbl) (dbl)
## 1 Illinois 22989317 1867527.7
## 2 Indiana 9613775 238971.6
## 3 Michigan 17059665 180245.0
## 4 Ohio 19264329 705930.2
## 5 Wisconsin 9678256 507461.2
```

There are several built-in summary functions in `dplyr` as displayed below. You can also build in your own functions as well.

* Center: mean(), median()
* Spread: sd(), IQR(), mad()
* Range: min(), max(), quantile()
* Position: first(), last(), nth()
* Count: n(), n\_distinct()
* Logical: any(), all()

### Arranging variables by value

Sometimes we wish to view observations in rank order for a particular variable(s). The function `arrange()` allows us to order data by variables in ascending or descending order. Let’s say we want to assess the average expenditures by division. We could apply the function `arrange()` at the end to order the divisions from lowest to highest expenditure for 2011. This makes it easier to see the significant differences between Divisions 8, 4, 1, and 6 as compared to Divisions 5, 7, 9, 3, and 2.

```r
sub_exp %>%
  group_by (Division) %>%
  summarise (Mean_2010 = mean (X2010, na.rm = TRUE),
             Mean_2011 = mean (X2011, na.rm = TRUE)) %>%
  arrange (Mean_2011)
  
## Source: local data frame [9 x 3]
## Division Mean_2010 Mean_2011
##    (int)     (dbl)    (dbl)
## 1      8   3894003  3882159
## 2      4   4672332  4672687
## 3      1   5121003  5222277
## 4      6   6161967  6267490
## 5      5  10975194 11023526
## 6      7  14916843 15000139
## 7      9  15540681 15468173
## 8      3  16322489 16270159
## 9      2  32415457 32877923
```

We can also apply a descending argument to rank order from highest to lowest. The following shows the same data but in descending order by applying `desc()` within the function `arrange()`.

```r
sub_exp %>%
 group_by (Division) %>%
 summarise (Mean_2010 = mean (X2010, na.rm = TRUE),
 Mean_2011 = mean (X2011, na.rm = TRUE)) %>%
 arrange (desc (Mean_2011))
 
## Source: local data frame [9 x 3]
##
## Division Mean_2010 Mean_2011
##    (int)    (dbl)    (dbl)
## 1      2 32415457 32877923
## 2      3 16322489 16270159
## 3      9 15540681 15468173
## 4      7 14916843 15000139
## 5      5 10975194 11023526
## 6      6  6161967  6267490
## 7      1  5121003  5222277
## 8      4  4672332  4672687
## 9      8  3894003  3882159
```

### Joining data sets

Often we have separate data frames that can have common and differing variables for similar observations and we wish to join these data frames together. `dplyr` offers multiple joining functions (`xxx_join()`) that provide alternative ways to join data frames:

* inner\_join()
* left\_join()
* right\_join()
* full\_join()
* semi\_join()
* anti\_join()

Our public education expenditure data represents then-year dollars. To make any accurate assessments of longitudinal trends and comparisons we need to adjust for inflation. I have the following data frame which provides inflation adjustment factors for base-year 2012 dollars.

```
##        Year Annual  Inflation
##   28   2007 207.342 0.9030811
##   29   2008 215.303 0.9377553
##   30   2009 214.537 0.9344190
##   31   2010 218.056 0.9497461
##   32   2011 224.939 0.9797251
##   33   2012 229.594 1.0000000 
```

To join to the expenditure data I obviously need to get my expenditure data in the proper form that allows me to join these two data frames. I can apply the following functions to accomplish this:

```
long_exp <- sub_exp %>%
 gather (Year, Expenditure, X2007:X2011) %>%
 separate (Year, into= c ("x", "Year"), sep = "X") %>%
 select (-x) %>%
 mutate (Year = as.numeric (Year))

head (long_exp)
##   Division      State Year Expenditure
## 1        6    Alabama 2007     6245031
## 2        9     Alaska 2007     1634316
## 3        8    Arizona 2007     7815720
## 4        7   Arkansas 2007     3997701
## 5        9 California 2007    57352599
## 6        8   Colorado 2007     6579053
```

I can now apply the function `left_join()` to join the inflation data to the expenditure data. This aligns the data in both data frames by the Year variable and then joins the remaining inflation data to the expenditure data frame as new variables.

```r
join_exp <- long_exp %>% left_join (inflation)
head (join_exp)

##   Division      State Year Expenditure  Annual Inflation
## 1        6    Alabama 2007     6245031 207.342 0.9030811
## 2        9     Alaska 2007     1634316 207.342 0.9030811
## 3        8    Arizona 2007     7815720 207.342 0.9030811
## 4        7   Arkansas 2007     3997701 207.342 0.9030811
## 5        9 California 2007    57352599 207.342 0.9030811
## 6        8   Colorado 2007     6579053 207.342 0.9030811
```

To illustrate the other joining methods we can use the a and b data frames as follows:

```r
a<-data.frame(x1=c("A","B","C"),
              x2=c(1,2,3))
b<-data.frame(x1=c("A","B","D"),
              x2=c(TRUE,FALSE,TRUE))
a
## x1 x2
## 1 A 1
## 2 B 2
## 3 C 3
b
## x1 x2
## 1 A TRUE
## 2 B FALSE
## 3 D TRUE

# include all of a, and join matching rows of b
left_join (a, b, by = "x1")
## x1 x2.x x2.y
## 1 A 1 TRUE
## 2 B 2 FALSE
## 3 C 3 NA

# include all of b, and join matching rows of a
right_join (a, b, by = "x1")
## x1 x2.x x2.y
## 1 A 1 TRUE
## 2 B 2 FALSE
## 3 D NA TRUE
# join data, retain only matching rows in both data frames

inner_join (a, b, by = "x1")
## x1 x2.x x2.y
## 1 A 1 TRUE
## 2 B 2 FALSE
# join data, retain all values, all rows

full_join (a, b, by = "x1")
## x1 x2.x x2.y
## 1 A 1 TRUE
## 2 B 2 FALSE
## 3 C 3 NA
## 4 D NA TRUE
# keep all rows in a that have a match in b

semi_join (a, b, by = "x1")
## x1 x2
## 1 A 1
## 2 B 2
# keep all rows in a that do not have a match in b

anti_join (a, b, by = "x1")
## x1 x2
## 1 C 3
```

There are additional `dplyr` functions for merging data sets worth exploring:

```r
intersect (y, z) # Rows that appear in both y and z
union (y, z)     # Rows that appear in either or both y and z
setdiff (y, z)   # Rows that appear in y but not z
bind_rows (y, z) # Append z to y as new rows
bind_cols (y, z) # Append z to y as new columns
```

### Creating new variables

Often we want to create a new variable that is a function of the current variables in our data frame or we may just want to add a new variable that is external to our existing variables. The function `mutate()` allows us to add new variables while preserving the existing variables. If we go back to our previous dataframe `join_exp` remember that we joined inflation rates to our non-inflation adjusted expenditures for public schools. The dataframe looks like:

```
##   Division      State Year Expenditure  Annual Inflation
## 1        6    Alabama 2007     6245031 207.342 0.9030811
## 2        9     Alaska 2007     1634316 207.342 0.9030811
## 3        8    Arizona 2007     7815720 207.342 0.9030811
## 4        7   Arkansas 2007     3997701 207.342 0.9030811
## 5        9 California 2007    57352599 207.342 0.9030811
## 6        8   Colorado 2007     6579053 207.342 0.9030811
```

If we wanted to adjust our annual expenditures for inflation we can use `mutate()` to create a new inflation-adjusted cost variable which we will name `Adj_Exp`:

```r
inflation_adj <- join_exp %>% mutate (Adj_Exp = Expenditure / Inflation)
head (inflation_adj)

##   Division      State Year Expenditure  Annual Inflation  Adj_Exp
## 1        6    Alabama 2007     6245031 207.342 0.9030811  6915249
## 2        9     Alaska 2007     1634316 207.342 0.9030811  1809711
## 3        8    Arizona 2007     7815720 207.342 0.9030811  8654505
## 4        7   Arkansas 2007     3997701 207.342 0.9030811  4426735
## 5        9 California 2007    57352599 207.342 0.9030811 63507696
## 6        8   Colorado 2007     6579053 207.342 0.9030811  7285119
```

Let's say we wanted to create a variable that rank-orders state-level expenditures (inflation-adjusted) for the year 2010 from the highest level of expenditures to the lowest.

```
rank_exp <- inflation_adj %>%
  filter (Year == 2010) %>%
  arrange ( desc (Adj_Exp)) %>%
  mutate (Rank = 1: length (Adj_Exp))
 
head (rank_exp)
##   Division      State Year Expenditure  Annual Inflation  Adj_Exp Rank
## 1        9 California 2010    58248662 218.056 0.9497461 61330774    1
## 2        2   New York 2010    50251461 218.056 0.9497461 52910417    2
## 3        7      Texas 2010    42621886 218.056 0.9497461 44877138    3
## 4        3   Illinois 2010    24695773 218.056 0.9497461 26002501    4
## 5        2 New Jersey 2010    24261392 218.056 0.9497461 25545135    5
## 6        5    Florida 2010    23349314 218.056 0.9497461 24584797    6
```

If you wanted to assess the percent change in cost for a particular state you can use the `lag()` function within the function `mutate()`:

```
inflation_adj %>%
  filter (State == "Ohio") %>%
  mutate (Perc_Chg = (Adj_Exp - lag (Adj_Exp)) / lag (Adj_Exp))
  
## Division State Year Expenditure  Annual Inflation  Adj_Exp     Perc_Chg
## 1      3  Ohio 2007    18251361 207.342 0.9030811 20210102           NA
## 2      3  Ohio 2008    18892374 215.303 0.9377553 20146378 -0.003153057
## 3      3  Ohio 2009    19387318 214.537 0.9344190 20747992  0.029862103
## 4      3  Ohio 2010    19801670 218.056 0.9497461 20849436  0.004889357
## 5      3  Ohio 2011    19988921 224.939 0.9797251 20402582 -0.021432441
```

You could also look at what percent of all US expenditures each state made up in 2011. In this case, we use `mutate()` to take each state’s inflation-adjusted expenditure and divide it by the sum of the entire inflation-adjusted expenditure column. We also apply a second function that provides the cumulative percent in rank order. This shows that in 2011, the top 8 states with the highest expenditures represented over 50 % of the total U.S. expenditures in K-12 public schools (The non-inflation adjusted Expenditure, Annual & Inflation columns are removed so that the columns don’t wrap on the screen view):

```
cum_pct <- inflation_adj %>%
  filter (Year == 2011) %>%
  arrange ( desc (Adj_Exp)) %>%
  mutate (Pct_of_Total = Adj_Exp/ sum (Adj_Exp),
          Cum_Perc = cumsum (Pct_of_Total)) %>%
  select (-Expenditure, -Annual, -Inflation) 
  
head (cum_pct, 8)

##   Division State Year  Adj_Exp Pct_of_Total  Cum_Perc
## 1 9   California 2011 58717324   0.10943237 0.1094324
## 2 2     New York 2011 52575244   0.09798528 0.2074177
## 3 7        Texas 2011 43751346   0.08154005 0.2889577
## 4 3     Illinois 2011 25062609   0.04670957 0.3356673
## 5 5      Florida 2011 24364070   0.04540769 0.3810750
## 6 2   New Jersey 2011 24128484   0.04496862 0.4260436
## 7 2 Pennsylvania 2011 23971218   0.04467552 0.4707191
## 8 3         Ohio 2011 20402582   0.03802460 0.5087437  
```

An alternative to `mutate()` is `transmute()` which creates a new variable and then drops the other variables. In essence, it allows you to create a new data frame with only the new variables created. We can perform the same string of functions as above but this time use `transmute()` to only keep the newly created variables.

```
inflation_adj %>%
 filter (Year == 2011) %>%
 arrange ( desc (Adj_Exp)) %>%
 transmute (Pct_of_Total = Adj_Exp/ sum (Adj_Exp),
 Cum_Perc = cumsum (Pct_of_Total)) %>%
 head ()

## Pct_of_Total Cum_Perc
## 1 0.10943237 0.1094324
## 2 0.09798528 0.2074177
## 3 0.08154005 0.2889577
## 4 0.04670957 0.3356673
## 5 0.04540769 0.3810750
## 6 0.04496862 0.4260436
```

Lastly, you can apply the `summarise` and `mutate` functions to multiple columns by using `across()` .
