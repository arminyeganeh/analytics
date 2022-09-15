---
description: Homework 4, Reading Instructions, 50 pages, 2 hours to complete
---

# Basic Programming in R

### Scripts

The idea behind a script is that, instead of typing your commands into the R console one at a time, you write them all in a text file. Then, once you’ve finished writing them and saved the text file, you can get R to execute all the commands in your file by using the function `source()` . Suppose you start out with a data set `myrawdata.csv`. What you want is a single document – let’s call it `mydataanalysis.R` – that stores all of the commands that you’ve used in order to do your data analysis ... Then, later on, instead of typing in all those commands again, you’d just tell R to run all of the commands that are stored in `mydataanalysis.R`. Also, in order to help you make sense of all those commands, what you’d want is the ability to add some notes or _comments_ within the file, so that anyone reading the document themselves would be able to understand what each of the commands actually does. But these comments wouldn’t get in the way: when you try to get R to run `mydataanalysis.R` it would be smart enough would recognize that these comments are for the benefit of humans, and so it would ignore them. Later on, you could tweak a few of the commands inside the file so that you can adapt an old analysis to be able to handle a new problem. And you could email your friends and colleagues a copy of this file so that they can reproduce your analysis themselves.

Let’s try using `x <- "hello world"` and `print(x)` as our commands. To create new script file in R studio, go to the “File” menu, select the “New” option, and then click on “R script”. This will open a new window within the “source” panel. Type the following block code into the script area:

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

And just like that, you’ve written your first program R. It really is that simple. When writing up your data analysis as a script, one thing that is generally a good idea is to include a lot of comments in the code. That way, if someone else tries to read it (or if you come back to it several days, weeks, months or years later) they can figure out what’s going on.

### Loops

Depending on how you write the script, you can have R repeat several commands, or skip over different commands, and so on. This topic is referred to as **flow control**, and the first concept to discuss in this respect is the idea of a **loop**. The basic idea is very simple: a loop is a block of code (i.e., a sequence of commands) that R will execute over and over again until some termination criterion is met. Looping is a very powerful idea. There are three different ways to construct a loop in R, based on the `while`, `for` and `repeat` functions. We will only discuss the first two in this course.

### While loop

`while`  is simple. The basic format of the loop looks like this:

```r
while (Condition) {
  Statement 1
  Statement 2
  etc.
}
```

The code corresponding to Condition needs to produce a logical value, either `TRUE` or `FALSE`. Whenever R encounters the statement `while` , it checks to see if the condition is `TRUE`. If it is, then R goes on to execute all of the commands inside the curly brackets, proceeding from top to bottom as usual. However, when it gets to the bottom of those statements, it moves back up to `while` . This continues endlessly until at some point the condition turns out to be `FALSE`. Once that happens, R jumps to the bottom of the loop (i.e., to the character `}` ) and then continues on with whatever commands appear next in the script.

To start with, let’s keep things simple, and use `while`  to calculate the smallest multiple of 17 that is greater than or equal to 1000. The point is to show how to write a `while` loop.&#x20;

```r
## --- whileexample.R
x <- 0
while ( x < 1000 ) {
  x <- x + 17
}
print( x )
## [1] 1003
```

When we run this script, R starts at the top and creates a new variable called x and assigns it a value of 0. It then moves down to the loop, and “notices” that the condition here is x < 1000. Since the current value of x is zero, the condition is true, so it enters the body of the loop (inside the curly braces). There’s only one command here, which instructs R to increase the value of x by 17. R then returns to the top of the loop and rechecks the condition. The value of x is now 17, but that’s still less than 1000, so the loop continues. This cycle will continue for a total of 59 iterations until x reaches a value of 1003. At this point, the loop stops, and R finally reaches line 5 of the script, prints out the value of x on screen, and then halts.

### For loop

The `for` loop is also pretty simple, though not quite as simple as `while` . The basic format of this loop goes like this:

```r
  For (var in vector) {
  Statement 1
  Statement 2
  etc.
}   
```

In a `for` loop, R runs a fixed number of iterations. We have a vector that has several elements, each one corresponding to a possible value of the variable `var`. In the first iteration of the loop, `var` is given a value corresponding to the first element of `vector`. In the second iteration of the loop `var` gets a value corresponding to the second value in `vector`, and so on. Once we’ve exhausted all of the values in `vector` the loop terminates and the flow of the program continues down the script. Once again, let’s use some very simple examples. Firstly, here is a program that just prints out the word “hello” three times and then stops:

```r
## --- forexample.R
for (i in 1:3) {
  print("hello")
}
## [1] "hello"
## [1] "hello"
## [1] "hello"
```

This is the simplest example of a `for` loop. The vector of possible values for the `i` variable just corresponds to the numbers from 1 to 3.&#x20;

However, there’s nothing that stops you from using something non-numeric as the vector of possible values, as the following example illustrates. This time around, we’ll use a character vector to control our loop, which in this case will be a vector of `words`. And what we will do in the loop is get R to convert the word to upper case letters, calculate the length of the word, and print it out. Here’s the script:

```r
## --- forexample2.R

#the words_
words <- c("it","was","the","dirty","end","of","winter")

#loop over the words_
for ( w in words ) {
  w.length <- nchar(w)                        # calculate the number of letters
  W <- toupper(w)                             # convert the word to upper case
  msg <- paste(W, "has", w.length, "letters") # a message to print
  print(msg)               # print it_
}

## [1] "IT has 2 letters"
## [1] "WAS has 3 letters"
## [1] "THE has 3 letters"
## [1] "DIRTY has 5 letters"
## [1] "END has 3 letters"
## [1] "OF has 2 letters"
## [1] "WINTER has 6 letters"
```

Again, pretty straightforward I hope.

### A more realistic example of a loop

To give you a sense of how you can use a loop in a more complex situation, let’s write a simple script to simulate the progression of a mortgage. Suppose we have a nice young couple who borrow $300000 from the bank, at an annual interest rate of 5%. The mortgage is a 30-year loan, so they need to pay it off within 360 months total. Our happy couple decide to set their monthly mortgage payment at $1600 per month. Will they pay off the loan in time or not? Only time will tell. Or, alternatively, we could simulate the whole process and get R to tell us. The script to run this is a fair bit more complicated.

```r
## --- mortgage.R

# set up
month <- 0        # count the number of months
balance <- 300000 # initial mortgage balance
payments <- 1600  # monthly payments
interest <- 0.05  # 5% interest rate per year
total.paid <- 0   # track what you've paid the bank

# convert annual interest to a monthly multiplier
monthly.multiplier <- (1+interest) ^ (1/12)

# keep looping until the loan is paid off...
while ( balance > 0 ) {

# do the calculations for this month
month <- month + 1                       # one more month
balance <- balance * monthly.multiplier  # add interest
balance <- balance - payments            # make payments
total.paid <- total.paid + payments      # track total paid

# print the results on screen
cat( "month", month, ": balance", round(balance), "\n")
  
} # end of loop

# print the total payments at the end
cat("total payments made", total.paid, "\n" )

## month 1 : balance 299622 
## month 2 : balance 299243 
## month 3 : balance 298862 
## month 4 : balance 298480 
## month 5 : balance 298096 
## ...
## month 355 : balance 46 
## month 356 : balance -1554 
## total payments made 569600
```

In the first block of code (under #set up) all we’re doing is specifying all the variables that define the problem. The statement `while`  tells R that it needs to keep looping until the balance reaches zero (or less, since it might be that the final payment of $1600 pushes the balance below zero). Then, inside the body of the loop, we have two different blocks of code. In the first bit, we do all the number crunching. Firstly we increase the value month by 1. Next, the bank charges the interest, so the balance goes up. Then, the couple makes their monthly payment and the balance goes down. Finally, we keep track of the total amount of money that the couple has paid so far, by adding the payments to the running tally. After having done all this number crunching, we tell R to issue the couple with a monthly statement, which just indicates how many months they’ve been paying the loan and how much money they still owe the bank.&#x20;

### Conditional statements

A second kind of flow control that programming languages provide is the ability to evaluate _**conditional statements**_. Unlike loops, which can repeat over and over again, a conditional statement only executes once, but it can switch between different possible commands depending on a CONDITION that is specified by the programmer. The power of these commands is that they allow the program itself to make choices, and in particular, to make different choices depending on the context in which the program is run. The most prominent of example of a conditional statement is the `if` statement, and the accompanying `else` statement. The basic format of an `if` statement in R is as follows:

```
     if ( CONDITION ) {
        STATEMENT1
        STATEMENT2
        ETC
     }
```

And the execution of the statement is pretty straightforward. If the CONDITION is true, then R will execute the statements contained in the curly braces. If the CONDITION is false, then it dose not. If you want to, you can extend the `if` statement to include an `else` statement as well, leading to the following syntax:

```
     if ( CONDITION ) {
        STATEMENT1
        STATEMENT2
        ETC
     } else {
        STATEMENT3
        STATEMENT4
        ETC
     }     
```

As you’d expect, the interpretation of this version is similar. If the CONDITION is true, then the contents of the first block of code (i.e., STATEMENT1, STATEMENT2, ETC) are executed; but if it is false, then the contents of the second block of code (i.e., STATEMENT3, STATEMENT4, ETC) are executed instead.

To give you a feel for how you can use `if` and `else` to do something useful, the example that I’ll show you is a script that prints out a different message depending on what day of the week you run it. We can do this making use of some of the tools that we discussed in Section [7.11.3](https://learningstatisticswithr.com/book/datahandling.html#dates). Here’s the script:

```
## --- ifelseexample.R
# find out what day it is...
today <- Sys.Date()       # pull the date from the system clock
day <- weekdays( today )  # what day of the week it is_

# now make a choice depending on the day...
if ( day == "Monday" ) {
  print( "I don't like Mondays" )
} else {
  print( "I'm a happy little automaton" )
}
```

```
## [1] "I'm a happy little automaton"
```

Since today happens to be a Friday, when I run the script here’s what happens:

```
source(file.path(projecthome,"scripts","ifelseexample.R"))
```

```
## [1] "I'm a happy little automaton"
```

There are other ways of making conditional statements in R. In particular, the `ifelse()` function and the `switch()` functions can be very useful in different contexts. However, my main aim in this chapter is to briefly cover the very basics, so I’ll move on.

\


