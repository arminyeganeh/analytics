---
description: Assignment 1, 450-600 lines, 3 hours to complete
---

# R Markdown

### Instructions

**Submission:** After reading the following tutorial, create your CV using R Markdown in a PDF format. The title should be your name, and you should include headings for education or employment. Each of the sections should include a bulleted list of jobs/degrees. Highlight the years in bold.

### Introduction

R Markdown provides a unified authoring framework for data science, combining your code, its results, and your prose commentary. R Markdown documents are fully reproducible and support dozens of output formats, like HTML, PDF, Word, etc.

R Markdown files are designed to be used:

* For communicating to decision-makers, who want to focus on the conclusions, not the code behind the analysis.
* For collaborating with data and research scientists, who are interested in both your conclusions and how you reached them.
* As an environment in which to _do_ data science, as a modern-day lab notebook where you can capture not only what you did, but also what you were thinking.

R Markdown integrates a number of R packages and external tools. This means that help is, by-and-large, not available through `?`. Instead, there are other resources:

* Markdown Quick Reference: _Help > Markdown Quick Reference_
* R Markdown Cheat Sheet: _Help > Cheatsheets > R Markdown Cheat Sheet_,
* R Markdown Reference Guide: _Help > Cheatsheets > R Markdown Reference Guide_.

### R Markdown basics

To get started with an `.Rmd` file, select _File > New File > R Markdown…_ in the menubar. RStudio will launch a wizard that you can use to pre-populate your file with useful content that reminds you how the key features of R Markdown work. Hit the button `OK` . You will see a template R Markdown script. Click on the button `Knit` (or press `Cmd/Ctrl + Shift + Enter`) to knit and save the file on your desktop. Now open the saved file to see what is saved and compare it with the R Markdown script. &#x20;

This is another sample R Markdown file, a plain text file that has the extension `.Rmd`:

````
---
title: "Diamond sizes"
date: 2016-08-25
output: html_document
---

```{r setup, include = FALSE}
library(ggplot2)
library(dplyr)

smaller <- diamonds %>% 
  filter(carat <= 2.5)
```

We have data about `r nrow(diamonds)` diamonds. Only 
`r nrow(diamonds) - nrow(smaller)` are larger than
2.5 carats. The distribution of the remainder is shown
below:

```{r, echo = FALSE}
smaller %>% 
  ggplot(aes(carat)) + 
  geom_freqpoly(binwidth = 0.01)
```
````

It contains three important types of content:

1. An (optional) **YAML header** surrounded by `---`s.
2. **Chunks** of R code surrounded by ` ``` `. The symbol `` ` `` is called acute, back quote, accent, etc.
3. Text mixed with simple text formatting like `# heading` and `_italics_`.

When you open an `.Rmd`, you get a notebook interface where code and output are interleaved. You can run each code chunk by clicking the icon `Run` (a play button at the top of the chunk) or by pressing `Cmd/Ctrl + Shift + Enter`. RStudio executes the code and displays the results inline with the code:

<figure><img src="https://d33wubrfki0l68.cloudfront.net/853becd7fc7d20e3a63f52b23f522d6f0d06e066/88250/rmarkdown/diamond-sizes-notebook.png" alt=""><figcaption><p>Running code chunks in R Markdown file</p></figcaption></figure>

To produce a complete report containing all text, code, and results, click `Knit` or press `Cmd/Ctrl + Shift + K`. You can also do this programmatically with `rmarkdown::render("1-example.Rmd")`. This will display the report in the viewer pane, and create a self-contained HTML file that you can share with others.&#x20;

To get started with your own `.Rmd` file, select _File > New File > R Markdown…_ in the menubar. RStudio will launch a wizard that you can use to pre-populate your file with useful content that reminds you how the key features of R Markdown work. The following sections dive into the three components of an R Markdown document in more detail: the header, the markdown text, and the code chunks.

### Text formatting with Markdown

Markdown is designed to be easy to read, easy to write, and easy to learn. The guide below shows how to use Pandoc’s Markdown, a slightly extended version that R Markdown understands:

```
Text formatting 
------------------------------------------------------------
*italic*  or _italic_
**bold**   __bold__
`code`
superscript^2^ and subscript~2~

Headings
------------------------------------------------------------
# 1st Level Header
## 2nd Level Header
### 3rd Level Header

Lists
------------------------------------------------------------
* Unordered list item 1
* Item 2
   + Item 2a
   + Item 2b

1. Ordered list item 1
2. Item 2
3. Item 3
    + Item 3a
    + Item 3b

Links and Images on the web or local files in the same directory
------------------------------------------------------------
<http://example.com>
[linked phrase](http://example.com)
![optional caption text](path/to/img.png)

Tables 
------------------------------------------------------------
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content CellCopy
```

The best way to learn these is simply to try them out. It will take a few days, but soon they will become second nature, and you won’t need to think about them. If you forget, you can get to a handy reference sheet with _Help > Markdown Quick Reference_.

### Code chunks

To run code inside an R Markdown document, you need to insert a chunk. There are three ways to do so:

* By manually typing the chunk delimiters ` ```{r} ` and ` ``` `
* The keyboard shortcut Cmd/Ctrl + Alt + I
* The “Insert” button icon in the editor toolbar

The keyboard shortcut will save you a lot of time in the long run. You can continue to run the code using the keyboard shortcut `Cmd/Ctrl + Enter`. However, chunks get a new keyboard shortcut: `Cmd/Ctrl + Shift + Enter`, which runs all the code in the chunk. Think of a chunk as a function. A chunk should be relatively self-contained and focused around a single task. The following sections describe the chunk header, which consists of ` ```{r ` followed by an optional chunk name, followed by comma separated options, followed by `}`. Next comes your R code and the chunk end is indicated by a final ` ``` `.

### Chunk name

Chunks can be given an optional name: ` ```{r by-name} `. This has three advantages:

1.  You can more easily navigate to specific chunks using the drop-down code navigator in the bottom-left of the script editor:

    ![](https://d33wubrfki0l68.cloudfront.net/6fcddff214345601f998805adce94ab0e21d8615/2a098/screenshots/rmarkdown-chunk-nav.png)
2. Graphics produced by the chunks will have useful names that make them easier to use elsewhere.&#x20;
3. You can set up networks of cached chunks to avoid re-performing expensive computations on every run.

There is one chunk name that imbues special behavior: `setup`. When you’re in notebook mode, the chunk named setup will be run automatically once, before any other code is run.

### Chunk options

Chunk output can be customized with **options**, arguments supplied to the chunk header. Knitr provides almost 60 options that you can use to customize your code chunks. Here we’ll cover the most important chunk options that you’ll use frequently. You can see the full list at [here](https://yihui.org/knitr/options/). The most important set of options controls if your code block is executed and what results are inserted in the finished report:

* `eval = FALSE` prevents code from being evaluated. This is useful for displaying example code, or for disabling a large block of code without commenting on each line.
* `include = FALSE` runs the code, but doesn’t show the code or results in the final document. Use this for setup code that you don’t want cluttering your report.
* `echo = FALSE` prevents code, but not the results from appearing in the finished file. Use this when writing reports aimed at people who don’t want to see the underlying R code.
* `message = FALSE` or `warning = FALSE` prevents messages or warnings from appearing in the finished file.
* `results = 'hide'` hides printed output; `fig.show = 'hide'` hides plots.
* `error = TRUE` causes the render to continue even if the code returns an error. This is rarely something you’ll want to include in the final version of your report but can be very useful if you need to debug exactly what is going on inside your `.Rmd`. It’s also useful if you’re teaching R and want to deliberately include an error. The default, `error = FALSE` causes knitting to fail if there is a single error in the document.

### Table

By default, R Markdown prints data frames and matrices as you’d see them in the console:

```
mtcars[1:5, ]
#>                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
#> Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
#> Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
#> Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
#> Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
#> Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2Copy
```

If you prefer that data be displayed with additional formatting you can use the [`knitr::kable`](https://rdrr.io/pkg/knitr/man/kable.html) function. The code below generates Table [27.1](https://r4ds.had.co.nz/r-markdown.html#tab:kable).

```
knitr::kable(
  mtcars[1:5, ], 
  caption = "A knitr table"
)
```

<figure><img src="../../.gitbook/assets/A knitr table.png" alt=""><figcaption><p>A knitr table</p></figcaption></figure>

You can read the documentation [`?knitr::kable`](https://rdrr.io/pkg/knitr/man/kable.html) to see the other customization ways.

### Global options

As you work more, you will discover that some of the default chunk options don’t fit your needs and you want to change them. You can do this by calling `knitr::opts_chunk$set()` in a code chunk. For example, when writing books and tutorials set:

```
knitr::opts_chunk$set(
  comment = "#>",
  collapse = TRUE
)
```

This uses my preferred comment formatting and ensures that the code and output are kept closely entwined. On the other hand, if you were preparing a report, you might set:

```
knitr::opts_chunk$set(
  echo = FALSE
)
```

That will hide the code by default to only show the chunks you deliberately choose to show (with `echo = TRUE`). You might consider setting `message = FALSE` and `warning = FALSE`, but that would make it harder to debug problems because you wouldn’t see any messages in the final document.

### Inline code

There is one other way to embed R code into an R Markdown document: directly into the text, with: `` `r ` ``. This can be very useful if you mention the properties of your data in the text. For example, in the example document I used at the start of the chapter I had:

> We have data about `` `r nrow(diamonds)` `` diamonds. Only `` `r nrow(diamonds) - nrow(smaller)` `` are larger than 2.5 carats. The distribution of the remainder is shown below:

When the report is knit, the results of these computations are inserted into the text:

> We have data about 53940 diamonds. Only 126 are larger than 2.5 carats. The distribution of the remainder is shown below:

When inserting numbers into text, [`format()`](https://rdrr.io/r/base/format.html) is your friend. It allows you to set the number of `digits` so you don’t print to a ridiculous degree of accuracy, and a `big.mark` to make numbers easier to read. I’ll often combine these into a helper function:

```
comma <- function(x) format(x, digits = 2, big.mark = ",")
comma(3452345)
#> [1] "3,452,345"
comma(.12358124331)
#> [1] "0.12"
```

### Bibliographies and citations

Pandoc can automatically generate citations and a bibliography in a number of styles. To use this feature, specify a bibliography file using the `bibliography` field in your file’s header. The field should contain a path from the directory that contains your `.Rmd` file to the file that contains the bibliography file:

```
bibliography: rmarkdown.bibCopy
```

You can use many common bibliography formats including BibLaTeX, BibTeX, endnote, medline.

To create a citation within your `.Rmd` file, use a key composed of ‘@’ + the citation identifier from the bibliography file. Then place the citation in square brackets. Here are some examples:

```
Separate multiple citations with a `;`: Blah blah [@smith04; @doe99].

