---
description: Week 3, Reading Instructions, 50 pages, 2 hours to complete
---

# Mapping

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with mapping spatial data in R. **Submission:** Create a map for the Distance Table to show the project site and neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

#### Task 1: Map the project site and services

The package `ggmap` can be used jointly with `ggplot2` to map spatial data in R.  Open a new R Markdown file and check to see if the data frame `table` which you created previously is shown in the Environment panel. The following template can assist you in mapping:

````r
```{project site, echo=FALSE}

install.packages("ggrepel") # This package creates professional map labels

library("ggmap")
library("ggplot2")
library("ggrepel")

# If not in the environment, load the data frame table saved as CSV 
table <- read.csv("table.csv"); table <- tab[-1]  

# Record project site for mapping
site <- data.frame(service = NA,
                   name = 'Project Site',
                   address = 'PASTE YOUR SITE ADRESS HERE',
                   latitude = PASTE YOUR SITE LAT HERE,
                   longitude = PASTE YOUR SITE LON HERE,
                   distance = NA,
                   walkingtime = NA)

# Record a quick neighborhood map using the function qmplot of ggmap
neighborhood <- qmplot(longitude,
                       latitude,
                       data = rbind(table, site),
                       maptype = "roadmap",
                       source = "google",
                       color = I("red"))

# Use the recorded neighborhood map and add site and service points and labels
neighborhood +                            # Create the base neighborhood map
  geom_point(aes(x = longitude,           # Create the site and service points
                 y = latitude),
             colour = 'red',
             data = rbind(table,site),
             size = 3) +
  geom_label_repel(aes(longitude,         # Create the labels
                 latitude,
                 label = name),
            data = rbind(table,site),
            family = 'Times', 
            size = 3, 
            box.padding = 0.2, 
            point.padding = 0.3,
            segment.color = 'grey50') +
  geom_point(aes(x = -80.42404818858138,  # Add a larger, square-shaped site point 
               y = 37.23564687976523),
           colour = 'darkblue',
           shape = 15,
           size = 3)
```
````

#### Task 2: Map the market area

The primary market area for any new residential community is defined as the geographic area from which future residents of the community would primarily be drawn and in which competitive housing alternatives are located. Housing market analysts may also define and analyze a secondary market area. The approximate boundaries of the market areas are typically defined using Census-defined administrative boundaries and the primary market boundary distances from the subject site are typically less than 10 miles.

The US Census Bureau administrative zones can be mapped using cartographic boundary files with the shapefile (.shp) format. A shapefile is a simple, nontopological format for storing the geometric location and attribute information of geographic features. Geographic features in a shapefile can be represented by points, lines, or polygons (areas). The workspace containing shapefiles may also contain dBASE tables, which can store additional attributes that can be joined to a shapefile's features.

To obtain your site's shapefile:

* [ ] Visit [https://www.census.gov/](https://www.census.gov/) and search the page "Cartographic Boundary Files - Shapefile"
* [ ] The first search result is typically the page we are looking for. Click to open the page.
* [ ] Scroll down to find the Census Tracts drop-down menu.
* [ ] Click on the state where the site is located and a zip file will start to download.&#x20;
* [ ] Move the downloaded zip file to your R Markdown working directory.&#x20;
* [ ] If you do not remember the working directory, find it using `getwd()` .&#x20;

````r
```{market area, echo=FALSE}

install.packages("sf") # Use the package Simple Features to read shapefiles into R
                          
library("ggmap")
library("ggplot2")
library("ggrepel")
library("sf")

# Unzip the Census Tract zipped shapefile
unzip("PASTE YOUR FILE NAME HERE.zip")
state <- read_sf("PASTE YOUR FILE NAME HERE.shp")
names(state) <- tolower(names(state))             # Convert column names to lower case

# Subset the state file using the city/county FIPS code
# Find FIPS code here: https://www.census.gov/library/reference/code-lists/ansi.html
county <- state[state$countyfp == "PASTE YOUR CITY/COUNTY FIPS CODE HERE",]

install.packages('rgdal')
library(rgdal)

basemap <- get_map('Montgomery County, VA', source="stamen", maptype="terrain", zoom = 10)

market <- readOGR(".","cb_2018_51_tract_500k")
market <- market[market$COUNTYFP=='121',]
market <- spTransform(market, CRS("+proj=longlat +datum=WGS84"))
market <- fortify(market)

marketmap <- ggmap(basemap)+
             geom_polygon(aes(x=long, y=lat, group=group), 
                          fill='grey', 
                          size=.2,
                          color='green', 
                          data= market, 
                          alpha=.5)

marketmap

```
````

