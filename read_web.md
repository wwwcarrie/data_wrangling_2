Reading data from website
================

## Two major paths

1.  there is data included as content on a webpage, and you want to
    ‘scrape’ those data

- table from Wikipedia
- reviews from Amazon
- cast and characters on IMBD

2.  there is a dedicated server holding data in a relatively usable
    form, and you want yo ask for those data

- open NYC data
- Data.gov
- Star Wars API (Application Programming Interfaces)

# scape table

workfolw for scraping data

- download html using `read_html()`
- extract nodes using `html_notes()` and your CSS selector
- extract content from nodes using `html_text()`,`html_table()`, etc

I want first table from \[this page\]
(<https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>)

read in the html

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html (url)
```

extract the table(s); focus on the first one

``` r
first_table <- html_nodes(drug_use_html, css = "table")[[1]] |> #use [[1]] extract the first item
  html_table() |> # to gengerate a table to read
  slice(-1) #elimate first row "notes"
```
