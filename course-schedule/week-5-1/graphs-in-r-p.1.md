---
description: Lab 4, 150-250 lines, 1 hour to complete
---

# Graphs in R P.1

### LA4 Instructions

Read this tutorial and apply the codes in R.

### Visualization

_“The greatest value of a picture is when it forces us to notice what we never expected to see.”_

— John Tukey

Visualizing data is one of the most important tasks facing the data analyst. It’s important for two distinct but closely related reasons. Firstly, there’s the matter of drawing “presentation graphics”: displaying your data in a clean, visually appealing fashion makes it easier for your reader to understand what you’re trying to tell them. Equally important, perhaps even more important, is the fact that graphs help you understand the data. To that end, it’s important to draw “exploratory graphics” that help you learn about the data as you go about analyzing it.

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/snowmap1-1.png" alt=""><figcaption><p><strong>Figure LA4.1</strong>: A stylized redrawing of John Snow’s original cholera map</p></figcaption></figure>

**Figure LA4.1** shows a redrawing of one of the most famous data visualizations of all time: John Snow’s 1854 map of cholera deaths. The map is elegant in its simplicity. In the background, we have a street map, which helps orient the viewer. Over the top, we see a large number of small dots, each one representing the location of a cholera case. The larger symbols show the location of water pumps, labeled by name. Even the most casual inspection of the graph makes it very clear that the source of the outbreak is almost certainly the Broad Street pump. Upon viewing this graph, Dr. Snow arranged to have the handle removed from the pump, ending the outbreak that had killed over 500 people. Such is the power of good data visualization.

The goals of this chapter are twofold: firstly, to discuss several fairly standard graphs that we use a lot when analyzing and presenting data, and secondly, to show you how to create these graphs in R. Most of the time you can produce a clean, high-quality graphic without having to learn very much about the low-level details of how R handles graphics.

R doesn’t really provide a single coherent graphics system. Instead, R itself provides a platform, and different people have built different graphical tools using that platform. Something that surprises most new R users is the discovery that R actually has independent graphics systems, e.g., **traditional graphics** (in the `graphics` package) and **grid graphics** (in the `grid` package). The grid universe relies heavily on two different packages – `lattice` and `ggplot2` – each of which provides a quite different visual style. For the purpose of this class, we will talk about the package `ggplot2`.

So let’s start painting.

### Scatter Plot

```r
# Load the matcars dataset in R
mtcars

# Scatter plot in basic R
plot(mtcars$wt, mtcars$mpg)

# Scatter plot in ggplot2
library(ggplot2)
ggplot(mtcars, aes(x = wt, y = mpg)) +
  geom_point()
```

![Scatter plot with base graphics](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-QUICK-SCATTER-BASE-1.png)![Scatter plot with ggplot2](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-QUICK-SCATTER-GGPLOT-1.png)

The first part, `ggplot()`, tell it to create a plot object, and the second part, `geom_point()`, tells it to add a layer of points to the plot.

The usual way to use `ggplot()` is to pass it a data frame (`mtcars`) and then tell it which columns to use for the x and y values. If you want to pass it two vectors for x and y values, you can use `data = NULL`, and then pass it the vectors. Keep in mind that ggplot2 is designed to work with data frames as the data source, not individual vectors, and that using it this way will only allow you to use a limited part of its capabilities.

```
ggplot(data = NULL, aes(x = mtcars$wt, y = mtcars$mpg)) +
  geom_point()
```

It is common to see `ggplot()` commands spread across multiple lines, so you may see the above code also written like this:





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
```

```
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
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

