---
description: Week 2, HA2, 450 lines, 3 hours to complete
---

# Homework A2

### HA2 Instructions

Read this tutorial and apply the codes in R.

#### Why do the Windows paths use the wrong slash?

Let’s suppose I’m on Windows. As before, I can find out what my current working directory is like this:

```
> getwd()
[1] "C:/Users/dan/
```

This seems about right, but you might be wondering why R is displaying a Windows path using the wrong type of slash. The answer is slightly complicated, and has to do with the fact that R treats `\` as a special character. To tell R not to treat `\` as a special character requires adding a skip character `\` . If you want to specify the working directory on a Windows computer, you need to use one of the following commands:

```
> setwd( "C:/Users/dan" )
> setwd( "C:\\Users\\dan" )
```

The Files panel shown in **Figure HA2.1** is a decent file browser, which can be used to set the working directory and even delete, rename, and load files.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/filepanel.png" alt=""><figcaption><p><strong>Figure HA2.1</strong> RStudio's Files panel </p></figcaption></figure>

At the top of the file panel you see some text that says “Home > Rbook > data”, which means the list below shows files are stored in that directory. To change the R working directory using the file panel click on the button “More”. This will bring up a little menu, and one of the options will be “Set as Working Directory”. If you select that option, then R really will change the working directory. You can tell that it has done so because a command like this appears in the console:

```
> setwd("~/Rbook/data")
```

### Loading and saving data

There are several different types of files that are likely to be relevant to us when doing data analysis. There are three in particular that are especially important from the perspective of this book:

* _Workspace files_ are those with a .Rdata file extension. This is the standard kind of file that R uses to store data and variables. They’re called “workspace files” because you can use them to save your whole workspace.
* _Comma separated value (CSV) files_ are those with a .csv file extension. These are just regular old text files, and they can be opened with almost any software. It’s quite typical for people to store data in CSV files, precisely because they’re so simple.
* _Script files_ are those with a .R file extension. These aren’t data files at all; rather, they’re used to save a collection of commands that you want R to execute later. They’re just text files, but we won’t make use of them now.

There are also several other kinds of data file that you might want to import into R. For instance, you might want to open Microsoft Excel spreadsheets (.xlsx files), or data files that have been saved in the native file formats for other statistics software, such as SPSS, SAS, Minitab, Stata or Systat. Finally, you might have to handle databases.

### Loading workspace files

When we used the `list.files()` command to list the contents of the `/Users/dan/Rbook/data` directory, the output referred to a file called booksales.Rdata. Let’s say we want to load the data from this file into the workspace. The way we do this is with the `load()` function. There are two arguments to this function, but the only one we’re interested in is

* `file`. This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.

Using the absolute file path, the command would look like this:

```
> load( file = "/Users/dan/Rbook/data/booksales.Rdata" )
```

but this is pretty lengthy. Given that the working directory is `/Users/dan/Rbook/data`, we could use a relative file path, like so:

```
> load(file = "../data/booksales.Rdata")
```

However, the preference is usually to change the working directory first and then load the file:

```
> setwd("../data")         # move to the data directory
> load("booksales.Rdata")  # load the data
```

Okay, so how do we open an .Rdata file using the RStudio file panel? All we have to do is to click on the file name. RStudio brings up a little dialog box asking to confirm that we do want to load this file. The following command then turns up in the console:

```
> load("~/Rbook/data/booksales.Rdata")
```

### Importing data from CSV files

One quite commonly used data format is the humble “comma separated value” file, also called a CSV file, and usually bearing the file extension .csv. CSV files are just plain old-fashioned text files, and what they store is basically just a table of data, illustrated in **Figure HA2.2**. As you can see, each row corresponds to a variable, and each row represents the book sales data for one month. The first row doesn’t contain actual data though: it has the names of the variables. On the left, we have opened the file using a spreadsheet program, which shows that the file is basically a table. On the right, the same file is open in a standard text editor, which shows how the file is formatted. The entries in the table are wrapped in quote marks and separated by commas.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/booksalescsv.jpg" alt=""><figcaption><p><strong>Figure HA2.2</strong> A CSV file </p></figcaption></figure>

If RStudio were not available to you, the easiest way to open this file would be to use the `read.csv()` function. For now there, are two arguments to the function that we will mention:

* `file` This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.
* `header` This is a logical value indicating whether or not the first row of the file contains variable names. The default value is `TRUE`.

Therefore, to import the CSV file, the command we need is:

```
books <- read.csv(file = "booksales.csv")
```

There are two very important points to notice here. Firstly, notice that we didn’t try to use the `load()` function, because that function is only meant to be used for .Rdata files. If you try to use `load()` on other types of data, you get an error. Secondly, notice that when we imported the CSV file we assigned the result to a variable, which we imaginatively called `books` . There’s a reason for this. The idea behind the file `.Rdata` is that it stores a whole workspace. So, if you had the ability to look inside the file yourself you’d see that the data file keeps track of all the variables and their names. So when you `load()` the file, R restores all those original names. CSV files are treated differently: as far as R is concerned, the CSV only stores one variable, but that variable is a big table. So when you import that table into the workspace, R expects you to give it a name. Let’s have a look at what we’ve got:

```
print(books)
```

```
##        Month Days Sales Stock.Levels
## 1    January   31     0         high
## 2   February   28   100         high
## 3      March   31   200          low
## 4      April   30    50          out
## 5        May   31     0          out
## 6       June   30     0         high
## 7       July   31     0         high
## 8     August   31     0         high
## 9  September   30     0         high
## 10   October   31     0         high
## 11  November   30     0         high
## 12  December   31     0         high
```

Clearly, it’s worked, but the format of this output is a bit unfamiliar. We haven’t seen anything like this before. What you’re looking at is a _data frame_, which is a very important kind of variable in R.

### Importing data from CSV files using RStudio

Yet again, it’s easier in RStudio. In the environment panel in RStudio you should see a button called “Import Dataset”. Click on that, and it will give you a couple of options: select the “From Text File…” option, and it will open up a very familiar dialog box asking you to select a file: if you’re on a Mac, it’ll look like the usual Finder window that you use to choose a file; on Windows it looks like an Explorer window. Assuming that you’re familiar with your own computer, so should have no problem finding the CSV file that you want to import! Find the one you want, then click on the “Open” button. When you do this, you’ll see a window that looks like the one in **Figure HA2.3**. &#x20;

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/import.png" alt=""><figcaption><p><strong>Figure HA2.3</strong> The RStudio window for importing a CSV</p></figcaption></figure>

In the top left corner, you need to type the name of the variable you R to create. By default, that will be the same as the file name: our file is called `booksales.csv`, so RStudio suggests the name `booksales`. Immediately below this are a few things that you can tweak to make sure that the data gets imported correctly:

* Heading. Does the first row of the file contain raw data, or does it contain headings for each variable? The `booksales.csv` file has a header at the top, so we select “yes”.
* Separator. What character is used to separate different entries? In most CSV files this will be a comma (it is “comma separated” after all). But you can change this if your file is different.
* Decimal. What character is used to specify the decimal point? In English speaking countries, this is almost always a period (i.e., `.`). That’s not universally true: many European countries use a comma. So you can change that if you need to.
* Quote. What character is used to denote a block of text? That’s usually going to be a double quote mark. It is for the `booksales.csv` file, so that’s what we select.

Once you’re happy, click “Import”. When you do, two commands appear in the R console:

```
booksales <- read.csv("~/Rbook/data/booksales.csv")
View(booksales)
```

The first of these commands is the one that loads the data. The second one will display a pretty table showing the data in RStudio.

#### 4.5.5 Saving a workspace file using `save`

Not surprisingly, saving data is very similar to loading data. Although RStudio provides a simple way to save files (see below), it’s worth understanding the actual commands involved. There are two commands you can use to do this, `save()` and `save.image()`. If you’re happy to save _all_ of the variables in your workspace into the data file, then you should use `save.image()`. And if you’re happy for R to save the file into the current working directory, all you have to do is this:

```
save.image( file = "myfile.Rdata" )
```

Since `file` is the first argument, you can shorten this to `save.image("myfile.Rdata")`; and if you want to save to a different directory, then (as always) you need to be more explicit about specifying the path to the file, just as we discussed in Section [4.4](https://learningstatisticswithr.com/book/mechanics.html#navigation). Suppose, however, I have several variables in my workspace, and I only want to save some of them. For instance, I might have this as my workspace:

```
who()
##   -- Name --   -- Class --   -- Size --
##   data         data.frame    3 x 2     
##   handy        character     1         
##   junk         numeric       1        
```

I want to save `data` and `handy`, but not `junk`. But I don’t want to delete `junk` right now, because I want to use it for something else later on. This is where the `save()` function is useful, since it lets me indicate exactly which variables I want to save. Here is one way I can use the `save` function to solve my problem:

```
save(data, handy, file = "myfile.Rdata")
```

Importantly, you _must_ specify the name of the `file` argument. The reason is that if you don’t do so, R will think that `"myfile.Rdata"` is actually a _variable_ that you want to save, and you’ll get an error message. Finally, I should mention a second way to specify which variables the `save()` function should save, which is to use the `list` argument. You do so like this:

```
save.me <- c("data", "handy")   # the variables to be saved
save( file = "booksales2.Rdata", list = save.me )   # the command to save them
```

#### 4.5.6 Saving a workspace file using RStudio

RStudio allows you to save the workspace pretty easily. In the environment panel (Figures [4.5](https://learningstatisticswithr.com/book/mechanics.html#fig:workspace) and [4.6](https://learningstatisticswithr.com/book/mechanics.html#fig:workspace2)) you can see the “save” button. There’s no text, but it’s the same icon that gets used on every computer everywhere: it’s the one that looks like a floppy disk. You know, those things that haven’t been used in about 20 years. Alternatively, go to the “Session” menu and click on the “Save Workspace As…” option.[56](https://learningstatisticswithr.com/book/mechanics.html#fn56) This will bring up the standard “save” dialog box for your operating system (e.g., on a Mac it’ll look a little bit like the loading dialog box in Figure [4.9](https://learningstatisticswithr.com/book/mechanics.html#fig:fileopen)). Type in the name of the file that you want to save it to, and all the variables in your workspace will be saved to disk. You’ll see an R command like this one

```
save.image("~/Desktop/Untitled.RData")
```

Pretty straightforward, really.

#### 4.5.7 Other things you might want to save

Until now, we’ve talked mostly about loading and saving _data_. Other things you might want to save include:

* _The output_. Sometimes you might also want to keep a copy of all your interactions with R, including everything that you typed in and everything that R did in response. There are some functions that you can use to get R to write its output to a file rather than to print onscreen (e.g., `sink()`), but to be honest, if you do want to save the R output, the easiest thing to do is to use the mouse to select the relevant text in the R console, go to the “Edit” menu in RStudio and select “Copy”. The output has now been copied to the clipboard. Now open up your favourite text editor or word processing software, and paste it. And you’re done. However, this will only save the contents of the console, not the plots you’ve drawn (assuming you’ve drawn some). We’ll talk about saving images later on.
* _A script_. While it is possible – and sometimes handy – to save the R output as a method for keeping a copy of your statistical analyses, another option that people use a lot (especially when you move beyond simple “toy” analyses) is to write _scripts_. A script is a text file in which you write out all the commands that you want R to run. You can write your script using whatever software you like. In real world data analysis writing scripts is a key skill – and as you become familiar with R you’ll probably find that most of what you do involves scripting rather than typing commands at the R prompt. However, you won’t need to do much scripting initially, so we’ll leave that until Chapter [8](https://learningstatisticswithr.com/book/scripting.html#scripting).

### 4.6 Useful things to know about variables

In Chapter [3](https://learningstatisticswithr.com/book/introR.html#introR) I talked a lot about variables, how they’re assigned and some of the things you can do with them, but there’s a lot of additional complexities. That’s not a surprise of course. However, some of those issues are worth drawing your attention to now. So that’s the goal of this section; to cover a few extra topics. As a consequence, this section is basically a bunch of things that I want to briefly mention, but don’t really fit in anywhere else. In short, I’ll talk about several different issues in this section, which are only loosely connected to one another.

#### 4.6.1 Special values

The first thing I want to mention are some of the “special” values that you might see R produce. Most likely you’ll see them in situations where you were expecting a number, but there are quite a few other ways you can encounter them. These values are `Inf`, `NaN`, `NA` and `NULL`. These values can crop up in various different places, and so it’s important to understand what they mean.

* _Infinity_ (`Inf`). The easiest of the special values to explain is `Inf`, since it corresponds to a value that is infinitely large. You can also have `-Inf`. The easiest way to get `Inf` is to divide a positive number by 0:

```
1 / 0
```

```
## [1] Inf
```

In most real world data analysis situations, if you’re ending up with infinite numbers in your data, then something has gone awry. Hopefully you’ll never have to see them.

* _Not a Number_ (`NaN`). The special value of `NaN` is short for “not a number”, and it’s basically a reserved keyword that means “there isn’t a mathematically defined number for this”. If you can remember your high school maths, remember that it is conventional to say that $$0/00/0$$ doesn’t have a proper answer: mathematicians would say that $$0/00/0$$ is _undefined_. R says that it’s not a number:

```
 0 / 0
