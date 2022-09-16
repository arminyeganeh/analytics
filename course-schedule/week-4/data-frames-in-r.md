---
description: Lab 4, 150-250 lines, 1 hour to complete
---

# Data Structures in R P1

### Identifying the structure

Prior to jumping into the data structures, it’s beneficial to understand two components of data structures - the structure and attributes. Given an object, the best way to understand what data structure it represents is to use the function `str()` which stands for structure and provides a compact display of the internal structure of an R object.

```r
# different data structures
vector <- 1:10
list <- list (item1 = 1:10, item2 = LETTERS[1:18])
matrix <- matrix (1:12, nrow = 4)
df <- data.frame (item1 = 1:18, item2 = LETTERS[1:18])

# identify the structure of each object
str (vector)
## int [1:10] 1 2 3 4 5 6 7 8 9 10

str (list)
## List of 2
## $ item1: int [1:10] 1 2 3 4 5 6 7 8 9 10
## $ item2: chr [1:18] "A" "B" "C" "D" …

str (matrix)
## int [1:4, 1:3] 1 2 3 4 5 6 7 8 9 10 …

str (df)
## 'data.frame': 18 obs. of 2 variables:
## $ item1: int 1 2 3 4 5 6 7 8 9 10 …
## $ item2: Factor w/ 18 levels "A","B","C","D",..: 1 2 3 4 5 6 7 8 9 10 …
```

R objects can have attributes, which are like metadata for the object. These metadata can be very useful in that they help to describe the object. For example, column names on a data frame help to tell us what data are contained in each of the columns. Some examples of R object attributes are:&#x20;

* names, dimnames&#x20;
* dimensions (e.g. matrices, arrays)&#x20;
* class (e.g. integer, numeric)
* length
* other user-defined attributes/metadata&#x20;

Attributes of an object (if any) can be accessed using the function `attributes()`. Not all R objects contain attributes, in which case the attributes() function returns NULL.

```r
# assess attributes of an object
attributes (df)
## $names
## [1] "item1" "item2"
##
## $row.names
## [1] 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18
##
## $class
## [1] " data.frame "

attributes (matrix)
## $dim
## [1] 4 3
# assess names of an object

names (df)
## [1] "item1" "item2"
# assess the dimensions of an object

dim (matrix)
## [1] 4 3
# assess the class of an object

class (list)
## [1] "list"
# access the length of an object

length (vector)
## [1] 10

# note that length will measure the number of items in
# a list or number of columns in a data frame
length (list)
## [1] 2
length (df)
## [1] 2 
```

### Creating and managing vectors

The operator `:` can be used to create a vector of integers between two specified numbers or the function `c()` can be used to create vectors of objects by concatenating elements together:

```r
# integer vector
w <- 8:17
w
## [1] 8 9 10 11 12 13 14 15 16 17

# double vector
x <- c (0.5, 0.6, 0.2)
x
## [1] 0.5 0.6 0.2

# logical vector
y1 <- c (TRUE, FALSE, FALSE)
y1
## [1] TRUE FALSE FALSE

# logical vector in shorthand
y2 <- c (T, F, F)
y2
## [1] TRUE FALSE FALSE
# Character vector
z <- c ("a", "b", "c")
z
## [1] "a" "b" "c"
```

You can also use the function `as.vector()` to initialize vectors or change the vector type:&#x20;

```r
v <- as.vector (8:17)
v
## [1] 8 9 10 11 12 13 14 15 16 17
# turn numerical vector to character

as.vector (v, mode = "character")
## [1] "8" "9" "10" "11" "12" "13" "14" "15" "16" "17"
```

All elements of a vector must be the same type, so when you attempt to combine different types of elements they will be coerced to the most flexible type possible:

```r
# numerics are turned to characters
str ( c ("a", "b", "c", 1, 2, 3))
## chr [1:6] "a" "b" "c" "1" "2" "3"

# logical are turned to numerics…
str ( c (1, 2, 3, TRUE, FALSE))
## num [1:5] 1 2 3 1 0

# or character
str ( c ("A", "B", "C", TRUE, FALSE))
## chr [1:5] "A" "B" "C" "TRUE" "FALSE"
```

