Strongs and factors
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

## Strings and regex

``` r
string_vec = c("my", "name", "is", "jeff")

#`str_etect` the presence or absence of a pattern in a string
str_detect(string_vec, "jeff") #does "jeff" exist in the string
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
#[1] FALSE FALSE FALSE  TRUE


#replace pattern
str_replace(string_vec, "jeff", "Jeff")
```

    ## [1] "my"   "name" "is"   "Jeff"

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )

#start with "i think" by using ^ at the beginning
str_detect(string_vec, "^i think")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
#end with "i think" by using $ at the end
str_detect(string_vec, "i think$")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
string_vec = c(
  "Y'all remember Pres. HW Bush?",
  "I saw a green bush",
  "BBQ and Bushwalking at Molonglo Gorge",
  "BUSH -- LIVE IN CONCERT!!"
)

#to detect both "Bush" and "bush"
str_detect(string_vec, "[Bb]ush")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
#[1]  TRUE  TRUE  TRUE FALSE
```

``` r
string_vec = c(
  "Time for a Pumpkin Spice Latte!",
  "went to the #pumpkinpatch last weekend",
  "Pumpkin Pie is obviously the best pie",
  "SMASHING PUMPKINS -- LIVE IN CONCERT!!"
  )

str_detect(string_vec,"[Pp]umpkin")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

#want to find number first, immediately follow by characters
str_detect(string_vec, "^[0-9][a-zA-Z]")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
#[1]  TRUE  TRUE FALSE  TRUE
```

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

#`^` target any special character at the beginning;
#`.` target any special character in the middle;
#`$` target ant special character at the end;
str_detect(string_vec, "7.11")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
#[1]  TRUE  TRUE FALSE  TRUE

#if we want to target only `7.11`, dectect actual dot `.`
str_detect(string_vec, "7\\.11")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
string_vec = c(
  'The CI is [2, 5]',
  ':-]',
  ':-[',
  'I found the answer on pages [6-7]'
  )

# to detect specific special character, using "\\"
str_detect(string_vec, "\\[")
```

    ## [1]  TRUE FALSE  TRUE  TRUE

``` r
#[1]  TRUE FALSE  TRUE  TRUE
```

## Factors

``` r
factor_vec = factor(c("male","male","female","female"))
factor_vec
```

    ## [1] male   male   female female
    ## Levels: female male

``` r
#[1] male   male   female female
#Levels: female male (based on alphabet)

as.numeric(factor_vec)
```

    ## [1] 2 2 1 1

``` r
#[1] 2 2 1 1; female = 2, male = 1 
```

what happen if i relevel ..

``` r
factor_vec = fct_relevel(factor_vec, "male")
factor_vec
```

    ## [1] male   male   female female
    ## Levels: male female

``` r
#[1] male   male   female female
#Levels: male female

as.numeric(factor_vec)
```

    ## [1] 1 1 2 2

``` r
#[1] 1 1 2 2; male = 1, female = 2
```

## NSDUH – Strings

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"


table_marj = 
  read_html(url)|> 
  html_node(css = "table") |> 
        # Extract the first table
  html_table() |>     # Convert HTML table to R data frame
  slice(-1) |>        # Eliminate the first row, which might contain notes
  as_tibble()         # Convert to tibble format
```

``` r
data_marj <- 
  table_marj |>
  select(-contains("P Value")) |>                 # Corrected to `select`
  pivot_longer(
    -State, 
    names_to = "age_year",
    values_to = "percent"                         # Corrected to `values_to`
  ) |> 
  separate(age_year, into = c("age", "year"), sep = "\\(") |> 
  mutate(
    year = str_replace(year, "\\)", ""),          # Removed closing parenthesis from `year`
    percent = str_replace(percent, "[a-c]$", ""), # Fixed typo: `present` to `percent`
    percent = as.numeric(percent)                 # Convert `percent` to numeric
  ) |> 
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West"))) # Corrected `fliter` to `filter`
```
