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

This seems about right, but you might be wondering why R is displaying a Windows path using the wrong type of slash. The answer is slightly complicated and has to do with the fact that R treats `\` as a special character. To tell R not to treat `\` as a special character requires adding a skip character `\` . If you want to specify the working directory on a Windows computer, you need to use one of the following commands:

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

### Saving a workspace file&#x20;

There are two commands you can use to do this, `save()` and `save.image()`. If you’re happy to save _all_ of the variables in your workspace into the data file, then you should use `save.image()`. And if you’re happy for R to save the file into the current working directory, all you do is:

```
save.image( file = "myfile.Rdata" )
```

Since `file` is the first argument, you can shorten this to `save.image("myfile.Rdata")`. Suppose, however, you have several variables in the workspace, and you only want to save some of them:

```
who()
##   -- Name --   -- Class --   -- Size --
##   data         data.frame    3 x 2     
##   handy        character     1         
##   junk         numeric       1        
save(data, handy, file = "myfile.Rdata")
```

Finally, a second way to specify which variables the function `save()` should save is to use the `list` argument:

```
save.me <- c("data", "handy")   # the variables to be saved
save(file = "booksales2.Rdata", list = save.me)   # the command to save them
```

RStudio allows you to save the workspace pretty easily. In the environment panel, you can see the “save” button. Alternatively, go to the “Session” menu and click on the “Save Workspace As…” option. This will bring up the standard “save” dialog box for your operating system. Pretty straightforward, really.

### Special values

Most likely you’ll see these in situations where you were expecting a number, but there are quite a few other ways you can encounter them. These values are `Inf`, `NaN`, `NA` and `NULL`. These values can crop up in various different places, and it’s important to understand what they mean.

* _Infinity_ (`Inf`). The easiest of the special values to explain is `Inf` since it corresponds to a value that is infinitely large. You can also have `-Inf` :

```
> 1 / 0
[1] Inf
```

* _Not a Number_ (`NaN`). This means “there isn’t a mathematically defined number for this”. Mathematicians would say $$0/0$$ is **undefined**. R says that it’s not a number, nevertheless, it’s still treated as a “numeric” value:

```
> 0 / 0
[1] NaN
```

* _Not available_ (`NA`). `NA` indicates that the value that is “supposed” to be stored here is missing. Note the difference between `NA` and `NaN`. For `NaN`, we really do know what’s supposed to be stored; it’s just that it happens to correspond to something like $$0/0$$ that doesn’t make any sense at all. In contrast, `NA` indicates that we actually don’t know what was supposed to be there. The information is _missing_.
* _No value_ (`NULL`). The `NULL` value takes this “absence” concept even further. It basically asserts that the variable genuinely has no value whatsoever. This is quite different to both `NaN` and `NA`. For `NaN` we actually know what the value is, because it’s something insane like $$0/0$$. For `NA`, we believe that there is supposed to be a value “out there”, but a dog ate our homework and so we don’t quite know what it is. But for `NULL` we strongly believe that there is _no value at all_.

### Assigning names to vector elements

One thing about the way that R prints out a vector is that the elements come out unlabelled. Suppose I’ve got data reporting the quarterly profits for some company. That is:

```
> profit <- c( 3.1, 0.1, -1.4, 1.1 )
> profit
[1]  3.1  0.1 -1.4  1.1
```

You can probably guess that the first element corresponds to the first quarter, the second element to the second quarter, and so on. This is where it can be helpful to assign `names` to each of the elements. Here’s how you do it:

```
> names(profit) <- c("Q1","Q2","Q3","Q4")
> profit
Q1   Q2   Q3   Q4 
3.1  0.1 -1.4  1.1
```

You can always delete the names again by using `names(profit) <- NULL`. It’s also worth noting that you don’t have to do this as a two-stage process:

```
> profit <- c( "Q1" = 3.1, "Q2" = 0.1, "Q3" = -1.4, "Q4" = 1.1 )
> profit
Q1   Q2   Q3   Q4 
3.1  0.1 -1.4  1.1
```

The _value_ of `profit[1]` is still `3.1`. Nevertheless, names aren’t purely cosmetic, since R allows you to pull out particular elements of the vector by referring to their names:

```
> profit["Q1"]
Q1 
3.1
```

To pull out the names themselves, just type `names(profit)`.

### Variable classes

As we’ve seen, R allows you to store different kinds of data. In particular, the variables we’ve defined so far have either been character data (text), numeric data, or logical data. Even R is smart enough to know you can’t multiply `"apples"` by `"oranges"`. It knows this because the quote marks are indicators that the variable is supposed to be treated as text, not as a number.

This is quite useful, but notice that it means that R makes a big distinction between `5` and `"5"`:

```
x <- "5"   # x is character
y <- "4"   # y is character
x * y
## Error in x * y: non-numeric argument to binary operator     
```

Okay, let’s suppose that I’ve forgotten what kind of data we stored in the variable `x` (which happens depressingly often). R provides a function that will let us find out. Or, more precisely, it provides _three_ functions: `class()`, `mode()` and `typeof()`. Why the heck does it provide three functions, you might be wondering? Basically, because R actually keeps track of three different kinds of information about a variable. The **class** of a variable is a “high level” classification, and it captures psychologically (or statistically) meaningful distinctions. For instance `"2011-09-12"` and `"my birthday"` are both text strings, but there’s an important difference between the two: one of them is a date:

```
> x <- "hello world"
class(x)
[1] "character"
x <- TRUE
class(x)
[1] "logical"
x <- 100
class(x)
[1] "numeric"
```

Exciting, no?



### Data frames

It’s now time to go back and deal with the somewhat confusing thing that happened in Section [**??**](https://learningstatisticswithr.com/book/mechanics.html#loadingcsv) when we tried to open up a CSV file. Apparently we succeeded in loading the data, but it came to us in a very odd looking format. At the time, I told you that this was a _**data frame**_. Now I’d better explain what that means.

#### 4.8.1 Introducing data frames

In order to understand why R has created this funny thing called a data frame, it helps to try to see what problem it solves. So let’s go back to the little scenario that I used when introducing factors in Section [4.7](https://learningstatisticswithr.com/book/mechanics.html#factors). In that section I recorded the `group` and `gender` for all 9 participants in my study. Let’s also suppose I recorded their ages and their `score` on “Dan’s Terribly Exciting Psychological Test”:

```
age <- c(17, 19, 21, 37, 18, 19, 47, 18, 19)
score <- c(12, 10, 11, 15, 16, 14, 25, 21, 29)
```

Assuming no other variables are in the workspace, if I type `who()` I get this:

```
who()
```

So there are four variables in the workspace, `age`, `gender`, `group` and `score`. And it just so happens that all four of them are the same size (i.e., they’re all vectors with 9 elements). Aaaand it just so happens that `age[1]` corresponds to the age of the first person, and `gender[1]` is the gender of that very same person, etc. In other words, you and I both know that all four of these variables correspond to the _same_ data set, and all four of them are organised in exactly the same way.

However, R _doesn’t_ know this! As far as it’s concerned, there’s no reason why the `age` variable has to be the same length as the `gender` variable; and there’s no particular reason to think that `age[1]` has any special relationship to `gender[1]` any more than it has a special relationship to `gender[4]`. In other words, when we store everything in separate variables like this, R doesn’t know anything about the relationships between things. It doesn’t even really know that these variables actually refer to a proper data set. The data frame fixes this: if we store our variables inside a data frame, we’re telling R to treat these variables as a single, fairly coherent data set.

To see how they do this, let’s create one. So how do we create a data frame? One way we’ve already seen: if we import our data from a CSV file, R will store it as a data frame. A second way is to create it directly from some existing variables using the `data.frame()` function. All you have to do is type a list of variables that you want to include in the data frame. The output of a `data.frame()` command is, well, a data frame. So, if I want to store all four variables from my experiment in a data frame called `expt` I can do so like this:

```
expt <- data.frame ( age, gender, group, score ) 
expt 
```

```
##   age gender   group score
## 1  17   male group 1    12
## 2  19   male group 1    10
## 3  21   male group 1    11
## 4  37   male group 2    15
## 5  18   male group 2    16
## 6  19 female group 2    14
## 7  47 female group 3    25
## 8  18 female group 3    21
## 9  19 female group 3    29
```

Note that `expt` is a completely self-contained variable. Once you’ve created it, it no longer depends on the original variables from which it was constructed. That is, if we make changes to the original `age` variable, it will _not_ lead to any changes to the age data stored in `expt`.



#### Pulling out the contents of the data frame using `$`

At this point, our workspace contains only the one variable, a data frame called `expt`. But as we can see when we told R to print the variable out, this data frame contains 4 variables, each of which has 9 observations. So how do we get this information out again? After all, there’s no point in storing information if you don’t use it, and there’s no way to use information if you can’t access it. So let’s talk a bit about how to pull information out of a data frame.

The first thing we might want to do is pull out one of our stored variables, let’s say `score`. One thing you might try to do is ignore the fact that `score` is locked up inside the `expt` data frame. For instance, you might try to print it out like this:

```
score
```

```
## Error in eval(expr, envir, enclos): object 'score' not found
```

This doesn’t work, because R doesn’t go “peeking” inside the data frame unless you explicitly tell it to do so. There’s actually a very good reason for this, which I’ll explain in a moment, but for now let’s just assume R knows what it’s doing. How do we tell R to look inside the data frame? As is always the case with R there are several ways. The simplest way is to use the `$` operator to extract the variable you’re interested in, like this:

```
expt$score
```

```
## [1] 12 10 11 15 16 14 25 21 29
```

#### 4.8.3 Getting information about a data frame

One problem that sometimes comes up in practice is that you forget what you called all your variables. Normally you might try to type `objects()` or `who()`, but neither of those commands will tell you what the names are for those variables inside a data frame! One way is to ask R to tell you what the _names_ of all the variables stored in the data frame are, which you can do using the `names()` function:

```
names(expt)
```

```
## [1] "age"    "gender" "group"  "score"
```

An alternative method is to use the `who()` function, as long as you tell it to look at the variables inside data frames. If you set `expand = TRUE` then it will not only list the variables in the workspace, but it will “expand” any data frames that you’ve got in the workspace, so that you can see what they look like. That is:

```
who(expand = TRUE)
```

or, since `expand` is the first argument in the `who()` function you can just type `who(TRUE)`. I’ll do that a lot in this book.

#### 4.8.4 Looking for more on data frames?

There’s a lot more that can be said about data frames: they’re fairly complicated beasts, and the longer you use R the more important it is to make sure you really understand them. We’ll talk a lot more about them in Chapter [7](https://learningstatisticswithr.com/book/datahandling.html#datahandling).

### 4.9 Lists

The next kind of data I want to mention are _**lists**_. Lists are an extremely fundamental data structure in R, and as you start making the transition from a novice to a savvy R user you will use lists all the time. I don’t use lists very often in this book – not directly – but most of the advanced data structures in R are built from lists (e.g., data frames are actually a specific type of list). Because lists are so important to how R stores things, it’s useful to have a basic understanding of them. Okay, so what is a list, exactly? Like data frames, lists are just “collections of variables.” However, unlike data frames – which are basically supposed to look like a nice “rectangular” table of data – there are no constraints on what kinds of variables we include, and no requirement that the variables have any particular relationship to one another. In order to understand what this actually _means_, the best thing to do is create a list, which we can do using the `list()` function. If I type this as my command:

```
Dan <- list( age = 34,
            nerd = TRUE,
            parents = c("Joe","Liz") 
)
```

R creates a new list variable called `Dan`, which is a bundle of three different variables: `age`, `nerd` and `parents`. Notice, that the `parents` variable is longer than the others. This is perfectly acceptable for a list, but it wouldn’t be for a data frame. If we now print out the variable, you can see the way that R stores the list:

```
print( Dan )
```

```
## $age
## [1] 34
## 
## $nerd
## [1] TRUE
## 
## $parents
## [1] "Joe" "Liz"
```

As you might have guessed from those `$` symbols everywhere, the variables are stored in exactly the same way that they are for a data frame (again, this is not surprising: data frames _are_ a type of list). So you will (I hope) be entirely unsurprised and probably quite bored when I tell you that you can extract the variables from the list using the `$` operator, like so:

```
Dan$nerd
```

```
## [1] TRUE
```

If you need to add new entries to the list, the easiest way to do so is to again use `$`, as the following example illustrates. If I type a command like this

```
Dan$children <- "Alex"
```

then R creates a new entry to the end of the list called `children`, and assigns it a value of `"Alex"`. If I were now to `print()` this list out, you’d see a new entry at the bottom of the printout. Finally, it’s actually possible for lists to contain other lists, so it’s quite possible that I would end up using a command like `Dan$children$age` to find out how old my son is. Or I could try to remember it myself I suppose.

### 4.10 Formulas

The last kind of variable that I want to introduce before finally being able to start talking about statistics is the _**formula**_. Formulas were originally introduced into R as a convenient way to specify a particular type of statistical model (see Chapter [15](https://learningstatisticswithr.com/book/regression.html#regression)) but they’re such handy things that they’ve spread. Formulas are now used in a lot of different contexts, so it makes sense to introduce them early.

Stated simply, a formula object is a variable, but it’s a special type of variable that specifies a relationship between other variables. A formula is specified using the “tilde operator” `~`. A very simple example of a formula is shown below:[62](https://learningstatisticswithr.com/book/mechanics.html#fn62)

```
formula1 <- out ~ pred
formula1
```

```
## out ~ pred
```

The _precise_ meaning of this formula depends on exactly what you want to do with it, but in broad terms it means “the `out` (outcome) variable, analysed in terms of the `pred` (predictor) variable”. That said, although the simplest and most common form of a formula uses the “one variable on the left, one variable on the right” format, there are others. For instance, the following examples are all reasonably common

```
formula2 <-  out ~ pred1 + pred2   # more than one variable on the right
formula3 <-  out ~ pred1 * pred2   # different relationship between predictors 
formula4 <-  ~ var1 + var2         # a 'one-sided' formula
```

and there are many more variants besides. Formulas are pretty flexible things, and so different functions will make use of different formats, depending on what the function is intended to do.

### 4.11 Generic functions

There’s one really important thing that I omitted when I discussed functions earlier on in Section [3.5](https://learningstatisticswithr.com/book/introR.html#usingfunctions), and that’s the concept of a _**generic function**_. The two most notable examples that you’ll see in the next few chapters are `summary()` and `plot()`, although you’ve already seen an example of one working behind the scenes, and that’s the `print()` function. The thing that makes generics different from the other functions is that their behaviour changes, often quite dramatically, depending on the `class()` of the input you give it. The easiest way to explain the concept is with an example. With that in mind, lets take a closer look at what the `print()` function actually does. I’ll do this by creating a formula, and printing it out in a few different ways. First, let’s stick with what we know:

```
my.formula <- blah ~ blah.blah    # create a variable of class "formula"
print( my.formula )               # print it out using the generic print() function
```

```
## blah ~ blah.blah
```

So far, there’s nothing very surprising here. But there’s actually a lot going on behind the scenes here. When I type `print( my.formula )`, what actually happens is the `print()` function checks the class of the `my.formula` variable. When the function discovers that the variable it’s been given is a formula, it goes looking for a function called `print.formula()`, and then delegates the whole business of printing out the variable to the `print.formula()` function.[63](https://learningstatisticswithr.com/book/mechanics.html#fn63) For what it’s worth, the name for a “dedicated” function like `print.formula()` that exists only to be a special case of a generic function like `print()` is a _**method**_, and the name for the process in which the generic function passes off all the hard work onto a method is called _**method dispatch**_. You won’t need to understand the details at all for this book, but you do need to know the gist of it; if only because a lot of the functions we’ll use are actually generics. Anyway, to help expose a little more of the workings to you, let’s bypass the `print()` function entirely and call the formula method directly:

```
print.formula( my.formula )       # print it out using the print.formula() method

