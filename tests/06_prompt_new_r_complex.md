## Input

Here I asked Claude to do something more complex. Specifically, I asked Claude
to pull data from the BLS and construct a model which relates interest rates
to unemployment. This is a very complex task which requires good programming
practice, an understanding of the domain, and use of many packages.

**Write an R script which pulls data from the BLS and models interest rates**
**and unemployment using the tidyverse.**

## Response 

Initially, Claude provided this response. This is an impressive shell of an
analysis, but it misses some key components. First, it references columns
which do not exist. Second, it calls `as_tibble()` on a list which cannot be
converted into a tibble, requiring manual intervention:

```r
# Load required packages
library(tidyverse) 
library(lubridate)
library(httr)

# Set date range to pull data for 
start_date <- ymd("2000-01-01")
end_date <- ymd("2022-12-01")

# API request to get unemployment data
unemp_url <- paste0("https://api.bls.gov/publicAPI/v2/timeseries/data/",
                    "LNS14000000?startYear=2000&endYear=2022")
unemp_response <- GET(unemp_url)
unemp_data <- content(unemp_response)

# Clean unemployment data
unemp_df <- unemp_data$Results$series[[1]]$data %>%
  as_tibble() %>%
  mutate(date = ymd(date)) %>%
  filter(date >= start_date & date <= end_date) %>%
  select(date, value) %>%
  rename(unemployment = value)

# API request to get interest rate data  
int_url <- paste0("https://api.bls.gov/publicAPI/v2/timeseries/data/",
                  "MORTGAGE30US?startYear=2000&endYear=2022")
int_response <- GET(int_url)
int_data <- content(int_response)

# Clean interest rate data
int_df <- int_data$Results$series[[1]]$data %>%
  as_tibble() %>%
  mutate(date = ymd(date)) %>%
  filter(date >= start_date & date <= end_date) %>%
  select(date, value) %>%
  rename(int_rate = value)

# Join unemployment and interest rate data
data <- left_join(unemp_df, int_df, by="date") 

# Model relationship  
model <- lm(int_rate ~ unemployment, data=data)

summary(model)
```