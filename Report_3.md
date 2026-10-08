# Part 1: Exploring TMDb via Discovery Queries

# Part 1: Exploring TMDb via Discovery Queries

```{r part-1, warning=FALSE, message=FALSE}
library(httr)
library(jsonlite)
library(dplyr)
library(tibble)

# API Authentication Key
api_key <- "f5d15466393ed930c8e0789be72f4cfd"

# Helper function to query TMDb and parse results into a tibble
get_tmdb_tibble <- function(url) {
  res <- GET(url)
  content_text <- content(res, as = "text", encoding = "UTF-8")
  parsed <- fromJSON(content_text, flatten = TRUE)
  
  if (!is.null(parsed$results)) {
    return(as_tibble(parsed$results))
  } else {
    return(tibble())
  }
}

# 1. Highest-grossing dramas from 2010 (Genre ID: 18)
url_dramas <- paste0(
  "[https://api.themoviedb.org/3/discover/movie](https://api.themoviedb.org/3/discover/movie)?",
  "api_key=", api_key,
  "&with_genres=18",
  "&primary_release_year=2010",
  "&sort_by=revenue.desc"
)
df_dramas <- get_tmdb_tibble(url_dramas)
head(df_dramas %>% select(title, release_date, revenue), 5)

# 2. Collaborative movies: Will Ferrell (ID: 23659) & Liam Neeson (ID: 3896)
url_coop <- paste0(
  "[https://api.themoviedb.org/3/discover/movie](https://api.themoviedb.org/3/discover/movie)?",
  "api_key=",

```