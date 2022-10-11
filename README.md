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
    -   <a href="#function-number-three-listbycity"
        id="toc-function-number-three-listbycity">Function Number Three:
        listByCity()</a>
    -   <a href="#function-number-four-listbydistance"
        id="toc-function-number-four-listbydistance">Function Number Four:
        listByDistance()</a>
    -   <a href="#function-number-five-listbytype"
        id="toc-function-number-five-listbytype">Function Number Five:
        listByType()</a>
    -   <a href="#function-number-six-listbysearch"
        id="toc-function-number-six-listbysearch">Function Number Six:
        listBySearch()</a>
-   <a href="#data-retreival-and-exploratory-analysis"
    id="toc-data-retreival-and-exploratory-analysis">Data Retreival and
    Exploratory Analysis</a>

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

In this section we will write the functions for data retrieval. These
functions will allow the user to customize their query to return
specific data. We will be using an API that is focused on breweries.
Each function will return a data frame with variables such as brewery
ID, name, type, address (street, city, and state variables), postal
code, country, longitude, latitude, phone number, and website URL to
name a few. Each function will have a brief description.

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
  return(as_tibble(finalAPI))
}
```

## Function Number Two: listByState()

The second function is a function that will return a list of breweries
of user-specified length and state. Default length is 20 and default
state is North Carolina.

``` r
listByState <- function(state = "North Carolina", length = 20) {
  
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
  return(as_tibble(finalAPI))
}
```

## Function Number Three: listByCity()

The third function is a function that will return a list of breweries of
user-specified length and city. Default length is 20 and default city is
San Diego, California.

``` r
listByCity <- function(city = "San Diego", length = 20) {
  
  #Make sure city is all lowercase with underscores instead of spaces.
  city <- tolower(city)
  city <- sub(" ", "_", city)
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint1 <- "breweries?by_city="
  endpoint2 <- "&per_page="
  fullURL = paste0(baseURL, endpoint1, city, endpoint2, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(as_tibble(finalAPI))
}
```

## Function Number Four: listByDistance()

The fourth function is a function that will return a list of breweries
of user-specified length and sort the results by distance from a
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
  return(as_tibble(finalAPI))
}
```

## Function Number Five: listByType()

The fifth function is a function that will return a list of breweries of
user-specified length and type. Default length is 20 and default type is
micro.

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
  return(as_tibble(finalAPI))
}
```

## Function Number Six: listBySearch()

The sixth function is a function that will return a list of breweries of
user-specified length based on a search term. Default length is 20 and
the user *must* specify a search term.

``` r
listBySearch <- function(search, length = 20) {
  
  #Make sure the search term is all lowercase with underscores instead of spaces.
  search <- tolower(search)
  search <- sub(" ", "_", search)
  
  #Create the full URL that will be used to retrieve the data.
  baseURL <- "https://api.openbrewerydb.org/"
  endpoint1 <- "breweries/search?query="
  endpoint2 <- "&per_page="
  fullURL = paste0(baseURL, endpoint1, search, endpoint2, length)
  
  #Get the API output.
  outputAPI <- GET(fullURL)
  
  #Parse the API output to get a data frame.
  finalAPI <- fromJSON(rawToChar(outputAPI$content))
  
  #Return the final data frame.
  return(as_tibble(finalAPI))
}
```

# Data Retreival and Exploratory Analysis

In this section we will use the functions from the previous section to
extract our data and then we will analyze it using the `tidyverse`
package!

Some requirements for this section are listed below:

-   You should pull data from at least two calls to your obtaining data
    function (possibly combining them into one).
-   You should create at least one new variable that is a function of
    other variables.
-   You should create some contingency tables.
-   You should create numerical summaries for some quantitative
    variables at each setting of some of your categorical variables.
-   You should create at least five plots utilizing coloring, grouping,
    etc. All plots should have nice labels and titles.
    -   You should have at least one bar plot, one histogram, one box
        plot, and one scatter plot.

“DELETE ABOVE TEXT LATER”

We maybe interested in the percentage of brewpub (A beer-focused
restaurant or restaurant/bar with a brewery on-premise) or bar (A bar.
No brewery equipment on premise) breweries in Wisconsin and North Dakota
as these two states were found to be the two states that consumed the
most alcohol. click
[Here](https://www.thecentersquare.com/wisconsin/this-is-where-wisconsin-ranks-among-the-drunkest-states-in-america/article_3ccd11a4-c261-563b-919a-e02a0254b6dd.html)
for more information.

``` r
wisconsin <- listByState("Wisconsin", 50)
ndakota <- listByState("North Dakota", 50)
combined_tibble <- rbind(wisconsin, ndakota)%>%
# we create a new helper variable "brewpub_or_bar_1_0" to display 1 if brewery type is either "brewpub or "bar". We delete this variable later on
  mutate(brewpub_or_bar_1_0=if_else((brewery_type == "brewpub"|brewery_type == "bar"), 1, 0))%>%

  
  group_by(state)%>%
  
  mutate(percent_brewpub_bar = mean(brewpub_or_bar_1_0)*100) %>% 
  
  # we delete the unwanted variable brewpub_or_bar_1_0
  
  select(-brewpub_or_bar_1_0)%>%
  
  select(percent_brewpub_bar,  everything())

combined_tibble
```

    ## # A tibble: 76 × 18
    ##    percent_b…¹ id    name  brewe…² street addre…³ addre…⁴ city  state count…⁵ posta…⁶ country
    ##          <dbl> <chr> <chr> <chr>   <chr>  <lgl>   <lgl>   <chr> <chr> <lgl>   <chr>   <chr>  
    ##  1          38 1840… 1840… micro   342 E… NA      NA      Milw… Wisc… NA      53207-… United…
    ##  2          38 3-sh… 3 Sh… micro   1837 … NA      NA      Sheb… Wisc… NA      53083-… United…
    ##  3          38 608-… 608 … planni… <NA>   NA      NA      La C… Wisc… NA      54603   United…
    ##  4          38 841-… 841 … brewpub 841 E… NA      NA      Whit… Wisc… NA      53190-… United…
    ##  5          38 8th-… 8th … brewpub 1132 … NA      NA      Sheb… Wisc… NA      53081-… United…
    ##  6          38 agon… Agon… planni… <NA>   NA      NA      Rice… Wisc… NA      54868-… United…
    ##  7          38 ahna… Ahna… micro   N9153… NA      NA      Algo… Wisc… NA      54201-… United…
    ##  8          38 ale-… Ale … region… 2002 … NA      NA      Madi… Wisc… NA      53704-… United…
    ##  9          38 alt-… ALT … brewpub 1808 … NA      NA      Madi… Wisc… NA      53704-… United…
    ## 10          38 angr… Angr… brewpub 10440… NA      NA      Hayw… Wisc… NA      54843-… United…
    ## # … with 66 more rows, 6 more variables: longitude <chr>, latitude <chr>, phone <chr>,
    ## #   website_url <chr>, updated_at <chr>, created_at <chr>, and abbreviated variable names
    ## #   ¹​percent_brewpub_bar, ²​brewery_type, ³​address_2, ⁴​address_3, ⁵​county_province,
    ## #   ⁶​postal_code

A “brewpub” according is defined as a beer-focused restaurant or
restaurant/bar with a brewery on-premise and “bar” is defined as a bar
with no brewery equipment on premise.

From the tibble above we can see that 38 percentage (length 50) of all
the brewery in Wisconsin are either a brewpub or a bar and in North
Dakota 34.6 percentage (length 50) of all the brewery in Wisconsin are
either a brewpub or a bar which could be a possibly explanation for the
high consumption of alcohol in those states.
