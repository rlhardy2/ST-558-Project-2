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
state is North Carolina (note: states should be lowercase and have an
underscore if they are two or more words).

``` r
listByState <- function(state = "north_carolina", length = 20) {
  
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

listByState()
```

    ## # A tibble: 20 × 17
    ##    id                   name  brewe…¹ street addre…² addre…³ city  state count…⁴ posta…⁵ country longi…⁶ latit…⁷ phone websi…⁸ updat…⁹ creat…˟
    ##    <chr>                <chr> <chr>   <chr>  <lgl>   <lgl>   <chr> <chr> <lgl>   <chr>   <chr>   <chr>   <chr>   <chr> <chr>   <chr>   <chr>  
    ##  1 1323-r-and-d-raleigh 1323… micro   1323 … NA      NA      Rale… Nort… NA      27603-… United… <NA>    <NA>    9199… http:/… 2022-0… 2022-0…
    ##  2 1718-ocracoke-brewi… 1718… brewpub 1129 … NA      NA      Ocra… Nort… NA      27960   United… -75.97… 35.107… 2529… http:/… 2022-0… 2022-0…
    ##  3 217-brew-works-wils… 217 … micro   217 S… NA      NA      Wils… Nort… NA      27893-… United… -77.91… 35.722… 2529… <NA>    2022-0… 2022-0…
    ##  4 34-degree-north-exp… 34 D… micro   4802 … NA      NA      Shal… Nort… NA      28470   United… -78.38… 33.972… <NA>  <NA>    2022-0… 2022-0…
    ##  5 3rd-degree-brewhous… 3rd … micro   1625 … NA      NA      Fuqu… Nort… NA      27526   United… -78.79… 35.591… 9192… http:/… 2022-0… 2022-0…
    ##  6 3rd-rock-brewing-co… 3rd … micro   134 I… NA      NA      Tren… Nort… NA      28585-… United… -77.36… 35.069… 2526… http:/… 2022-0… 2022-0…
    ##  7 andrews-brewing-co-… Andr… micro   575 A… NA      NA      Andr… Nort… NA      28901-… United… -83.81… 35.199… 8283… http:/… 2022-0… 2022-0…
    ##  8 angry-troll-brewing… Angr… brewpub 222 E… NA      NA      Elkin Nort… NA      28621-… United… <NA>    <NA>    3362… http:/… 2022-0… 2022-0…
    ##  9 appalachian-mountai… Appa… micro   163 B… NA      NA      Boone Nort… NA      28607-… United… -81.66… 36.203… 8282… http:/… 2022-0… 2022-0…
    ## 10 archetype-brewing-a… Arch… micro   265 H… NA      NA      Ashe… Nort… NA      28806-… United… <NA>    <NA>    8285… http:/… 2022-0… 2022-0…
    ## 11 asheville-brewing-c… Ashe… brewpub 77 Co… NA      NA      Ashe… Nort… NA      28801-… United… -82.55… 35.591… 8282… http:/… 2022-0… 2022-0…
    ## 12 ass-clown-brewing-c… Ass … micro   10620… NA      NA      Corn… Nort… NA      28031-… United… <NA>    <NA>    9805… http:/… 2022-0… 2022-0…
    ## 13 aviator-brewing-com… Avia… region… 209 T… NA      NA      Fuqu… Nort… NA      27526-… United… -78.80… 35.616… 9195… http:/… 2022-0… 2022-0…
    ## 14 balsam-falls-brewin… Bals… micro   506 W… NA      NA      Sylva Nort… NA      28779-… United… -83.22… 35.373… 8286… <NA>    2022-0… 2022-0…
    ## 15 bark-brewing-compan… Bark… brewpub 3021 … NA      NA      Gree… Nort… NA      27403-… United… -79.84… 36.062… 3368… http:/… 2022-0… 2022-0…
    ## 16 barking-duck-brewin… Bark… micro   4400 … NA      NA      Mint… Nort… NA      28227-… United… <NA>    <NA>    9809… http:/… 2022-0… 2022-0…
    ## 17 barrel-culture-brew… Barr… propri… 4913 … NA      NA      Durh… Nort… NA      27713-… United… -78.88… 35.894… <NA>  <NA>    2022-0… 2022-0…
    ## 18 bdd-brewing-company… BDD … micro   1147 … NA      NA      Rock… Nort… NA      27804   United… -77.80… 35.958… 9196… <NA>    2022-0… 2022-0…
    ## 19 bear-creek-brews-be… Bear… micro   10538… NA      NA      Bear… Nort… NA      27207-… United… <NA>    <NA>    9192… http:/… 2022-0… 2022-0…
    ## 20 bearwaters-brewing-… Bear… brewpub 101 P… NA      NA      Cant… Nort… NA      28716-… United… -82.84… 35.531… 8282… http:/… 2022-0… 2022-0…
    ## # … with abbreviated variable names ¹​brewery_type, ²​address_2, ³​address_3, ⁴​county_province, ⁵​postal_code, ⁶​longitude, ⁷​latitude,
    ## #   ⁸​website_url, ⁹​updated_at, ˟​created_at

# Data Retreival and Parsing

Here we will use the functions from the previous section to get our
data.

# Exploratory Data Analysis
