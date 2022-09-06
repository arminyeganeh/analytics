---
description: Week 4, Lab A4, 150-250 lines, 1 hour to complete
---

# Drawing Graphs

### LA4 Instructions

Read this tutorial and apply the codes in R.

### Visualization

_“The greatest value of a picture is when it forces us to notice what we never expected to see.”_&#x20;

— John Tukey

Visualizing data is one of the most important tasks facing the data analyst. It’s important for two distinct but closely related reasons. Firstly, there’s the matter of drawing “presentation graphics”: displaying your data in a clean, visually appealing fashion makes it easier for your reader to understand what you’re trying to tell them. Equally important, perhaps even more important, is the fact that graphs help you understand the data. To that end, it’s important to draw “exploratory graphics” that help you learn about the data as you go about analyzing it.&#x20;

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/snowmap1-1.png" alt=""><figcaption><p><strong>Figure LA4.1</strong>: A stylized redrawing of John Snow’s original cholera map</p></figcaption></figure>

**Figure LA4.1** shows a redrawing of one of the most famous data visualizations of all time: John Snow’s 1854 map of cholera deaths. The map is elegant in its simplicity. In the background, we have a street map, which helps orient the viewer. Over the top, we see a large number of small dots, each one representing the location of a cholera case. The larger symbols show the location of water pumps, labeled by name. Even the most casual inspection of the graph makes it very clear that the source of the outbreak is almost certainly the Broad Street pump. Upon viewing this graph, Dr. Snow arranged to have the handle removed from the pump, ending the outbreak that had killed over 500 people. Such is the power of good data visualization.

The goals of this chapter are twofold: firstly, to discuss several fairly standard graphs that we use a lot when analyzing and presenting data, and secondly, to show you how to create these graphs in R. Most of the time you can produce a clean, high-quality graphic without having to learn very much about the low-level details of how R handles graphics.&#x20;

R doesn’t really provide a single coherent graphics system. Instead, R itself provides a platform, and different people have built different graphical tools using that platform. Something that surprises most new R users is the discovery that R actually has independent graphics systems, e.g., **traditional graphics** (in the `graphics` package) and **grid graphics** (in the `grid` package). The grid universe relies heavily on two different packages – `lattice` and `ggplot2` – each of which provides a quite different visual style. For the purpose of this class, we will talk about the package `ggplot2`.&#x20;

&#x20;So let’s start painting.

Let’s use our first graph to answer a question: Do cars with big engines use more fuel than cars with small engines? You probably already have an answer, but try to make your answer precise. What does the relationship between engine size and fuel efficiency look like? Is it positive? Negative? Linear? Nonlinear?

You can test your answer with the dataframe `mpg` found in `ggplot2` . The dataframe `mpg` contains observations collected by the US Environmental Protection Agency on 38 car models.

```
mpg
A tibble: 234 x 11
  manufacturer model displ  year   cyl trans      drv     cty   hwy fl    class 
  <chr>        <chr> <dbl> <int> <int> <chr>      <chr> <int> <int> <chr> <chr> 
1 audi         a4      1.8  1999     4 auto(l5)   f        18    29 p     compa…
2 audi         a4      1.8  1999     4 manual(m5) f        21    29 p     compa…
3 audi         a4      2    2008     4 manual(m6) f        20    31 p     compa…
4 audi         a4      2    2008     4 auto(av)   f        21    30 p     compa…
5 audi         a4      2.8  1999     6 auto(l5)   f        16    26 p     compa…
6 audi         a4      2.8  1999     6 manual(m5) f        18    26 p     compa…
# … with 228 more rows
```

Among the variables in `mpg` are:

1. `displ`, a car’s engine size, in liters.
2. `hwy`, a car’s fuel efficiency on the highway, in miles per gallon (mpg). A car with a low fuel efficiency consumes more fuel than a car with a high fuel efficiency when they travel the same distance.

To plot `mpg`, run this code to put `displ` on the x-axis and `hwy` on the y-axis:

```
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```

<figure><img src="https://d33wubrfki0l68.cloudfront.net/91aebc6de4de928abc810433b752274ba6a46d58/a78b1/visualize_files/figure-html/unnamed-chunk-3-1.png" alt=""><figcaption><p><strong>Figure LA4.2</strong> Relationship between engine size and fuel efficiency</p></figcaption></figure>

The plot shows a negative relationship between engine size (`displ`) and fuel efficiency (`hwy`). In other words, cars with big engines use more fuel. Does this confirm or refute your hypothesis about fuel efficiency and engine size?

