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
## (int) (int) (fctr) (int)
## 1 1 2006 Qtr.1 15
## 2 1 2007 Qtr.1 12
## 3 1 2008 Qtr.1 22
## 4 1 2009 Qtr.1 10
## 5 2 2006 Qtr.1 12
## 6 2 2007 Qtr.1 16
## 7 2 2008 Qtr.1 13
## 8 2 2009 Qtr.1 23
## 9 3 2006 Qtr.1 11
## 10 3 2007 Qtr.1 13
## 11 3 2008 Qtr.1 17
## 12 3 2009 Qtr.1 14
## 13 1 2006 Qtr.2 16
## 14 1 2007 Qtr.2 13
## 15 1 2008 Qtr.2 22
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
## (int) (int) (int) (int) (int) (int)
## 1 1 2006 15 16 19 17
## 2 1 2007 12 13 27 23
## 3 1 2008 22 22 24 20
## 4 1 2009 10 14 20 16
## 5 2 2006 12 13 25 18
## 6 2 2007 16 14 21 19
## 7 2 2008 13 11 29 15
## 8 2 2009 23 20 26 20
## 9 3 2006 11 12 22 16
## 10 3 2007 13 11 27 21
## 11 3 2008 17 12 23 19
## 12 3 2009 14 9 31 24
```



