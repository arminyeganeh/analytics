---
description: Week 2, Lab A2, 150 lines, 1 hour to complete
---

# R Concepts

### LA2 Instructions

Read this tutorial and apply the codes in R.

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

One of the main selling points for R is that there are thousands of packages that have been written for it, and these are all available online. So whereabouts online are these packages to be found, and how do we download and install them? There is a big repository of packages called the “Comprehensive R Archive Network” (CRAN), and the easiest way of getting and installing a new package is from one of the many CRAN mirror sites. Conveniently for us, R provides a function called `install.packages()` that you can use to do this. Even _more_ conveniently, the RStudio team runs its own CRAN mirror and RStudio has a clean interface that lets you install packages without having to learn how to use the `install.packages()` command[48](https://learningstatisticswithr.com/book/mechanics.html#fn48)

Using the RStudio tools is, again, dead simple. In the top left hand corner of the packages panel (Figure [4.1](https://learningstatisticswithr.com/book/mechanics.html#fig:packagepanel)) you’ll see a button called “Install Packages”. If you click on that, it will bring up a window like the one shown in Figure [4.2](https://learningstatisticswithr.com/book/mechanics.html#fig:packageinstalla).

![The package installation dialog box in RStudio](https://learningstatisticswithr.com/book/img/mechanics/installpackage.png)

Figure 4.2: The package installation dialog box in RStudio

There are a few different buttons and boxes you can play with. Ignore most of them. Just go to the line that says “Packages” and start typing the name of the package that you want. As you type, you’ll see a dropdown menu appear (Figure [4.3](https://learningstatisticswithr.com/book/mechanics.html#fig:packageinstallb)), listing names of packages that start with the letters that you’ve typed so far.

![When you start typing, you'll see a dropdown menu suggest a list of possible packages that you might want to install](https://learningstatisticswithr.com/book/img/mechanics/installpackage2.png)

Figure 4.3: When you start typing, you’ll see a dropdown menu suggest a list of possible packages that you might want to install

You can select from this list, or just keep typing. Either way, once you’ve got the package name that you want, click on the install button at the bottom of the window. When you do, you’ll see the following command appear in the R console:

```
install.packages("psych")
```

This is the R command that does all the work. R then goes off to the internet, has a conversation with CRAN, downloads some stuff, and installs it on your computer. You probably don’t care about all the details of R’s little adventure on the web, but the `install.packages()` function is rather chatty, so it reports a bunch of gibberish that you really aren’t all that interested in:

```
trying URL 'http://cran.rstudio.com/bin/macosx/contrib/3.0/psych_1.4.1.tgz'
Content type 'application/x-gzip' length 2737873 bytes (2.6 Mb)
opened URL
==================================================
downloaded 2.6 Mb


The downloaded binary packages are in
    /var/folders/cl/thhsyrz53g73q0w1kb5z3l_80000gn/T//RtmpmQ9VT3/downloaded_packages
```

Despite the long and tedious response, all thar really means is “I’ve installed the psych package”. I find it best to humour the talkative little automaton. I don’t actually read any of this garbage, I just politely say “thanks” and go back to whatever I was doing.

#### 4.2.6 Updating R and R packages

Every now and then the authors of packages release updated versions. The updated versions often add new functionality, fix bugs, and so on. It’s generally a good idea to update your packages periodically. There’s an `update.packages()` function that you can use to do this, but it’s probably easier to stick with the RStudio tool. In the packages panel, click on the “Update Packages” button. This will bring up a window that looks like the one shown in Figure [4.4](https://learningstatisticswithr.com/book/mechanics.html#fig:updatepackages). In this window, each row refers to a package that needs to be updated. You can to tell R which updates you want to install by checking the boxes on the left. If you’re feeling lazy and just want to update everything, click the “Select All” button, and then click the “Install Updates” button. R then prints out a _lot_ of garbage on the screen, individually downloading and installing all the new packages. This might take a while to complete depending on how good your internet connection is. Go make a cup of coffee. Come back, and all will be well.

![The RStudio dialog box for updating packages](https://learningstatisticswithr.com/book/img/mechanics/updatepackages.png)

Figure 4.4: The RStudio dialog box for updating packages

About every six months or so, a new version of R is released. You can’t update R from within RStudio (not to my knowledge, at least): to get the new version you can go to the CRAN website and download the most recent version of R, and install it in the same way you did when you originally installed R on your computer. This used to be a slightly frustrating event, because whenever you downloaded the new version of R, you would lose all the packages that you’d downloaded and installed, and would have to repeat the process of re-installing them. This was pretty annoying, and there were some neat tricks you could use to get around this. However, newer versions of R don’t have this problem so I no longer bother explaining the workarounds for that issue.

#### 4.2.7 What packages does this book use?

There are several packages that I make use of in this book. The most prominent ones are:

* `lsr`. This is the _Learning Statistics with R_ package that accompanies this book. It doesn’t have a lot of interesting high-powered tools: it’s just a small collection of handy little things that I think can be useful to novice users. As you get more comfortable with R this package should start to feel pretty useless to you.
* `psych`. This package, written by William Revelle, includes a lot of tools that are of particular use to psychologists. In particular, there’s several functions that are particularly convenient for producing analyses or summaries that are very common in psych, but less common in other disciplines.
* `car`. This is the _Companion to Applied Regression_ package, which accompanies the excellent book of the same name by (Fox and Weisberg [2011](https://learningstatisticswithr.com/book/mechanics.html#ref-Fox2011)). It provides a lot of very powerful tools, only some of which we’ll touch in this book.

Besides these three, there are a number of packages that I use in a more limited fashion: `gplots`, `sciplot`, `foreign`, `effects`, `R.matlab`, `gdata`, `lmtest`, and probably one or two others that I’ve missed. There are also a number of packages that I refer to but don’t actually use in this book, such as `reshape`, `compute.es`, `HistData` and `multcomp` among others. Finally, there are a number of packages that provide more advanced tools that I hope to talk about in future versions of the book, such as `sem`, `ez`, `nlme` and `lme4`. In any case, whenever I’m using a function that isn’t in the core packages, I’ll make sure to note this in the text.
