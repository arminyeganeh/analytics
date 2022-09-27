---
description: Assignment 3, 450-600 lines, 3 hours to complete
---

# Distance Table

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with batch geocoding and measuring distances in R. Geocoding refers to providing geographical coordinates corresponding to a location on earth. **Submission:** Create a Distance Table for the neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

| Service       | Name              | Address | Lat | Lon | Dis | Time |
| ------------- | ----------------- | ------- | --- | --- | --- | ---- |
| Dining        | Fast food 1       |         |     |     |     |      |
|               | Fast food 2       |         |     |     |     |      |
|               | Restaurant 1      |         |     |     |     |      |
|               | Restaurant 2      |         |     |     |     |      |
| Education     | Public library    |         |     |     |     |      |
|               | Elementary school |         |     |     |     |      |
|               | High school       |         |     |     |     |      |
|               | College           |         |     |     |     |      |
| Finance       | Bank 1            |         |     |     |     |      |
|               | Bank 2            |         |     |     |     |      |
| Fire fighting | Fire station      |         |     |     |     |      |
| Healthcare    | Hospital          |         |     |     |     |      |
|               | Medical care 1    |         |     |     |     |      |
|               | Medical care 2    |         |     |     |     |      |
|               | Pharmacy 1        |         |     |     |     |      |
|               | Pharmacy 2        |         |     |     |     |      |
| Recreation    | Recreation 1      |         |     |     |     |      |
|               | Recreation 2      |         |     |     |     |      |
| Security      | Police station    |         |     |     |     |      |
| Shipping      | Postal service 1  |         |     |     |     |      |
|               | Postal service 2  |         |     |     |     |      |
| Shopping      | Convenience 1     |         |     |     |     |      |
|               | Grocery 1         |         |     |     |     |      |
|               | Grocery 2         |         |     |     |     |      |
|               | Merchandise 1     |         |     |     |     |      |
|               | Merchandise 2     |         |     |     |     |      |

#### Task 1: Create the distance table

Use Google to detect the addresses of the neighborhood services and record the addresses in a data frame in an R Markdown file. The following code can assist:

````r
```{services, echo=TRUE}

# Create a data frame, including columns for services, names, and addresses 
table <- data.frame (service = c("Dining",
                                 "Dining",
                                 "Dining",
                                 "Dining",
                                 "Education",
                                 "Education",
                                 "Education",
                                 "Education"),
                     name = c("Wendy's",
                              "Black Salt",
                              "Cellar Restaurant",
                              "Cabo Fish Taco",
                              "MFRL - Blacksburg Library",
                              "Gilbert Linkous Elementary School",
                              "Blacksburg High school",
                              "Pamplin College of Business"),
                     address = c("701 N Main St, Blacksburg, VA 24060",
                                 "610 N Main St suite a, Blacksburg, VA 24060",
                                 "302 N Main St, Blacksburg, VA 24060",
                                 "117 S Main St, Blacksburg, VA 24060",
                                 "200 Miller St SW, Blacksburg, VA 24060",
                                 "813 Toms Creek Rd, Blacksburg, VA 24060",
                                 "3401 Bruin Ln, Blacksburg, VA 24060",
                                 "1030 Pamplin Hall, Blacksburg, VA 24061"))

# Add columns for latitude, longitude, distance, and walking time 
table['latitude'] <- NA
table['longitude'] <- NA
table['distance'] <- NA
table['walkingtime'] <- NA
```
````

#### Task 2: Obtain a Geocoding API from Google

Geocoding can be simply achieved in R using the function `geocode()` of the package `ggmap`. The geocode function uses Google Geocoding API to turn addresses from text to latitude and longitude pairs. Geographic coordinates are used to place markers or position the map or find addresses (reverse geocoding). The Google Geocoding API service requires individual sign-up and billing enablement. The billing enablement is a bit downer, but, the number of addresses in this assignment is far below the limit at which pricing starts, which means this assignment can be performed at zero cost. To obtain your individual API:

* [ ] Visit [https://console.cloud.google.com](https://console.cloud.google.com) and create a new project
* [ ] Navigate to APIs & Services and search for and enable Google Geocoding API.
* [ ] Register your credit card information and obtain a Google Geocoding API key.

#### Task 3: Geocoding in R with the package `ggmap`

Geographic coordinates are used to place markers or position the map.&#x20;

````r
```{geocoding test, echo=FALSE}

# Install and load the packages "ggmap" and "ggplot2"
install.packages("ggmap")
install.packages("ggplot2")

library("ggmap")
library("ggplot2")

# Set Google Maps API Key
ggmap::register_google(key="PASTE YOUR GOOGLE MAPS API KEY HERE")

# Test Google Geocoding API Service
test <- geocode("Google Building 40, 1600 Amphitheatre Pkwy, Mountain View, CA 94043")

test
## A tibble: 1 x 2
##  lon   lat
##  <dbl> <dbl>
##1 -122.  37.4
````

The result of the function `geocode` of the package `ggmap` is a tibble. Therefore, what we called `test` in the code block above, is a tibble, a modern reimagining of the data frame. For simplicity, let's treat it as a data frame. The following code block is a loop, using which we can geocode all the addresses in the data frame `table` that we have already created. After running this code, you should be able to see the coordinates of the addresses added to the data frame columns `latitude` and `longitude`.

````r
```{batch geocoding, echo=FALSE}

# Batch geocoding
for (i in 1:nrow(table)) {
test <- geocode(table$address[i])
table$longitude[i] <- test$'lon'[1]
table$latitude[i] <- test$'lat'[1]
}
```
````

#### Task 4: Computing distances in R using the package `gmapsdistance`

The package `gmapsdistance` can be used to obtain Google Maps distance and travel times by mode of transportation between an origin and one or more destination points. Before using this package, you need to visit [https://console.cloud.google.com](https://console.cloud.google.com) and enable the Distance Matrix API service. Once this service is enabled, you can use the same Google Maps API key that you used previously to geocode addresses. Use the following code to calculate walking distances and times between your site and service locations:

````r
```{calculating distances, echo=TRUE}

# Install and load the package gmapsdistance
install.packages("gmapsdistance")
library("gmapsdistance")

# Set the API for the package gmapsdistance
set.api.key("PASTE YOUR GOOGLE MAPS API KEY HERE")

# Test and record the first distance and walking time list
distance.mat<- gmapsdistance(origin = "PASTE YOUR FULL SITE ADDRESS HERE",
                             destination = "PASTE YOUR FIRST SERVICE ADDRESS HERE",
                             mode = "walking") # The result is a list format

# Record the first list item, which is distance, converting meters to miles 
table$distance[1]<-distance.mat$Distance[1]/1609

# Record the second list item, which is walking time, converting seconds to minutes 
table$walkingtime[1]<-distance.mat$Time[1]/60 

# Use a for loop to repeat the process for all services
for (i in 1:nrow(table)) { 
  distance.mat<- gmapsdistance(origin = "PASTE YOUR FULL SITE ADDRESS HERE",
                             destination = table$address[i],
                             mode = "walking")
    table$distance[i]<-distance.mat$Distance[1]/1609
  table$walkingtime[i]<-distance.mat$Time[1]/60
}

# Use the package dplyr to round decimals in multiple columns. 
install.packages("dplyr")
library(dplyr)
table %>% mutate(across(where(is.numeric), round, 2))
```
````

Hopefully, this was not too complicated. In the next assignment, we will create maps using the service coordinates. You can learn more about the spatial analysis packages used here by reading reference manuals:

* [geosphere](https://cran.r-project.org/web/packages/geosphere/geosphere.pdf)
* [ggmap](https://cran.r-project.org/web/packages/ggmap/ggmap.pdf)
* [gmapsdistance](https://cran.r-project.org/web/packages/gmapsdistance/gmapsdistance.pdf)
