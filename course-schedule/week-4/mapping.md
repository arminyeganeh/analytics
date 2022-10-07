# Mapping

## Mapping

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with mapping points in R using geographical coordinates. **Submission:** Create a Location Map for the neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service locations. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

### **Task 1: Obtain a geographic boundary file**

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
market <- readOGR(".","cb_2018_51_tract_500k")  # Set your tract boundary file
market <- market[market$COUNTYFP=='121',]
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