## Appears to be deprecated
```

There’s no difference in the output at all. But this shouldn’t surprise you because it was actually the `print.formula()` method that was doing all the hard work in the first place. The `print()` function itself is a lazy bastard that doesn’t do anything other than select which of the methods is going to do the actual printing.

Okay, fair enough, but you might be wondering what would have happened if `print.formula()` didn’t exist? That is, what happens if there isn’t a specific method defined for the class of variable that you’re using? In that case, the generic function passes off the hard work to a “default” method, whose name in this case would be `print.default()`. Let’s see what happens if we bypass the `print()` formula, and try to print out `my.formula` using the `print.default()` function:

```
print.default( my.formula )      # print it out using the print.default() method
```

```
## blah ~ blah.blah
## attr(,"class")
## [1] "formula"
## attr(,".Environment")
## <environment: R_GlobalEnv>
```

Hm. You can kind of see that it is trying to print out the same formula, but there’s a bunch of ugly low-level details that have also turned up on screen. This is because the `print.default()` method doesn’t know anything about formulas, and doesn’t know that it’s supposed to be hiding the obnoxious internal gibberish that R produces sometimes.

At this stage, this is about as much as we need to know about generic functions and their methods. In fact, you can get through the entire book without learning any more about them than this, so it’s probably a good idea to end this discussion here.

### 4.12 Getting help

The very last topic I want to mention in this chapter is where to go to find help. Obviously, I’ve tried to make this book as helpful as possible, but it’s not even close to being a comprehensive guide, and there’s thousands of things it doesn’t cover. So where should you go for help?

#### 4.12.1 How to read the help documentation

I have somewhat mixed feelings about the help documentation in R. On the plus side, there’s a lot of it, and it’s very thorough. On the minus side, there’s a lot of it, and it’s very thorough. There’s so much help documentation that it sometimes doesn’t help, and most of it is written with an advanced user in mind. Often it feels like most of the help ﬁles work on the assumption that the reader already understands everything about R except for the speciﬁc topic that it’s providing help for. What that means is that, once you’ve been using R for a long time and are beginning to get a feel for how to use it, the help documentation is awesome. These days, I ﬁnd myself really liking the help ﬁles (most of them anyway). But when I ﬁrst started using R I found it very dense.

To some extent, there’s not much I can do to help you with this. You just have to work at it yourself; once you’re moving away from being a pure beginner and are becoming a skilled user, you’ll start ﬁnding the help documentation more and more helpful. In the meantime, I’ll help as much as I can by trying to explain to you what you’re looking at when you open a help ﬁle. To that end, let’s look at the help documentation for the `load()` function. To do so, I type either of the following:

```
?load 
help("load")
```

When I do that, R goes looking for the help ﬁle for the “load” topic. If it ﬁnds one, Rstudio takes it and displays it in the help panel. Alternatively, you can try a fuzzy search for a help topic

```
??load 
help.search("load")
```

This will bring up a list of possible topics that you might want to follow up in. Regardless, at some point you’ll ﬁnd yourself looking at an actual help ﬁle. And when you do, you’ll see there’s a quite a lot of stuﬀ written down there, and it comes in a pretty standardised format. So let’s go through it slowly, using the “`load`” topic as our example. Firstly, at the very top we see this:

**Reload Saved Datasets**

**Description**

Reload datasets written with the function `save`.

Fairly straightforward. The next section describes how the function is used:

**Usage**

```
load(file, envir = parent.frame(), verbose = FALSE)
```

In this instance, the usage section is actually pretty readable. It’s telling you that there are two arguments to the `load()` function: the ﬁrst one is called `file`, and the second one is called `envir`. It’s also telling you that there is a default value for the envir argument; so if the user doesn’t specify what the value of envir should be, then R will assume that `envir = parent.frame()`. In contrast, the file argument has no default value at all, so the user must specify a value for it. So in one sense, this section is very straightforward.

The problem, of course, is that you don’t know what the `parent.frame()` function actually does, so it’s hard for you to know what the `envir = parent.frame()` bit is all about. What you could do is then go look up the help documents for the `parent.frame()` function (and sometimes that’s actually a good idea), but often you’ll ﬁnd that the help documents for those functions are just as dense (if not more dense) than the help ﬁle that you’re currently reading. As an alternative, my general approach when faced with something like this is to skim over it, see if I can make any sense of it. If so, great. If not, I ﬁnd that the best thing to do is ignore it. In fact, the ﬁrst time I read the help ﬁle for the load() function, I had no idea what any of the `envir` related stuﬀ was about. But fortunately I didn’t have to: the default setting here (i.e., `envir = parent.frame()`) is actually the thing you want in about 99% of cases, so it’s safe to ignore it.

Basically, what I’m trying to say is: don’t let the scary, incomprehensible parts of the help ﬁle intimidate you. Especially because there’s often some parts of the help ﬁle that will make sense. Of course, I guarantee you that sometimes this strategy will lead you to make mistakes… often embarrassing mistakes. But it’s still better than getting paralysed with fear.

So, let’s continue on. The next part of the help documentation discusses each of the arguments, and what they’re supposed to do:

**Arguments**

| `file`    | a (readable binary-mode) [connection](https://learningstatisticswithr.com/base/help/connection) or a character string giving the name of the file to load (when [tilde expansion](https://learningstatisticswithr.com/base/help/tilde%20expansion) is done). |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `envir`   | the environment where the data should be loaded.                                                                                                                                                                                                             |
| `verbose` | should item names be printed during loading?                                                                                                                                                                                                                 |

Okay, so what this is telling us is that the `file` argument needs to be a string (i.e., text data) which tells R the name of the ﬁle to load. It also seems to be hinting that there’s other possibilities too (e.g., a “binary mode connection”), and you probably aren’t quite sure what “tilde expansion” means[64](https://learningstatisticswithr.com/book/mechanics.html#fn64). But overall, the meaning is pretty clear.

Turning to the `envir` argument, it’s now a little clearer what the Usage section was babbling about. The `envir` argument speciﬁes the name of an environment (see Section 4.3 if you’ve forgotten what environments are) into which R should place the variables when it loads the ﬁle. Almost always, this is a no-brainer: you want R to load the data into the same damn environment in which you’re invoking the `load()` command. That is, if you’re typing `load()` at the R prompt, then you want the data to be loaded into your workspace (i.e., the global environment). But if you’re writing your own function that needs to load some data, you want the data to be loaded inside that function’s private workspace. And in fact, that’s exactly what the `parent.frame()` thing is all about. It’s telling the `load()` function to send the data to the same place that the `load()` command itself was coming from. As it turns out, if we’d just ignored the envir bit we would have been totally safe. Which is nice to know.

Moving on, next up we get a detailed description of what the function actually does:

**Details**

`load` can load **R** objects saved in the current or any earlier format. It can read a compressed file (see [`save`](https://learningstatisticswithr.com/base/help/save)) directly from a file or from a suitable connection (including a call to [`url`](https://learningstatisticswithr.com/base/help/url)).

A not-open connection will be opened in mode `“rb”` and closed after use. Any connection other than a [`gzfile`](https://learningstatisticswithr.com/base/help/gzfile) or [`gzcon`](https://learningstatisticswithr.com/base/help/gzcon) connection will be wrapped in [`gzcon`](https://learningstatisticswithr.com/base/help/gzcon) to allow compressed saves to be handled: note that this leaves the connection in an altered state (in particular, binary-only), and that it needs to be closed explicitly (it will not be garbage-collected).

Only **R** objects saved in the current format (used since **R** 1.4.0) can be read from a connection. If no input is available on a connection a warning will be given, but any input not in the current format will result in a error.

Loading from an earlier version will give a warning about the ‘magic number’: magic numbers `1971:1977` are from **R** < 0.99.0, and `RD[ABX]1` from **R** 0.99.0 to **R** 1.3.1. These are all obsolete, and you are strongly recommended to re-save such files in a current format.

The `verbose` argument is mainly intended for debugging. If it is `TRUE`, then as objects from the file are loaded, their names will be printed to the console. If `verbose` is set to an integer value greater than one, additional names corresponding to attributes and other parts of individual objects will also be printed. Larger values will print names to a greater depth.

Objects can be saved with references to namespaces, usually as part of the environment of a function or formula. Such objects can be loaded even if the namespace is not available: it is replaced by a reference to the global environment with a warning. The warning identifies the first object with such a reference (but there may be more than one).

Then it tells you what the output value of the function is:

**Value**

A character vector of the names of objects created, invisibly.

This is usually a bit more interesting, but since the `load()` function is mainly used to load variables into the workspace rather than to return a value, it’s no surprise that this doesn’t do much or say much. Moving on, we sometimes see a few additional sections in the help ﬁle, which can be diﬀerent depending on what the function is:

**Warning**

Saved **R** objects are binary files, even those saved with `ascii = TRUE`, so ensure that they are transferred without conversion of end of line markers. `load` tries to detect such a conversion and gives an informative error message.

`load(<file>)` replaces all existing objects with the same names in the current environment (typically your workspace, [`.GlobalEnv`](https://learningstatisticswithr.com/base/help/.GlobalEnv)) and hence potentially overwrites important data. It is considerably safer to use `envir =` to load into a different environment, or to [`attach`](https://learningstatisticswithr.com/base/help/attach)`(file)` which `load()`s into a new entry in the [`search`](https://learningstatisticswithr.com/base/help/search) path.

**Note**

`file` can be a UTF-8-encoded filepath that cannot be translated to the current locale.

Yeah, yeah. Warning, warning, blah blah blah. Towards the bottom of the help ﬁle, we see something like this, which suggests a bunch of related topics that you might want to look at. These can be quite helpful:

**See Also**

[`save`](https://learningstatisticswithr.com/base/help/save), [`download.file`](https://learningstatisticswithr.com/base/help/download.file); further [`attach`](https://learningstatisticswithr.com/base/help/attach) as wrapper for `load()`.

For other interfaces to the underlying serialization format, see [`unserialize`](https://learningstatisticswithr.com/base/help/unserialize) and [`readRDS`](https://learningstatisticswithr.com/base/help/readRDS).

Finally, it gives you some examples of how to use the function(s) that the help ﬁle describes. These are supposed to be proper R commands, meaning that you should be able to type them into the console yourself and they’ll actually work. Sometimes it can be quite helpful to try the examples yourself. Anyway, here they are for the “`load`” help ﬁle:

**Examples**

```
## save all data
xx <- pi # to ensure there is some data
save(list = ls(all = TRUE), file= "all.rda")
rm(xx)

