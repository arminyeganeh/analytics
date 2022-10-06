---
description: Reading 2, 450-600 lines, 3 hours to complete
---

# Managing Files in R

## Managing Files in R

#### Instructions

Read this tutorial and apply the codes in R. No submission is required.

#### Windows paths

Let’s suppose I’m on Windows. As before, I can find out what my current working directory is like this:

```r
getwd()
## [1] "C:/Users/dan/
```

This seems about right, but you might be wondering why R is displaying a Windows path using the wrong type of slash. The answer is slightly complicated and has to do with the fact that R treats `\` as a special character. To tell R not to treat `\` as a special character requires adding a skip character `\` . If you want to specify the working directory on a Windows computer, you need to use one of the following commands:

```r
setwd( "C:/Users/dan" )
setwd( "C:\\Users\\dan" )
```

The Files panel shown in **Figure HA2.1** is a decent file browser, which can be used to set the working directory and even delete, rename, and load files.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/filepanel.png" alt=""><figcaption><p><strong>Figure HA2.1</strong> RStudio's Files panel</p></figcaption></figure>

At the top of the file panel you see some text that says “Home > Rbook > data”, which means the list below shows files are stored in that directory. To change the R working directory using the file panel click on the button “More”. This will bring up a little menu, and one of the options will be “Set as Working Directory”. If you select that option, then R really will change the working directory. You can tell that it has done so because a command like this appears in the console:

```r
setwd("~/Rbook/data")
```

#### Loading and saving data

There are several different types of files that are likely to be relevant to us when doing data analysis. There are three in particular that are especially important from the perspective of this book:

* _Workspace files_ are those with a **.Rdata file extension**. This is the standard kind of file that R uses to store data and variables. They’re called “workspace files” because you can use them to save your whole workspace.
* _Comma separated value (CSV) files_ are those with a **.csv file extension**. These are just regular old text files, and they can be opened with almost any software. It’s quite typical for people to store data in CSV files, precisely because they’re so simple.
* _Script files_ are those with a **.R file extension**. These aren’t data files at all; rather, they’re used to save a collection of commands that you want R to execute later. They’re just text files, but we won’t make use of them now.

There are also several other kinds of data file that you might want to import into R. For instance, you might want to open Microsoft Excel spreadsheets (.xlsx files), or data files that have been saved in the native file formats for other statistics software, such as SPSS, SAS, Minitab, Stata or Systat. Finally, you might have to handle databases.

#### Loading workspace files

When we used the `list.files()` command to list the contents of the `/Users/dan/Rbook/data` directory, the output referred to a file called booksales.Rdata. Let’s say we want to load the data from this file into the workspace. The way we do this is with the `load()` function. There are two arguments to this function, but the only one we’re interested in is

* `file`. This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.

Using the absolute file path, the command would look like this:

```r
load( file = "/Users/dan/Rbook/data/booksales.Rdata" )
```

but this is pretty lengthy. Given that the working directory is `/Users/dan/Rbook/data`, we could use a relative file path, like so:

```r
load(file = "../data/booksales.Rdata")
```

However, the preference is usually to change the working directory first and then load the file:

```r
setwd("../data")         # move to the data directory
load("booksales.Rdata")  # load the data
```

Okay, so how do we open an .Rdata file using the RStudio file panel? All we have to do is to click on the file name. RStudio brings up a little dialog box asking to confirm that we do want to load this file. The following command then turns up in the console:

```r
load("~/Rbook/data/booksales.Rdata")
```

The workspace environment will also list your user-defined objects such as vectors, matrices, data frames, lists, and functions. To identify or remove the objects (i.e. vectors, data frames, user-defined functions, etc.) in your current R environment:

```r
# list all objects
ls()

# identify if an R object with a given name is present
exists("object_name")

# remove defined object from the environment
rm ("object_name")

# you can remove multiple objects by using the c() function
rm(c("object1", "object2"))

