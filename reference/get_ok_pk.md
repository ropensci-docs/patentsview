# Get OK primary key

This function suggests column(s) that you could use for the `pk`
argument in
[`unnest_pv_data`](https://docs.ropensci.org/patentsview/reference/unnest_pv_data.md),
based on the endpoint you searched. It will return a potential primary
key - either a single column or a composite set of columns - for the
endpoint.

## Usage

``` r
get_ok_pk(endpoint)
```

## Arguments

- endpoint:

  The endpoint which you would like to know a potential primary key for.

## Value

The column names that represent a single row for the given endpoint.

## Examples

``` r
get_ok_pk(endpoint = "inventor")
#> [1] "inventor_id"
get_ok_pk(endpoint = "patent/foreign_citation")
#> [1] "patent_id"         "citation_sequence"
```
