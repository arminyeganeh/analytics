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
