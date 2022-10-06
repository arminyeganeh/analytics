---
description: Reading 1, 450-600 lines, 3 hours to complete
---

# Intro to R P2

## Intro to R & RStudio P2

#### Instructions

Read this tutorial and apply the codes in R. No submission is required.

#### Rules & conventions for naming variables

Variable names (`sales` and `revenue`) have just been English-language words written using lowercase letters. However, R allows a lot more flexibility when it comes to naming variables:

* Variable names can take any upper case, lower case, or numeric characters, as well as period and underscore characters.
* Variable names cannot include spaces: therefore `my sales` is not valid.
* Variable names are case sensitive: that is, `Sales` and `sales` are _different_ variable names.
* Variable names must start with a letter or, for special purposes, a period.
* Variable names cannot be one of the reserved keywords: `if`, `else`, `repeat`, `while`, `function`, `for`, `in`, `next`, `break`, `TRUE`, `FALSE`, `NULL`, `Inf`, `NaN`, `NA`, `NA_integer_`, `NA_real_`, `NA_complex_`, and finally, `NA_character_`.

You aren’t obliged to follow these conventions, but it’s generally a good idea to follow them:

* Use informative variable names. As a general rule, using meaningful names like `sales` and `revenue` is preferred over arbitrary ones like `variable1` and `variable2`.
* Use short variable names. So we much prefer to use a name like `sales` over a name like `sales.for.this.book.that.you.are.reading`.
* Conventional naming styles for multi-word variables include separating words using periods, like `my.new.salary` or underscores, like `my_new_salary`.

#### Using functions in calculations

The symbols `+`, `-`, `*` and so on are examples of operators. However, in order to do more advanced calculations and statistical operations, you’re going to need functions. To get started, suppose I wanted to take the square root of 225. Since the square root of 255 is the same thing as raising 225 to the power of 0.5, I could use the power operator `^`, just like we did earlier:

```
> 225 ^ 0.5
[1] 15
```

However, there’s a second way that we can do this, since R also provides the **square root function**, `sqrt()`. To calculate the square root of 255 using this function, what I do is insert the number `225` in the parentheses:

```
> sqrt(225)
[1] 15
```

When we use a function to do something, we generally refer to this as **calling** the function, and the values that we type into the function (there can be more than one) are referred to as the **arguments** of that function. One function that we will need to use is the **absolute value function**. Compared to the square root function, it’s extremely simple: it just converts negative numbers to positive numbers and leaves positive numbers alone. R provides the function `abs()` for this purpose:

```
> abs(-21)
[1] 21
```

R allows us to put functions together and even combine functions with operators:

```
> sqrt(1 + abs(-8))
[1] 3
```

#### Function arguments, names, and defaults

There are two more fairly important things about functions: named arguments, and default values for arguments. To understand what these two concepts are all about, let's use the function `round()` to round some value to the nearest whole number:

```
> round(3.1415)
[1] 3
```

Suppose you only wanted to round it to two decimal places, which is `3.14` . The function `round()` supports this, by allowing you to input a second argument to the function that specifies the number of decimal places that you want to round the number to:

```
> round(3.14165, 2)
[1] 3.14
```

What’s happening here is that we’ve specified _two_ arguments: the first argument is the number that needs to be rounded, and the second argument is the number of decimal places that it should be rounded to, and the two arguments are separated by a comma. In this simple example, it’s quite easy to remember which argument comes first and which one comes second, but for more complicated functions this is not easy. Most R functions make use of **argument names**. For the `round()` function, the number that needs to be rounded is specified using the argument `x` , and the number of decimal points that you want it rounded to is specified using the argument `digits`:

```
> round(x = 3.1415, digits = 2)
[1] 3.14
```

Specifying the arguments by name involves more typing, but it’s easier to read and safer because it doesn’t matter what order you type them in. But if you don’t use the argument names, then you have to input the arguments in the correct order:

```
> round(digits = 2, x = 3.1415)
[1] 3.14
```

