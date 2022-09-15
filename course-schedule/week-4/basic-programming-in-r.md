---
description: Homework 4, Reading Instructions, 50 pages, 2 hours to complete
---

# Basic Programming in R

### Scripts

The idea behind a script is that, instead of typing your commands into the R console one at a time, you write them all in a text file. Then, once you’ve finished writing them and saved the text file, you can get R to execute all the commands in your file by using the function `source()` . Suppose you start out with a data set `myrawdata.csv`. What you want is a single document – let’s call it `mydataanalysis.R` – that stores all of the commands that you’ve used in order to do your data analysis ... Then, later on, instead of typing in all those commands again, you’d just tell R to run all of the commands that are stored in `mydataanalysis.R`. Also, in order to help you make sense of all those commands, what you’d want is the ability to add some notes or _comments_ within the file, so that anyone reading the document themselves would be able to understand what each of the commands actually does. But these comments wouldn’t get in the way: when you try to get R to run `mydataanalysis.R` it would be smart enough would recognize that these comments are for the benefit of humans, and so it would ignore them. Later on, you could tweak a few of the commands inside the file so that you can adapt an old analysis to be able to handle a new problem. And you could email your friends and colleagues a copy of this file so that they can reproduce your analysis themselves.

Let’s try using `x <- "hello world"` and `print(x)` as our commands. Type the following block code in your R script (i.e., the top left area in RStudio).

```r
## --- hello.R
x <- "hello world!"
print(x)
```

The line at the top is the filename and not part of the script itself. Below that, you can see the two R commands that make up the script itself. Then save the document as `hello.R` . When R asks you where to save the file, save it to whatever folder you’re using as your working directory in R. To obtain your current working directory, type the function `getwd()` in the R console (i.e., the bottom left area in RStudio) and hit Enter. Once the file is saved, quit RStudio using `Ctrl+Q` . So how do we run the script? Assuming that the `hello.R` file has been saved to your working directory, then you can start RStudio and run the script using the following command:

```r
source( "hello.R" )
```

When you type this command, R opens up the script file: it then reads each command in the file in the same order that they appear in the file and executes those commands in that order. The simple script that we have contains two commands. The first one creates a variable `x` and the second one prints it on screen. So, when we run the script, this is what we see on screen:

```r
source("hello.R")
## [1] "hello world!"
```

And just like that, you’ve written your first program R. It really is that simple.

\