You can add arbitrary comments inside the square brackets: 
Blah blah [see @doe99, pp. 33-35; also @smith04, ch. 1].

Remove the square brackets to create an in-text citation: @smith04 
says blah, or @smith04 [p. 33] says blah.

Add a `-` before the citation to suppress the author's name: 
Smith says blah [-@smith04].Copy
```

When R Markdown renders your file, it will build and append a bibliography to the end of your document. The bibliography will contain each of the cited references from your bibliography file, but it will not contain a section heading. As a result, it is common practice to end your file with a section header for the bibliography, such as `# References` or `# Bibliography`.

You can change the style of your citations and bibliography by referencing a CSL (citation style language) file in the `csl` field:

```
bibliography: rmarkdown.bib
csl: apa.cslCopy
```

As with the bibliography field, your csl file should contain a path to the file. Here I assume that the csl file is in the same directory as the .Rmd file. A good place to find CSL style files for common bibliography styles is [http://github.com/citation-style-language/styles](http://github.com/citation-style-language/styles).

### Learning more

R Markdown is still relatively young and is still growing rapidly. The best place to stay on top of innovations is the official R Markdown website: [http://rmarkdown.rstudio.com](http://rmarkdown.rstudio.com/). Collaboration is a vital part of modern data science, and you can make your life much easier by using version control tools, like Git and GitHub. Free resources that will teach you about Git include:

1. “Happy Git with R”: a user-friendly introduction to Git and GitHub from R users, by Jenny Bryan. The book is freely available online: [http://happygitwithr.com](http://happygitwithr.com/)
2. The “Git and GitHub” chapter of _R Packages_, by Hadley. You can also read it for free online: [http://r-pkgs.had.co.nz/git.html](http://r-pkgs.had.co.nz/git.html).
