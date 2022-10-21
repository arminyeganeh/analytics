---
description: Reading 6, 450-600 lines, 3 hours to complete
---

# Graphs in R P.2

<figure><img src="../../.gitbook/assets/essential_graphs (1) (1).png" alt=""><figcaption></figcaption></figure>

### Bar graph

Use `ggplot()` with `geom_col()` and specify what variables you want on the x- and y-axes:

```r
# Intall and load gcookbook for the pg_mean data set
install.packages("gcookbook")
library(gcookbook)  
library(ggplot2)

ggplot(pg_mean, aes(x = group, y = weight)) +
  geom_col()
```

![Bar graph of values with a discrete x-axis](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-BASIC-BAR-1.png)

By default, bar graphs use a dark grey for the bars. To use a color fill, use `fill`. Also, by default, there is no outline around the fill. To add an outline, use `colour`.&#x20;

```
ggplot(pg_mean, aes(x = group, y = weight)) +
  geom_col(fill = "lightblue", colour = "black")
```

![A single fill and outline color for all bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-BASIC-BAR-SINGLE-FILL-1.png)

### Group bar graph

In this example, we’ll use the `cabbage_exp` data set, which has two categorical variables, `Cultivar` and `Date`, and one continuous variable, `Weight`:

```r
library(gcookbook)  # Load gcookbook for the cabbage_exp data set
cabbage_exp
#>   Cultivar Date Weight        sd  n         se
#> 1      c39  d16   3.18 0.9566144 10 0.30250803
#> 2      c39  d20   2.80 0.2788867 10 0.08819171
#> 3      c39  d21   2.74 0.9834181 10 0.31098410
#> 4      c52  d16   2.26 0.4452215 10 0.14079141
#> 5      c52  d20   3.11 0.7908505 10 0.25008887
#> 6      c52  d21   1.47 0.2110819 10 0.06674995

ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(position = "dodge")
```

![Graph with grouped bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-GROUPED-BAR-1.png)

To add a black outline, use `colour = "black"` inside `geom_col()`. To set the colors, you can use `scale_fill_brewer()` or `scale_fill_manual()`. We will use the `Pastel1` palette from `RColorBrewer`:

```
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(position = "dodge", colour = "black") +
  scale_fill_brewer(palette = "Pastel1")
```

![Grouped bars with black outline and a different color palette](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-GROUPED-BAR-OUTLINE-1.png)

Other aesthetics, such as `colour` (the color of the outlines of the bars) or `linestyle`, can also be used for grouping variables, but `fill` is probably what you’ll want to use.

### Colors in bar graph

We’ll use the `uspopchange` data set for this example. It contains the percentage change in population for the US states from 2000 to 2010. We’ll take the top 10 fastest-growing states and graph their percentage change. We’ll also color the bars by region (Northeast, South, North Central, or West).

First, take the top 10 states:

```r
library(gcookbook) # Load gcookbook for the uspopchange data set
library(dplyr)

upc <- uspopchange %>%
  arrange(desc(Change)) %>%
  slice(1:10)

upc
#>             State Abb Region Change
#> 1          Nevada  NV   West   35.1
#> 2         Arizona  AZ   West   24.6
#> 3            Utah  UT   West   23.8
#>  ...<4 more rows>...
#> 8         Florida  FL  South   17.6
#> 9        Colorado  CO   West   16.9
#> 10 South Carolina  SC  South   15.3
```

Now we can make the graph, mapping Region to fill:

```
ggplot(upc, aes(x = Abb, y = Change, fill = Region)) +
  geom_col()
```

![A variable mapped to fill](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-FILL-1.png)

The default colors aren’t the most appealing, so you may want to set them using `scale_fill_brewer()` or `scale_fill_manual()`. With this example, we’ll use the latter, and we’ll set the outline color of the bars to black, with `colour="black"` . Note that _setting_ occurs outside of `aes()`, while _mapping_ occurs within `aes()`:

```r
ggplot(upc, aes(x = reorder(Abb, Change), y = Change, fill = Region)) +
  geom_col(colour = "black") +
  scale_fill_manual(values = c("#669933", "#FFCC66")) +
  xlab("State")
```

![Graph with different colors, black outlines, and sorted by percentage change](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-FILL-MANUAL-1.png)

