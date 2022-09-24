---
description: Assignment 3, 450-600 lines, 3 hours to complete
---

# Distance Table

### Instructions

The purpose of this assignment is to create a Distance Table for the neighborhood services identified in the Site Description assignment using the following template. **Rubric:** Include all the service categories described in the template in close proximity. Submit the R Markdown in a PDF format by the due date. **Hint:** Follow the tasks described below.

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

#### Task 1

Using Google, detect the addresses of the neighborhood services identified in the Site Description assignment and record the addresses in R Markdown using the following sample code block:

````r
```{r services, echo=FALSE}
# Create a data frame for services 
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

table['latitude']<-NA
table['longitude']<-NA
table['distance']<-NA
```
````

&#x20;and fill out the following Distance Table in R Markdown.&#x20;

* [ ] Images should be located in the same directory as `Rmd.` using `getwd()` and `setwd()` .&#x20;

****

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
