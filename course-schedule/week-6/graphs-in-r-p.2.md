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



* [**4** Line Graphs](https://r-graphics.org/CHAPTER-LINE-GRAPH.html)
  * [**4.1** Making a Basic Line Graph](https://r-graphics.org/RECIPE-LINE-GRAPH-BASIC-LINE.html)
  * [**4.2** Adding Points to a Line Graph](https://r-graphics.org/RECIPE-LINE-GRAPH-POINTS.html)
  * [**4.3** Making a Line Graph with Multiple Lines](https://r-graphics.org/RECIPE-LINE-GRAPH-MULTIPLE-LINE.html)
  * [**4.4** Changing the Appearance of Lines](https://r-graphics.org/RECIPE-LINE-GRAPH-LINE-APPEARANCE.html)
  * [**4.5** Changing the Appearance of Points](https://r-graphics.org/RECIPE-LINE-GRAPH-POINT-APPEARANCE.html)
  * [**4.6** Making a Graph with a Shaded Area](https://r-graphics.org/RECIPE-LINE-GRAPH-AREA.html)
  * [**4.7** Making a Stacked Area Graph](https://r-graphics.org/RECIPE-LINE-GRAPH-STACKED-AREA.html)
  * [**4.8** Making a Proportional Stacked Area Graph](https://r-graphics.org/RECIPE-LINE-GRAPH-PROPORTIONAL-STACKED-AREA.html)
  * [**4.9** Adding a Confidence Region](https://r-graphics.org/RECIPE-LINE-GRAPH-REGION.html)
* [**5** Scatter Plots](https://r-graphics.org/CHAPTER-SCATTER.html)
  * [**5.1** Making a Basic Scatter Plot](https://r-graphics.org/RECIPE-SCATTER-BASIC-SCATTER.html)
  * [**5.2** Grouping Points Together using Shapes or Colors](https://r-graphics.org/RECIPE-SCATTER-GROUPED-SCATTER.html)
  * [**5.3** Using Different Point Shapes](https://r-graphics.org/RECIPE-SCATTER-SHAPES.html)
  * [**5.4** Mapping a Continuous Variable to Color or Size](https://r-graphics.org/RECIPE-SCATTER-CONTINUOUS-SCATTER.html)
  * [**5.5** Dealing with Overplotting](https://r-graphics.org/RECIPE-SCATTER-OVERPLOT.html)
  * [**5.6** Adding Fitted Regression Model Lines](https://r-graphics.org/RECIPE-SCATTER-FITLINES.html)
  * [**5.7** Adding Fitted Lines from an Existing Model](https://r-graphics.org/RECIPE-SCATTER-FITLINES-MODEL.html)
  * [**5.8** Adding Fitted Lines from Multiple Existing Models](https://r-graphics.org/RECIPE-SCATTER-FITLINES-MODEL-MULTI.html)
  * [**5.9** Adding Annotations with Model Coefficients](https://r-graphics.org/RECIPE-SCATTER-FITLINES-TEXT.html)
  * [**5.10** Adding Marginal Rugs to a Scatter Plot](https://r-graphics.org/RECIPE-SCATTER-RUG.html)
  * [**5.11** Labeling Points in a Scatter Plot](https://r-graphics.org/RECIPE-SCATTER-LABELS.html)
  * [**5.12** Creating a Balloon Plot](https://r-graphics.org/RECIPE-SCATTER-BALLOON.html)
  * [**5.13** Making a Scatter Plot Matrix](https://r-graphics.org/RECIPE-SCATTER-SPLOM.html)
* [**6** Summarized Data Distributions](https://r-graphics.org/CHAPTER-DISTRIBUTION.html)
  * [**6.1** Making a Basic Histogram](https://r-graphics.org/RECIPE-DISTRIBUTION-BASIC-HIST.html)
  * [**6.2** Making Multiple Histograms from Grouped Data](https://r-graphics.org/RECIPE-DISTRIBUTION-MULTI-HIST.html)
  * [**6.3** Making a Density Curve](https://r-graphics.org/RECIPE-DISTRIBUTION-BASIC-DENSITY.html)
  * [**6.4** Making Multiple Density Curves from Grouped Data](https://r-graphics.org/RECIPE-DISTRIBUTION-MULTI-DENSITY.html)
  * [**6.5** Making a Frequency Polygon](https://r-graphics.org/RECIPE-DISTRIBUTION-FREQPOLY.html)
  * [**6.6** Making a Basic Box Plot](https://r-graphics.org/RECIPE-DISTRIBUTION-BASIC-BOXPLOT.html)
  * [**6.7** Adding Notches to a Box Plot](https://r-graphics.org/RECIPE-DISTRIBUTION-BOXPLOT-NOTCH.html)
  * [**6.8** Adding Means to a Box Plot](https://r-graphics.org/RECIPE-DISTRIBUTION-BOXPLOT-MEAN.html)
  * [**6.9** Making a Violin Plot](https://r-graphics.org/RECIPE-DISTRIBUTION-VIOLIN.html)
  * [**6.10** Making a Dot Plot](https://r-graphics.org/RECIPE-DISTRIBUTION-DOT-PLOT.html)
  * [**6.11** Making Multiple Dot Plots for Grouped Data](https://r-graphics.org/RECIPE-DISTRIBUTION-DOT-PLOT-MULTI.html)
  * [**6.12** Making a Density Plot of Two-Dimensional Data](https://r-graphics.org/RECIPE-DISTRIBUTION-DENSITY2D.html)
* [**7** Annotations](https://r-graphics.org/CHAPTER-ANNOTATE.html)
  * [**7.1** Adding Text Annotations](https://r-graphics.org/RECIPE-ANNOTATE-TEXT.html)
  * [**7.2** Using Mathematical Expressions in Annotations](https://r-graphics.org/RECIPE-ANNOTATE-TEXT-MATH.html)
  * [**7.3** Adding Lines](https://r-graphics.org/RECIPE-ANNOTATE-LINES.html)
  * [**7.4** Adding Line Segments and Arrows](https://r-graphics.org/RECIPE-ANNOTATE-SEGMENT.html)
  * [**7.5** Adding a Shaded Rectangle](https://r-graphics.org/RECIPE-ANNOTATE-RECT.html)
  * [**7.6** Highlighting an Item](https://r-graphics.org/RECIPE-ANNOTATE-HIGHLIGHT.html)
  * [**7.7** Adding Error Bars](https://r-graphics.org/RECIPE-ANNOTATE-ERROR-BAR.html)
  * [**7.8** Adding Annotations to Individual Facets](https://r-graphics.org/RECIPE-ANNOTATE-FACET.html)
* [**8** Axes](https://r-graphics.org/CHAPTER-AXES.html)
  * [**8.1** Swapping X- and Y-Axes](https://r-graphics.org/RECIPE-AXES-SWAP-AXES.html)
  * [**8.2** Setting the Range of a Continuous Axis](https://r-graphics.org/RECIPE-AXES-RANGE.html)
  * [**8.3** Reversing a Continuous Axis](https://r-graphics.org/RECIPE-AXES-REVERSE.html)
  * [**8.4** Changing the Order of Items on a Categorical Axis](https://r-graphics.org/RECIPE-AXIS-ORDER.html)
  * [**8.5** Setting the Scaling Ratio of the X- and Y-Axes](https://r-graphics.org/RECIPE-AXES-SCALE.html)
  * [**8.6** Setting the Positions of Tick Marks](https://r-graphics.org/RECIPE-AXES-SET-TICKS.html)
  * [**8.7** Removing Tick Marks and Labels](https://r-graphics.org/RECIPE-AXIS-REMOVE-TICKS.html)
  * [**8.8** Changing the Text of Tick Labels](https://r-graphics.org/RECIPE-AXES-TICK-LABEL.html)
  * [**8.9** Changing the Appearance of Tick Labels](https://r-graphics.org/RECIPE-AXES-TICK-LABEL-APPEARANCE.html)
  * [**8.10** Changing the Text of Axis Labels](https://r-graphics.org/RECIPE-AXES-AXIS-LABEL.html)
  * [**8.11** Removing Axis Labels](https://r-graphics.org/RECIPE-AXES-AXIS-LABEL-REMOVE.html)
  * [**8.12** Changing the Appearance of Axis Labels](https://r-graphics.org/RECIPE-AXES-AXIS-LABEL-APPEARANCE.html)
  * [**8.13** Showing Lines Along the Axes](https://r-graphics.org/RECIPE-AXES-AXIS-LINES.html)
  * [**8.14** Using a Logarithmic Axis](https://r-graphics.org/RECIPE-AXES-AXIS-LOG.html)
  * [**8.15** Adding Ticks for a Logarithmic Axis](https://r-graphics.org/RECIPE-AXES-AXIS-LOG-TICKS.html)
  * [**8.16** Making a Circular Plot](https://r-graphics.org/RECIPE-AXES-POLAR.html)
  * [**8.17** Using Dates on an Axis](https://r-graphics.org/RECIPE-AXES-AXIS-DATE.html)
  * [**8.18** Using Relative Times on an Axis](https://r-graphics.org/RECIPE-AXES-TIME-REL.html)
* [**9** Controlling the Overall Appearance of Graphs](https://r-graphics.org/CHAPTER-APPEARANCE.html)
  * [**9.1** Setting the Title of a Graph](https://r-graphics.org/RECIPE-APPEARANCE-TITLE.html)
  * [**9.2** Changing the Appearance of Text](https://r-graphics.org/RECIPE-APPEARANCE-TEXT-APPEARANCE.html)
  * [**9.3** Using Themes](https://r-graphics.org/RECIPE-APPEARANCE-THEME.html)
  * [**9.4** Changing the Appearance of Theme Elements](https://r-graphics.org/RECIPE-APPEARANCE-THEME-MODIFY.html)
  * [**9.5** Creating Your Own Themes](https://r-graphics.org/RECIPE-APPEARANCE-THEME-CREATE.html)
  * [**9.6** Hiding Grid Lines](https://r-graphics.org/RECIPE-APPEARANCE-HIDE-GRIDLINES.html)
* [**10** Legends](https://r-graphics.org/CHAPTER-LEGEND.html)
  * [**10.1** Removing the Legend](https://r-graphics.org/RECIPE-LEGEND-REMOVE.html)
  * [**10.2** Changing the Position of a Legend](https://r-graphics.org/RECIPE-LEGEND-POSITION.html)
  * [**10.3** Changing the Order of Items in a Legend](https://r-graphics.org/RECIPE-LEGEND-ORDER.html)
  * [**10.4** Reversing the Order of Items in a Legend](https://r-graphics.org/RECIPE-LEGEND-REVERSE.html)
  * [**10.5** Changing a Legend Title](https://r-graphics.org/RECIPE-LEGEND-TITLE-TEXT.html)
  * [**10.6** Changing the Appearance of a Legend Title](https://r-graphics.org/RECIPE-LEGEND-TITLE-APPEARANCE.html)
  * [**10.7** Removing a Legend Title](https://r-graphics.org/RECIPE-LEGEND-TITLE-REMOVE.html)
  * [**10.8** Changing the Labels in a Legend](https://r-graphics.org/RECIPE-LEGEND-LABEL-TEXT.html)
  * [**10.9** Changing the Appearance of Legend Labels](https://r-graphics.org/RECIPE-LEGEND-LABEL-APPEARANCE.html)
  * [**10.10** Using Labels with Multiple Lines of Text](https://r-graphics.org/RECIPE-LEGEND-LABEL-MULTILINE.html)
* [**11** Facets](https://r-graphics.org/CHAPTER-FACET.html)
  * [**11.1** Splitting Data into Subplots with Facets](https://r-graphics.org/RECIPE-FACET-BASIC.html)
  * [**11.2** Using Facets with Different Axes](https://r-graphics.org/RECIPE-FACET-FREE.html)
  * [**11.3** Changing the Text of Facet Labels](https://r-graphics.org/RECIPE-FACET-LABEL-TEXT.html)
  * [**11.4** Changing the Appearance of Facet Labels and Headers](https://r-graphics.org/RECIPE-FACET-LABEL-APPEARANCE.html)
* [**12** Using Colors in Plots](https://r-graphics.org/CHAPTER-COLORS.html)
  * [**12.1** Setting the Colors of Objects](https://r-graphics.org/RECIPE-COLORS-SETTING.html)
  * [**12.2** Representing Variables with Colors](https://r-graphics.org/RECIPE-COLORS-MAPPING.html)
  * [**12.3** Using a Colorblind-Friendly Palette](https://r-graphics.org/RECIPE-COLORS-PALETTE-DISCRETE-COLORBLIND.html)
  * [**12.4** Using a Different Palette for a Discrete Variable](https://r-graphics.org/RECIPE-COLORS-PALETTE-DISCRETE.html)
  * [**12.5** Using a Manually Defined Palette for a Discrete Variable](https://r-graphics.org/RECIPE-COLORS-PALETTE-DISCRETE-MANUAL.html)
  * [**12.6** Using a Manually Defined Palette for a Continuous Variable](https://r-graphics.org/RECIPE-COLORS-PALETTE-CONTINUOUS.html)
  * [**12.7** Coloring a Shaded Region Based on Value](https://r-graphics.org/RECIPE-COLORS-AREA-VALUE.html)
* [**13** Miscellaneous Graphs](https://r-graphics.org/CHAPTER-MISCGRAPH.html)
  * [**13.1** Making a Correlation Matrix](https://r-graphics.org/RECIPE-MISCGRAPH-CORRMATRIX.html)
  * [**13.2** Plotting a Function](https://r-graphics.org/RECIPE-MISCGRAPH-FUNCTION.html)
  * [**13.3** Shading a Subregion Under a Function Curve](https://r-graphics.org/RECIPE-MISCGRAPH-FUNCTION-SHADE.html)
  * [**13.4** Creating a Network Graph](https://r-graphics.org/RECIPE-MISCGRAPH-GRAPH.html)
  * [**13.5** Using Text Labels in a Network Graph](https://r-graphics.org/RECIPE-MISCGRAPH-GRAPH-LABEL.html)
  * [**13.6** Creating a Heat Map](https://r-graphics.org/RECIPE-MISCGRAPH-HEATMAP.html)
  * [**13.7** Creating a Three-Dimensional Scatter Plot](https://r-graphics.org/RECIPE-MISCGRAPH-3D-SCATTER.html)
  * [**13.8** Adding a Prediction Surface to a Three-Dimensional Plot](https://r-graphics.org/RECIPE-MISCGRAPH-3D-SCATTER-MODEL.html)
  * [**13.9** Saving a Three-Dimensional Plot](https://r-graphics.org/RECIPE-MISCGRAPH-3D-SAVE.html)
  * [**13.10** Animating a Three-Dimensional Plot](https://r-graphics.org/RECIPE-MISCGRAPH-3D-ANIMATE.html)
  * [**13.11** Creating a Dendrogram](https://r-graphics.org/RECIPE-MISCGRAPH-DENDROGRAM.html)
  * [**13.12** Creating a Vector Field](https://r-graphics.org/RECIPE-MISCGRAPH-VECTORFIELD.html)
  * [**13.13** Creating a QQ Plot](https://r-graphics.org/RECIPE-MISCGRAPH-QQ.html)
  * [**13.14** Creating a Graph of an Empirical Cumulative Distribution Function](https://r-graphics.org/RECIPE-MISCGRAPH-ECDF.html)
  * [**13.15** Creating a Mosaic Plot](https://r-graphics.org/RECIPE-MISCGRAPH-MOSAIC.html)
  * [**13.16** Creating a Pie Chart](https://r-graphics.org/RECIPE-MISCGRAPH-PIE.html)
  * [**13.17** Creating a Map](https://r-graphics.org/RECIPE-MISCGRAPH-MAP.html)
  * [**13.18** Creating a Choropleth Map](https://r-graphics.org/RECIPE-MISCGRAPH-CHOROPLETH.html)
  * [**13.19** Making a Map with a Clean Background](https://r-graphics.org/RECIPE-MISCGRAPH-MAP-BACKGROUND.html)
  * [**13.20** Creating a Map from a Shapefile](https://r-graphics.org/RECIPE-MISCGRAPH-MAP-SHAPEFILE.html)

[\
](https://r-graphics.org/preface.html)