This example also uses the `reorder()` function to reorder the levels of the factor `Abb` based on the values of `Change`. In this particular case it makes sense to sort the bars by their height, instead of in alphabetical order.

### Negative and positive bars

We’ll use a subset of the climate data and create a new column called pos, which indicates whether the value is positive or negative:

```r
library(gcookbook) # Load gcookbook for the climate data set
library(dplyr)

climate_sub <- climate %>%
  filter(Source == "Berkeley" & Year >= 1900) %>%
  mutate(pos = Anomaly10y >= 0)
  
climate_sub
#>       Source Year Anomaly1y Anomaly5y Anomaly10y Unc10y   pos
#> 1   Berkeley 1900        NA        NA     -0.171  0.108 FALSE
#> 2   Berkeley 1901        NA        NA     -0.162  0.109 FALSE
#> 3   Berkeley 1902        NA        NA     -0.177  0.108 FALSE
#>  ...<99 more rows>...
#> 103 Berkeley 2002        NA        NA      0.856  0.028  TRUE
#> 104 Berkeley 2003        NA        NA      0.869  0.028  TRUE
#> 105 Berkeley 2004        NA        NA      0.884  0.029  TRUE
```

Once we have the data, we can make the graph and map pos to the fill color. Notice that we use position=“identity” with the bars. This will prevent a warning message about stacking not being well-defined for negative numbers:

```r
ggplot(climate_sub, aes(x = Year, y = Anomaly10y, fill = pos)) +
  geom_col(position = "identity")
```

![Different colors for positive and negative values](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-COLOR-NEG-1.png)

There are a few problems with the first attempt. First, the colors are probably the reverse of what we want: usually, blue means cold, and red means hot. Second, the legend is redundant and distracting. We can change the colors with `scale_fill_manual()` and remove the legend with `guide = FALSE`. We’ll also add a thin black outline around each of the bars by setting `colour` and specifying `size`, which is the thickness of the outline (in millimeters):

```
ggplot(climate_sub, aes(x = Year, y = Anomaly10y, fill = pos)) +
  geom_col(position = "identity", colour = "black", size = 0.25) +
  scale_fill_manual(values = c("#CCEEFF", "#FFDDDD"), guide = FALSE)
#> Warning: It is deprecated to specify `guide = FALSE` to remove a guide.
#> Please use `guide = "none"` instead.
```

![Graph with customized colors and no legend](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-COLOR-NEG2-1.png)

### Bar width and spacing

To make the bars narrower or wider, set `width` in `geom_col()`. The default value is 0.9; larger values make the bars wider, and smaller values make the bars narrower. For example, for standard-width bars:

```r
library(gcookbook) # Load gcookbook for the pg_mean data set
ggplot(pg_mean, aes(x = group, y = weight)) +
  geom_col()

# For narrower bars:
ggplot(pg_mean, aes(x = group, y = weight)) +
  geom_col(width = 0.5)
  
# For wider bars (maximum width of 1):
ggplot(pg_mean, aes(x = group, y = weight)) +
  geom_col(width = 1)
```

![Different bar widths](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-WIDTH-1.png)![Different bar widths](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-WIDTH-2.png)![Different bar widths](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-WIDTH-3.png)

For grouped bars, the default is to have no space between bars within each group. To add space between bars within a group, reduce the width and set the value for `position_dodge` to be larger than `width` .

```r
# For a grouped bar graph with narrow bars:
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(width = 0.5, position = "dodge")

# With some space between the bars:
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(width = 0.5, position = position_dodge(0.7))
```

![Bar graph with narrow grouped bars (left); With space between the bars (right)](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-WIDTH-DODGE-1.png)![Bar graph with narrow grouped bars (left); With space between the bars (right)](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-WIDTH-DODGE-2.png)

The first graph used `position = "dodge"`, and the second graph used `position = position_dodge()`. This is because `position = "dodge"` is simply shorthand for `position = position_dodge()` with the default value of 0.9, but when we want to set a specific value, we need to use the more verbose form. The default `width` for bars is 0.9, and the default value used for `position_dodge()` is the same. To be more precise, the value of `width` in `position_dodge()` is `NULL`, which tells ggplot2 to use the same value as the width from `geom_bar()`.

### Stacked bar graph

Use `geom_bar()` and map a variable `fill`. This will put `Date` on the x-axis and use `Cultivar` for the fill color.

