# Get list of retrievable fields

Get a vector of fields that you can retrieve from a given API endpoint
(i.e., the fields you can pass to the `fields` argument in
[`search_pv`](https://docs.ropensci.org/patentsview/reference/search_pv.md)).
You can limit these fields to only cover certain entity group(s) as well
(which is recommended, given the large number of possible fields for
each endpoint).

## Usage

``` r
get_fields(endpoint, groups = NULL)
```

## Arguments

- endpoint:

  The API endpoint whose field list you want to get. See
  [`get_endpoints`](https://docs.ropensci.org/patentsview/reference/get_endpoints.md)
  for a list of the 27 endpoints.

- groups:

  A character vector giving the group(s) whose fields you want returned.
  A value of `NULL` indicates that you want all of the endpoint's fields
  (i.e., do not filter the field list based on group membership). Use
  the `fieldsdf` table (e.g.,
  `unique(fieldsdf[fieldsdf$endpoint == "patent", "group"])`) to see
  which groups you can specify for a given endpoint.

## Value

A character vector with field names.

## Examples

``` r
# Get all top level (non-nested) fields for the patent endpoint:
fields <- get_fields(endpoint = "patent", groups = "patents")

# ...Then pass to search_pv:
if (FALSE) { # \dontrun{

search_pv(
  query = '{"_gte":{"patent_date":"2007-01-04"}}',
  fields = fields
)
} # }
# Get patent and assignee-level fields for the patent endpoint:
fields <- get_fields(endpoint = "patent", groups = c("assignees", "patents"))

if (FALSE) { # \dontrun{
# ...Then pass to search_pv:
search_pv(
  query = '{"_gte":{"patent_date":"2007-01-04"}}',
  fields = fields
)
} # }
```
