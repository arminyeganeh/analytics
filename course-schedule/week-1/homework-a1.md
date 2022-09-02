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

&#x20;When we use a function to do something, we generally refer to this as **calling** the function, and the values that we type into the function (there can be more than one) are referred to as the **arguments** of that function. One function that we will need to use is the **absolute value function**. Compared to the square root function, it’s extremely simple: it just converts negative numbers to positive numbers, and leaves positive numbers alone. R provides the `abs()` function that you can use for this purpose:

```
> abs(-21)
[1] 21
```

R allows us to put functions together and even combine functions with operators:

```
> sqrt(1 + abs(-8))
[1] 3
```

#### Function arguments, their names and their defaults

There’s two more fairly important things that you need to understand about how functions work in R, and that’s the use of “named” arguments, and default values" for arguments. Not surprisingly, that’s not to say that this is the last we’ll hear about how functions work, but they are the last things we desperately need to discuss in order to get you started. To understand what these two concepts are all about, I’ll introduce another function. The `round()` function can be used to round some value to the nearest whole number. For example, I could type this:

```
round( 3.1415 )
```

```
## [1] 3
```

Pretty straightforward, really. However, suppose I only wanted to round it to two decimal places: that is, I want to get `3.14` as the output. The `round()` function supports this, by allowing you to input a second argument to the function that specifies the number of decimal places that you want to round the number to. In other words, I could do this:

```
round( 3.14165, 2 )
```

```
## [1] 3.14
```

What’s happening here is that I’ve specified _two_ arguments: the first argument is the number that needs to be rounded (i.e., `3.1415`), the second argument is the number of decimal places that it should be rounded to (i.e., `2`), and the two arguments are separated by a comma. In this simple example, it’s quite easy to remember which one argument comes first and which one comes second, but for more complicated functions this is not easy. Fortunately, most R functions make use of _**argument names**_. For the `round()` function, for example the number that needs to be rounded is specified using the `x` argument, and the number of decimal points that you want it rounded to is specified using the `digits` argument. Because we have these names available to us, we can specify the arguments to the function by name. We do so like this:

```
round( x = 3.1415, digits = 2 )
```

```
## [1] 3.14
```

Notice that this is kind of similar in spirit to variable assignment (Section [3.4](https://learningstatisticswithr.com/book/introR.html#assign)), except that I used `=` here, rather than `<-`. In both cases we’re specifying specific values to be associated with a label. However, there are some differences between what I was doing earlier on when creating variables, and what I’m doing here when specifying arguments, and so as a consequence it’s important that you use `=` in this context.

As you can see, specifying the arguments by name involves a lot more typing, but it’s also a lot easier to read. Because of this, the commands in this book will usually specify arguments by name,[31](https://learningstatisticswithr.com/book/introR.html#fn31) since that makes it clearer to you what I’m doing. However, one important thing to note is that when specifying the arguments using their names, it doesn’t matter what order you type them in. But if you don’t use the argument names, then you have to input the arguments in the correct order. In other words, these three commands all produce the same output…

```
round( 3.14165, 2 )
```

```
## [1] 3.14
```

```
round( x = 3.1415, digits = 2 )
```

```
## [1] 3.14
```

```
round( digits = 2, x = 3.1415 )
```

```
## [1] 3.14
```

but this one does not…

```
round( 2, 3.14165 )
```

```
## [1] 2
```

How do you find out what the correct order is? There’s a few different ways, but the easiest one is to look at the help documentation for the function (see Section [4.12](https://learningstatisticswithr.com/book/mechanics.html#help). However, if you’re ever unsure, it’s probably best to actually type in the argument name.

Okay, so that’s the first thing I said you’d need to know: argument names. The second thing you need to know about is default values. Notice that the first time I called the `round()` function I didn’t actually specify the `digits` argument at all, and yet R somehow knew that this meant it should round to the nearest whole number. How did that happen? The answer is that the `digits` argument has a _**default value**_ of `0`, meaning that if you decide not to specify a value for `digits` then R will act as if you had typed `digits = 0`. This is quite handy: the vast majority of the time when you want to round a number you want to round it to the nearest whole number, and it would be pretty annoying to have to specify the `digits` argument every single time. On the other hand, sometimes you actually do want to round to something other than the nearest whole number, and it would be even more annoying if R didn’t allow this! Thus, by having `digits = 0` as the default value, we get the best of both worlds.
