# List of query functions

A list of functions that make it easy to write PatentsView queries. See
the details section below for a list of the 14 functions, as well as the
[writing queries
vignette](https://docs.ropensci.org/patentsview/articles/writing-queries.html)
for further details.

## Usage

``` r
qry_funs
```

## Format

An object of class `list` of length 14.

## Value

An object of class `pv_query`. This is basically just a simple list with
a print method attached to it.

## Details

**1. Comparison operator functions**  

There are 6 comparison operator functions that work with fields of type
integer, float, date, or string:

- `eq` - Equal to

- `neq` - Not equal to

- `gt` - Greater than

- `gte` - Greater than or equal to

- `lt` - Less than

- `lte` - Less than or equal to

There are 2 comparison operator functions that only work with fields of
type string:

- `begins` - The string begins with the value string

- `contains` - The string contains the value string

There are 3 comparison operator functions that only work with fields of
type fulltext:

- `text_all` - The text contains all the words in the value string

- `text_any` - The text contains any of the words in the value string

- `text_phrase` - The text contains the exact phrase of the value string

**2. Array functions**  

There are 2 array functions:

- `and` - Both members of the array must be true

- `or` - Only one member of the array must be true

**3. Negation function**  

There is 1 negation function:

- `not` - The comparison is not true

## Examples

``` r
qry_funs$eq(patent_date = "2001-01-01")
#> {"_eq":{"patent_date":"2001-01-01"}}

qry_funs$not(qry_funs$eq(patent_date = "2001-01-01"))
#> {"_not":{"_eq":{"patent_date":"2001-01-01"}}}
```