![Figure LA4.4 Mapping class to transparency](https://d33wubrfki0l68.cloudfront.net/f9280bbd15f46cc9d67a8fa30085c147ccee89c4/402ac/visualize\_files/figure-html/unnamed-chunk-9-1.png)

![Figure LA4.5 Mapping class to shape](https://d33wubrfki0l68.cloudfront.net/1ae399aecbf37c65219a1610aa1b9700a6355834/5d06b/visualize\_files/figure-html/unnamed-chunk-9-2.png)

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
* The shape of a point as a number, as shown in **Figure LA4.7**.

<figure><img src="https://d33wubrfki0l68.cloudfront.net/e28a1b57b6622cf67fd8a7e01c6a9955914f8fe9/635be/visualize_files/figure-html/shapes-1.png" alt=""><figcaption><p><strong>Figure LA4.7</strong> Identifying point shapes using numbers</p></figcaption></figure>

There are some seeming duplicates: for example, 0, 15, and 22 are all squares. The difference comes from the interaction of the `colour` and `fill` aesthetics. The hollow shapes (0–14) have a border determined by `colour`; the solid shapes (15–20) are filled with `colour`; the filled shapes (21–24) have a border of `colour` and are filled with `fill`.

One way to add additional variables is with aesthetics. Another way, particularly useful for categorical variables, is to split your plot into **facets**, subplots that each display one subset of the data.

To facet your plot by a single variable, use `facet_wrap()`. The first argument of `facet_wrap()` should be a formula, which you create with [`~`](https://rdrr.io/r/base/tilde.html) followed by a variable name (here “formula” is the name of a data structure in R, not a synonym for “equation”). The variable that you pass to `facet_wrap()` should be discrete.

```
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
```

<figure><img src="https://d33wubrfki0l68.cloudfront.net/cfc07b44ca9549597084bb18593c6aa115725240/8843c/visualize_files/figure-html/unnamed-chunk-12-1.png" alt=""><figcaption><p><strong>Figure LA4.8</strong> Facets are subplots that each display one subset of data</p></figcaption></figure>

### Geometric objects

How are these two plots similar?

![Figure LA4.9 Plotting data using geom\_point](https://d33wubrfki0l68.cloudfront.net/91aebc6de4de928abc810433b752274ba6a46d58/4e9f7/visualize\_files/figure-html/unnamed-chunk-17-1.png) ![Figure LA4.10 Plotting data using geom\_smooth](https://d33wubrfki0l68.cloudfront.net/43f153577e9c8e7f012c0606cbfbeb4d2e9ce409/17627/visualize\_files/figure-html/unnamed-chunk-17-2.png)

Both plots contain the same x and y variables and both describe the same data. But the plots are not identical. Each plot uses a different visual object to represent the data. In ggplot2 syntax, we say that they use different **geoms**. A **geom** is the geometrical object that a plot uses to represent data. People often describe plots by the type of geom that the plot uses. For example, bar charts use **bar geoms**, line charts use **line geoms**, boxplots use **boxplot geoms**, and so on. Scatterplots break the trend; they use the point geom. As we see above, you can use different geoms to plot the same data. The plot on the left uses the point geom, and the plot on the right uses the smooth geom, a smooth line fitted to the data.

To change the geom in your plot, change the geom function that you add to `ggplot()`. For instance, to make the plots above, you can use this code:

```
# left
ggplot(data = mpg) + 
geom_point(mapping = aes(x = displ, y = hwy))

# right
ggplot(data = mpg) + 
geom_smooth(mapping = aes(x = displ, y = hwy))Copy
```

Every geom function in ggplot2 takes a `mapping` argument. However, not every aesthetic works with every geom. You could set the shape of a point, but you couldn’t set the “shape” of a line. On the other hand, you _could_ set the line type of a line. `geom_smooth()` will draw a different line, with a different line type, for each unique value of the variable that you map to line type.

```
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv))
```

<figure><img src="https://d33wubrfki0l68.cloudfront.net/f6d3025401c2bf94d85e1871ea67cddf48b52504/31750/visualize_files/figure-html/unnamed-chunk-19-1.png" alt=""><figcaption><p><strong>Figure LA4.11</strong> Defining Line type using geom_smooth</p></figcaption></figure>

Here `geom_smooth()` separates the cars into three lines based on their `drv` value, which describes a car’s drivetrain. One line describes all of the points with a `4` value, one line describes all of the points with an `f` value, and one line describes all of the points with an `r` value. Here, `4` stands for four-wheel drive, `f` for front-wheel drive, and `r` for rear-wheel drive. If this sounds strange, we can make it more clear by overlaying the lines on top of the raw data and then coloring everything according to `drv`.

![](https://d33wubrfki0l68.cloudfront.net/bc8810a1ee5f0f71e36fcc9743f44dbe00af7765/10159/visualize\_files/figure-html/unnamed-chunk-20-1.png)

Notice that this plot contains two geoms in the same graph! If this makes you excited, buckle up. We will learn how to place multiple geoms in the same plot very soon. ggplot2 provides over 40 geoms, and extension packages provide even more (see [https://exts.ggplot2.tidyverse.org/gallery/](https://exts.ggplot2.tidyverse.org/gallery/) for a sampling). The best way to get a comprehensive overview is the ggplot2 cheatsheet, which you can find at [http://rstudio.com/resources/cheatsheets](http://rstudio.com/resources/cheatsheets). To learn more about any single geom, use help: `?geom_smooth`.

Many geoms, like `geom_smooth()`, use a single geometric object to display multiple rows of data. For these geoms, you can set the `group` aesthetic to a categorical variable to draw multiple objects. ggplot2 will draw a separate object for each unique value of the grouping variable. In practice, ggplot2 will automatically group the data for these geoms whenever you map an aesthetic to a discrete variable (as in the `linetype` example). It is convenient to rely on this feature because the group aesthetic by itself does not add a legend or distinguishing features to the geoms.

```
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))
    
ggplot(data = mpg) +
  geom_smooth(
    mapping = aes(x = displ, y = hwy, color = drv),
    show.legend = FALSE
  )
```

![](https://d33wubrfki0l68.cloudfront.net/b3dd4727d3724ee297d97b826ce5c8b63e3c20cd/2a193/visualize\_files/figure-html/unnamed-chunk-21-2.png) ![](https://d33wubrfki0l68.cloudfront.net/5b179db1649be5e494a1294b4c0ba57d8fec25c1/7d618/visualize\_files/figure-html/unnamed-chunk-21-3.png)

To display multiple geoms in the same plot, add multiple geom functions to `ggplot()`:

```
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))Copy
```

![](https://d33wubrfki0l68.cloudfront.net/737dd0bae0fa6100c991e2ca6c4f7dea77de7718/a3667/visualize\_files/figure-html/unnamed-chunk-22-1.png)

This, however, introduces some duplication in our code. Imagine if you wanted to change the y-axis to display `cty` instead of `hwy`. You’d need to change the variable in two places, and you might forget to update one. You can avoid this type of repetition by passing a set of mappings to `ggplot()`. ggplot2 will treat these mappings as global mappings that apply to each geom in the graph. In other words, this code will produce the same plot as the previous code:

```
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()Copy
```

If you place mappings in a geom function, ggplot2 will treat them as local mappings for the layer. It will use these mappings to extend or overwrite the global mappings _for that layer only_. This makes it possible to display different aesthetics in different layers.

```
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth()Copy
```

![](https://d33wubrfki0l68.cloudfront.net/882211b74e43014a23eaca047e0196c9e00c039c/b67af/visualize\_files/figure-html/unnamed-chunk-24-1.png)

You can use the same idea to specify different `data` for each layer. Here, our smooth line displays just a subset of the `mpg` dataset, the subcompact cars. The local data argument in `geom_smooth()` overrides the global data argument in `ggplot()` for that layer only.

```
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE)Copy
```

![](https://d33wubrfki0l68.cloudfront.net/5f1620468f0bede8ea7efb83bd82f6409d0ee71e/5d90d/visualize\_files/figure-html/unnamed-chunk-25-1.png)

(You’ll learn how [`filter()`](https://rdrr.io/r/stats/filter.html) works in the chapter on data transformations: for now, just know that this command selects only the subcompact cars.)

### 3.7 Statistical transformations

Next, let’s take a look at a bar chart. Bar charts seem simple, but they are interesting because they reveal something subtle about plots. Consider a basic bar chart, as drawn with `geom_bar()`. The following chart displays the total number of diamonds in the `diamonds` dataset, grouped by `cut`. The `diamonds` dataset comes in ggplot2 and contains information about \~54,000 diamonds, including the `price`, `carat`, `color`, `clarity`, and `cut` of each diamond. The chart shows that more diamonds are available with high quality cuts than with low quality cuts.

```
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut))Copy
```

![](https://d33wubrfki0l68.cloudfront.net/7f4cb432b8891f01b8097e31f286cb54d7473ced/5471c/visualize\_files/figure-html/unnamed-chunk-29-1.png)

On the x-axis, the chart displays `cut`, a variable from `diamonds`. On the y-axis, it displays count, but count is not a variable in `diamonds`! Where does count come from? Many graphs, like scatterplots, plot the raw values of your dataset. Other graphs, like bar charts, calculate new values to plot:

* bar charts, histograms, and frequency polygons bin your data and then plot bin counts, the number of points that fall in each bin.
* smoothers fit a model to your data and then plot predictions from the model.
* boxplots compute a robust summary of the distribution and then display a specially formatted box.

The algorithm used to calculate new values for a graph is called a **stat**, short for statistical transformation. The figure below describes how this process works with `geom_bar()`.

![](https://d33wubrfki0l68.cloudfront.net/70a3b18a1128c785d8676a48c005ee9b6a23cc00/7283c/images/visualization-stat-bar.png)

You can learn which stat a geom uses by inspecting the default value for the `stat` argument. For example, `?geom_bar` shows that the default value for `stat` is “count”, which means that `geom_bar()` uses `stat_count()`. `stat_count()` is documented on the same page as `geom_bar()`, and if you scroll down you can find a section called “Computed variables”. That describes how it computes two new variables: `count` and `prop`.

You can generally use geoms and stats interchangeably. For example, you can recreate the previous plot using `stat_count()` instead of `geom_bar()`:

```
ggplot(data = diamonds) + 
  stat_count(mapping = aes(x = cut))Copy
```

![](https://d33wubrfki0l68.cloudfront.net/7f4cb432b8891f01b8097e31f286cb54d7473ced/31892/visualize\_files/figure-html/unnamed-chunk-31-1.png)

This works because every geom has a default stat; and every stat has a default geom. This means that you can typically use geoms without worrying about the underlying statistical transformation. There are three reasons you might need to use a stat explicitly:

1.  You might want to override the default stat. In the code below, I change the stat of `geom_bar()` from count (the default) to identity. This lets me map the height of the bars to the raw values of a $$yy$$ variable. Unfortunately when people talk about bar charts casually, they might be referring to this type of bar chart, where the height of the bar is already present in the data, or the previous bar chart where the height of the bar is generated by counting rows.

    ```
    demo <- tribble(
      ~cut,         ~freq,
      "Fair",       1610,
      "Good",       4906,
      "Very Good",  12082,
      "Premium",    13791,
      "Ideal",      21551
    )

    ggplot(data = demo) +
      geom_bar(mapping = aes(x = cut, y = freq), stat = "identity")Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/81c814ab218bb1c7755393ff5800dc4331fdcf95/99bbd/visualize\_files/figure-html/unnamed-chunk-32-1.png)

    (Don’t worry that you haven’t seen [`<-`](https://rdrr.io/r/base/assignOps.html) or `tribble()` before. You might be able to guess at their meaning from the context, and you’ll learn exactly what they do soon!)
2.  You might want to override the default mapping from transformed variables to aesthetics. For example, you might want to display a bar chart of proportion, rather than count:

    ```
    ggplot(data = diamonds) + 
      geom_bar(mapping = aes(x = cut, y = stat(prop), group = 1))Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/acddaca9974d1bc66d491397c9a2af7ec470e4ff/48062/visualize\_files/figure-html/unnamed-chunk-33-1.png)

    To find the variables computed by the stat, look for the help section titled “computed variables”.
3.  You might want to draw greater attention to the statistical transformation in your code. For example, you might use `stat_summary()`, which summarises the y values for each unique x value, to draw attention to the summary that you’re computing:

    ```
    ggplot(data = diamonds) + 
      stat_summary(
        mapping = aes(x = cut, y = depth),
        fun.min = min,
        fun.max = max,
        fun = median
      )Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/4f7c8830283cb7009346ed85a8196b06959f5abd/78175/visualize\_files/figure-html/unnamed-chunk-34-1.png)

ggplot2 provides over 20 stats for you to use. Each stat is a function, so you can get help in the usual way, e.g. `?stat_bin`. To see a complete list of stats, try the ggplot2 cheatsheet.

### 3.8 Position adjustments

There’s one more piece of magic associated with bar charts. You can colour a bar chart using either the `colour` aesthetic, or, more usefully, `fill`:

```
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, colour = cut))
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut))Copy
```

![](https://d33wubrfki0l68.cloudfront.net/32f8d7d22823068da65f3ebb57ba0d45b2e4b949/9599d/visualize\_files/figure-html/unnamed-chunk-36-1.png) ![](https://d33wubrfki0l68.cloudfront.net/2dae716dcaf549bc1f8483dfd50ed23a3996762c/7d18e/visualize\_files/figure-html/unnamed-chunk-36-2.png)

Note what happens if you map the fill aesthetic to another variable, like `clarity`: the bars are automatically stacked. Each colored rectangle represents a combination of `cut` and `clarity`.

```
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity))Copy
```

![](https://d33wubrfki0l68.cloudfront.net/8110840c1d10cedddc535053908825859595544a/f7d5e/visualize\_files/figure-html/unnamed-chunk-37-1.png)

The stacking is performed automatically by the **position adjustment** specified by the `position` argument. If you don’t want a stacked bar chart, you can use one of three other options: `"identity"`, `"dodge"` or `"fill"`.

*   `position = "identity"` will place each object exactly where it falls in the context of the graph. This is not very useful for bars, because it overlaps them. To see that overlapping we either need to make the bars slightly transparent by setting `alpha` to a small value, or completely transparent by setting `fill = NA`.

    ```
    ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + 
      geom_bar(alpha = 1/5, position = "identity")
    ggplot(data = diamonds, mapping = aes(x = cut, colour = clarity)) + 
      geom_bar(fill = NA, position = "identity")Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/eeb585504858f548f4ed91a08a9ff159b7c6e86b/9ec57/visualize\_files/figure-html/unnamed-chunk-38-1.png) ![](https://d33wubrfki0l68.cloudfront.net/259eecdc6939ac6a634a2ac2dbd748a14ffbeb75/e6fcd/visualize\_files/figure-html/unnamed-chunk-38-2.png)

    The identity position adjustment is more useful for 2d geoms, like points, where it is the default.
*   `position = "fill"` works like stacking, but makes each set of stacked bars the same height. This makes it easier to compare proportions across groups.

    ```
    ggplot(data = diamonds) + 
      geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill")Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/b714164c434b166e783ee626f26a0272e21d77c3/b0cb0/visualize\_files/figure-html/unnamed-chunk-39-1.png)
*   `position = "dodge"` places overlapping objects directly _beside_ one another. This makes it easier to compare individual values.

    ```
    ggplot(data = diamonds) + 
      geom_bar(mapping = aes(x = cut, fill = clarity), position = "dodge")Copy
    ```

    ![](https://d33wubrfki0l68.cloudfront.net/df1b0254da67bacb41a51381d310864d3a5a7dc7/10834/visualize\_files/figure-html/unnamed-chunk-40-1.png)

There’s one other type of adjustment that’s not useful for bar charts, but it can be very useful for scatterplots. Recall our first scatterplot. Did you notice that the plot displays only 126 points, even though there are 234 observations in the dataset?

![](https://d33wubrfki0l68.cloudfront.net/91aebc6de4de928abc810433b752274ba6a46d58/acd97/visualize\_files/figure-html/unnamed-chunk-41-1.png)

The values of `hwy` and `displ` are rounded so the points appear on a grid and many points overlap each other. This problem is known as **overplotting**. This arrangement makes it hard to see where the mass of the data is. Are the data points spread equally throughout the graph, or is there one special combination of `hwy` and `displ` that contains 109 values?

You can avoid this gridding by setting the position adjustment to “jitter”. `position = "jitter"` adds a small amount of random noise to each point. This spreads the points out because no two points are likely to receive the same amount of random noise.

```
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), position = "jitter")Copy
```

![](https://d33wubrfki0l68.cloudfront.net/74df0885fea263841788c5f4742ac79dde6e023c/8142b/visualize\_files/figure-html/unnamed-chunk-42-1.png)

Adding randomness seems like a strange way to improve your plot, but while it makes your graph less accurate at small scales, it makes your graph _more_ revealing at large scales. Because this is such a useful operation, ggplot2 comes with a shorthand for `geom_point(position = "jitter")`: `geom_jitter()`.

To learn more about a position adjustment, look up the help page associated with each adjustment: `?position_dodge`, `?position_fill`, `?position_identity`, `?position_jitter`, and `?position_stack`.
