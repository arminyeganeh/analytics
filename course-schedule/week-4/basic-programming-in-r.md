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

A second kind of flow control that programming languages provide is the ability to evaluate **conditional statements**. Unlike loops, which can repeat over and over again, a conditional statement only executes once, but it can switch between different possible commands depending on the condition that is specified by the programmer. The power of these commands is that they allow the program itself to make choices, and in particular, to make different choices depending on the context in which the program is run. The most prominent example of a conditional statement is the `if` and the accompanying `else` statements. The basic format of an `if` statement in R is:

```r
if (Condition) {
Statement1
Statement2
etc.
}
```

And the execution of the statement is pretty straightforward. If the condition is true, then R will execute the statements contained in the curly braces. If the condition is false, then it dose not. If you want to, you can extend the `if` statement to include an `else` statement as well:

```
if (Condition) {
Statement1
Statement2
etc.
} else {
Statement3
Statement4
etc.
}     
```

If the condition is false, then the contents of the second block of code (i.e., Statement3, Statement4, etc) are executed instead. To give you a feel for how you can use `if` and `else` to do something useful, the example is a script that prints out a different message depending on what day of the week you run it:

```r
## --- ifelseexample.R
# find out what day it is...
today <- Sys.Date()       # pull the date from the system clock
day <- weekdays(today)    # what day of the week it is

# Make a choice depending on the day...
if (day == "Monday") {
  print("I don't like Mondays")
} else {
  print("I'm a happy little automaton")
}
## [1] "I'm a happy little automaton"
```

There are other ways of making conditional statements in R. In particular, the `ifelse()` function and the `switch()` functions can be very useful in different contexts.&#x20;

```
library(lsr)
who()
```

An important thing to recognise here is that the two internal variables that the `quadruple()` function makes use of, `x` and `y`, stay internal. That is, if we inspect the contents of the workspace,

```
## [1] 40
```

```
my.var <- quadruple(10)
print(my.var)
```

And now that we’ve created the `quadruple()` function, we can call it just like any other function And if I want to store the output as a variable, I can do this:

```
## [1] "function"
```

```
class( quadruple )
```

nothing appears to have happened, but there is a new object created in the workspace called `quadruple`. Not surprisingly, if we ask R to tell us what kind of object it is, it tells us that it is a function:

```
source(file.path(projecthome,"scripts","functionexample.R"))
```

When we run this script, as follows

```
## --- functionexample.R
quadruple <- function(x) {
  y <- x*4
  return(y)
} 
```

To give a simple example of this, let’s create a function called `quadruple()` which multiplies its inputs by four. In keeping with the approach taken in the rest of the chapter, I’ll use a script to do this:

What this does is create a function with the name FNAME, which has arguments ARG1, ARG2 and so forth. Whenever the function is called, R executes the statements in the curly braces, and then outputs the contents of VALUE to the user. Note, however, that R does not execute the commands inside the function in the workspace. Instead, what it does is create a temporary local environment: all the internal statements in the body of the function are executed there, so they remain invisible to the user. Only the final results in the VALUE are returned to the workspace.

```
     FNAME <- function ( ARG1, ARG2, ETC ) {
        STATEMENT1
        STATEMENT2
        ETC
        return( VALUE )
     }
```