With **ggplot2**, you begin a plot with the function `ggplot()`. `ggplot()` creates a coordinate system that you can add layers to. The first argument is the dataset to use in the graph. So `ggplot(data = mpg)` creates an empty graph, but it’s not very interesting so we are not going to show it here. You complete your graph by adding one or more layers to `ggplot()`. The function `geom_point()` adds a layer of points to your plot, which creates a scatterplot. ggplot2 comes with many geom functions that each add a different type of layer to a plot.

Each geom function in ggplot2 takes a `mapping` argument. This defines how variables in your dataset are mapped to visual properties. `mapping` is always paired with `aes()`, and the `x` and `y` arguments of `aes()` specify which variables to map to the x and y axes. ggplot2 looks for the mapped variables in the `data` , in this case, `mpg`.

You can add a third variable, like `class`, to a two-dimensional scatterplot by mapping it to an **aesthetic**. An aesthetic is a visual property of the objects in your plot. Aesthetics include things like the size, shape, or color of your points. You can convey information about your data by mapping the aesthetics in your plot to the variables in your dataset. For example, you can map the colors of points to the `class` variable to reveal the class of each car. To map an aesthetic to a variable, associate the name of the aesthetic to the name of the variable inside `aes()`. ggplot2 will automatically assign a unique level of the aesthetic (here a unique color) to each unique value of the variable, a process known as **scaling**. ggplot2 will also add a legend that explains which levels correspond to which values.

The colors reveal that many of the unusual points are two-seater cars. These cars don’t seem like hybrids, and are, in fact, sports cars! Sports cars have large engines like SUVs and pickup trucks, but small bodies like midsize and compact cars. In hindsight, these cars were unlikely to be hybrids since they have large engines.

```
ggplot(data = mpg) + 
geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

<figure><img src="https://d33wubrfki0l68.cloudfront.net/8b89c5554ed6108359d59909d441dbeb010e8802/9f366/visualize_files/figure-html/unnamed-chunk-7-1.png" alt=""><figcaption><p><strong>Figure LA4.3</strong> Mapping class to color</p></figcaption></figure>

Or we could have mapped `class` to the alpha aesthetic, which controls the transparency of the points, or to the shape aesthetic, which controls the shape of the points.

```
# Left
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, alpha = class))

# Right
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))Copy
```

![Figure LA4.4 Mapping class to transparency](https://d33wubrfki0l68.cloudfront.net/f9280bbd15f46cc9d67a8fa30085c147ccee89c4/402ac/visualize\_files/figure-html/unnamed-chunk-9-1.png) ![Figure LA4.5 Mapping class to shape](https://d33wubrfki0l68.cloudfront.net/1ae399aecbf37c65219a1610aa1b9700a6355834/5d06b/visualize\_files/figure-html/unnamed-chunk-9-2.png)

What happened to the SUVs? ggplot2 will only use six shapes at a time. By default, additional groups will go unplotted when you use the shape aesthetic.

You can also _set_ the aesthetic properties of your geom manually. For example, we can make all of the points in our plot blue:

```
ggplot(data = mpg) + 
geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

<figure><img src="https://d33wubrfki0l68.cloudfront.net/c36c62d7f70e30e3307295d6408b8d3b61e3c56a/a6987/visualize_files/figure-html/unnamed-chunk-10-1.png" alt=""><figcaption><p><strong>Figure LA4.6</strong> Manually setting the aesthetic properties of geom</p></figcaption></figure>

Here, the color doesn’t convey information about a variable, but only changes the appearance of the plot. To set an aesthetic manually, set the aesthetic by name as an argument of your geom function; i.e. it goes _outside_ of `aes()`. You’ll need to pick a level that makes sense for that aesthetic:

* The name of a color as a character string.
* The size of a point in mm.
* The shape of a point as a number, as shown in Figure [3.1](https://r4ds.had.co.nz/data-visualisation.html#fig:shapes).

![R has 25 built in shapes that are identified by numbers. There are some seeming duplicates: for example, 0, 15, and 22 are all squares. The difference comes from the interaction of the \`colour\` and \`fill\` aesthetics. The hollow shapes (0--14) have a border determined by \`colour\`; the solid shapes (15--20) are filled with \`colour\`; the filled shapes (21--24) have a border of \`colour\` and are filled with \`fill\`.](https://d33wubrfki0l68.cloudfront.net/e28a1b57b6622cf67fd8a7e01c6a9955914f8fe9/635be/visualize\_files/figure-html/shapes-1.png)

Figure 3.1: R has 25 built in shapes that are identified by numbers. There are some seeming duplicates: for example, 0, 15, and 22 are all squares. The difference comes from the interaction of the `colour` and `fill` aesthetics. The hollow shapes (0–14) have a border determined by `colour`; the solid shapes (15–20) are filled with `colour`; the filled shapes (21–24) have a border of `colour` and are filled with `fill`.



