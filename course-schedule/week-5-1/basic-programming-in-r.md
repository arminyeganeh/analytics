---
description: Homework 4, Reading Instructions, 50 pages, 2 hours to complete
---

# Basic Programming in R

### Writing scripts

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

### Syntax

The maximum number of characters on a single line of code should be 80 or less. If you are using RStudio you can have a margin displayed so you know when you need to break to a new line. 6 This allows your code to be printed on a normal 8.5 × 11 page with a reasonably sized font. Also, when indenting your code use two spaces rather than using tabs. The only exception is if a line break occurs inside parentheses. In this case, align the wrapped line with the first character inside the parenthesis:

```r
super_long_name <- seq(ymd_hm("2015-1-1 0:00"),
                       ymd_hm("2015-1-1 12:00"),
                       by = "hour")
```

Proper spacing within your code also helps with readability. Place spaces around all infi x operators ( = , + , - , <- , etc.). The same rule applies when using = in function calls. Always put a space after a comma, and never before.

```r
# Good
average <- mean (feet / 12 + inches, na.rm = TRUE)

# Bad
average<-mean(feet/12+inches,na.rm=TRUE)
```

There’s a small exception to this rule: : , :: and ::: don’t need spaces around them.

```r
# Good
x <- 1:10
base::get

# Bad
x <- 1 : 10
base :: get
```

### Loops

Depending on how you write the script, you can have R repeat several commands, or skip over different commands, and so on. This topic is referred to as **flow control**, and the first concept to discuss in this respect is the idea of a **loop**. The basic idea is very simple: a loop is a block of code (i.e., a sequence of commands) that R will execute over and over again until some termination criterion is met. Looping is a very powerful idea. There are three different ways to construct a loop in R, based on the `while`, `for` and `repeat` functions. We will only discuss the first two in this course.

### While loop

`while` is simple. The basic format of the loop looks like this:

```r
while (Condition) {
Statement 1
Statement 2
etc.
}
```

The code corresponding to Condition needs to produce a logical value, either `TRUE` or `FALSE`. Whenever R encounters the statement `while` , it checks to see if the condition is `TRUE`. If it is, then R goes on to execute all of the commands inside the curly brackets, proceeding from top to bottom as usual. However, when it gets to the bottom of those statements, it moves back up to `while` . This continues endlessly until at some point the condition turns out to be `FALSE`. Once that happens, R jumps to the bottom of the loop (i.e., to the character `}` ) and then continues on with whatever commands appear next in the script.

To start with, let’s keep things simple, and use `while` to calculate the smallest multiple of 17 that is greater than or equal to 1000. The point is to show how to write a `while` loop.

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

This is the simplest example of a `for` loop. The vector of possible values for the `i` variable just corresponds to the numbers from 1 to 3.

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

