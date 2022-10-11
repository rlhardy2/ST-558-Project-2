ST 558 Project 2
================
Matthew Sookoo and Rachel Hardy
2022-10-10

-   <a href="#introduction" id="toc-introduction">Introduction</a>
-   <a href="#required-packages" id="toc-required-packages">Required
    packages</a>
-   <a href="#writing-the-functions" id="toc-writing-the-functions">Writing
    the Functions</a>
    -   <a href="#function-number-one-listbreweries"
        id="toc-function-number-one-listbreweries">Function Number One:
        listBreweries()</a>
    -   <a href="#function-number-two" id="toc-function-number-two">Function
        Number Two:</a>
-   <a href="#data-retreival-and-parsing"
    id="toc-data-retreival-and-parsing">Data Retreival and Parsing</a>
-   <a href="#exploratory-data-analysis"
    id="toc-exploratory-data-analysis">Exploratory Data Analysis</a>

# Introduction

Our goal with this project is to create a vignette about contacting an
API using functions we’ve created to query, parse, and return
well-structured data. We’ll then use our functions to obtain data from
the API and do some exploratory data analysis.

# Required packages

To use the functions for interacting with the API, the following
packages are used:

-   `tidyverse`: Tons of useful features for data manipulation and
    visualization
-   `jsonlite`: API interaction
-   `httr`: Used to make http requests in R language as it provides a
    wrapper for the curl package
-   `knitr`: Displaying tables in a markdown-friendly way

``` r
library(tidyverse)
library(jsonlite)
library(httr)
library(knitr)
```

# Writing the Functions

Here we will write the functions for data retrieval.These functions will
allow the user to customize their query to return specific data. We will
be using an API that is focused on breweries in the United States. Each
function will return a data frame with variables such as brewery ID,
name, type, address (street, city, and state variables), postal code,
country, longitude, latitude, phone number, and website URL to name a
few. Each function will have a brief description.

## Function Number One: listBreweries()

The first function is a function that will return a list of
user-specified length. Default length is 20 and maximum length is 50!

``` r
listBreweries <- function(length = 20) {
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint <- "breweries?per_page="
  fullURL = paste0(baseURL, endpoint, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  parsedAPI <- fromJSON(rawToChar(myAPI$content))
  
  #Return the final tibble.
  return(parsedAPI)
}
```

## Function Number Two:

# Data Retreival and Parsing

Here we will use the functions from the previous section to get our
data.

# Exploratory Data Analysis
