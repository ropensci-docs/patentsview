# Search PatentsView

This makes an HTTP request to the PatentsView API for data matching the
user's query.

## Usage

``` r
search_pv(
  query,
  fields = NULL,
  endpoint = "patent",
  subent_cnts = lifecycle::deprecated(),
  mtchd_subent_only = lifecycle::deprecated(),
  page = lifecycle::deprecated(),
  per_page = lifecycle::deprecated(),
  size = 1000,
  after = NULL,
  all_pages = FALSE,
  sort = NULL,
  method = "GET",
  error_browser = lifecycle::deprecated(),
  exclude_withdrawn = NULL,
  api_key = Sys.getenv("PATENTSVIEW_API_KEY"),
  ...
)
```

## Arguments

- query:

  The query that the API will use to filter records. `query` can come in
  any one of the following forms:

  - A character string with valid JSON.  
    E.g., `'{"_gte":{"patent_date":"2007-01-04"}}'`

  - A list which will be converted to JSON by `search_pv`.  
    E.g., `list("_gte" = list("patent_date" = "2007-01-04"))`

  - An object of class `pv_query`, which you create by calling one of
    the functions found in the
    [`qry_funs`](https://docs.ropensci.org/patentsview/reference/qry_funs.md)
    list...See the [writing queries
    vignette](https://docs.ropensci.org/patentsview/articles/writing-queries.html)
    for details.  
    E.g., `qry_funs$gte(patent_date = "2007-01-04")`

- fields:

  A character vector of the fields that you want returned to you. A
  value of `NULL` indicates to the API that it should return the default
  fields for that endpoint. Acceptable fields for a given endpoint can
  be found in the `fieldsdf` data frame (`View(fieldsdf)`) or by using
  [`get_fields`](https://docs.ropensci.org/patentsview/reference/get_fields.md).
  Nested fields can be fully qualified, e.g., "application.filing_date"
  or you can use the group name that the field belongs to if you want
  all of the nested fields for that group.

  Note: The primary key columns for a given endpoint will be appended to
  your list of fields within `search_pv`. You can see the
  [`get_ok_pk`](https://docs.ropensci.org/patentsview/reference/get_ok_pk.md)
  to determine what those columns will be for a given endpoint.

  Note: If you specify all fields in a given group using their full
  qualified names, the group name will be substituted in the HTTTP
  request. This helps make HTTP requests shorter. This substitution will
  not happen when you specify all of the primary-entity fields (e.g.,
  passing `get_fields("patent", "patents")` into `search_pv` would not
  substitute use the group name "patents" in place of all of the
  fields).

- endpoint:

  The web service resource you wish to search. Use
  [`get_endpoints()`](https://docs.ropensci.org/patentsview/reference/get_endpoints.md)
  to list the available endpoints.

- subent_cnts:

  **\[deprecated\]** This is always FALSE in the new version of the API
  as the total counts of unique subentities is no longer available.

- mtchd_subent_only:

  **\[deprecated\]** This is always FALSE in the new version of the API
  as non-matched subentities will always be returned.

- page:

  **\[deprecated\]** The new version of the API does not use `page` as a
  parameter for paging, it uses `after`.

- per_page:

  **\[deprecated\]** The API now uses `size`

- size:

  The number of records that should be returned per page. This value can
  be as high as 1,000 (e.g., `size = 1000`).

- after:

  A list of sort key values that defaults to NULL. This exposes the
  API's paging parameter for users who want to implement their own
  paging. It cannot be set when `all_pages = TRUE` as the R package
  manipulates it for users automatically. See [result set
  paging](https://docs.ropensci.org/patentsview/articles/result-set-paging.html)

- all_pages:

  Do you want to download all possible pages of output? If
  `all_pages = TRUE`, the value of `size` is ignored.

- sort:

  A named character vector where the name indicates the field to sort by
  and the value indicates the direction of sorting (direction should be
  either "asc" or "desc"). For example, `sort = c("patent_id" = "asc")`
  or  
  `sort = c("patent_id" = "asc", "patent_date" = "desc")`. `sort = NULL`
  (the default) means the API will use the default sort column for your
  given endpoint. You must include any fields that you wish to sort by
  in `fields`.

- method:

  The HTTP method that you want to use to send the request. Possible
  values include "GET" or "POST". Use the POST method when your query is
  very long (say, over 2,000 characters in length).

- error_browser:

  \`r lifecycle::badge("deprecated"

- exclude_withdrawn:

  only used by the patent endpoint, if FALSE withdrawn patents in the
  database can be returned by a query. The API defaults this to TRUE,
  not returning withdrawn patents in the database that met the query
  parameter.

- api_key:

  API key, it defaults to Sys.getenv("PATENTSVIEW_API_KEY"). Request a
  key
  [here](https://patentsview-support.atlassian.net/servicedesk/customer/portals).

- ...:

  Curl options passed along to httr2's
  [`req_options`](https://httr2.r-lib.org/reference/req_options.html)
  when we do GETs or POSTs.

## Value

A list with the following three elements:

- data:

  A list with one element - a named data frame containing the data
  returned by the server. Each row in the data frame corresponds to a
  single value for the primary entity, as defined by the endpoint's
  primary key. For example, if you search the assignee endpoint, then
  the data frame will be on the assignee-level, where each row
  corresponds to a single assignee (primary key would be `assignee_id`).
  Fields that are not on the assignee-level would be returned in list
  columns.

- query_results:

  Entity counts across all pages of output (not just the page returned
  to you).

- request:

  Details of the HTTP request that was sent to the server. When you set
  `all_pages = TRUE`, you will only get a sample request. In other
  words, you will not be given multiple requests for the multiple calls
  that were made to the server (one for each page of results).

## Examples

``` r
if (FALSE) { # \dontrun{

search_pv(query = '{"_gt":{"patent_year":2010}}')

search_pv(
  query = qry_funs$gt(patent_year = 2010),
  fields = get_fields("patent", c("patents", "assignees"))
)

search_pv(
  query = qry_funs$gt(patent_year = 2010),
  method = "POST",
  fields = "patent_id",
  sort = c("patent_id" = "asc")
)

search_pv(
  query = qry_funs$eq(inventor_name_last = "Crew"),
  endpoint = "inventor",
  all_pages = TRUE
)

search_pv(
  query = qry_funs$contains(assignee_individual_name_last = "Smith"),
  endpoint = "assignee"
)

search_pv(
  query = qry_funs$contains(inventors.inventor_name_last = "Smith"),
  endpoint = "patent",
  timeout = 40
)

search_pv(
  query = qry_funs$eq(patent_id = "11530080"),
  fields = "application"
)

search_pv(
  query = qry_funs$eq(patent_id = "9494444"),  # a withdrawn patent in the pview dbs
  fields = c("patent_id", "patent_date", "withdrawn"),
  exclude_withdrawn = FALSE
)

search_pv(
  query = qry_funs$eq(withdrawn = TRUE),
  fields = c("patent_id", "patent_date", "withdrawn"),
  exclude_withdrawn = FALSE
)
} # }
```
