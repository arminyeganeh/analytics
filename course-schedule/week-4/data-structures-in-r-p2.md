---
description: Reading 4, 450-600 lines, 3 hours to complete
---

# Data Structures in R P2

## Data Structures in R P2

### Creating and managing lists

A list is an R structure that allows you to combine elements of different types and lengths. This can include a list embedded within a list. Many statistical outputs are provided as a list as well; therefore, it is critical to understand how to work with lists. In this section, we will illustrate how to create lists, add additional elements to preexisting lists, add attributes to lists, and subset lists.

### Creating lists

To create a list we can use the function `list()`. Note how each of the four list items below is of different classes (integer, character, logical, and numeric) and different lengths.

```r
l <- list (1:3, "a", c (TRUE, FALSE, TRUE), c (2.5, 4.2))
str (l)
## List of 4
## $ : int [1:3] 1 2 3
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE
## $ : num [1:2] 2.5 4.2
# a list containing a list

l <- list (1:3, list (letters[1:5], c (TRUE, FALSE, TRUE)))
str (l)
## List of 2
## $ : int [1:3] 1 2 3
## $ :List of 2
## ..$ : chr [1:5] "a" "b" "c" "d" …
## ..$ : logi [1:3] TRUE FALSE TRUE
```

### Adding on to lists

To add additional list components to a list we can leverage the functions `list()` and `append()`:

```r
l1 <- list (1:3, "a", c (TRUE, FALSE, TRUE))
str (l1)
## List of 3
## $ : int [1:3] 1 2 3
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE
```

If we add the new elements with `list()` it will create a list of two components, component 1 will be a nested list of the original list, and component 2 will be the new elements added:

```r
l2 <- list (l1, c (2.5, 4.2))
str (l2)
## List of 2
## $ :List of 3
## ..$ : int [1:3] 1 2 3
## ..$ : chr "a"
## ..$ : logi [1:3] TRUE FALSE TRUE
## $ : num [1:2] 2.5 4.2 
```

To simply add a fourth list component without creating nested lists we use the function `append()`:

```r
l3 <- append (l1, list ( c (2.5, 4.2)))
str (l3)
## List of 4
## $ : int [1:3] 1 2 3
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE
## $ : num [1:2] 2.5 4.2
```

Alternatively, we can also add a new list component by utilizing the ‘$’ sign and naming the new item:

```r
l3$item4 <- "new list item"
str (l3)
## List of 5
## $ : int [1:3] 1 2 3
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE
## $ : num [1:2] 2.5 4.2
## $ item4: chr "new list item"
```

To add individual elements to a specific list component we need to introduce some subsetting.

```r
str (l1)
## List of 3
## $ : int [1:3] 1 2 3
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE
```

To add additional values to a list item you need to subset for that specific list item and then you can use the c() function to add the additional elements to that list item:

```r
l1[[1]] <- c (l1[[1]], 4:6)
str (l1)
## List of 3
## $ : int [1:6] 1 2 3 4 5 6
## $ : chr "a"
## $ : logi [1:3] TRUE FALSE TRUE

l1[[2]] <- c (l1[[2]], c ("b", "c", "d"))
str (l1)
## List of 3
## $ : int [1:6] 1 2 3 4 5 6
## $ : chr [1:4] "a" "b" "c" "d"
## $ : logi [1:3] TRUE FALSE TRUE 
```

### Adding attributes to lists

The attributes that you can add to lists include names, general comments, and specific list item comments. Currently, our l1 list has no attributes:

```
attributes (l1)
## NULL
```

We can add names to lists in two ways. First, we can use names() to assign names to list items in a pre-existing list. Second, we can add names to a list when we are creating a list.

```r
# adding names to a pre-existing list
names (l1) <- c ("item1", "item2", "item3")
str (l1)
## List of 3
## $ item1: int [1:6] 1 2 3 4 5 6
## $ item2: chr [1:4] "a" "dding" "to a" "list"
## $ item3: logi [1:3] TRUE FALSE TRUE
attributes (l1)
## $names
## [1] "item1" "item2" "item3"

# adding names when creating lists
l2 <- list (item1 = 1:3, item2 = letters[1:5], item3 = c (T, F, T, T))
str (l2)
## List of 3
## $ item1: int [1:3] 1 2 3
## $ item2: chr [1:5] "a" "b" "c" "d" …
## $ item3: logi [1:4] TRUE FALSE TRUE TRUE
attributes (l2)
## $names
## [1] "item1" "item2" "item3"
```

We can also add comments to lists. As previously mentioned, comments act as a note to the user without changing how the object behaves. With lists, we can add a general comment to the list using comment() and we can also add comments to specific list items with `attr()`:

```r
# adding a general comment to list l2 with comment ()
comment (l2) <- "This is a comment on a list"
str (l2)
## List of 3
## $ item1: int [1:3] 1 2 3
## $ item2: chr [1:5] "a" "b" "c" "d" …
## $ item3: logi [1:4] TRUE FALSE TRUE TRUE
## - attr(*, "comment")= chr "This is a comment on a list"

attributes (l2)
## $names
## [1] "item1" "item2" "item3"
##
## $comment
## [1] "This is a comment on a list"
# adding a comment to a specific list item with attr ()

attr (l2, "item2") <- "Comment for item2"
str (l2)
## List of 3
## $ item1: int [1:3] 1 2 3
## $ item2: chr [1:5] "a" "b" "c" "d" …
## $ item3: logi [1:4] TRUE FALSE TRUE TRUE
## - attr(*, "comment")= chr "This is a comment on a list"
## - attr(*, "item2")= chr "Comment for item2"

attributes (l2)
## $names
## [1] "item1" "item2" "item3"
##
## $comment
## [1] "This is a comment on a list"
##
## $item2
## [1] "Comment for item2" 
```

### Subsetting lists

To subset lists, we can utilize the operator single bracket `[ ]` , double brackets `[[ ]]` , or dollar sign `$` . Each approach provides a specific purpose and can be combined in different ways to achieve the following subsetting objectives:

* Subset list and preserve output as a list
* Subset list and simplify output
* Subset list to get elements out of a list
* Subset list with a nested list

### Subsetting lists and preserving output as a list

To extract one or more list items while preserving1 the output in list format use the \[ ] operator:

```r
# extract first list item
l2[1]
## $item1
## [1] 1 2 3

# same as above but using the item's name
l2["item1"]
## $item1
## [1] 1 2 3

# extract multiple list items
l2[ c (1,3)]
## $item1
## [1] 1 2 3
##
## $item3
## [1] TRUE FALSE TRUE TRUE

# same as above but using the items' names
l2[ c ("item1", "item3")]
## $item1
## [1] 1 2 3
##
## $item3
## [1] TRUE FALSE TRUE TRUE
```

### Subsetting lists and simplifying outputs

To extract one or more list items while simplifying the output, use the operator `[[ ]]` or `$`:

```r
# extract first list item and simplify to a vector
l2[[1]]
## [1] 1 2 3

# same as above but using the item's name
l2[["item1"]]
## [1] 1 2 3

# same as above but using the $ operator
l2$item1
## [1] 1 2 3
```

One thing that differentiates the operator `[[` from `$` is that `[[` can be used with computed indices. The operator `$` can only be used with literal names.

### Subsetting lists to take elements out of a list

To extract individual elements out of a specific list item, combine the operator `[[` or `$` with `[` :

```r
# extract the third element from the second list item
l2[[2]][3]
## [1] "c"

# same as above but using the item's name
l2[["item2"]][3]
## [1] "c"

# same as above but using the $ operator
l2$item2[3]
## [1] "c"
```

### Subsetting lists with a nested list

If you have nested lists you can expand the ideas above to extract items and elements. We’ll use the following list `l3` which has a nested list in `item2`.

```r
l3 <- list (item1 = 1:3,
            item2 = list (item2a = letters[1:5],
                          item3b = c (T, F, T, T)))
str (l3)
## List of 2
## $ item1: int [1:3] 1 2 3
## $ item2:List of 2
## ..$ item2a: chr [1:5] "a" "b" "c" "d" …
## ..$ item3b: logi [1:4] TRUE FALSE TRUE TRUE
```

If the goal is to subset `l3` to extract the nested list item item2a from item2, we can perform this in multiple ways.

```r
# preserve the output as a list
l3[[2]][1]
## $item2a
## [1] "a" "b" "c" "d" "e"

# same as above but simplify the output
l3[[2]][[1]]
## [1] "a" "b" "c" "d" "e"

# same as above with names
l3[["item2"]][["item2a"]]
## [1] "a" "b" "c" "d" "e"

# same as above with `$` operator
l3$item2$item2a
## [1] "a" "b" "c" "d" "e"
# extract individual element from a nested list item
l3[[2]][[1]][3]
## [1] "c" 
```

### Creating and managing data frames

A data frame is the most common way of storing data in R and, generally, is the data structure most often used for data analyses. Under the hood, a data frame is a list of equal-length vectors . Each element of the list can be thought of as a column and the length of each element of the list is the number of rows. As a result, data frames can store different classes of objects in each column (i.e. numeric, character, factor). In essence, the easiest way to think of a data frame is as an Excel worksheet that contains columns of different types of data but are all of equal length rows. In this chapter I will illustrate how to create data frames , add additional elements to pre- existing data frames , add attributes to data frames , and subset data frames .