In the first block of code (under #set up) all we’re doing is specifying all the variables that define the problem. The statement `while` tells R that it needs to keep looping until the balance reaches zero (or less, since it might be that the final payment of $1600 pushes the balance below zero). Then, inside the body of the loop, we have two different blocks of code. In the first bit, we do all the number crunching. Firstly we increase the value month by 1. Next, the bank charges the interest, so the balance goes up. Then, the couple makes their monthly payment and the balance goes down. Finally, we keep track of the total amount of money that the couple has paid so far, by adding the payments to the running tally. After having done all this number crunching, we tell R to issue the couple with a monthly statement, which just indicates how many months they’ve been paying the loan and how much money they still owe the bank.

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

There are other ways of making conditional statements in R. In particular, the `ifelse()` function and the `switch()` functions can be very useful in different contexts.

### Writing functions

In this section, we learn how to create our own functions. Here is the syntax that you use to create a function:

```r
Fname <- function (Arg1, Arg2, etc.) {
Statement1
Statement2
etc.
return(value)
}
```

What this does is create a function with the name `Fname` which has arguments `Arg1`, `Arg2`, and so forth. Whenever the function is called, R executes the statements in the curly braces and then outputs the contents of `value` to the user. Note, however, that R does not execute the commands inside the function in the workspace. Instead, what it does is create a temporary local environment: all the internal statements in the body of the function are executed there, so they remain invisible to the user. Only the final results are returned to the workspace. Let’s create a function called `quadruple()` which multiplies its inputs by four:

```r
## --- functionexample.R
quadruple <- function(x) {
y <- x*4
return(y)
} 
```

Not surprisingly, if we ask R to tell us what kind of object it is, it tells us that it is a function. And now that we’ve created the `quadruple()` function, we can call it just like any other function And if I want to store the output as a variable, I can do this:

```r
class(quadruple)
## [1] "function"

my.var <- quadruple(10)
print(my.var)
## [1] 40
```

When you type the name of a function at the command line, R prints out the underlying source code that we used to define the function in the first place. In the case of the `quadruple()` function, this is quite helpful to us – we can read this code and actually see what the function does.

Now, let’s have a look at two slightly more complicated functions. Let’s start by looking at the first one:

```r
## --- functionexample2.R
pow <- function(x, y = 1) {
out <- x^y  # raise x to the power y
return(out)
}
```

As you can see from looking at the code for this function, it has two arguments `x` and `y`, and all it does is raise `x` to the power of `y`:

```r
pow(x=3, y=2)
## [1] 9
```

The interesting thing about this function isn’t what it does, since R already has has perfectly good mechanisms for calculating powers. Rather, notice that when we defined the function, we specified `y=1` when listing the arguments. That’s the default value for `y`. So if we enter a command without specifying a value for `y` then the function assumes that we want `y=1`:

```r
pow(x=3)
## [1] 3
```

However, since we didn’t specify any default value for `x` when we defined the `pow()` function, we always need to input a value for `x`. If we don’t R will spit out an error message.

The argument `...` is a special construct in R which is only used within functions. It is used as a way of matching against multiple user inputs: in other words, `...` is used as a mechanism to allow the user to enter as many inputs as they like. Consider the following script:

```r
## --- functionexample3.R
doubleMax <- function( ... ) {  
max.val <- max( ... )          # find the largest value in ... 
out <- 2 * max.val             # double it
return( out )
}
```

When we type `source("functionexample3.R")`, R creates the `doubleMax()` function. You can type in as many inputs as you like. The function `doubleMax()` identifies the largest value in the inputs by passing all the user inputs to the function `max()` and then doubles it:

```r
doubleMax( 1,2,5 )
## [1] 10
```

With the exception of primitive functions all R functions have three parts:

* body(): the code inside the function
* formals(): the list of arguments used to call the function
* environment(): the mapping of the location(s) of the function’s variables

Let’s build a function that calculates the present value (PV) of a single future sum. The equation for a single sum PV is:

$$
PV =\frac{ FV}{(1+r)^n}
$$

where FV is the future value, r is the interest rate, and n is the number of periods.

```r
PV <- function(FV, r, n) { 
PV <- FV / (1 + r)^n 
round(PV, 2)
}
```

To perform the PV() function we can call the arguments in different ways.

<pre class="language-r"><code class="lang-r"># using argument names
PV(FV = 1000, r = .08, n = 5)
## [1] 680.58

# same as above but without using names (aka "positional matching")
<strong>PV(1000, .08, 5)
</strong>## [1] 680.58

# if using names you can change the order
PV(r = .08, FV = 1000, n = 5)
## [1] 680.58

# if not using names you must insert arguments in proper order
# in this e.g. the function assumes FV = .08, r = 1000, and n = 5
PV(.08, 1000, 5)
## [1] 0</code></pre>

Note that when building a function you can also set default values for arguments. In our original PV() we did not provide any default values so if we do not supply all the argument parameters an error will be returned. However, if we set default values then the function will use the stated default if any parameters are missing:

<pre class="language-r"><code class="lang-r"># creating default argument values
PV &#x3C;- function(FV = 1000, r = .08, n = 5) {
<strong>PV &#x3C;- FV / (1 + r)^n
</strong>round(PV, 2)
}

# function will use default n value
PV(1000, .08)
## [1] 680.58
r
# specifying a different n value
PV(1000, .08, 3)
## [1] 793.83</code></pre>

