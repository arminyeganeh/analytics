---
description: Week 3, Reading Instructions, 50 pages, 2 hours to complete
---

# Mapping

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with mapping spatial data in R. **Submission:** Create a map for the Distance Table to show the project site and neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

#### Task 1: Map the project site and services

The package `ggmap` can be used jointly with `ggplot2` to map spatial data in R.  Open a new R Markdown file and check to see if the data frame `table` which you created previously is shown in the Environment panel. The following template can assist you in mapping:

````r
```{services, echo=FALSE}

install.packages("ggrepel") # This package creates professional map labels

library("ggmap")
library("ggplot2")
library("ggrepel")

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
                       data = rbind(table,site),
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