### Adding on to vectors

To add additional elements to a pre-existing vector we can continue to leverage the function `c()`. Also, note that vectors are always flat so nested `c()` functions will not add additional dimensions to the vector:

```r
v1 <- 8:17
c (v1, 18:22)
## [1] 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22
```

### Adding attributes to vectors

The attributes that you can add to vectors include names and comments. If we continue with our vector `v1` we can see that the vector currently has no attributes:

```r
attributes (v1)
## NULL
```

We can add names to vectors using two approaches. The first uses `names()` to assign names to each element of the vector. The second approach is to assign names when creating the vector.

```r
# assigning names to a pre-existing vector
names (v1) <- letters[1: length (v1)]
v1
## a b c d e f g h i j
## 8 9 10 11 12 13 14 15 16 17

attributes (v1)
## $names
## [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j"

# adding names when creating vectors
v2 <- c (name1 = 1, name2 = 2, name3 = 3)
v2
## name1 name2 name3
## 1 2 3

attributes (v2)
## $names
## [1] "name1" "name2" "name3" 
```

We can also add comments to vectors to act as a note to the user. This does not change how the vector behaves; rather, it simply acts as a form of metadata for the vector.

```r
comment (v1) <- "This is a comment on a vector"
v1
## a b c d e f g h i j
## 8 9 10 11 12 13 14 15 16 17

attributes (v1)
## $names
## [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j"
##
## $comment
## [1] "This is a comment on a vector"
```

### Subsetting vectors

The four main ways to subset a vector include combining square brackets \[ ] with:&#x20;

* Positive integers
* Negative integers
* Logical values
* Names&#x20;

You can also subset with double brackets \[\[ ]] for simplifying subsets.

### Subsetting with positive integers

Subsetting with positive integers returns the elements at the specified positions:

```r
v1
## a b c d e f g h i j
## 8 9 10 11 12 13 14 15 16 17

v1[2]
## b
## 9

v1[2:4]
## b c d
## 9 10 11

v1[ c (2, 4, 6, 8)]
## b d f h
## 9 11 13 15

# note that you can duplicate index positions
v1[ c (2, 2, 4)]
## b b d
## 9 9 11
```

### Subsetting with negative integers

Subsetting with negative integers will omit the elements at the specified positions:

```
v1[-1]
## b c d e f g h i j
## 9 10 11 12 13 14 15 16 17

v1[- c (2, 4, 6, 8)]
## a c e g i j
## 8 10 12 14 16 17
```

Subsetting with logical values

Subsetting with logical values will select the elements where the corresponding logical value is `TRUE`:

```
v1[ c (TRUE, FALSE, TRUE, FALSE, TRUE, TRUE, TRUE, FALSE, FALSE, TRUE)]
## a c e f g j
## 8 10 12 13 14 17
v1[v1 < 12]
## a b c d
## 8 9 10 11
v1[v1 < 12 | v1 > 15]
## a b c d i j
## 8 9 10 11 16 17
# if logical vector is shorter than the length of the vector being
# subsetted, it will be recycled to be the same length
v1[ c (TRUE, FALSE)]
## a c e g i
## 8 10 12 14 16 
```

### Subsetting with names

Subsetting with names will return the elements with the matching names specified:

```r
v1["b"]
## b
## 9

v1[ c ("a", "c", "h")]
## a c h
## 8 10 15 
```

### Simplifying vs. preserving

It is important to understand the difference between simplifying and preserving when subsetting. Simplifying subsets returns the simplest possible data structure that can represent the output. Preserving subsets keeps the structure of the output the same as the input. For vectors, subsetting with single brackets \[ ] preserves while subsetting with double brackets \[\[ ]] simplifies. The change you will notice when simplifying vectors is the removal of names.

```r
v1[1]
## a
## 8

v1[[1]]
## [1] 8
```