If a function performs multiple tasks and therefore has multiple results to report then we have to include the c() function inside the function to display all the results. If you do not include the c() function then the function output will only return the last expression:

```r
good <- function(x, y) {
 output1 <- 2 * x + y
 output2 <- x + 2 * y
 output3 <- 2 * x + 2 * y
 output4 <- x / y
 c(output1, output2, output3, output4)
}
good(1, 2)
## [1] 4.0 5.0 6.0 0.5
```

Furthermore, when we have a function that performs multiple tasks (i.e. computes multiple computations) then it is often useful to save the results in a list.

```r
good_list <- function(x, y) {
 output1 <- 2 * x + y
 output2 <- x + 2 * y
 output3 <- 2 * x + 2 * y
 output4 <- x / y
 c(list(Output1 = output1, Output2 = output2,
 Output3 = output3, Output4 = output4))
}
good_list(1, 2)
## $Output1
## [1] 4
##
## $Output2
## [1] 5
##
## $Output3
## [1] 6
##
## $Output4
## [1] 0.5
```

If you want to save a function to be used at other times and within other scripts there are two main ways to do this. One way is to build a package which we do not cover in this course but is discussed in more detail in Hadley Wickhams R Packages book, which is openly available at http://r-pkgs.had.co.nz/. Another option, and the one discussed here, is to save the function in a script. For example, we can save a script that contains the PV() function and save this script as PV.R. If we want to use the PV function in this new script we can simply read in the function by sourcing the script using source("PV.R"). Now, you will notice that we have the PV() function in our global environment and can use it as normal. Note that if you are working in a different directory than where the PV.R file is located you will need to include the proper path to access the relevant directory.

### Implicit loops (Apply family)

In addition to providing the explicit looping structures via `while` and `for`, R also provides a collection of functions for **implicit loops**. These are functions that carry out operations very similar to those that you’d normally use a loop for. However, instead of typing out the whole loop, the whole thing is done with a single command.

The apply family consists of vectorized functions, which minimize your need to explicitly create loops. These functions will apply a specified function to a data object, and the primary difference is in the object class to which the function is applied (e.g., list vs. matrix) and the object class that will be returned from the function. The following presents the most common forms of apply functions but realize that additional functions exist.

**Pro Tip:** When working with very large data tables, maximizing the use of functions (thus, minimizing Loop and Apply) and maximizing the use of vectors, matrices, and lists (thus, minimizing Data Frames) can significantly reduce processing time.

### apply() for matrices and data frames

The function `apply()` is most often used to apply a function to the rows or columns (margins) of matrices or data frames. Using `apply()` is not always faster than using a loop function, but it is highly compact and can be written in one line. The syntax is as follows:

```r
# syntax of apply function
apply (x, MARGIN, FUN, …)
```

* x is the matrix or dataframe
* MARGIN is a vector giving the subscripts to which the function will be applied over, e.g., 1 indicates rows, 2 indicates columns, and c(1, 2) indicates rows and columns.
* FUN is the function to be applied
* … is for any other arguments to be passed to the function

To provide examples let’s use the data table `mtcars` provided in R :

<pre class="language-r"><code class="lang-r"># show first few rows of mtcars
head (mtcars)
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

# get the mean of each column
<strong>apply (mtcars, 2, mean)
</strong>##        mpg        cyl       disp         hp       drat         wt  
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250
##       qsec         vs         am       gear       carb  
##  17.848750   0.437500   0.406250   3.687500   2.812500

# get column quantiles (notice the quantile percents as row names) 
apply (mtcars, 2, quantile, probs = c (0.10, 0.25, 0.50, 0.75, 0.90))
##        mpg cyl    disp    hp  drat      wt    qsec vs am gear carb
## 10% 14.340   4  80.610  66.0 3.007 1.95550 15.5340  0  0    3    1
## 25% 15.425   4 120.825  96.5 3.080 2.58125 16.8925  0  0    3    2
## 50% 19.200   6 196.300 123.0 3.695 3.32500 17.7100  0  0    4    2
## 75% 22.800   8 326.000 180.0 3.920 3.61000 18.9000  1  1    4    4
## 90% 30.090   8 396.000 243.5 4.209 4.04750 19.9900  1  1    5    4   </code></pre>

