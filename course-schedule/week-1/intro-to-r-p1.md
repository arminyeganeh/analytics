---
description: Lab 1, 150-250 lines, 1 hour to complete
---

# Intro to R P1

## Intro to R & RStudio P1

#### Instructions

Read this tutorial and apply the codes in R. No submission is required.

#### Installing R

R is freely distributed online, and can be downloaded from:

> [http://cran.r-project.org/](http://cran.r-project.org/)

At the top of the page – under the heading “Download and Install R” – there are separate links for Windows, Mac, and Linux users. About every six months, a new update becomes available.

**Installing R on a Windows computer**

On the R homepage, you’ll find a link at the top of the page with the text “Download R for Windows”. If you click on that, it will take you to a page that offers you a few options. Again, at the top of the page, you’ll be told to click on a link that says **Install R for the first time**. This will take you to a page that has a prominent link at the top called **Download R for Windows**. Click on that and your browser should start downloading an R installer file. After R is installed on your system, you should install RStudio.

**Installing R on a Mac computer**

On the R homepage, you’ll find a link at the top of the page with the text “Download R for macOS”. If you click on that, it will take you to a page that offers you a few options. There’s a fairly prominent link on the page just under Latest Release, with a “.pkg” format. Click on that link and you’ll start downloading the installer file. Once downloaded, open the file by double-clicking and follow all the instructions. Once finished, you’ll find a file called `R.app` in the Applications folder. After R is installed on your system, you should install RStudio.

**Installing RStudio**

RStudio is an Integrated Development Environment (IDE), providing a free, open-source, professional interface to R, the underlying statistical language. RStudio can be downloaded from:

> [http://www.rstudio.com/](https://www.rstudio.com/products/rstudio/download/)

When you visit the RStudio website, find and download **RStudio Desktop**. After choosing the desktop version it will take you to a page that shows several possible downloads based on the operating system. The webpage automatically recommends the download that is most appropriate for your computer. Click on the appropriate link, and the RStudio installer file will start downloading. Once finished downloading, open the installer file in the usual way to install RStudio.

#### RStudio Interface

After installing, you can start R by opening RStudio. To illustrate what RStudio looks like, **Figure LA1** shows a screenshot of an R session in progress. There could be very small differences in RStudio's appearance between Mac and Windows systems. The upper-left area — called script editor, source, or program — is where you type, edit, and run R scripts. The bottom-left area — called R console — is where you see the execution of the script. You can code directly in this window but your code cannot be saved. It is best to use this window when you are simply performing calculator-type functions. This is also where your outputs will be presented when you run code in your script. In the top-right area, the environment pane, you see the objects and values created and used in the program, as well as other relevant information. The bottom-right area is where you have access to multiple additional tabs, which will be discussed later in detail.

#### Starting up R

A common way to start learning R is using it as a simple calculator.

Click on the script area and type `10 + 20` . Click on the _Run_ button located on the script pane. What you see on the console is:

```
> 10 + 20
[1] 30
```

The symbol `>` is the _command prompt_, the \_\_ `10 + 20` part is a _command_, and the part printed in the next line `[1] 30` is R's _response_ to your command. For now, think of `[1] 30` as if R were saying “the answer to the 1st question you asked is 30”.

#### Autocorrection

Always make sure you type _exactly what you mean_. In general, you absolutely _must_ be precise in what you say to R l in its interpretation as there is no equivalent to “autocorrect” in R. There are some situations in which R does show some flexibility. For instance, R ignores redundant spacing in the following cases:

```
> 10 + 20
[1] 30
> 10  +  20
[1] 30
```

However, inserting spaces in the middle of a word results in an error. For instance, try the following commands, individually, to obtain information about how to cite R:

```
citation()
citation ()
cit ation()
```

#### Basic arithmetic operations

Let’s try basic arithmetic operations in R. Table LA.1 lists the operators that correspond to the basic arithmetic of addition, subtraction, multiplication, division, and power.

Table LA.1: Basic arithmetic operations in R.

| operation      | operator | example input | example output |
| -------------- | :------: | :-----------: | :------------: |
| addition       |    `+`   |     10 + 2    |       12       |
| subtraction    |    `-`   |     9 - 3     |        6       |
| multiplication |    `*`   |     5 \* 5    |       25       |
| division       |    `/`   |     10 / 3    |        3       |
| power          |    `^`   |     5 ^ 2     |       25       |

#### Order of operations

In most situations where you would want to use a calculator, you might want to do multiple calculations. R lets you do this, just by typing in longer commands. Try:

```
> 1 + 2 * 4
[1] 9
```

In such cases, you need to know the order of operations that R uses, the **BEDMAS** order. That is, first calculate inside **B**rackets `()`, then **E**xponents `^`, then **D**ivision `/` and **M**ultiplication `*`, then **A**ddition `+` and **S**ubtraction `-`. So, to continue the example above, if we want to force R to calculate the part `1+2` before the multiplication, we would have to enclose it in brackets:

```
> (1 + 2) * 4
[1] 12 
```

What to expect when you have two operations that have the same priority: that is, how does R resolve ties? For instance, how does R solve a problem like `4 / 2 * 3` ? The answer, in this case, is that R goes from _left to right_, so in this case, the division step would come first:

```
> 4 / 2 * 3
[1] 6
```

Remember that brackets always come first. So, if you’re ever unsure about what order R will do things in, an easy solution is to enclose the thing you want it to do first in brackets. There’s nothing stopping you from typing (4 / 2) \* 3.

#### Variable assignment

One of the most important things to be able to do in R (or any programming language, for that matter) is to store information in variables. At a conceptual level, you can think of a variable as a label for one or more pieces of information. When doing statistical analysis in R, all of your data will be stored as variables in R, but you create variables for other things too. Let’s start by creating variables to store our numbers.

Suppose you want to calculate how much money you are going to make from writing a book. There are several different numbers you might want to store. Let’s assume the book will sell one copy per student in your class. So, let’s create a variable called `sales` to which you assign a value, which is `350`, using the assignment operator `<-`. Here’s how we do it:

```
> sales <- 350
```

When you run this command, R creates a variable called `sales` and gives it a value of `350`. You can check that this has happened by looking at the environment pane or asking R to print the variable. The simplest way to do _that_ is to type the name of the variable and run it.

```
> sales
[1] 350
```

There are several different ways of making assignments. In addition to the `<-` operator, we can also use `->` and `=` to perform the same operation. `<-` and `->` are identical, just in a “left form” and a “right form.” So if I wanted to define my `sales` variable using `->`, I would write it like this:

```
350 -> sales
```

In addition to defining a `sales` variable that counts the number of copies you are going to sell, you can also create a variable called `royalty`, indicating how much money you get per copy. Let’s say that your royalties are about $7 per book:

```
sales <- 350
royalty <- 7
```

R allows you to multiply `sales` by `royalty`. Not surprisingly, you can assign the output of this calculation to a new variable, such as `revenue`.

```
> sales * royalty
[1] 2450
> revenue <- sales * royalty
> revenue
[1] 2450
```

We can reassign the value of a variable, based on its current value. For instance, suppose that one of your students loves the book so much that she donates an extra $550. The simplest way to capture this is by a command like this:

```
> revenue <- revenue + 550
> revenue
[1] 3000
```

In this calculation, R has taken the old value of `revenue` (i.e., 2450) and added 550 to that value, producing a value of 3000. This new value is assigned to the `revenue` variable, overwriting its previous value. In any case, we now know that I’m expecting to make $3000 off this. Pretty sweet, I thinks to myself. Or at least, that’s what I thinks until I do a few more calculation and work out what the implied hourly wage I’m making off this looks like.
