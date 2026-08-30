# With qry_funs

This function evaluates whatever code you pass to it in the environment
of the
[`qry_funs`](https://docs.ropensci.org/patentsview/reference/qry_funs.md)
list. This allows you to cut down on typing when writing your queries.
If you want to cut down on typing even more, you can try assigning the
[`qry_funs`](https://docs.ropensci.org/patentsview/reference/qry_funs.md)
list into your global environment with:
`list2env(qry_funs, envir = globalenv())`.

## Usage

``` r
with_qfuns(code, envir = parent.frame())
```

## Arguments

- code:

  Code to evaluate. See example.

- envir:

  Where should R look for objects present in `code` that aren't present
  in
  [`qry_funs`](https://docs.ropensci.org/patentsview/reference/qry_funs.md).

## Value

The result of `code` - i.e., your query.

## Examples

``` r
# Without with_qfuns, we have to do:
qry_funs$and(
  qry_funs$gte(patent_date = "2007-01-01"),
  qry_funs$text_phrase(patent_abstract = c("computer program")),
  qry_funs$or(
    qry_funs$eq(inventors.inventor_name_last = "Ihaka"),
    qry_funs$eq(inventors.inventor_name_last = "Chris")
  )
)
#> {"_and":[{"_gte":{"patent_date":"2007-01-01"}},{"_text_phrase":{"patent_abstract":"computer program"}},{"_or":[{"_eq":{"inventors.inventor_name_last":"Ihaka"}},{"_eq":{"inventors.inventor_name_last":"Chris"}}]}]}

# ...With it, this becomes:
with_qfuns(
  and(
    gte(patent_date = "2007-01-01"),
    text_phrase(patent_abstract = c("computer program")),
    or(
      eq(inventors.inventor_name_last = "Ihaka"),
      eq(inventors.inventor_name_last = "Chris")
    )
  )
)
#> {"_and":[{"_gte":{"patent_date":"2007-01-01"}},{"_text_phrase":{"patent_abstract":"computer program"}},{"_or":[{"_eq":{"inventors.inventor_name_last":"Ihaka"}},{"_eq":{"inventors.inventor_name_last":"Chris"}}]}]}
```