# basically removes everything in the working environment -- use with caution!
rm(list = ls() )
```

#### Importing data from CSV files

One quite commonly used data format is the humble “comma separated value” file, also called a CSV file, and usually bearing the file extension .csv. CSV files are just plain old-fashioned text files, and what they store is basically just a table of data, illustrated in **Figure HA2.2**. As you can see, each row corresponds to a variable, and each row represents the book sales data for one month. The first row doesn’t contain actual data though: it has the names of the variables. On the left, we have opened the file using a spreadsheet program, which shows that the file is basically a table. On the right, the same file is open in a standard text editor, which shows how the file is formatted. The entries in the table are wrapped in quote marks and separated by commas.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/booksalescsv.jpg" alt=""><figcaption><p><strong>Figure HA2.2</strong> A CSV file</p></figcaption></figure>

If RStudio were not available to you, the easiest way to open this file would be to use the `read.csv()` function. For now there, are two arguments to the function that we will mention:

* `file` This should be a character string that specifies a path to the file that needs to be loaded. You can use an absolute path or a relative path to do so.
* `header` This is a logical value indicating whether or not the first row of the file contains variable names. The default value is `TRUE`.

Therefore, to import the CSV file, the command we need is:

```r
books <- read.csv(file = "booksales.csv")
```

There are two very important points to notice here. Firstly, notice that we didn’t try to use the `load()` function, because that function is only meant to be used for .Rdata files. If you try to use `load()` on other types of data, you get an error. Secondly, notice that when we imported the CSV file we assigned the result to a variable, which we imaginatively called `books` . There’s a reason for this. The idea behind the file `.Rdata` is that it stores a whole workspace. So, if you had the ability to look inside the file yourself you’d see that the data file keeps track of all the variables and their names. So when you `load()` the file, R restores all those original names. CSV files are treated differently: as far as R is concerned, the CSV only stores one variable, but that variable is a big table. So when you import that table into the workspace, R expects you to give it a name. Let’s have a look at what we’ve got:

```r
print(books)
```

```r
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

#### Importing data from CSV files

Yet again, it’s easier in RStudio. In the environment panel in RStudio you should see a button called “Import Dataset”. Click on that, and it will give you a couple of options: select the “From Text File…” option, and it will open up a very familiar dialog box asking you to select a file: if you’re on a Mac, it’ll look like the usual Finder window that you use to choose a file; on Windows it looks like an Explorer window. Assuming that you’re familiar with your own computer, so should have no problem finding the CSV file that you want to import! Find the one you want, then click on the “Open” button. When you do this, you’ll see a window that looks like the one in **Figure HA2.3**.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/import.png" alt=""><figcaption><p><strong>Figure HA2.3</strong> The RStudio window for importing a CSV</p></figcaption></figure>

In the top left corner, you need to type the name of the variable you R to create. By default, that will be the same as the file name: our file is called `booksales.csv`, so RStudio suggests the name `booksales`. Immediately below this are a few things that you can tweak to make sure that the data gets imported correctly:

* Heading. Does the first row of the file contain raw data, or does it contain headings for each variable? The `booksales.csv` file has a header at the top, so we select “yes”.
* Separator. What character is used to separate different entries? In most CSV files this will be a comma (it is “comma separated” after all). But you can change this if your file is different.
* Decimal. What character is used to specify the decimal point? In English speaking countries, this is almost always a period (i.e., `.`). That’s not universally true: many European countries use a comma. So you can change that if you need to.
* Quote. What character is used to denote a block of text? That’s usually going to be a double quote mark. It is for the `booksales.csv` file, so that’s what we select.

Once you’re happy, click “Import”. When you do, two commands appear in the R console:

```r
booksales <- read.csv("~/Rbook/data/booksales.csv")
View(booksales)
```

The first of these commands is the one that loads the data. The second one will display a pretty table showing the data in RStudio.

`read.table()` is a multipurpose work-horse function in base R for importing data. The functions `read.csv()` and `read.delim()` are special cases of `read.table()` in which the defaults have been adjusted for efficiency. Compared to the equivalent base functions, functions in the package `readr` are around 10× faster. `read_csv()` maintains the full variable name (whereas read.csv eliminates any spaces in variable names and fills it with ‘.’).

#### Importing data from Excel files

With Excel still being the spreadsheet software of choice, it is important to be able to efficiently import and export data from these files. Often, R users will simply resort to exporting the Excel file as a CSV file and then import into R using `read.csv`; however, this is far from efficient. To import data directly from Excel you can use the package readxl. The available arguments allow you to change the data as you import it. Some examples are provided:

```r
library (readxl)
mydata <- read_excel ("mydata.xlsx", sheet = "Sheet5")

# change variable names by skipping the first row
# and using col_names to set the new names
read_excel ("mydata.xlsx", sheet = "Sheet5", skip = 1,
            col_names = paste ("Var", 1:5))
            
# sometimes missing values are set as a sentinel value
# rather than just left blank - (i.e. "999")
# we can change these to missing values with na argument
read_excel ("mydata.xlsx", sheet = "Sheet6", na = "999")
```

#### Importing tabular and Excel files stored online

A quick perusal of Data.gov illustrates nearly 188,510 examples. In fact, we can provide our first example of importing online tabular data by downloading the Data.gov CSV file that lists all the federal agencies that supply data to Data.gov.

```r
# the url for the online CSV
url <- "Enter the url for the online CSV here"

# use read.csv to import
data_gov <- read.csv (url)
```

#### Saving a workspace file

There are two commands you can use to do this, `save()` and `save.image()`. If you’re happy to save _all_ of the variables in your workspace into the data file, then you should use `save.image()`. And if you’re happy for R to save the file into the current working directory, all you do is:

```r
save.image( file = "myfile.Rdata" )
```

Since `file` is the first argument, you can shorten this to `save.image("myfile.Rdata")`. Suppose, however, you have several variables in the workspace, and you only want to save some of them:

```r
who()
##   -- Name --   -- Class --   -- Size --
##   data         data.frame    3 x 2     
##   handy        character     1         
##   junk         numeric       1        
save(data, handy, file = "myfile.Rdata")
```

Finally, a second way to specify which variables the function `save()` should save is to use the `list` argument:

```r
save.me <- c("data", "handy")   # the variables to be saved
save(file = "booksales2.Rdata", list = save.me)   # the command to save them
```

RStudio allows you to save the workspace pretty easily. In the environment panel, you can see the “save” button. Alternatively, go to the “Session” menu and click on the “Save Workspace As…” option. This will bring up the standard “save” dialog box for your operating system. Pretty straightforward, really.

#### Exporting data

Although getting data into R is essential, getting data out of R can be just as important. This section will cover how to export data to text files and Excel files and save R data objects.

To export the data table `df` to a CSV file we can use `write.csv()`. Additional arguments allow you to exclude row and column names, specify what to use for missing values, add or remove quotations around character strings, etc.

```r
# write to a csv file
write.csv (df, file = "export_csv")

# write to a csv and save in a different directory
write.csv (df, file = "/folder/subfolder/subsubfolder/export_csv")

# write to a csv file with added arguments
write.csv (df, file = "export_csv", row.names = FALSE, na = "MISSING!")

# write to a tab delimited text files
write.delim (df, file = "export_txt")

# provides same results as read.delim
write.table (df, file = "export_txt", sep="\t")
```

#### Special values

Most likely you’ll see these in situations where you were expecting a number, but there are quite a few other ways you can encounter them. These values are `Inf`, `NaN`, `NA` and `NULL`. These values can crop up in various different places, and it’s important to understand what they mean.

* _Infinity_ (`Inf`). The easiest of the special values to explain is `Inf` since it corresponds to a value that is infinitely large. You can also have `-Inf` :

```r
1 / 0
## [1] Inf
```

* _Not a Number_ (`NaN`). This means “there isn’t a mathematically defined number for this”. Mathematicians would say $$0/0$$ is **undefined**. R says that it’s not a number, nevertheless, it’s still treated as a “numeric” value:

```r
0 / 0
## [1] NaN
```

* _Not available_ (`NA`). `NA` indicates that the value that is “supposed” to be stored here is missing. Note the difference between `NA` and `NaN`. For `NaN`, we really do know what’s supposed to be stored; it’s just that it happens to correspond to something like $$0/0$$ that doesn’t make any sense at all. In contrast, `NA` indicates that we actually don’t know what was supposed to be there. The information is _missing_.
* _No value_ (`NULL`). The `NULL` value takes this “absence” concept even further. It basically asserts that the variable genuinely has no value whatsoever. This is quite different to both `NaN` and `NA`. For `NaN` we actually know what the value is, because it’s something insane like $$0/0$$. For `NA`, we believe that there is supposed to be a value “out there”, but a dog ate our homework and so we don’t quite know what it is. But for `NULL` we strongly believe that there is _no value at all_.