```
library(gcookbook) # Load gcookbook for the cabbage_exp data set
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col()
```

![Stacked bar graph](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-STACKED-BAR-1.png)

To understand how the graph is made, it’s useful to see how the data is structured. There are three levels of `Date` and two levels of `Cultivar`, and for each combination there is a value for `Weight`:

```
cabbage_exp
#>   Cultivar Date Weight        sd  n         se
#> 1      c39  d16   3.18 0.9566144 10 0.30250803
#> 2      c39  d20   2.80 0.2788867 10 0.08819171
#> 3      c39  d21   2.74 0.9834181 10 0.31098410
#> 4      c52  d16   2.26 0.4452215 10 0.14079141
#> 5      c52  d20   3.11 0.7908505 10 0.25008887
#> 6      c52  d21   1.47 0.2110819 10 0.06674995
```

By default, the stacking order of the bars is the same as the order of items in the legend. For some data sets it might make sense to reverse the order of the legend. To do this, you can use the `guides` function and specify which aesthetic for which the legend should be reversed. In this case, it’s `fill`:

```
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col() +
  guides(fill = guide_legend(reverse = TRUE))
```

![Stacked bar graph with reversed legend order](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-STACKED-BAR-REVLEVELS-1.png)

If you’d like to reverse the stacking order of the bars, use `position_stack(reverse = TRUE)`. You’ll also need to reverse the order of the legend for it to match the order of the bars. For a more polished graph, we’ll use `scale_fill_brewer()` to get a different color palette and use `colour="black"` to get a black outline as we did earlier.

```
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(position = position_stack(reverse = TRUE)) +
  guides(fill = guide_legend(reverse = TRUE))
```

![Stacked bar graph with reversed stacking order](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-STACKED-BAR-REVSTACK-1.png)

### Bar graph labels

Add `geom_text()` to your graph. It requires a mapping for x, y, and the text itself. By setting `vjust` (the vertical justification), it is possible to move the text above or below the tops of the bars.

```r
library(gcookbook) # Load gcookbook for the cabbage_exp data set

# Below the top
ggplot(cabbage_exp, aes(x = interaction(Date, Cultivar), y = Weight)) +
  geom_col() +
  geom_text(aes(label = Weight), vjust = 1.5, colour = "white")

# Above the top
ggplot(cabbage_exp, aes(x = interaction(Date, Cultivar), y = Weight)) +
  geom_col() +
  geom_text(aes(label = Weight), vjust = -0.2)
```

![Labels under the tops of bars (left); Labels above bars (right)](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-LABEL-1.png)![Labels under the tops of bars (left); Labels above bars (right)](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-LABEL-2.png)

Another common scenario is to add labels for a bar graph of _counts_ instead of values. To do this, use `geom_bar()`, which adds bars whose height is proportional to the number of rows, and then use `geom_text()` with counts. We need to tell `geom_text()` to use the `"count"` statistic to compute the number of rows for each x value, and then, to use those computed counts as the labels, we use the aesthetic mapping `aes(label = ..count..)`.

```
ggplot(mtcars, aes(x = factor(cyl))) +
  geom_bar() +
  geom_text(aes(label = ..count..), stat = "count", vjust = 1.5, colour = "white")
```

![Bar graph of counts with labels under the tops of bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-GRAPH-COUNT-LABEL-1.png)

By setting the vertical justification (`vjust`), the _y_ coordinates of the labels appear below or above the bar tops. One drawback of this is that when the label is above the top of the bar, it can go off the top of the plotting area. To fix this, you can manually set the _y_ limits, or you can set the _y_ positions of the text _above_ the bars and not change the vertical justification. One drawback to changing the text’s _y_ position is that if you want to place the text fully above or below the bar top, the value to add will depend on the _y_ range of the data; in contrast, changing `vjust` to a different value will always move the text the same distance relative to the height of the bar:

```r
# Adjust y limits to be a little higher
ggplot(cabbage_exp, aes(x = interaction(Date, Cultivar), y = Weight)) +
  geom_col() +
  geom_text(aes(label = Weight), vjust = -0.2) +
  ylim(0, max(cabbage_exp$Weight) * 1.05)
  
# Map y positions slightly above bar top - y range of plot will auto-adjust
ggplot(cabbage_exp, aes(x = interaction(Date, Cultivar), y = Weight)) +
  geom_col() +
  geom_text(aes(y = Weight + 0.1, label = Weight))
```

