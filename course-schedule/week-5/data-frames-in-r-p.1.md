---
description: Lab 4, 150-250 lines, 1 hour to complete
---

# Data Frames in R P.1

A data frame is the most common way of storing data in R and, generally, is the data structure most often used for data analyses. Under the hood, a data frame is a list of equal-length vectors . Each element of the list can be thought of as a column and the length of each element of the list is the number of rows. As a result, data frames can store different classes of objects in each column (i.e. numeric, character, factor). In essence, the easiest way to think of a data frame is as an Excel worksheet that contains columns of different types of data but are all of equal length rows. In this chapter I will illustrate how to create data frames , add additional elements to pre- existing data frames , add attributes to data frames , and subset data frames .

### Creating Data Frames&#x20;

Data frames are usually created by reading in a dataset using `read.table()` or `read.csv()`; this will be covered in the importing and scraping data chapters. However, data frames can also be created explicitly with the data.frame() function or they can be coerced from other types of objects like lists. In this case, I’ll create a simple data frame df and assess its basic structure:&#x20;

```r
df <- data.frame (col1 = 1:3, 
                  col2 = c ("this", "is", "text"), 
                  col3 = c (TRUE, FALSE, TRUE), 
                  col4 = c (2.5, 4.2, pi))
                  
# assess the structure of a data frame
str (df)

## 'data.frame': 3 obs. of 4 variables:
## $ col1: int 1 2 3
## $ col2: Factor w/ 3 levels "is","text","this": 3 1 2
## $ col3: logi TRUE FALSE TRUE
## $ col4: num 2.5 4.2 3.14

# number of rows
nrow (df)
## [1] 3

# number of columns
ncol (df)
## [1] 4
```

```r
```