#### Assigning names to vector elements

One thing about the way that R prints out a vector is that the elements come out unlabelled. Suppose I’ve got data reporting the quarterly profits for some company. That is:

```r
profit <- c( 3.1, 0.1, -1.4, 1.1 )
profit
## [1]  3.1  0.1 -1.4  1.1
```

You can probably guess that the first element corresponds to the first quarter, the second element to the second quarter, and so on. This is where it can be helpful to assign `names` to each of the elements. Here’s how you do it:

```r
names(profit) <- c("Q1","Q2","Q3","Q4")
profit
## Q1   Q2   Q3   Q4 
## 3.1  0.1 -1.4  1.1
```

You can always delete the names again by using `names(profit) <- NULL`. It’s also worth noting that you don’t have to do this as a two-stage process:

```r
profit <- c( "Q1" = 3.1, "Q2" = 0.1, "Q3" = -1.4, "Q4" = 1.1 )
profit
## Q1   Q2   Q3   Q4 
## 3.1  0.1 -1.4  1.1
```

The _value_ of `profit[1]` is still `3.1`. Nevertheless, names aren’t purely cosmetic, since R allows you to pull out particular elements of the vector by referring to their names:

```r
profit["Q1"]
## Q1 
## 3.1
```

To pull out the names themselves, just type `names(profit)`.

#### Variable classes

As we’ve seen, R allows you to store different kinds of data. In particular, the variables we’ve defined so far have either been character data (text), numeric data, or logical data. Even R is smart enough to know you can’t multiply `"apples"` by `"oranges"`. It knows this because the quote marks are indicators that the variable is supposed to be treated as text, not as a number.

This is quite useful, but notice that it means that R makes a big distinction between `5` and `"5"`:

```r
x <- "5"   # x is character
y <- "4"   # y is character
x * y
## Error in x * y: non-numeric argument to binary operator     
```

Okay, let’s suppose that I’ve forgotten what kind of data we stored in the variable `x` (which happens depressingly often). R provides a function that will let us find out. Or, more precisely, it provides _three_ functions: `class()`, `mode()` and `typeof()`. Why the heck does it provide three functions, you might be wondering? Basically, because R actually keeps track of three different kinds of information about a variable. The **class** of a variable is a “high level” classification, and it captures psychologically (or statistically) meaningful distinctions. For instance `"2011-09-12"` and `"my birthday"` are both text strings, but there’s an important difference between the two: one of them is a date:

```r
x <- "hello world"
class(x)
## [1] "character"

x <- TRUE
class(x)
## [1] "logical"

x <- 100
class(x)
## [1] "numeric"
```

Exciting, no?

#### Data frames

In order to understand why R has created this funny thing called a data frame, it helps to try to see what problem it solves. So let’s assume we record 9 individuals' test scores:

```r
group <- c(1,1,1,2,2,2,3,3,3)
gender <- c("M", "M", "M", "M", "M", "F", "F", "F", "F")
age <- c(17, 19, 21, 37, 18, 19, 47, 18, 19)
score <- c(12, 10, 11, 15, 16, 14, 25, 21, 29)
```

So there are four variables in the workspace, `age`, `gender`, `group` and `score`. And it just so happens that all four of them are the same size (i.e., they’re all vectors with 9 elements). And it just so happens that `age[1]` corresponds to the age of the first person, and `gender[1]` is the gender of that very same person, etc. In other words, we both know that all four of these variables correspond to the _same_ data set, and all four of them are organised in exactly the same way.

However, R _doesn’t_ know this! As far as it’s concerned, there’s no reason why the `age` variable has to be the same length as the `gender` variable; and there’s no particular reason to think that `age[1]` has any special relationship to `gender[1]` any more than it has a special relationship to `gender[4]`. In other words, when we store everything in separate variables like this, R doesn’t know anything about the relationships between things. The data frame fixes this: if we store our variables inside a data frame, we’re telling R to treat these variables as a single, fairly coherent data set.

So how do we create a data frame? One way we’ve already seen: if we import our data from a CSV file, R will store it as a data frame. A second way is to create it directly from some existing variables using the `data.frame()` function. All you have to do is type a list of variables that you want to include in the data frame. The output of the command `data.frame()` is, well, a data frame. So, if we want to store all the four variables in a data frame called `expt` :