For grouped bar graphs, you also need to specify position=position\_dodge() and give it a value for the dodging width. The default dodge width is 0.9. Because the bars are narrower, you might need to use size to specify a smaller font to make the labels fit. The default value of size is 5, so we’ll make it smaller by using 3.

```
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(position = "dodge") +
  geom_text(
    aes(label = Weight),
    colour = "white", size = 3,
    vjust = 1.5, position = position_dodge(.9)
  )
```

![Labels on grouped bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-LABEL-GROUPED-1.png)

Putting labels on stacked bar graphs requires finding the cumulative sum for each stack. To do this, first make sure the data is sorted properly – if it isn’t, the cumulative sum might be calculated in the wrong order. We’ll use the `arrange()` function from the dplyr package. Note that we have to use the `rev()` function to reverse the order of `Cultivar`:

```r
library(dplyr)
# Sort by the Date and Cultivar columns
ce <- cabbage_exp %>%
  arrange(Date, rev(Cultivar))
```

Once we make sure the data is sorted properly, we’ll use `group_by()` to chunk it into groups by `Date`, then calculate a cumulative sum of `Weight` within each chunk:

```r
# Get the cumulative sum
ce <- ce %>%
  group_by(Date) %>%
  mutate(label_y = cumsum(Weight))
ce
#> # A tibble: 6 × 7
#> # Groups:   Date [3]
#>   Cultivar Date  Weight    sd     n     se label_y
#>   <fct>    <fct>  <dbl> <dbl> <int>  <dbl>   <dbl>
#> 1 c52      d16     2.26 0.445    10 0.141     2.26
#> 2 c39      d16     3.18 0.957    10 0.303     5.44
#> 3 c52      d20     3.11 0.791    10 0.250     3.11
#> 4 c39      d20     2.8  0.279    10 0.0882    5.91
#> 5 c52      d21     1.47 0.211    10 0.0667    1.47
#> 6 c39      d21     2.74 0.983    10 0.311     4.21
ggplot(ce, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col() +
  geom_text(aes(y = label_y, label = Weight), vjust = 1.5, colour = "white")
```

![Labels on stacked bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-LABEL-STACKED-1.png)

When using labels, changes to the stacking order are best done by modifying the order of levels in the factor before taking the cumulative sum. The other method of changing the stacking order, by specifying breaks in a scale, won’t work properly, because the order of the cumulative sum won’t be the same as the stacking order.

To put the labels in the middle of each bar, there must be an adjustment to the cumulative sum, and the _y_ offset in `geom_bar()` can be removed:

```
ce <- cabbage_exp %>%
  arrange(Date, rev(Cultivar))
# Calculate y position, placing it in the middle
ce <- ce %>%
  group_by(Date) %>%
  mutate(label_y = cumsum(Weight) - 0.5 * Weight)
ggplot(ce, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col() +
  geom_text(aes(y = label_y, label = Weight), colour = "white")
```

![Labels in the middle of stacked bars](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-LABEL-STACKED-MIDDLE-1.png)

For a more polished graph, we’ll change the colors, add labels in the middle with a smaller font using `size`, add a “kg” suffix using `paste`, and make sure there are always two digits after the decimal point by using `format()`:

```r
ggplot(ce, aes(x = Date, y = Weight, fill = Cultivar)) +
  geom_col(colour = "black") +
  geom_text(aes(y = label_y, label = paste(format(Weight, nsmall = 2), "kg")), size = 4) +
  scale_fill_brewer(palette = "Pastel1")
```