The second thing you need to know about is **default values**. Notice that the first time we called the `round()` function we didn’t actually specify the `digits` argument at all, and yet R somehow knew that this meant it should round to the nearest whole number. How did that happen? The answer is that the `digits` argument has a default value of `0`. This is quite handy: the vast majority of the time when you want to round a number you want to round it to the nearest whole number, and it would be pretty annoying to have to specify the `digits` argument every single time.

#### RStudio's help with commands

There are a _lot_ of R functions, all of which have their own arguments. You’re probably also worried that you’re going to have to remember all of them! Thankfully, it’s not that bad. In fact, very few data analysts bother to try to remember all the commands. What they really do is use tricks to make their lives easier. The first is to use the internet. If you don’t know how a particular R function works, Google it. Second, you can look up the R help documentation. Third, there are simple tricks that RStudio makes available as follows.

The first thing is the autocomplete ability in RStudio. Start typing the name of the function that you want. RStudio will then display a little window like the one shown in **Figure HA1.1**. The window has two panels. On the left, there’s a list of variables and functions that start with the typed letters, and some grey text that tells you in which package the variable/function is stored. The panel on the right displays information about the `round()` function. You can see that there are a few things that start with the typed letters. You can use the up and down arrow keys to select the one that you want and hit the Tab or the Enter key. Or, if none of the options look right to you, you can hit the Escape key to make the window go away.

#### Command history

R automatically keeps track of your command history. The simplest way is to use the up and down arrow keys in the R console. The second way to get access to your command history is to look at the history panel in RStudio. If you double-click on one of the commands, it will be copied to the R console.

#### Storing numbers as a vector

In R, the name for a variable that can store multiple values is a **vector**. So let’s create one. Let’s stick to the textbook example. Suppose the textbook company sends you sales data on a monthly basis. Let’s suppose there were 100 sales in February, 200 sales in March and 50 sales in April, and no other sales for the rest of the year. What we would like is `sales.by.month`. The first number stored should be `0` since there were no sales in January and the second should be `100`. The way to do this in R is to use the **combine** function `c()` and type the numbers in a comma-separated list:

```
> sales.by.month <- c(0, 100, 200, 50, 0, 0, 0, 0, 0, 0, 0, 0)
> sales.by.month
[1]   0 100 200  50   0   0   0   0   0   0   0   0
```

To use the correct terminology here, we have a single variable called `sales.by.month`, a vector that consists of 12 **elements**. Now, the next thing to understand is how to pull that information back out again. Suppose we want to pull out the February sales data only. February is the second month of the year, so let’s try this:

```
> sales.by.month[2]
[1] 100
```

Notice that the output is `[1] 100`, not `[2] 100`. This is because R is extremely literal. When we typed `sales.by.month[2]`, we asked R to find exactly _one_ thing, and that one thing happens to be the second element of our `sales.by.month` vector. To store the February sales, we could create the variable `february.sales` :

```
> february.sales <- sales.by.month[2]
> february.sales
[1] 100
```

#### Working with vectors

Sometimes you’ll want to change the values stored in a vector. One possibility would be to assign the whole vector again from the beginning, using `c()`. But that’s a lot of typing. Also, it’s a little wasteful: why should R have to redefine the sales figures for all 12 months, when only the 5th one is wrong? Fortunately, we can tell R to change only the 5th element, using this trick:

```
> sales.by.month[5] <- 25
> sales.by.month
[1]   0 100 200  50  25   0   0   0   0   0   0   0
```

Another way to edit variables is to use the `edit()` or `fix()` functions.

You often find yourself wanting to know how many elements there are in a vector. You can use the `length()` function to do this:

```
> length(x = sales.by.month)
[1] 12
```

Secondly, you often want to alter all of the elements of a vector at once. For instance, suppose you wanted to figure out how much money you made each month. You may want to get $7 per book, thus, what you want to do is multiply each element in the `sales.by.month` vector by 7:

```
> sales.by.month * 7
[1]    0  700 1400  350  175    0    0    0    0    0    0    0
```

In other words, when you multiply a vector by a single number, all elements in the vector get multiplied. The same is true for addition, subtraction, division, and taking powers. Suppose you wanted to know how much money was made per day, rather than per month. Since not every month has the same number of days, you need to do something slightly different:

```
> days.per.month <- c(31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
> profit <- sales.by.month * 7
> profit / days.per.month
[1]  0.000000 25.000000 45.161290 11.666667  5.645161  0.000000  0.000000
[8]  0.000000  0.000000  0.000000  0.000000  0.000000
```

Notice that the second element of the output is 25 because R has divided the second element of `profit` (i.e. 700) by the second element of `days.per.month` (i.e. 28). Similarly, the third element of the output is equal to 1400 divided by 31, and so on.

#### Storing text data

A lot of the time data will be numeric in nature, but sometimes data really needs to be described using text. To create a variable that stores the word “hello”, we can type this:

```
> greeting <- "hello"
> greeting
[1] "hello"
```

When interpreting this, it’s important to recognize that the quotation marks here aren’t part of the string itself. They just tell R to treat the characters that they enclose are text data, known as a **character string**. In other words, R treats `"hello"` as a string containing the word “hello”; but if you had typed `hello` , R would go looking for a variable by that name! You can also use `'hello'` to specify a character string. Of course, there’s no reason why we can’t create a vector of character strings:

```
months <- c("January", "February", "March", "April", "May", "June",
            "July", "August", "September", "October", "November", 
            "December")
```

This is a **character vector** containing 12 elements, each of which is the name of a month. So if you wanted R to find the name of the fourth month, all you would do is this:

```
> months[4]
[1] "April"
```

So far, most of the functions that we have seen (i.e., `sqrt()`, `abs()` and `round()`) only make sense when applied to numeric data (e.g., you can’t calculate the square root of “hello”), and we’ve seen one function that can be applied to pretty much any variable or vector (i.e., `length()`). The function `nchar()` counts the number of individual characters that make up a string.

```
> nchar(x = greeting)
[1] 5
```

That makes sense, since there are in fact 5 letters in the string `"hello"`. Better yet, you can apply `nchar()` to whole vectors: So, for instance, to ask R how many letters there are in the names of each of the 12 months:

```
> nchar(x = months)
[1] 7 8 5 5 3 4 4 6 9 7 8 8
```

#### Storing “true or false” data

A key concept in R is the idea of a **logical value**. A logical value is an assertion about whether something is true or false. This is implemented in R in a pretty straightforward way. There are two logical values, namely `TRUE` and `FALSE`. If you want R to make an explicit judgement, you can use a command like this:

```
> 2 + 2 == 4
[1] TRUE
> 2 + 2 == 5
[1] FALSE
```

What we’ve done here is use the **equality operator** `==` to force R to make a “true or false” judgment. You probably won’t be surprised to discover that we can combine logical operations with other operations and functions in a more complicated way, like this:

```
> 3*3 + 4*4 == 5*5
[1] TRUE
```

There are several other logical operators that you can use, corresponding to some basic mathematical concepts.

| operation                | operator | example input    | answer  |
| ------------------------ | -------- | ---------------- | ------- |
| less than                | <        | 2 < 3            | `TRUE`  |
| less than or equal to    | <=       | 2 <= 2           | `TRUE`  |
| greater than             | >        | 2 > 3            | `FALSE` |
| greater than or equal to | >=       | 2 >= 2           | `TRUE`  |
| equal to                 | ==       | 2 == 3           | `FALSE` |
| not equal to             | !=       | 2 != 3           | `TRUE`  |
| not                      | !        | !(1==1)          | `FALSE` |
| or                       | \|       | (1==1) \| (2==3) | `TRUE`  |
| and                      | &        | (1==1) & (2==3)  | `FALSE` |

For instance, if we ask R to assess the claim that “either $$2+2=4$$ _or_ $$2+2=5$$” R would say that it’s true. Since it’s an “either-or” statement, all we need is for one of the two parts to be true. That’s what the `|` operator does:

```
> (2+2 == 4) | (2+2 == 5)
[1] TRUE
```

On the other hand, if we ask R to assess the claim that “both $$2+2=4$$ _and_ $$2+2=5$$” R would say that it’s false. Since this is an _and_ statement we need both parts to be true. And that’s what the `&` operator does:

```
> (2+2 == 4) & (2+2 == 5)
[1] FALSE
```

Finally, there’s the _not_ operator, which is simple but annoying to describe in English. If we ask R to assess the claim that “it is not true that $$2+2=5$$”:

```
> ! (2+2 == 5)
[1] TRUE
> 2+2 != 5
[1] TRUE
```

Up to this point, we’ve seen _numeric data_ and _character data_. So you might not be surprised to discover that these `TRUE` and `FALSE` values that R has been producing are actually a third kind of data, called _logical data_. When asking R if `2 + 2 == 5` it says `[1] FALSE` in reply, which is information that we can store in variables. For instance, we could create a variable called `two.plus.two.equals.five` which would store R’s opinion:

```
> two.plus.two.equals.five <- 2 + 2 == 5
> two.plus.two.equals.five
[1] FALSE
> two.plus.two.equals.five <- FALSE
> two.plus.two.equals.five
[1] FALSE
```

Better yet, because it’s kind of tedious to type `TRUE` or `FALSE` over and over again, R provides you with the case-sensitive shortcuts `T` and `F` :

```
> two.plus.two.equals.five <- F
> two.plus.two.equals.five
[1] FALSE
```

#### Vectors of logical data

You can store vectors of logical values in the same way that you can store vectors of numbers and text data:

```
> x <- c(TRUE, TRUE, FALSE)
> x
[1]  TRUE  TRUE FALSE
```

We could use the `sales.by.month` vector and ask R whether any books were sold each month:

```
> sales.by.month > 0
[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
[12] FALSE
> any.sales.this.month <- sales.by.month > 0
any.sales.this.month
[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
[12] FALSE
```

In other words, `any.sales.this.month` is a logical vector whose elements are `TRUE` only if the corresponding element of `sales.by.month` is greater than zero.

#### Applying logical operation to text

We can ask R if the word `"cat"` is the same as the word `"dog"`, like this:

```
"cat" == "dog"
[1] FALSE
"cat" == "cat"
[1] TRUE
```

R is not at all tolerant when it comes to grammar and spacing. If two strings differ in any way whatsoever, R will say that they’re not equal to each other, as the following examples indicate:

```
" cat" == "cat"
[1] FALSE
"cat" == "CAT"
[1] FALSE
"cat" == "c a t"
[1] FALSE
```

#### Indexing vectors

So far, whenever we’ve had to get information out of a vector, all we’ve done is typed something like `months[4]` ; and when we do this R prints out the fourth element of the `months` vector. In this section, we’ll see two additional tricks for getting information out of the vector.

One very useful thing we can do is pull out more than one element at a time. Suppose we wanted the data for February, March and April. We could use the vector `c(2,3,4)` to indicate which elements we want R to pull out. Notice that the order matters here.

```
> sales.by.month[c(2,3,4)]
[1] 100 200  50
> sales.by.month[c(4,3,2)]
[1]  50 200 100
```

A second thing to be aware of is that R provides you with handy shortcuts for very common situations. For instance, suppose that we wanted to extract everything from the 2nd month through to the 8th month:

```
> sales.by.month[c(2,3,4,5,6,7,8)]
[1] 100 200  50  25   0   0   0
```

To help make this easier, R lets you use `2:8` as shorthand for `c(2,3,4,5,6,7,8)`, which makes things a lot simpler:

```
> 2:8
[1] 2 3 4 5 6 7 8
> sales.by.month[2:8]
[1] 100 200  50  25   0   0   0
```

At this point, we can introduce an extremely useful tool called **logical indexing**. We created a logical vector `any.sales.this.month`, whose elements are `TRUE` for any month in which at least one book was sold, and `FALSE` for all the others. However, that big long list of `TRUE`s and `FALSE`s is a little bit hard to read, so what we’d like to do is to have R select the names of the `months` for any book was sold. Earlier on, we created a vector `months` that contains the names of each of the months. What we need to do is this:

```
> months[sales.by.month>0]
[1] "February" "March" "April" "May"
```

To understand what’s happening here, it’s helpful to notice that `sales.by.month > 0` is the same logical expression that we used to create the `any.sales.this.month` vector in the last section. In fact, we could have just done this:

```
> months[any.sales.this.month]
[1] "February" "March" "April" "May"
> sales.by.month[sales.by.month > 0]
[1] 100 200  50  25
```

We can do the same thing with text. Suppose that – to continue the saga of the textbook sales – we later find out that the bookshop only had sufficient stocks for a few months of the year. Early in the year they had `"high"` stocks, which then dropped to `"low"` levels, and in fact for one month they were `"out"` of copies of the book for a while before they were able to replenish them. We might have a variable called `stock.levels` which looks like this:

```
> stock.levels<-c("high", "high", "low", "out", "out", "high",
                "high", "high", "high", "high", "high", "high")
> stock.levels
[1] "high" "high" "low"  "out"  "out"  "high" "high" "high" "high" "high"
[11] "high" "high"
```

Thus, if we want to know the months for which the bookshop was out of book, we could apply the logical indexing trick, but with the character vector `stock.levels`, like this:

```
> months[stock.levels == "out"]
[1] "April" "May"
```

Alternatively, if we want to know when the bookshop was either low on copies or out of copies, we could do this:

```
> months[stock.levels == "out" | stock.levels == "low"]
[1] "March" "April" "May"
```

or this

```
months[stock.levels != "high" ]
[1] "March" "April" "May"
```

Either way, we get the answer we want. Logical indexing is a very basic, yet very powerful way to manipulate data. It does take a bit of practice to become completely comfortable using logical indexing, so it’s a good idea to play around with these sorts of commands. Try creating a few different variables of your own, and then ask yourself questions like “how do I get R to spit out all the elements that are \[blah]”. Practice makes perfect, and it’s only by practicing logical indexing that you’ll perfect the art of yelling frustrated insults at your computer.

#### Quitting R

<figure><img src="https://learningstatisticswithr.com/book/img/introR/Rstudio_quit.png" alt=""><figcaption><p><strong>Figure HA1.2</strong> The RStudio closing dialogbox</p></figcaption></figure>

R has a function, called `q()` that you can use to quit, which is pretty handy if you’re running R in a terminal window. Regardless of what method you use to quit R, when you do so for the first time R will probably ask you if you want to save the “workspace image”. If you’re using RStudio, you’ll see a dialog box that looks like the one shown in **Figure HA1.2**. If you’re using a text based interface you’ll see this:

```
> q()
Save workspace image? [y/n/c]: 
```

The `y/n/c` part here is short for “yes/no/ cancel”. Type `y` if you want to save, `n` if you don’t, and `c` if you’ve changed your mind and you don’t want to quit after all. What does this actually _mean_? t R wants to know if you want to save all those variables that you’ve been creating in a “default” data file, which it will automatically reload for you next time you open R. You can change the settings so that it never asks this again. You can do this in RStudio really easily: use the menu system to find the RStudio option; the dialog box that comes up will give you an option to tell R never to whine about this again. See **Figure HA1.3**. On a Mac, you can open this window by going to the “RStudio” menu and selecting “Preferences”. On a Windows machine you go to the “Tools” menu and select “Global Options”. Under the “General” tab you’ll see an option that reads “Save workspace to .Rdata on exit”. By default this is set to “ask”. If you want R to stop asking, change it to “never”.

<figure><img src="https://learningstatisticswithr.com/book/img/introR/Rstudio_options.png" alt=""><figcaption><p><strong>Figure HA1.3</strong> The RStudio Options window</p></figcaption></figure>

#### Comments

The **comment character** `#` has a simple meaning: it tells R to ignore everything else you’ve written on this line. You won’t have much need of the `#` character immediately, but it’s very useful later on when writing scripts. For instance, if you read this:

```
seeker <- 3.1415           # create the first variable
lover <- 2.7183            # create the second variable
keeper <- seeker * lover   # now multiply them to create a third one
print(keeper)              # print out the value of 'keeper'
[1] 8.539539
```

```
## [1] 8.539539
```

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/Rstudiopackages.png" alt=""><figcaption><p><strong>Figure HA1.4</strong> RStudio's packages panel</p></figcaption></figure>

Yep. All good.
