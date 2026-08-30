# Unnest PatentsView data

This function converts a single data frame that has subentity-level list
columns in it into multiple data frames, one for each entity/subentity.
The multiple data frames can be merged together using the primary key
variable specified by the user (see the [relational
data](https://r4ds.had.co.nz/relational-data.html) chapter in "R for
Data Science" for an in-depth introduction to joining tabular data).

## Usage

``` r
unnest_pv_data(data, pk = lifecycle::deprecated())
```

## Arguments

- data:

  The data returned by
  [`search_pv`](https://docs.ropensci.org/patentsview/reference/search_pv.md).
  This is the first element of the three-element result object you got
  back from `search_pv`. It should be a list of length 1, with one data
  frame inside it. See examples.

- pk:

  **\[deprecated\]**. should be the unique identifier for the primary
  entity. For example, if you used the patent endpoint in your call to
  `search_pv`, you could specify `pk = "patent_id"`. **This identifier
  has to have been included in your `fields` vector when you called
  `search_pv`**. You can use
  [`get_ok_pk`](https://docs.ropensci.org/patentsview/reference/get_ok_pk.md)
  to suggest a potential primary key for your data.

## Value

A list with multiple data frames, one for each entity/subentity. Each
data frame will have the `pk` column in it, so you can link the tables
together as needed.

## Examples

``` r
if (FALSE) { # \dontrun{

fields <- c(
  "patent_id", "patent_title",
  "inventors.inventor_city", "inventors.inventor_country"
)
res <- search_pv(query = '{"_gte":{"patent_year":2015}}', fields = fields)
unnest_pv_data(data = res$data)
} # }
```