```

```
## [1] NaN
```

Nevertheless, it’s still treated as a “numeric” value. To oversimplify, `NaN` corresponds to cases where you asked a proper numerical question that genuinely has _no meaningful answer_.

* _Not available_ (`NA`). `NA` indicates that the value that is “supposed” to be stored here is missing. To understand what this means, it helps to recognise that the `NA` value is something that you’re most likely to see when analysing data from real world experiments. Sometimes you get equipment failures, or you lose some of the data, or whatever. The point is that some of the information that you were “expecting” to get from your study is just plain missing. Note the difference between `NA` and `NaN`. For `NaN`, we really do know what’s supposed to be stored; it’s just that it happens to correspond to something like $$0/00/0$$ that doesn’t make any sense at all. In contrast, `NA` indicates that we actually don’t know what was supposed to be there. The information is _missing_.
* _No value_ (`NULL`). The `NULL` value takes this “absence” concept even further. It basically asserts that the variable genuinely has no value whatsoever. This is quite different to both `NaN` and `NA`. For `NaN` we actually know what the value is, because it’s something insane like $$0/00/0$$. For `NA`, we believe that there is supposed to be a value “out there”, but a dog ate our homework and so we don’t quite know what it is. But for `NULL` we strongly believe that there is _no value at all_.

#### 4.6.2 Assigning names to vector elements

One thing that is sometimes a little unsatisfying about the way that R prints out a vector is that the elements come out unlabelled. Here’s what I mean. Suppose I’ve got data reporting the quarterly profits for some company. If I just create a no-frills vector, I have to rely on memory to know which element corresponds to which event. That is:

```
profit <- c( 3.1, 0.1, -1.4, 1.1 )
profit
```

```
## [1]  3.1  0.1 -1.4  1.1
```

You can probably guess that the first element corresponds to the first quarter, the second element to the second quarter, and so on, but that’s only because I’ve told you the back story and because this happens to be a very simple example. In general, it can be quite difficult. This is where it can be helpful to assign `names` to each of the elements. Here’s how you do it:

```
names(profit) <- c("Q1","Q2","Q3","Q4")
profit
```

```
##   Q1   Q2   Q3   Q4 
##  3.1  0.1 -1.4  1.1
```

This is a slightly odd looking command, admittedly, but it’s not too difficult to follow. All we’re doing is assigning a vector of labels (character strings) to `names(profit)`. You can always delete the names again by using the command `names(profit) <- NULL`. It’s also worth noting that you don’t have to do this as a two stage process. You can get the same result with this command:

```
profit <- c( "Q1" = 3.1, "Q2" = 0.1, "Q3" = -1.4, "Q4" = 1.1 )
profit
```

```
##   Q1   Q2   Q3   Q4 
##  3.1  0.1 -1.4  1.1
```

The important things to notice are that (a) this does make things much easier to read, but (b) the names at the top aren’t the “real” data. The _value_ of `profit[1]` is still `3.1`; all I’ve done is added a _name_ to `profit[1]` as well. Nevertheless, names aren’t purely cosmetic, since R allows you to pull out particular elements of the vector by referring to their names:

```
profit["Q1"]
```

```
##  Q1 
## 3.1
```

And if I ever need to pull out the names themselves, then I just type `names(profit)`.

#### 4.6.3 Variable classes

As we’ve seen, R allows you to store different kinds of data. In particular, the variables we’ve defined so far have either been character data (text), numeric data, or logical data.[57](https://learningstatisticswithr.com/book/mechanics.html#fn57) It’s important that we remember what kind of information each variable stores (and even more important that R remembers) since different kinds of variables allow you to do different things to them. For instance, if your variables have numerical information in them, then it’s okay to multiply them together:

```
x <- 5   # x is numeric
y <- 4   # y is numeric
x * y    
```

```
## [1] 20
```

But if they contain character data, multiplication makes no sense whatsoever, and R will complain if you try to do it:

```
x <- "apples"   # x is character
y <- "oranges"  # y is character
x * y           
```

```
## Error in x * y: non-numeric argument to binary operator
```

Even R is smart enough to know you can’t multiply `"apples"` by `"oranges"`. It knows this because the quote marks are indicators that the variable is supposed to be treated as text, not as a number.

This is quite useful, but notice that it means that R makes a big distinction between `5` and `"5"`. Without quote marks, R treats `5` as the number five, and will allow you to do calculations with it. With the quote marks, R treats `"5"` as the textual character five, and doesn’t recognise it as a number any more than it recognises `"p"` or `"five"` as numbers. As a consequence, there’s a big difference between typing `x <- 5` and typing `x <- "5"`. In the former, we’re storing the number `5`; in the latter, we’re storing the character `"5"`. Thus, if we try to do multiplication with the character versions, R gets stroppy:

```
x <- "5"   # x is character
y <- "4"   # y is character
x * y     
```

```
## Error in x * y: non-numeric argument to binary operator
```

Okay, let’s suppose that I’ve forgotten what kind of data I stored in the variable `x` (which happens depressingly often). R provides a function that will let us find out. Or, more precisely, it provides _three_ functions: `class()`, `mode()` and `typeof()`. Why the heck does it provide three functions, you might be wondering? Basically, because R actually keeps track of three different kinds of information about a variable:

1. The _**class**_ of a variable is a “high level” classification, and it captures psychologically (or statistically) meaningful distinctions. For instance `"2011-09-12"` and `"my birthday"` are both text strings, but there’s an important difference between the two: one of them is a date. So it would be nice if we could get R to recognise that `"2011-09-12"` is a date, and allow us to do things like add or subtract from it. The class of a variable is what R uses to keep track of things like that. Because the class of a variable is critical for determining what R can or can’t do with it, the `class()` function is very handy.
2. The _**mode**_ of a variable refers to the format of the information that the variable stores. It tells you whether R has stored text data or numeric data, for instance, which is kind of useful, but it only makes these “simple” distinctions. It can be useful to know about, but it’s not the main thing we care about. So I’m not going to use the `mode()` function very much.[58](https://learningstatisticswithr.com/book/mechanics.html#fn58)
3. The _**type**_ of a variable is a very low level classification. We won’t use it in this book, but (for those of you that care about these details) this is where you can see the distinction between integer data, double precision numeric, etc. Almost none of you actually will care about this, so I’m not even going to bother demonstrating the `typeof()` function.

For purposes, it’s the `class()` of the variable that we care most about. Later on, I’ll talk a bit about how you can convince R to “coerce” a variable to change from one class to another (Section [7.10](https://learningstatisticswithr.com/book/datahandling.html#coercion)). That’s a useful skill for real world data analysis, but it’s not something that we need right now. In the meantime, the following examples illustrate the use of the `class()` function:

```
x <- "hello world"     # x is text
class(x)
```

```
## [1] "character"
```

```
x <- TRUE     # x is logical 
class(x)
```

```
## [1] "logical"
```

```
x <- 100     # x is a number
class(x)
```

```
## [1] "numeric"
```

Exciting, no?

### 4.7 Factors

Okay, it’s time to start introducing some of the data types that are somewhat more specific to statistics. If you remember back to Chapter [2](https://learningstatisticswithr.com/book/studydesign.html#studydesign), when we assign numbers to possible outcomes, these numbers can mean quite different things depending on what kind of variable we are attempting to measure. In particular, we commonly make the distinction between _nominal_, _ordinal_, _interval_ and _ratio_ scale data. How do we capture this distinction in R? Currently, we only seem to have a single numeric data type. That’s probably not going to be enough, is it?

A little thought suggests that the numeric variable class in R is perfectly suited for capturing ratio scale data. For instance, if I were to measure response time (RT) for five different events, I could store the data in R like this:

```
RT <- c(342, 401, 590, 391, 554)
```

where the data here are measured in milliseconds, as is conventional in the psychological literature. It’s perfectly sensible to talk about “twice the response time”, $$2×RT2×RT$$, or the “response time plus 1 second”, $$RT+1000RT+1000$$, and so both of the following are perfectly reasonable things for R to do:

```
2 * RT
```

```
## [1]  684  802 1180  782 1108
```

```
RT + 1000
```

```
## [1] 1342 1401 1590 1391 1554
```

And to a lesser extent, the “numeric” class is okay for interval scale data, as long as we remember that multiplication and division aren’t terribly interesting for these sorts of variables. That is, if my IQ score is 110 and yours is 120, it’s perfectly okay to say that you’re 10 IQ points smarter than me[59](https://learningstatisticswithr.com/book/mechanics.html#fn59), but it’s not okay to say that I’m only 92% as smart as you are, because intelligence doesn’t have a natural zero.[60](https://learningstatisticswithr.com/book/mechanics.html#fn60) We might even be willing to tolerate the use of numeric variables to represent ordinal scale variables, such as those that you typically get when you ask people to rank order items (e.g., like we do in Australian elections), though as we will see R actually has a built in tool for representing ordinal data (see Section [7.11.2](https://learningstatisticswithr.com/book/datahandling.html#orderedfactors)) However, when it comes to nominal scale data, it becomes completely unacceptable, because almost all of the “usual” rules for what you’re allowed to do with numbers don’t apply to nominal scale data. It is for this reason that R has _**factors**_.