In this section I want to talk about functions again. Functions were introduced in Section [3.5](https://learningstatisticswithr.com/book/introR.html#usingfunctions), but you’ve learned a lot about R since then, so we can talk about them in more detail. In particular, I want to show you how to create your own. To stick with the same basic framework that I used to describe loops and conditionals, here’s the syntax that you use to create a function:

### Writing functions

we see everything in our workspace from this chapter including the `quadruple()` function itself, as well as the `my.var` variable that we just created.

Now that we know how to create our own functions in R, it’s probably a good idea to talk a little more about some of the other properties of functions that I’ve been glossing over. To start with, let’s take this opportunity to type the name of the function at the command line without the parentheses:

```
quadruple
```

```
## function(x) {
##   y <- x*4
##   return(y)
## }
```

As you can see, when you type the name of a function at the command line, R prints out the underlying source code that we used to define the function in the first place. In the case of the `quadruple()` function, this is quite helpful to us – we can read this code and actually see what the function does. For other functions, this is less helpful, as we saw back in Section [3.5](https://learningstatisticswithr.com/book/introR.html#usingfunctions) when we tried typing `citation` rather than `citation()`.

#### 8.4.1 Function arguments revisited

Okay, now that we are starting to get a sense for how functions are constructed, let’s have a look at two, slightly more complicated functions that I’ve created. The source code for these functions is contained within the `functionexample2.R` and `functionexample3.R` scripts. Let’s start by looking at the first one:

```
## --- functionexample2.R
pow <- function( x, y = 1) {
  out <- x^y  # raise x to the power y
  return( out )
}
```

and if we type `source("functionexample2.R")` to load the `pow()` function into our workspace, then we can make use of it. As you can see from looking at the code for this function, it has two arguments `x` and `y`, and all it does is raise `x` to the power of `y`. For instance, this command

```
pow(x=3, y=2)
```

```
## [1] 9
```

calculates the value of $$3232$$. The interesting thing about this function isn’t what it does, since R already has has perfectly good mechanisms for calculating powers. Rather, notice that when I defined the function, I specified `y=1` when listing the arguments? That’s the default value for `y`. So if we enter a command without specifying a value for `y`, then the function assumes that we want `y=1`:

```
pow( x=3 )
```

```
## [1] 3
```

However, since I didn’t specify any default value for `x` when I defined the `pow()` function, we always need to input a value for `x`. If we don’t R will spit out an error message.

So now you know how to specify default values for an argument. The other thing I should point out while I’m on this topic is the use of the `...` argument. The `...` argument is a special construct in R which is only used within functions. It is used as a way of matching against multiple user inputs: in other words, `...` is used as a mechanism to allow the user to enter as many inputs as they like. I won’t talk at all about the low-level details of how this works at all, but I will show you a simple example of a function that makes use of it. To that end, consider the following script:

```
## --- functionexample3.R
doubleMax <- function( ... ) {  
  max.val <- max( ... )   # find the largest value in ... 
  out <- 2 * max.val      # double it
  return( out )
}
```

When we type `source("functionexample3.R")`, R creates the `doubleMax()` function. You can type in as many inputs as you like. The `doubleMax()` function identifies the largest value in the inputs, by passing all the user inputs to the `max()` function, and then doubles it. For example:

```
doubleMax( 1,2,5 )
```

```
## [1] 10
```

#### 8.4.2 There’s more to functions than this

There’s a lot of other details to functions that I’ve hidden in my description in this chapter. Experienced programmers will wonder exactly how the “scoping rules” work in R,[138](https://learningstatisticswithr.com/book/scripting.html#fn138) or want to know how to use a function to create variables in other environments[139](https://learningstatisticswithr.com/book/scripting.html#fn139), or if function objects can be assigned as elements of a list[140](https://learningstatisticswithr.com/book/scripting.html#fn140) and probably hundreds of other things besides. However, I don’t want to have this discussion get too cluttered with details, so I think it’s best – at least for the purposes of the current book – to stop here.

### 8.5 Implicit loops

There’s one last topic I want to discuss in this chapter. In addition to providing the explicit looping structures via `while` and `for`, R also provides a collection of functions for _**implicit loops**_. What I mean by this is that these are functions that carry out operations very similar to those that you’d normally use a loop for. However, instead of typing out the whole loop, the whole thing is done with a single command. The main reason why this can be handy is that – due to the way that R is written – these implicit looping functions are usually about to do the same calculations much faster than the corresponding explicit loops. In most applications that beginners might want to undertake, this probably isn’t very important, since most beginners tend to start out working with fairly small data sets and don’t usually need to undertake extremely time consuming number crunching. However, because you often see these functions referred to in other contexts, it may be useful to very briefly discuss a few of them.

The first and simplest of these functions is `sapply()`. The two most important arguments to this function are `X`, which specifies a vector containing the data, and `FUN`, which specifies the name of a function that should be applied to each element of the data vector. The following example illustrates the basics of how it works:

```
words <- c("along", "the", "loom", "of", "the", "land")
sapply( X = words, FUN = nchar )
```

```
## along   the  loom    of   the  land 
##     5     3     4     2     3     4
```

Notice how similar this is to the second example of a `for` loop in Section [8.2.2](https://learningstatisticswithr.com/book/scripting.html#for). The `sapply()` function has implicitly looped over the elements of `words`, and for each such element applied the `nchar()` function to calculate the number of letters in the corresponding word.

The second of these functions is `tapply()`, which has three key arguments. As before `X` specifies the data, and `FUN` specifies a function. However, there is also an `INDEX` argument which specifies a grouping variable.[141](https://learningstatisticswithr.com/book/scripting.html#fn141) What the `tapply()` function does is loop over all of the different values that appear in the `INDEX` variable. Each such value defines a group: the `tapply()` function constructs the subset of `X` that corresponds to that group, and then applies the function `FUN` to that subset of the data. This probably sounds a little abstract, so let’s consider a specific example, using the `nightgarden.Rdata` file that we used in Chapter [7](https://learningstatisticswithr.com/book/datahandling.html#datahandling).

```
gender <- c( "male","male","female","female","male" )
age <- c( 10,12,9,11,13 )
tapply( X = age, INDEX = gender, FUN = mean )
```

```
##   female     male 
## 10.00000 11.66667
```

In this extract, what we’re doing is using `gender` to define two different groups of people, and using their `ages` as the data. We then calculate the `mean()` of the ages, separately for the males and the females. A closely related function is `by()`. It actually does the same thing as `tapply()`, but the output is formatted a bit differently. This time around the three arguments are called `data`, `INDICES` and `FUN`, but they’re pretty much the same thing. An example of how to use the `by()` function is shown in the following extract:

```
by( data = age, INDICES = gender, FUN = mean )
```

```
## gender: female
## [1] 10
## -------------------------------------------------------- 
## gender: male
## [1] 11.66667
```

The `tapply()` and `by()` functions are quite handy things to know about, and are pretty widely used. However, although I do make passing reference to the `tapply()` later on, I don’t make much use of them in this book.

Before moving on, I should mention that there are several other functions that work along similar lines, and have suspiciously similar names: `lapply`, `mapply`, `apply`, `vapply`, `rapply` and `eapply`. However, none of these come up anywhere else in this book, so all I wanted to do here is draw your attention to the fact that they exist.

### Nested loops

