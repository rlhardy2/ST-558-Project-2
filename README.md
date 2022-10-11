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
    -   <a href="#function-number-two-listbystate"
        id="toc-function-number-two-listbystate">Function Number Two:
        listByState()</a>
    -   <a href="#function-number-three-listbydistance"
        id="toc-function-number-three-listbydistance">Function Number Three:
        listByDistance()</a>
    -   <a href="#function-number-four-listbytype"
        id="toc-function-number-four-listbytype">Function Number Four:
        listByType()</a>
-   <a href="#data-retreival-and-parsing"
    id="toc-data-retreival-and-parsing">Data Retreival and Parsing</a>
-   <a href="#exploratory-data-analysis"
    id="toc-exploratory-data-analysis">Exploratory Data Analysis</a>

# Introduction

Our goal with this project is to create a vignette about contacting an
API using functions we’ve created to query, parse, and return
well-structured data. We’ll then use our functions to obtain data from
the API and do some exploratory data analysis!

# Required packages

To use the functions for interacting with the API, the following
packages are used:

-   `tidyverse`: Tons of useful features for data manipulation and
    visualization!
-   `jsonlite`: Used for API interaction.
-   `httr`: Used to make http requests in R language as it provides a
    wrapper for the curl package.
-   `knitr`: Used for displaying tables in a markdown-friendly way.

# Writing the Functions

Here we will write the functions for data retrieval. These functions
will allow the user to customize their query to return specific data. We
will be using an API that is focused on breweries. Each function will
return a data frame with variables such as brewery ID, name, type,
address (street, city, and state variables), postal code, country,
longitude, latitude, phone number, and website URL to name a few. Each
function will have a brief description.

## Function Number One: listBreweries()

The first function is a function that will return a list of breweries of
user-specified length. Default length is 20 and maximum length is 50.

``` r
listBreweries <- function(length = 20) {
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint <- "breweries?per_page="
  fullURL = paste0(baseURL, endpoint, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(finalAPI)
}
```

## Function Number Two: listByState()

The second function is a function that will return a list of breweries
of user-specified length and state. Default length is 20 and default
state is North Carolina.

``` r
listByState <- function(state = "north_carolina", length = 20) {
  
  #Make sure state is all lowercase with underscores instead of spaces.
  state <- tolower(state)
  state <- sub(" ", "_", state)
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint1 <- "breweries?by_state="
  endpoint2 <- "&per_page="
  fullURL = paste0(baseURL, endpoint1, state, endpoint2, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(finalAPI)
}
```

## Function Number Three: listByDistance()

The third function is a function that will return a list of breweries of
user-specified length and sort the results by distance from a
user-specified origin point (latitude, longitude). Default length is 20
and default origin point is Raleigh, North Carolina (35.7796, -78.6382).

``` r
listByDistance <- function(lat = 35.7796, long = -78.6382, length = 20) {
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint1 <- "breweries?by_dist="
  endpoint2 <- "&per_page="
  fullURL = paste0(baseURL, endpoint1, lat, ",", long, endpoint2, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(finalAPI)
}
```

## Function Number Four: listByType()

The fourth function is a function that will return a list of breweries
of user-specified length and type. Default length is 20 and default type
is micro.

The different types of breweries are listed below:

-   `micro`: Most craft breweries. For example, Samuel Adams is still
    considered a micro brewery.
-   `nano`: An extremely small brewery which typically only distributes
    locally.
-   `regional`: A regional location of an expanded brewery. Ex. Sierra
    Nevada’s Asheville, NC location.
-   `brewpub`: A beer-focused restaurant or restaurant/bar with a
    brewery on-premise.
-   `large`: A very large brewery. Likely not for visitors. Ex.
    Miller-Coors. (deprecated)
-   `planning`: A brewery in planning or not yet opened to the public.
-   `bar`: A bar. No brewery equipment on premise. (deprecated)
-   `contract`: A brewery that uses another brewery’s equipment.
-   `proprietor`: Similar to contract brewing but refers more to a
    brewery incubator.
-   `closed`: A location which has been closed.

``` r
listByType <- function(type = "micro", length = 20) {
  
  #Make sure type is all lowercase.
  type <- tolower(type)
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint1 <- "breweries?by_type="
  endpoint2 <- "&per_page="
  fullURL = paste0(baseURL, endpoint1, type, endpoint2, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(finalAPI)
}
```

# Data Retreival and Parsing

Here we will use the functions from the previous section to get our
data.

# Exploratory Data Analysis
