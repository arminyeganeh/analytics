# Good Graphs

"The starting principle is that all of our decisions should be guided by the usage of the graph. The usage of a graph is a summary concept to capture what we want to show and to whom. Its main elements are purpose, focus, and audience. **Table** explains these concepts and gives some examples ... Once usage is clear, the first set of decisions to make are about how we convey information: how to show what we want to show. For those decisions it is helpful to understand the entire graph as the overlay of three graphical objects:&#x20;

1 . Geometric object: the geometric visualization of the information we want to convey, such as a set of bars, a set of points, a line; multiple geometric objects may be combined.

Scaffolding: elements that support understanding the geometric object, such as axes, labels, and legends.

Annotation: adding anything else to emphasize specific values or explain more detail.&#x20;

Table 3.1 Usage of a graph Concept Explanation Typical cases Examples Purpose Focus Audience The message that the graph should convey One graph, one message To whom the graph wants to convey its message Main conclusion of the analysis An important feature of the data Documenting many features of a variable Multiple related graphs for multiple messages Wide audience Non-data-analysts with domain knowledge Analysts y and x are positively associated There are extreme values of y at the right tail of the distribution All potentially important properties of the distribution of y A histogram of ythat identifies extreme values, plus a box plot of y that summarizes many other features of its distribution Journalists Decision makers Fellow data analysts, or our future selves when we want to reproduce the analysis&#x20;

When we design a graph, there are many decisions to make. In particular, we need to decide how the information is conveyed: we need to choose a geometric object, which is the main object of our graph that visualizes the information we want to show. The geometric object is often abbreviated as a geom. The same information may be conveyed with the help of different geometric objects, such as bar charts for a histogram or a curved line for a density plot. In practice, a graph may contain more than one geometric object, such as a set of points together with a line, or multiple lines. Choosing the details of the geometric object, or objects, is called encoding. Encoding is about how we convey the information we want using the data we have, and it means choosing elements such as height, position, color shade. For a graph with a set of bars as the geometric object, the information may be encoded in the height of these bars. But we need to make additional choices, too. These include general ones such as color and shade as well as choices specific to the geometric object, such as width of the bars or lines, or size of the dots. In principle, a graph can be built up freely from all kinds of graphical objects. Data visualization experts and designers tend to follow this bottom-up approach. In contrast, most data analysts start with choosing a predefined type of graph: a single geometric object or a set of some geometric objects and a scaffolding, possibly some annotation. One graph type is the histogram that we introduced earlier in this chapter. Histograms are made of a set of bars as a geometric object, with the information in the data (frequency) encoded in the height of the ban The scaffolding includes an x axis with information on the bins, and a y axis denoting either the absolute (count) or relative frequency (percent). We'll introduce many more graph types in this chapter and subsequent chapters of the textbook. Table 3.2 offers some details and advice. The next step is scaffolding: deciding on the supporting features of the graph such as axes, labels, and titles. This decision includes content as well as format, such as font type and size. Table 3.3 summarizes and explains the most important elements of scaffolding. Table 3.2 The geometric object, its encoding, and graph types Concept General advice Examples Geometric object One or more geoms Encoding Graph type Pick an object suitable for the information to be conveyed May combine more than one geom to support message or add context Pick one encoding only Can pick a standard object to convey information Set of bars comparing quantity A line showing value over time Dots for the values of a time series variable over time, together with a trend line Histogram: height of bars encodes information (frequency) Don't apply different colors or shades Histogram: bars to show frequency Scatterplot: values of two variables shown as a set of dots Table 3.3 Scaffolding Element General advice Examples Graph title Title should be part of the text; it Swimming pool ticket sales and temperature should be short emphasizing main message Swimming pool sales fluctuate with weather Axis title Each axis should have a title, with the Distance to city center (miles) name of the actual variable and unit of measurement Household income (thousand US dollars) Axis labels Value numbers on each axis, next to 0, 2, 4, 6, 8, 10, 12 14 miles for distance to city tics, should be easy to read center Gridlines Add horizontal and, if applicable, Vertical gridlines for histogram vertical gridlines to help reading off Both horizontal and vertical gridlines for numbers scatterplots Legends Add legend to explain different Two groups, such as "male" and "female" elements of the geometric object Time series graphs for two variables; it's best to Legends are best if next to the element they explain put legends next to each line Fonts Large enough size so the audience can read them Font size "10"

Lastly, we may add annotation — if there is something else we want to add or emphasize. Such additional information can help put the graph into context, emphasize some part of it. The two main elements of annotation are notes and visual emphasis, see Table 3.4 for some advice.