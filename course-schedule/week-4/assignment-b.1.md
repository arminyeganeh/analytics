---
description: Week 3, Reading Instructions, 50 pages, 2 hours to complete
---

# Mapping

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with mapping spatial data in R. **Submission:** Create a map for the Distance Table to show the project site and neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

The package `ggmap` can be used jointly with `ggplot2` to map spatial data in R. Before using this package, log in to your account at [https://console.cloud.google.com](https://console.cloud.google.com) and enable the Maps Static API service. Once this service is enabled, it shows up in the Enabled APIs list of the APIs section. You can use the same Google Maps API key that you used previously to source base maps for this assignment from Google.&#x20;

#### Task 1: Map the project site and services

Open a new R Markdown file and check to see if the data frame `table` which you created previously is shown in the Environment panel. The following template can assist you in mapping:

````r
```{services, echo=FALSE}

install.packages("ggrepel") # This package helps create professional map labels

library("ggmap")
library("ggplot2")
library("ggrepel")



````