```r
expt <- data.frame ( age, gender, group, score ) 
expt 
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

Once you’ve created it, it no longer depends on the original variables from which it was constructed. That is if we make changes to the original `age` variable, it will not lead to any changes to the age data stored in `expt`.

How do we tell R to look inside the data frame? As is always the case with R there are several ways. The simplest way is to use the operator `$` to extract the variable you’re interested in:

```r
expt$score
## [1] 12 10 11 15 16 14 25 21 29
```

One problem that sometimes comes up in practice is that you forget what you called all your variables. Normally you might try to type `objects()` or `who()`, but neither of those commands will tell you what the names are for those variables inside a data frame! One way is to ask R to tell you the names of all the variables stored in the data frame using the function `names()` :

```r
names(expt)
## [1] "age" "gender" "group" "score"
```

An alternative method is to use the `who()` function, as long as you tell it to look at the variables inside data frames. If you set `expand = TRUE` then it will not only list the variables in the workspace, but it will “expand” any data frames that you’ve got in the workspace so that you can see what they look like. or, since `expand` is the first argument in the `who()` function you can just type `who(TRUE)`:

```
who(expand = TRUE)
```

#### Lists

The next kind of data we want to mention is a **list**. Lists are an extremely fundamental data structure in R, and as you start making the transition from a novice to a savvy R user you will use lists all the time. Most advanced data structures in R are built from lists (e.g., data frames are actually a specific type of list). Because lists are so important to how R stores things, it’s useful to have a basic understanding of them. Okay, so what is a list, exactly? Like data frames, lists are just “collections of variables.” However, unlike data frames – which are basically supposed to look like a nice “rectangular” table of data – there are no constraints in a list on what kinds of variables we include, and no requirement that the variables have any particular relationship to one another. In order to understand what this actually _means_, the best thing to do is create a list, which we can do using the `list()` function. If I type this as my command:

```r
Dan <- list(age = 34,
            nerd = TRUE,
            parents = c("Joe","Liz") 
)
```

R creates a new list variable called `Dan`, which is a bundle of three different variables: `age`, `nerd` and `parents`. Notice, that the `parents` variable is longer than the others. This is perfectly acceptable for a list, but it wouldn’t be for a data frame. If we now print out the variable, you can see the way that R stores the list:

```r
print( Dan )
$age
## [1] 34

$nerd
## [1] TRUE

$parents
## [1] "Joe" "Liz"
```

As you might have guessed from those `$` symbols everywhere, the variables are stored in exactly the same way that they are for a data frame (again, this is not surprising: data frames _are_ a type of list). You can extract the variables from the list using the `$` operator, like so:

```
Dan$nerd
## [1] TRUE
```

If you need to add new entries to the list, the easiest way to do so is to again use `$` :

```
Dan$children <- "Alex"
```

then R creates a new entry to the end of the list called `children` and assigns it a value of `"Alex"`. Finally, it’s actually possible for lists to contain other lists, so it’s quite possible that we would end up using `Dan$children$age` to find out how old Dan's son is.

#### Formulas

The last kind of variable that we want to see before finally being able to start talking about statistics is **Formula**. Stated simply, a formula object is a variable, but it’s a special type of variable that specifies a relationship between other variables. A formula is specified using the “tilde operator” `~`:

```r
formula1 <- out ~ pred
formula1
## out ~ pred
```

The _precise_ meaning of this formula depends on exactly what you want to do with it, but in broad terms, it means “the `out` (outcome) variable analyzed in terms of the `pred` (predictor) variable”:

```r
formula2 <-  out ~ pred1 + pred2   # more than one variable on the right
formula3 <-  out ~ pred1 * pred2   # different relationship between predictors 
formula4 <-  ~ var1 + var2         # a 'one-sided' formula
```

Formulas are pretty flexible things, and so different functions will make use of different formats, depending on what the function is intended to do.

#### Getting help

So where should you go for help? once you’re moving away from being a pure beginner to becoming a skilled user, you’ll start ﬁnding the help documentation more and more helpful. You can look at the help documentation for the `load()` function using one of the following:

```r
?load 
help("load")
```