![Customized stacked bar graph with labels](https://r-graphics.org/R-Graphics-Cookbook-2e\_files/figure-html/FIG-BAR-LABEL-STACKED-FINAL-1.png)

### R Graphics Cookbook&#x20;

[R Graphics Cookbook](https://r-graphics.org/) is a practical guide that provides more than 150 recipes to generate high-quality graphs quickly, without having to comb through all the details of R’s graphing systems. Each recipe tackles a specific problem with a solution you can apply to your own project and includes a discussion of how and why the recipe works. You can use the online version of the book for free as a reference to become familiar with a wide range of ggplot2 capabilities as follows. For a comprehensive reference on ggplot2 capabilities, see this [ggplot2 Reference](https://ggplot2.tidyverse.org/reference/index.html).&#x20;

### Line graphs

* Making a basic line graph
* Adding points to a line graph
* Making a line graph with multiple lines
* Changing the appearance of lines
* Changing the appearance of points
* Making a graph with a shaded area
* Making a stacked area graph
* Making a proportional stacked area graph
* Adding a confidence region

### Scatter plots

* Making a basic scatter plot
* Grouping points together using shapes or colors
* Using different point shapes
* Mapping a continuous variable to color or size
* Dealing with overplotting
* Adding fitted regression model lines
* Adding fitted lines from an existing model
* Adding fitted lines from multiple existing models
* Adding annotations with model coefficients
* Adding marginal rugs to a scatter plot
* Labeling points in a scatter plot
* Creating a balloon plot
* Making a scatter plot matrix

### Summarized data distributions

* Making a basic histogram
* Making multiple histograms from grouped data
* Making a density curve
* Making multiple density curves from grouped data
* Making a frequency polygon
* Making a basic box plot
* Adding notches to a box plot
* Adding means to a box plot
* Making a violin plot
* Making a dot plot
* Making multiple dot plots for grouped data
* Making a density plot of two-dimensional data

### Annotations

* Adding text annotations
* Using mathematical expressions in annotations
* Adding lines
* Adding line segments and arrows
* Adding a shaded rectangle
* Highlighting an item
* Adding error bars
* Adding annotations to individual facets

### Axes

* Swapping x- and y-axes
* Setting the range of a continuous axis
* Reversing a continuous axis
* Changing the order of items on a categorical axis
* Setting the scaling ratio of the x- and y-axes
* Setting the positions of tick marks
* Removing tick marks and labels
* Changing the text of tick labels
* Changing the appearance of tick labels
* Changing the text of axis labels
* Removing axis labels
* Changing the appearance of axis labels
* Showing lines along the axes
* Using a logarithmic axis
* Adding ticks for a logarithmic axis
* Making a circular plot
* Using dates on an axis
* Using relative times on an axis

### Controlling the overall appearance of graphs

* Controlling the overall appearance of graphs&#x20;
* Setting the title of a graph
* Changing the appearance of the text
* Using themes
* Changing the appearance of theme elements&#x20;
* Creating your own themes&#x20;
* Hiding grid lines

### Legends&#x20;

* Removing the legend&#x20;
* Changing the position of a legend&#x20;
* Changing the order of items in a legend&#x20;
* Reversing the order of items in a legend&#x20;
* Changing a legend title&#x20;
* Changing the appearance of a legend title&#x20;
* Removing a legend title&#x20;
* Changing the labels in a legend&#x20;
* Changing the appearance of legend labels&#x20;
* Using labels with multiple lines of text

### Facets&#x20;

* Splitting data into subplots with facets&#x20;
* Using facets with different axes&#x20;
* Changing the text of facet labels&#x20;
* Changing the appearance of facet labels and headers&#x20;

### Using colors in plots&#x20;

* Setting the colors of objects&#x20;
* Representing variables with colors&#x20;
* Using a colorblind-friendly palette&#x20;
* Using a different palette for a discrete variable&#x20;
* Using a manually defined palette for a discrete variable&#x20;
* Using a manually defined palette for a continuous variable&#x20;
* Coloring a shaded region based on value 13&#x20;

### Miscellaneous graphs&#x20;

* Making a correlation matrix&#x20;
* Plotting a function&#x20;
* Shading a subregion under a function curve&#x20;
* Creating a network graph&#x20;
* Using text labels in a network graph&#x20;
* Creating a heat map&#x20;
* Creating a three-dimensional scatter plot&#x20;
* Adding a prediction surface to a three-dimensional plot&#x20;
* Saving a three-dimensional plot&#x20;
* Animating a three-dimensional plot&#x20;
* Creating a dendrogram&#x20;
* Creating a vector field&#x20;
* Creating a QQ plot
* Creating a graph of an empirical cumulative distribution function&#x20;
* Creating a mosaic plot&#x20;
* Creating a pie chart&#x20;
* Creating a map&#x20;
* Creating a choropleth map&#x20;
* Making a map with a clean background&#x20;
* Creating a map from a shapefile

[\
](https://r-graphics.org/preface.html)