### Creating data frames&#x20;

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
## $ col2: chr  "this" "is" "text"
## $ col3: logi TRUE FALSE TRUE
## $ col4: num 2.5 4.2 3.14

# number of rows
nrow (df)
## [1] 3

# number of columns
ncol (df)
## [1] 4
```

### Adding on to data frames&#x20;

We can leverage the function `cbind()` for adding columns to a data frame. Note that one of the objects being combined must already be a data frame otherwise `cbind()` could produce a matrix.

```r
df
##  col1 col2  col3     col4
##1    1 this  TRUE 2.500000
##2    2   is FALSE 4.200000
##3    3 text  TRUE 3.141593

cbind (df, v4)
##  col1 col2  col3     col4 v4
##1    1 this  TRUE 2.500000  A
##2    2   is FALSE 4.200000  B
##3    3 text  TRUE 3.141593  C
```

We can also use the function `rbind()` to add data frame rows together. However, severe caution should be taken because this can cause changes in the classes of the columns. For instance, our data frame df currently consists of an integer, character, logical, and numeric variables.

```r
adding_df <- data.frame (col1 = 4,
                         col2 = "R",
                         col3 = FALSE,
                         col4 = 1.1,
                         stringsAsFactors = FALSE)

df3 <- rbind (df, adding_df)
df3
##   col1 col2  col3     col4
## 1    1 this  TRUE 2.500000
## 2    2   is FALSE 4.200000
## 3    3 text  TRUE 3.141593
## 4    4    R FALSE 1.100000

str (df3)
## 'data.frame':	4 obs. of  4 variables:
##  $ col1: num  1 2 3 4
##  $ col2: chr  "this" "is" "text" "R"
##  $ col3: logi  TRUE FALSE TRUE FALSE
##  $ col4: num  2.5 4.2 3.14 1.1
```

There are better ways to join data frames together than to use cbind() and rbind(). We will cover some of those later when transforming data with the **dplyr package**.

### Adding attributes to data frames

Similar to matrices, data frames will have a dimension attribute. In addition, data frames can also have additional attributes such as row names, column names, and comments. We can illustrate with data frame `df`.

```r
df
##   col1 col2  col3     col4
## 1    1 this  TRUE 2.500000
## 2    2   is FALSE 4.200000
## 3    3 text  TRUE 3.141593

dim(df)
## [1] 3 4

attributes(df)
## $names
## [1] "col1" "col2" "col3" "col4"
## $class
## [1] "data.frame"
## $row.names
## [1] 1 2 3
```

Currently, df does not have row names but we can add them with `rownames()` . We can also change the existing column names by using `colnames()` or `names()`. Lastly, just like vectors, lists, and matrices, we can add a comment to a data frame without affecting how it operates.

```r
# add row names
rownames (df) <- c ("row1", "row2", "row3")

# add/change column names with colnames()
colnames (df) <- c ("col.1", "col.2", "col.3", "col.4")

# adding a comment attribute
comment (df) <- "adding a comment to a data frame"

attributes(df)
## $names
## [1] "col.1" "col.2" "col.3" "col.4"
## $class
## [1] "data.frame"
## $row.names
## [1] "row1" "row2" "row3"
## $comment
## [1] "adding a comment to a data frame" 
```

### Subsetting data frames&#x20;

Data frames possess the characteristics of both lists and matrices: if you subset with a single vector, they behave like lists and will return the selected columns with all rows; if you subset with two vectors, they behave like matrices and can be subset by row and column:

```r
df
##      col.1 col.2 col.3    col.4
## row1     1  this  TRUE 2.500000
## row2     2    is FALSE 4.200000
## row3     3  text  TRUE 3.141593

# subsetting columns like a list
df[c ("col.2", "col.4")] 
##      col.2    col.4
## row1  this 2.500000
## row2    is 4.200000
## row3  text 3.141593

# subsetting columns like a matrix
df[ , c ("col.2", "col.4")]
##      col.2    col.4
## row1  this 2.500000
## row2    is 4.200000
## row3  text 3.141593

# subsetting by row numbers
df[2:3, ]
##      col.1 col.2 col.3    col.4
## row2     2    is FALSE 4.200000
## row3     3  text  TRUE 3.141593 

# subsetting by row names
df[ c ("row2", "row3"), ]
##      col.1 col.2 col.3    col.4
## row2     2    is FALSE 4.200000
## row3     3  text  TRUE 3.141593 

# subset for both rows and columns
df[1:2, c (1, 3)]
##      col.1 col.3
## row1     1  TRUE
## row2     2 FALSE

# use a vector to subset
v <- c (1, 2, 4)
df[ , v]
##      col.1 col.2    col.4
## row1     1  this 2.500000
## row2     2    is 4.200000
## row3     3  text 3.141593

```
