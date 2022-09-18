---
description: Lab 5, 150-250 lines, 1 hour to complete
---

# Simplifying R with %>%

Removing duplication is an important principle to keep in mind with your code; however, equally important is to keep your code efficient and readable. The package `magrittr` is a powerful tool to have in your data wrangling toolkit. The package has two primary aims: “to decrease development time and to improve readability and maintainability of code.”&#x20;

### Pipe (%>%) Operator

The principal function provided by `magrittr` is `%>%`, or what’s called the “pipe” operator. This operator will forward a value, or the result of an expression, into the next function call/expression. For instance, a function to filter data can be written as:

```r
filter (data, variable == numeric_value)
# or
data %>% filter(variable == numeric_value)
```

Both functions complete the same task and the benefit of using `%>%` may not be immediately evident; however, when you desire to perform multiple functions its advantage becomes obvious. For instance, if we want to filter some data, group it by categories, summarize it, and then order the summarized results we could write it out three different ways. Don’t worry, you’ll learn how to operate these specific functions in the next section:

```r
library (magrittr)
library (dplyr)

arrange (
  summarize (
    group_by (
      filter (mtcars, carb > 1),cyl),
    Avg_mpg = mean (mpg),
    desc (Avg_mpg))

## Source: local data frame [3 x 2]
##
## cyl Avg_mpg
## (dbl) (dbl)
## 1 4 25.90
## 2 6 19.74
## 3 8 15.10
```

This first option is considered a **nested option** such that functions are nested within one another. Historically, this has been the traditional way of integrating code; however, it becomes extremely difficult to read what exactly the code is doing and it also becomes easier to make mistakes when making updates to your code. To make things more readable, people often move to the following approach…

```r
a <- filter (mtcars, carb > 1)
b <- group_by (a, cyl)
c <- summarise (b, Avg_mpg = mean (mpg))
d <- arrange (c, desc (Avg_mpg))
print (d)

## Source: local data frame [3 x 2]
##
## cyl Avg_mpg
## (dbl) (dbl)
## 1 4 25.90
## 2 6 19.74
## 3 8 15.10 
```

By sequencing multiple functions in this way you are likely saving multiple outputs that are not very informative to you or others; rather, the only reason you save them is to insert them into the next function to eventually get the final output you desire. This **multiple objects option** inevitably creates unnecessary copies. To provide the same readability (or even better), we can use %>% to string these arguments together without unnecessary object creation…

```r
mtcars %>%
  filter (carb > 1) %>%
  group_by (cyl) %>%
  summarise (Avg_mpg = mean (mpg)) %>%
  arrange (desc (Avg_mpg))
  
## Source: local data frame [3 x 2]
##
## cyl Avg_mpg
## (dbl) (dbl)
## 1 4 25.90
## 2 6 19.74
## 3 8 15.10
```

This final option which integrates %>% operators makes for more efficient and legible code. It is efficient in that it doesn’t save unnecessary objects (as in option 2) and performs as effectively (as both options 1 and 2) but makes your code more readable in the process. It is legible in that you can read this as you would read normal prose (**we read the %>% as “ and then”**) - “take `mtcars` and then `filter` and then `group_by` and then `summarize` and then `arrange`.”

