---
description: Assignment 3, 450-600 lines, 3 hours to complete
---

# Distance Table

### Instructions

**Objective:** This objective of this assignment is to familiarize learners with batch geocoding with R and Google Geocoding API. Geocoding refers to providing geographical coordinates corresponding to a location on earth. **Submission:** Create a Distance Table for the neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

| Service       | Name              | Address | Latitude | Longitude | Distance |
| ------------- | ----------------- | ------- | -------- | --------- | -------- |
| Dining        | Fast food 1       |         |          |           |          |
|               | Fast food 2       |         |          |           |          |
|               | Restaurant 1      |         |          |           |          |
|               | Restaurant 2      |         |          |           |          |
| Education     | Public library    |         |          |           |          |
|               | Elementary school |         |          |           |          |
|               | High school       |         |          |           |          |
|               | College           |         |          |           |          |
| Finance       | Bank 1            |         |          |           |          |
|               | Bank 2            |         |          |           |          |
| Fire fighting | Fire station      |         |          |           |          |
| Healthcare    | Hospital          |         |          |           |          |
|               | Medical care 1    |         |          |           |          |
|               | Medical care 2    |         |          |           |          |
|               | Pharmacy 1        |         |          |           |          |
|               | Pharmacy 2        |         |          |           |          |
| Recreation    | Recreation 1      |         |          |           |          |
|               | Recreation 2      |         |          |           |          |
| Security      | Police station    |         |          |           |          |
| Shipping      | Postal service 1  |         |          |           |          |
|               | Postal service 2  |         |          |           |          |
| Shopping      | Convenience 1     |         |          |           |          |
|               | Grocery 1         |         |          |           |          |
|               | Grocery 2         |         |          |           |          |
|               | Merchandise 1     |         |          |           |          |
|               | Merchandise 2     |         |          |           |          |

#### Task 1: Create the distance table

Use Google to detect the addresses of the neighborhood services and record the addresses in a data frame in an R Markdown file. The following code can assist:

````r
```{r services, echo=FALSE}
# Create a data frame, including columns for services, names, and addresses 
table<-data.frame(service=c("Dining",
                            "Dining",
                            "Dining",
                            "Dining",
                            "Education",
                            "Education",
                            "Education",
                            "Education"),
                  name=c("Wendy's",
                         "Black Salt",
                         "Cellar Restaurant",
                         "Cabo Fish Taco",
                         "MFRL - Blacksburg Library",
                         "Gilbert Linkous Elementary School",
                         "Blacksburg High school",
                         "Pamplin College of Business"),
                  address=c("701 N Main St, Blacksburg, VA 24060",
                            "610 N Main St suite a, Blacksburg, VA 24060",
                            "302 N Main St, Blacksburg, VA 24060",
                            "117 S Main St, Blacksburg, VA 24060",
                            "200 Miller St SW, Blacksburg, VA 24060",
                            "813 Toms Creek Rd, Blacksburg, VA 24060",
                            "3401 Bruin Ln, Blacksburg, VA 24060",
                            "1030 Pamplin Hall, Blacksburg, VA 24061"))

# Add columns for latitude, longitude, and distance 
table['latitude']<-NA
table['longitude']<-NA
table['distance']<-NA
```
````

#### Task 2: Obtain a Geocoding API from Google

Geocoding can be simply achieved in R using the function `geocode()` of the package `ggmap`. The geocode function uses Google Geocoding API to turn addresses from text to latitude and longitude pairs. Geographic coordinates are used to place markers or position the map. This API also converts geographic coordinates into an address (reverse geocoding). However, the Google Geocoding API service requires individual sign-up and billing enablement. The billing enablement is a bit downer, but, the number of addresses in this assignment is far below the limit at which pricing starts, which means this assignment can be performed at zero cost. To obtain your individual API:

* [ ] Visit [https://console.cloud.google.com](https://console.cloud.google.com) and create a new project
* [ ] Navigate to APIs & Services and search for and enable Google Geocoding API.
* [ ] Register your credit card information and obtain a Google Geocoding API key.

#### Task 3: Geocoding in R with the package `ggmap`

Once the API key is obtained, Google to detect the addresses of the neighborhood services and record the addresses in a data frame in an R Markdown file. The following code can assist:

````r
```{geocoding test, echo=FALSE}
# Install and load the packages "ggmap" and "ggplot2"
install.packages("ggmap")
install.packages("ggplot2")

library("ggmap")
library("ggplot2")

#Set Google Geocoding API Key
ggmap::register_google(key="PASTE YOUR Google Geocoding API KEY HERE")

#Test Google Geocoding API Key
test<- geocode("Google Building 40, 1600 Amphitheatre Pkwy, Mountain View, CA 94043")

test
## A tibble: 1 x 2
##  lon   lat
##  <dbl> <dbl>
##1 -122.  37.4
````

The result of the function `geocode` of the package `ggmap` is a tibble. Therefore, what we called `test` in the code block above, is a tibble, a modern reimagining of the data frame. For simplicity, let's treat it as a data frame. The following code block is a loop, using which we can geocode all the addresses in the data frame `table` that we have already created. After running this code, you should be able to see the coordinates of the addresses added to the data frame columns `latitude` and `longitude`.

````
```{geocoding addresses, echo=FALSE}

for (i in 1:nrow(table)) {
test<-geocode(table$address[i])
table$longitude[i]<-test$'lon'[1]
table$latitude[i]<-test$'lat'[1]
}
```
````

#### Task 4: Computing distances

