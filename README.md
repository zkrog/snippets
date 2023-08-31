# snippets

Mutate multiple columns, keeping original and appending names to the new
ones.

``` r
library(dplyr)

data.frame(x1 = rnorm(5, mean = 100),
           x2 = rnorm(5, mean = 1000)) |> 
  mutate(across(everything(),
                .fns = list(log = ~log(.)),
                .names = '{fn}_{col}'))
```

             x1       x2   log_x1   log_x2
    1  97.90491 1000.651 4.583997 6.908406
    2  99.97810 1000.149 4.604951 6.907904
    3 103.01215 1000.832 4.634847 6.908587
    4  98.03969 1001.224 4.585372 6.908979
    5  99.93607 1000.441 4.604531 6.908196

Scraping Excel files embedded in a website. FHWA TVT example.

``` r
library(rio)
library(rvest)

# main web page with links
URL <- 'https://www.fhwa.dot.gov/policyinformation/travel_monitoring/tvt.cfm'
pg <- read_html(URL)

# dataframe with links on webpage
root <- 'https://www.fhwa.dot.gov'
df <- data.frame(links = paste0(root, html_attr(html_nodes(pg, 'a'), 'href')))

# import Excel file
xlsx_url <- df$links[grep('.xlsx', df$links)][1]
xlsx_df <- import(xlsx_url, which = 6) # "which" selects the sheet name/number for Excel files
```
