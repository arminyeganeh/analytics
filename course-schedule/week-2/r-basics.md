---
description: Lab 2, 150-250 lines, 1 hour to complete
---

# R Basics

### Instructions

Read this tutorial and apply the codes in R. No submission is required.

### Installing and loading packages

A package is basically just a big collection of functions, data sets, and other R objects that are all grouped together under a common name. Some packages are already installed when you put R on your computer, but the vast majority of them of R packages are on the internet. There’s a critical distinction that you need to understand, which is the difference between having a package installed on your computer, and having a package loaded in R. As of this writing, there are just over 5000 R packages freely available “out there” on the internet. When you install R on your computer, about 30 or so come bundled with the basic R installation. So right now there are about 30 packages “installed” on your computer, and another 5000 or so that are not installed. Just because something is on your computer doesn’t mean R can use it. In order for R to be able to use one of your 30 or so installed packages, that package must also be “loaded”. Generally, when you open up R, only a few of these packages are actually loaded. Basically what it boils down to is this:

* A package must be installed before it can be loaded.
* A package must be loaded before it can be used.

In the lower right hand panel in RStudio, you’ll see a tab labelled “Packages”. Click on the tab, and you’ll see a list of packages that looks something like **Figure HA1.4.** Every row in the panel corresponds to a different package, and every column is a useful piece of information about that package. Going from left to right, here’s what each column is telling you:

* The check box on the far left column indicates whether or not the package is loaded.
* The one word of text immediately to the right of the check box is the name of the package.
* The short passage of text next to the name is a brief description of the package.
* The number next to the description tells you what version of the package you have installed.
* The little x-mark next to the version number is used to uninstall the package.

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/Rstudiopackages.png" alt=""><figcaption><p>RStudio's Packages Dialogue</p></figcaption></figure>

To install packages from CRAN you can use the following code:

```r
# install packages from CRAN 
install.packages ("packagename")
```

### Loading a package

The package `foreign` is a collection of tools that are very handy when R needs to interact with files that are produced by other software packages (e.g., SPSS). It comes bundled with R, so it’s one of the ones that you have installed already, but it won’t be one of the ones loaded. Inside the package `foreign` is a function called `read.spss()`. It’s a handy little function that you can use to import an SPSS data file into R, so let’s pretend we want to use it. Currently, `foreign`  isn’t loaded, so if I ask R to tell me if it knows about a function called `read.spss()` it tells me that there’s no such thing…

```
> exists("read.spss")
[1] FALSE
```

Now let’s load the package. In RStudio, the process is dead simple: go to the package tab, find the entry for the `foreign` package, and check the box on the left-hand side. The moment that you do this, you’ll see a command like this appear in the R console:

```
> library("foreign", lib.loc="C:/Program Files/R/R-3.0.2/library")
```

The bit `lib.loc` will look slightly different on Macs versus Windows, because that part of the command is just RStudio telling R where to look to find the installed packages. The bit `lib.loc` is almost always unnecessary. Unless you’ve taken to installing packages in idiosyncratic places R already knows where to look. So in the vast majority of cases, the command to load the `foreign` package is just this:

```
> library("foreign")
```

You can use the RStudio package panel to do all your package loading for you. The only reason we include the command `library()` is to create a reminder to make sure that we have the relevant package loaded and to see if our attempt to load the package actually worked. Let’s see if R now knows about the existence of the `read.spss()` function…

```
> exists("read.spss")
[1] TRUE
```

### Unloading a package

Sometimes, especially after a long session of working with R, you find yourself wanting to get rid of some of those packages that you’ve loaded. The RStudio package panel makes this exactly as easy as loading the package in the first place. Find the entry corresponding to the package you want to unload and uncheck the box and the package is unloaded.

```
> detach("package:foreign", unload=TRUE)
Warning: 'foreign' namespace cannot be unloaded:
namespace 'foreign' is imported by 'rio', 'psych' so cannot be unloaded
```

We can verify this by seeing if the `read.spss()` function still `exists()`:

```
exists("read.spss")
[1] FALSE
```

Nope. Definitely gone.

For the most part, packages are quite unrelated to each other as they do different things, but there are cases when two loaded packages both contain functions with identical names. This creates a naming conflict. The answer is R uses whichever package you loaded most recently, and it tells you this very explicitly. The output will tell you that the function in the first package is no longer accessible to you. It’s been hidden (or “masked”) from you by the one in the second, most recent package.

### Downloading new packages

One of the main selling points for R is that there are thousands of packages that have been written for it, and these are all available online. There is a big repository of packages called the “Comprehensive R Archive Network” (CRAN), and the easiest way of getting and installing a new package is from one of the many CRAN mirror sites. Conveniently for us, R provides a function called `install.packages()` that you can use to do this. Even _more_ conveniently, the RStudio team runs its own CRAN mirror and RStudio has a clean interface that lets you install packages without having to learn how to use the `install.packages()` command. Using the RStudio tools is, again, dead simple. In the top left-hand corner of the packages panel, you’ll see a button called “Install Packages”. If you click on that, it will bring up a window like the one shown in **Figure LA2.1**.&#x20;

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/installpackage2.png" alt=""><figcaption><p><strong>Figure LA2.1</strong> Package installation dialogue box</p></figcaption></figure>

You can select from this list, or just keep typing. Either way, once you’ve got the package name that you want, click on the install button at the bottom of the window. When you do, you’ll see the following command appear in the R console:

```
> install.packages("psych")

trying URL 'http://cran.rstudio.com/bin/macosx/contrib/3.0/psych_1.4.1.tgz'
Content type 'application/x-gzip' length 2737873 bytes (2.6 Mb)
opened URL
==================================================
downloaded 2.6 Mb


The downloaded binary packages are in
    /var/folders/cl/thhsyrz53g73q0w1kb5z3l_80000gn/T//RtmpmQ9VT3/downloaded_packages
```

This is the R command that does all the work. R then goes off to the internet, has a conversation with CRAN, downloads some stuff, and installs it on your computer. You probably don’t care about all the details of R’s little adventure on the web, but the `install.packages()` function is rather chatty, so it reports a bunch of gibberish that you really aren’t all that interested in, saying “I’ve installed the psych package”.&#x20;

Every now and then the authors of packages release updated versions. The updated versions often add new functionality, fix bugs, and so on. It’s generally a good idea to update your packages periodically. There’s the function `update.packages()`  that you can use to do this, but it’s probably easier to stick with the RStudio tool, using the “Update Packages” button located in the packages panel.&#x20;

### Managing the workspace

Let's create the three variables; `seeker`, `lover`, and `keeper`. These three variables are the contents of your **workspace**, also referred to as the **global environment**. The workspace is a key concept in R, we’ll talk a lot about what it is and how to manage its contents.

```
seeker <- 3.1415
lover <- 2.7183
keeper <- seeker * lover
print(keeper)
[1] 8.539539
```

The RStudio Environment panel, **Figure LA2.2**, shows you the contents of the workspace. The view shown above is the list view. To switch to the grid view, click on the menu item on the top right that currently reads "List". The first thing that you need to know how to do is to examine the contents of the workspace. If you’re using RStudio, you will probably find that the easiest way to do this is to use the “Environment” panel. If you’re using the command line, then the function `objects()` may come in handy:

```
objects()
```

&#x20;

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/workspacepanel2.png" alt=""><figcaption><p><strong>Figure LA2.2</strong> Environment dialogue box in Grid mode</p></figcaption></figure>

There are also several other functions that you can use, including `ls()` which is pretty much identical to `objects()`, and `ls.str()` which you can use to get a fairly detailed description of all the variables in the workspace. In fact, the package `lsr`  actually includes its own function that you can use for this purpose, called `who()`. The reason for using `who()` is pretty straightforward: The command `objects()` isn’t quite informative enough, because the only thing it prints out is the name of each variable; but the function `ls.str()` is too informative. The function `who()` is a compromise between the two. First, now that we’ve got the `lsr` package installed, we need to load it. The result includes a description of name, class, and size of all objects:

```
> install.packages("lsr")
> library(lsr)
> who()
##    -- Name --             -- Class --     -- Size --
##    keep                   numeric         1         
##    lover                  numeric         1         
##    seeker                 numeric         1         
```

### Removing variables from the workspace

There’s no “undo” option for variable removal. Once a variable is removed, it’s gone forever unless you save it to disk. We will see how to do that later, but since we have no need for these variables at all, we can safely get rid of them. In RStudio, the easiest way to remove variables is to use the environment panel. Assuming that you’re in Grid view, check the boxes next to the variables that you want to delete, then click on the Clear button at the top of the panel. When you do this, RStudio will show a dialog box asking you to confirm that you really do want to delete the variables. Suppose you don’t access to RStudio, and you still want to remove variables. This is where the **remove** function `rm()` comes in handy:

```
rm(seeker, lover)
```

### Navigating the file system

Regardless of whether you’re using Windows, Mac OS, or Linux, every file on the computer is assigned a human-readable address, and every address has the same basic structure: it describes a **path** that starts from a **root**, through a series of **folders** (or if you’re an old-school computer user, directories) and finally ends up at the file. On a Windows computer, the root is the hard drive that stores all your files, C, and most file names on Windows begin with C. After that comes the folders, and on Windows, the folder names are separated by a `\` symbol. So, the complete path to your book on your Windows computer might be something like this:

```
C:\Users\danRbook\LSR.pdf
```

On Linux, Unix, and Mac OS systems, the addresses look a little different, but they’re more or less identical in spirit. Instead of using the backslash, folders are separated using a forward slash, and unlike Windows, they don’t treat the physical drive as being the root of the file system. So, the path to this book on my Mac might be something like this:

```
/Users/dan/Rbook/LSR.pdf
```

So that’s what we mean by the “path” to a file. The next concept to grasp is the idea of a **working directory** and how to change it. The working path is “whatever folder I’m currently looking at”. Suppose that you are currently looking for files in Explorer (if you’re using Windows) or using Finder (on a Mac). The folder you currently have open is the user path (i.e., `C:\Users\dan` or `/Users/dan`). That’s the current working path. We can talk to R about moving _from_ our current location _to_ a new one. What that means is that we might want to specify a new location in relation to our current location. A relative path refers to a location that is relative to a current path. Relative paths make use of two special symbols, a dot `.` , and a double-dot `..` , which translate into the current path and the parent path. Double dots are used for moving up in the hierarchy. A single dot represents the current path:

| absolute path (i.e., from root) | relative path (i.e. from C:) |
| ------------------------------- | ---------------------------- |
| C:\Users\dan\danRbook           | .                            |
| C:\Users\dan                    | ..                           |
| C:\Users                        | ..\\..                       |
| C:\Users\dan\danRbook\nerdstuff | .\nerdstuff                  |

There’s one last thing: the `~` directory. It’s quite common on computers that have multiple users to define `~` to be the user’s home directory. It is possible to define other directories in terms of their relationship to the home directory. For example, an alternative way to describe the location of the `LSR.pdf` file on your Mac would be

```
~danRbook\LSR.pdf
```

When you want to load or save a file in R it’s important to know what the working directory (i.e., path) is. You can find out by using the `getwd()` command. For the moment, let’s assume that we are using Mac OS or Linux. Here’s what happens:

```
> getwd()
[1] "/Users/dan"
> path.expand("~")
[1] "/Users/dan"
```

We can change the working directory quite easily using `setwd()`. The function `setwd()` has only one argument, `dir`, a character string specifying a path to a directory, or a path relative to the working directory. Since we are currently at the directory `/Users/dan` the following two are equivalent:

```
setwd("/Users/dan/Rbook/data")
setwd("./Rbook/data")
```

We can type `list.files()` command to get a listing of all the files in that directory.
