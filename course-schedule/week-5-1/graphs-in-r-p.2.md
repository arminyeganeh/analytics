---
description: Reading 6, 450-600 lines, 3 hours to complete
---

# Graphs in R P.2

<figure><img src="../../.gitbook/assets/essential_graphs (2).png" alt=""><figcaption></figcaption></figure>

### Bar Graph

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

### Group Bar Graph

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

### Using colors in a bar graph

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

### Stacked Bar Graph

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





