---
description: Lab A1 Instructions
---

# Intro to R & RStudio

### Installing R

R is freely distributed online, and can be downloaded from:

> [http://cran.r-project.org/](http://cran.r-project.org/)

At the top of the page – under the heading “Download and Install R” – there are separate links for Windows, Mac, and Linux users. About every six months, a new update becomes available.&#x20;

#### Installing R on a Windows computer

On the R homepage, you’ll find a link at the top of the page with the text “Download R for Windows”. If you click on that, it will take you to a page that offers you a few options. Again, at the top of the page, you’ll be told to click on a link that says **Install R for the first time**. This will take you to a page that has a prominent link at the top called **Download R for Windows**. Click on that and your browser should start downloading an R installer file. After R is installed on your system, you should install RStudio.

#### Installing R on a Mac computer

On the R homepage, you’ll find a link at the top of the page with the text “Download R for macOS”. If you click on that, it will take you to a page that offers you a few options. There’s a fairly prominent link on the page just under Latest Release, with a “.pkg” format. Click on that link and you’ll start downloading the installer file. Once downloaded, open the file by double-clicking and follow all the instructions. Once finished, you’ll find a file called `R.app` in the Applications folder.  After R is installed on your system, you should install RStudio.

#### Installing RStudio

RStudio is an Integrated Development Environment (IDE), providing a free, open-source, professional interface to R, the underlying statistical language. RStudio can be downloaded from:

> [http://www.rstudio.com/](https://www.rstudio.com/products/rstudio/download/)

When you visit the RStudio website, find and download **RStudio Desktop**. After choosing the desktop version it will take you to a page that shows several possible downloads based on the operating system. The webpage automatically recommends the download that is most appropriate for your computer. Click on the appropriate link, and the RStudio installer file will start downloading. Once finished downloading, open the installer file in the usual way to install RStudio.&#x20;

### RStudio Interface

After installing, you can start R by opening RStudio. To illustrate what RStudio looks like, **Figure LA1** shows a screenshot of an R session in progress. There could be very small differences in RStudio's appearance between Mac and Windows systems. The upper-left area — called script, source, or program — is where you type, edit, and run R scripts. The bottom-left area — called R console — is where you see the execution of the script. In the top-right area, the environment pane, you see the objects and values created and used in the program, as well as other relevant information. The bottom-right area is where you have access to multiple additional tabs, which will be discussed later in detail.

<figure><img src="../../.gitbook/assets/rstudio_interface.png" alt=""><figcaption><p><strong>Figure LA1:</strong> RStudio with 4 panes</p></figcaption></figure>

### Starting up R

A common way to start learning R is using it as a simple calculator.&#x20;

Click on the script area and type `10 + 20` . Click on the _Run_ button located on the script pane. What you see on the console is:

```
> 10 + 20
[1] 30
```

The symbol `>` is the _command prompt_, the __ `10 + 20` part is a _command_, and the part printed in the next line `[1] 30`  is R's _response_ to your command. For now, think of `[1] 30` as if R were saying “the answer to the 1st question you asked is 30”.

### Autocorrection

Always make sure you type _exactly what you mean_. In general, you absolutely _must_ be precise in what you say to R l in its interpretation as there is no equivalent to “autocorrect” in R. There are some situations in which R does show some flexibility. For instance, R ignores redundant spacing in the following cases:

```
> 10 + 20
[1] 30
> 10  +  20
[1] 30
```

However, inserting spaces in the middle of a word results in an error.  For instance, try the following commands, individually, to obtain information about how to cite R:

```
citation()
citation ()
cit ation()
```

### Basic arithmetic operations in R

Let’s try basic arithmetic operations in R. Table LA.1 lists the operators that correspond to the basic arithmetic of addition, subtraction, multiplication, division, and power.

Table LA.1: Basic arithmetic operations in R.

| operation      | operator | example input | example output |
| -------------- | :------: | :-----------: | :------------: |
| addition       |    `+`   |     10 + 2    |       12       |
| subtraction    |    `-`   |     9 - 3     |        6       |
| multiplication |    `*`   |     5 \* 5    |       25       |
| division       |    `/`   |     10 / 3    |        3       |
| power          |    `^`   |     5 ^ 2     |       25       |