## restore the saved values to the current environment
local({
   load("all.rda")
   ls()
})

xx <- exp(1:3)
## restore the saved values to the user's workspace
load("all.rda") ## which is here *equivalent* to
## load("all.rda", .GlobalEnv)
## This however annihilates all objects in .GlobalEnv with the same names !
xx # no longer exp(1:3)
rm(xx)
attach("all.rda") # safer and will warn about masked objects w/ same name in .GlobalEnv
ls(pos = 2)
##  also typically need to cleanup the search path:
detach("file:all.rda")

## clean up (the example):
unlink("all.rda")


## Not run: 
con <- url("http://some.where.net/R/data/example.rda")
## print the value to see what objects were created.
print(load(con))
close(con) # url() always opens the connection

## End(Not run)
```

As you can see, they’re pretty dense, and not at all obvious to the novice user. However, they do provide good examples of the various diﬀerent things that you can do with the `load()` function, so it’s not a bad idea to have a look at them, and to try not to ﬁnd them too intimidating.

#### 4.12.2 Other resources

* The Rseek website (www.rseek.org). One thing that I really find annoying about the R help documentation is that it’s hard to search properly. When coupled with the fact that the documentation is dense and highly technical, it’s often a better idea to search or ask online for answers to your questions. With that in mind, the Rseek website is great: it’s an R specific search engine. I find it really useful, and it’s almost always my first port of call when I’m looking around.
* The R-help mailing list (see [http://www.r-project.org/mail.html](http://www.r-project.org/mail.html) for details). This is the official R help mailing list. It can be very helpful, but it’s _very_ important that you do your homework before posting a question. The list gets a lot of traffic. While the people on the list try as hard as they can to answer questions, they do so for free, and you _really_ don’t want to know how much money they could charge on an hourly rate if they wanted to apply market rates. In short, they are doing you a favour, so be polite. Don’t waste their time asking questions that can be easily answered by a quick search on Rseek (it’s rude), make sure your question is clear, and all of the relevant information is included. In short, read the posting guidelines carefully ([http://www.r-project.org/posting-guide.html](http://www.r-project.org/posting-guide.html)), and make use of the `help.request()` function that R provides to check that you’re actually doing what you’re expected.

### 4.13 Summary

This chapter continued where Chapter [3](https://learningstatisticswithr.com/book/introR.html#introR) left off. The focus was still primarily on introducing basic R concepts, but this time at least you can see how those concepts are related to data analysis:

* [Installing, loading and updating packages](https://learningstatisticswithr.com/book/mechanics.html#packageinstall). Knowing how to extend the functionality of R by installing and using packages is critical to becoming an effective R user
* Getting around. Section [4.3](https://learningstatisticswithr.com/book/mechanics.html#workspace) talked about how to manage your workspace and how to keep it tidy. Similarly, Section [4.4](https://learningstatisticswithr.com/book/mechanics.html#navigation) talked about how to get R to interact with the rest of the file system.
* [Loading and saving data](https://learningstatisticswithr.com/book/mechanics.html#load). Finally, we encountered actual data files. Loading and saving data is obviously a crucial skill, one we discussed in Section [4.5](https://learningstatisticswithr.com/book/mechanics.html#load).
* [Useful things to know about variables](https://learningstatisticswithr.com/book/mechanics.html#useful). In particular, we talked about special values, element names and classes.
* More complex types of variables. R has a number of important variable types that will be useful when analysing real data. I talked about factors in Section [4.7](https://learningstatisticswithr.com/book/mechanics.html#factors), data frames in Section [4.8](https://learningstatisticswithr.com/book/mechanics.html#dataframes), lists in Section [4.9](https://learningstatisticswithr.com/book/mechanics.html#lists) and formulas in Section [4.10](https://learningstatisticswithr.com/book/mechanics.html#formulas).
* [Generic functions](https://learningstatisticswithr.com/book/mechanics.html#generics). How is it that some function seem to be able to do lots of different things? Section [4.11](https://learningstatisticswithr.com/book/mechanics.html#generics) tells you how.
* [Getting help](https://learningstatisticswithr.com/book/mechanics.html#help). Assuming that you’re not looking for counselling, Section [4.12](https://learningstatisticswithr.com/book/mechanics.html#help) covers several possibilities. If you are looking for counselling, well, this book really can’t help you there. Sorry.

Taken together, Chapters [3](https://learningstatisticswithr.com/book/introR.html#introR) and [4](https://learningstatisticswithr.com/book/mechanics.html#mechanics) provide enough of a background that you can finally get started doing some statistics! Yes, there’s a lot more R concepts that you ought to know (and we’ll talk about some of them in Chapters[7](https://learningstatisticswithr.com/book/datahandling.html#datahandling) and[8](https://learningstatisticswithr.com/book/scripting.html#scripting)), but I think that we’ve talked quite enough about programming for the moment. It’s time to see how your experience with programming can be used to do some data analysis…
