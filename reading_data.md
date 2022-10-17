Reading data from the web
================

## Srape table from web:

1.  I want table 1 from \[this page\]
    <http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

2.  extract table(s)

``` r
drug_use_html %>% 
  html_nodes(css = "table") #this results in extracting all 15 tables, we only want table 1. but still not tibble
```

    ## {xml_nodeset (15)}
    ##  [1] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [2] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [3] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [4] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [5] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [6] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [7] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [8] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [9] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [10] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [11] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [12] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [13] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [14] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [15] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...

3.  focus on the first table
4.  first()
5.  parse html to table via \`html_table’
6.  use slice(-1) to remove first row
7.  use as_tibble to make a tibble

``` r
tabl_marj =
drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>%  #extract first table, also available 'nth' 'last'
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## Star Wars movie info: get data from html

``` r
url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(url)
```

1.  grab elements I want:

``` r
title_vec = 
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()

gross_rev_vec =
  swm_html %>% 
  html_nodes(css = ".text-small:nth-child(7) span:nth-child(5)") %>% 
  html_text()

runtime_vec =
  swm_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df =
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec,
    runtime = runtime_vec
  )
```
