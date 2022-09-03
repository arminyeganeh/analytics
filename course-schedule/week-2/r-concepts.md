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

One of the main selling points for R is that there are thousands of packages that have been written for it, and these are all available online. There is a big repository of packages called the “Comprehensive R Archive Network” (CRAN), and the easiest way of getting and installing a new package is from one of the many CRAN mirror sites. Conveniently for us, R provides a function called `install.packages()` that you can use to do this. Even _more_ conveniently, the RStudio team runs its own CRAN mirror and RStudio has a clean interface that lets you install packages without having to learn how to use the `install.packages()` command. Using the RStudio tools is, again, dead simple. In the top left-hand corner of the packages panel, you’ll see a button called “Install Packages”. If you click on that, it will bring up a window like the one shown in **Figure LA2.1**.&#x20;

<figure><img src="https://learningstatisticswithr.com/book/img/mechanics/installpackage2.png" alt=""><figcaption><p><strong>Figure LA2.1</strong> The package installation dialog box in RStudio</p></figcaption></figure>

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
