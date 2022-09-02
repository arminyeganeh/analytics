---
description: Homework Instructions, 4500 words, 3 hours
---

# Homework A1

### HA1 Instructions

Read the following R tutorial.

### Rules & conventions for naming variables

Variable names (`sales` and `revenue`) have just been English-language words written using lowercase letters. However, R allows a lot more flexibility when it comes to naming variables:

* Variable names can take any upper case, lower case, or numeric characters, as well as period and underscore characters.&#x20;
* Variable names cannot include spaces: therefore `my sales` is not valid.
* Variable names are case sensitive: that is, `Sales` and `sales` are _different_ variable names.
* Variable names must start with a letter or, for special purposes, a period.
* Variable names cannot be one of the reserved keywords: `if`, `else`, `repeat`, `while`, `function`, `for`, `in`, `next`, `break`, `TRUE`, `FALSE`, `NULL`, `Inf`, `NaN`, `NA`, `NA_integer_`, `NA_real_`, `NA_complex_`, and finally, `NA_character_`.&#x20;

You aren’t obliged to follow these conventions, but it’s generally a good idea to follow them:

* Use informative variable names. As a general rule, using meaningful names like `sales` and `revenue` is preferred over arbitrary ones like `variable1` and `variable2`.
* Use short variable names. So we much prefer to use a name like `sales` over a name like `sales.for.this.book.that.you.are.reading`.&#x20;
* Conventional naming styles for multi-word variables include separating words using periods, like `my.new.salary` or underscores, like `my_new_salary`.&#x20;

### Using functions in calculations

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

&#x20;When we use a function to do something, we generally refer to this as **calling** the function, and the values that we type into the function (there can be more than one) are referred to as the **arguments** of that function. One function that we will need to use is the **absolute value function**. Compared to the square root function, it’s extremely simple: it just converts negative numbers to positive numbers and leaves positive numbers alone. R provides the function `abs()` for this purpose:

```
> abs(-21)
[1] 21
```

R allows us to put functions together and even combine functions with operators:

```
> sqrt(1 + abs(-8))
[1] 3
```

### Function arguments, names, and defaults

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

### RStudio's help with commands

There are a _lot_ of R functions, all of which have their own arguments. You’re probably also worried that you’re going to have to remember all of them! Thankfully, it’s not that bad. In fact, very few data analysts bother to try to remember all the commands. What they really do is use tricks to make their lives easier. The first is to use the internet. If you don’t know how a particular R function works, Google it. Second, you can look up the R help documentation. Third, there are simple tricks that RStudio makes available as follows.

The first thing is the autocomplete ability in RStudio. Start typing the name of the function that you want. RStudio will then display a little window like the one shown in **Figure HA1**. The window has two panels. On the left, there’s a list of variables and functions that start with the typed letters, and some grey text that tells you in which package the variable/function is stored. The panel on the right displays information about the `round()` function. You can see that there are a few things that start with the typed letters. You can use the up and down arrow keys to select the one that you want and hit the Tab or the Enter key. Or, if none of the options look right to you, you can hit the Escape key to make the window go away.

<figure><img src="../../.gitbook/assets/rstudio_autosuggestion.png" alt=""><figcaption><p><strong>Figure HA1:</strong> RStudio autocomplete</p></figcaption></figure>

### Command history

R automatically keeps track of your command history. The simplest way is to use the up and down arrow keys in the R console. The second way to get access to your command history is to look at the history panel in RStudio. If you double-click on one of the commands, it will be copied to the R console.

### Storing numbers as a vector

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

### Working with vectors

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

Notice that the second element of the output is 25 because R has divided the second element of `profit` (i.e. 700) by the second element of `days.per.month` (i.e. 28). Similarly, the third element of the output is equal to 1400 divided by 31, and so on.&#x20;

### Storing text data

A lot of the time your data will be numeric in nature, but not always. Sometimes your data really needs to be described using text, not using numbers. To create a variable that stores the word “hello”, we can type this:

```
greeting <- "hello"
greeting
```

```
## [1] "hello"
```

When interpreting this, it’s important to recognise that the quote marks here _aren’t_ part of the string itself. They’re just something that we use to make sure that R knows to treat the characters that they enclose as a piece of text data, known as a _**character string**_. In other words, R treats `"hello"` as a string containing the word “hello”; but if I had typed `hello` instead, R would go looking for a variable by that name! You can also use `'hello'` to specify a character string.

Okay, so that’s how we store the text. Next, it’s important to recognise that when we do this, R stores the entire word `"hello"` as a _single_ element: our `greeting` variable is _not_ a vector of five different letters. Rather, it has only the one element, and that element corresponds to the entire character string `"hello"`. To illustrate this, if I actually ask R to find the first element of `greeting`, it prints the whole string:

```
greeting[1]
```

```
## [1] "hello"
```

Of course, there’s no reason why I can’t create a vector of character strings. For instance, if we were to continue with the example of my attempts to look at the monthly sales data for my book, one variable I might want would include the names of all 12 `months`.[36](https://learningstatisticswithr.com/book/introR.html#fn36) To do so, I could type in a command like this

```
months <- c("January", "February", "March", "April", "May", "June",
            "July", "August", "September", "October", "November", 
            "December")
```

This is a _**character vector**_ containing 12 elements, each of which is the name of a month. So if I wanted R to tell me the name of the fourth month, all I would do is this:

```
months[4]
```

```
## [1] "April"
```

