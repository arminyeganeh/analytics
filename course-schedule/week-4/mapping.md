# Mapping

## Mapping

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with mapping points in R using geographical coordinates. **Submission:** Create a Location Map for the neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service locations. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

### **Task 1: Obtain a geographic boundary file**

The cartographic boundary files are simplified representations of selected geographic areas from the Census Bureau’s MAF/TIGER geographic database. These boundary files are specifically designed for small**-**scale thematic mapping. The cartographic boundary files are available in shapefile and KML format. A shapefile is a geospatial data format for use in geographic information system (GIS) software.

* [ ] Visit [Census Shapefiles](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)
* [ ] &#x20;Scroll down to Census Tracts dropdown menu and select your project's state
* [ ] Move the downloaded file to your working directory. Hint: `getwd()`

Check out the following page to obtain a Census Tract&#x20;

### **Task 2: Create a county map**

````r
```{mapping, echo=TRUE}

# Package rgdal helps match data coordinates to the base map's projection 
install.packages('rgdal') 
library(rgdal)

library("ggmap")
library("ggplot2")

# Set Google Maps API Key into ggmap
ggmap::register_google(key="PASTE YOUR GOOGLE MAPS API KEY HERE")

# Store a Google base map
basemap <- get_map('Montgomery County, VA',     # Set your county or city
                   source="stamen", 
                   maptype="terrain", 
                   zoom = 10)

# Read a tract shapefile
unzip("CENSUS TRACT FILE.zip")
market <- readOGR(".","SHAPE FILE NAME WITHOUT .shp EXT")  # Set tract boundary file
market <- market[market$COUNTYFP=='COUNTY FIPS CODE',]
market <- spTransform(market, CRS("+proj=longlat +datum=WGS84"))
market <- fortify(market)

marketmap <- ggmap(basemap)+
             geom_polygon(aes(x=long, y=lat, group=group), 
                          fill='red', 
                          size=.2,
                          color='red', 
                          data= market, 
                          alpha=.1)

# Check to see if the map is correct
marketmap                                      
                          
```
````

