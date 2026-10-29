# Retrieve Linked Data

Some of the endpoints now return HATEOAS style links to get more data.
E.g., the patent endpoint may return a link such as:
"https://search.patentsview.org/api/v1/inventor/fl:th_ln:jefferson-1/".
Use this function to fetch details from those links.

## Usage

``` r
retrieve_linked_data(url, api_key = Sys.getenv("PATENTSVIEW_API_KEY"), ...)
```

## Arguments

- url:

  A link that was returned by the API on a previous call.

- api_key:

  API key, it defaults to Sys.getenv("PATENTSVIEW_API_KEY"). Request a
  key
  [here](https://patentsview-support.atlassian.net/servicedesk/customer/portals).

- ...:

  Curl options passed along to httr2's
  [`req_options`](https://httr2.r-lib.org/reference/req_options.html)
  when we do GETs or POSTs.

## Examples

``` r
if (FALSE) { # \dontrun{

retrieve_linked_data(
  "https://search.patentsview.org/api/v1/cpc_group/G01S7:4811/"
)
} # }
```