### sapply() for lists… output simplified

The function `lapply()` does the following simple series of operations:

1. it loops over a list, iterating over each element in that list
2. it applies a function to each element of the list (a function that you specify)
3. and returns a list (the l is for “list”).

The function `sapply()` behaves similarly to `lapply()`. The only real difference is in the return value. `sapply()` will try to simplify the result of `lapply()` if possible. Essentially, `sapply()` calls `lapply()` on its input and then applies the following algorithm:

* If the result is a list where every element is length 1, then a vector is returned
* If the result is a list where every element is a vector of the same length (> 1), a matrix is returned.
* If neither of the above simplifications can be performed then a list is returned

The following example illustrates the basics of how it works:

```r
words <- c("along", "the", "loom", "of", "the", "land")
sapply(X = words, FUN = nchar)
## along   the  loom    of   the  land 
##     5     3     4     2     3     4
```

The function `sapply()` has implicitly looped over the elements of `words` and for each such element applied the `nchar()` function to calculate the number of letters in the corresponding word. To illustrate the differences we can use the previous example using a list with the beaver data and compare the `sapply` and `lapply` outputs:

```r
# list of R's built-in beaver data
beaver_data <- list (beaver1 = beaver1, beaver2 = beaver2)

# get the mean of each list item and return as a list
lapply (beaver_data, function(x) round (apply (x, 2, mean), 2))

## $beaver1
##    day    time    temp   activ 
## 346.20 1312.02   36.86    0.05 

## $beaver2
##    day    time    temp   activ 
## 307.13 1446.20   37.60    0.62  

# get the mean of each list item and simplify the output
sapply (beaver_data, function(x) round (apply (x, 2, mean), 2))

##       beaver1 beaver2
## day    346.20  307.13
## time  1312.02 1446.20
## temp    36.86   37.60
## activ    0.05    0.62
```

### tapply() for vectors

tapply() is used to apply a function over subsets of a vector . It is primarily used when we have the following circumstances:

1. A dataset that can be broken up into groups (via categorical variables - aka factors )
2. We desire to break the dataset up into groups
3. Within each group, we want to apply a function The arguments to tapply() are as follows: • x is a vector • INDEX is a factor or a list of factors (or else they are coerced to factors ) • FUN is a function to be applied • … contains other arguments to be passed FUN • simplify , should we simplify the result?

syntax of tapply function

tapply (x, INDEX, FUN, …, simplify = TRUE)

The second of these functions is `tapply()` which has three key arguments. As before `X` specifies the data, and `FUN` specifies a function. However, there is also an `INDEX` argument, which specifies a grouping variable. What the `tapply()` function does is loop over all of the different values that appear in the `INDEX` variable. Each such value defines a group: the `tapply()` function constructs the subset of `X` that corresponds to that group, and then applies the function `FUN` to that subset of the data:

```r
gender <- c("male","male","female","female","male")
age <- c(10,12,9,11,13)
tapply(X = age, INDEX = gender, FUN = mean)
##   female     male 
## 10.00000 11.66667
```

In this extract, what we’re doing is using `gender` to define two different groups of people, and using `ages` as the data. We then calculate the mean of the ages, separately for the males and the females. A closely related function is `by()`. It actually does the same thing as `tapply()` but the output is formatted a bit differently. This time around the three arguments are called `data`, `INDICES` and `FUN`, but they are pretty much the same thing:

```r
by(data = age, INDICES = gender, FUN = mean)
## gender: female
## [1] 10
## -------------------------------------------------------- 
## gender: male
## [1] 11.66667
```

The `tapply()` and `by()` functions are quite handy things to know about, and are pretty widely used. However, although I do make passing reference to the `tapply()` later on, I don’t make much use of them in this book.

Before moving on, I should mention that there are several other functions that work along similar lines, and have suspiciously similar names: `lapply`, `mapply`, `apply`, `vapply`, `rapply` and `eapply`. However, none of these come up anywhere else in this book, so all I wanted to do here is draw your attention to the fact that they exist.

### Nested loops
