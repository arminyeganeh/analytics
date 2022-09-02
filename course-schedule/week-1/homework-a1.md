---
description: Homework Instructions, 4500 words, 3 hours
---

# Homework A1

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

R automatically keeps track of your command history. The simplest way is to use the up and down arrow keys in the R console. The second way to get access to your command history is to look at the history panel in RStudio. If you double-click on one of the commands, it will be copied to the R console.&#x20;

