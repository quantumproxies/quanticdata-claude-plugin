---
name: structured-data
description: Pull typed rows from a named source instead of scraping it by hand — search results, maps and reviews, marketplaces, jobs, news, app stores, developer registries, finance, public records and open research data. Use when the user wants a table rather than prose ("get me the top N", "what are the reviews", "list the jobs for", "prices for this product across sellers"), or when they name a site the collectors already cover.
---

# Structured data from collectors

74 ready-made collectors return typed rows for a semantic input — a keyword and a
city, a product id, a company domain, a repository, a ticker. No selectors, no
parser to maintain, and you are billed per delivered row: failures cost nothing.

## Always start with the catalogue

Call `list_collectors` first. It is free, and it gives you each collector's
`inputSchema`, an example input, the price per row and its current health. Do not
guess a slug — a wrong one costs a round trip and tells the user nothing.

Narrow it with `category` when the domain is obvious: `local`, `ecommerce`,
`jobs`, `news`, `travel`, `leads`, `finance`, `dev`, `gaming`, `osint`,
`research`, `classifieds`, `knowledge`.

## Running one

`run_collector` takes `slug` and an `input` object matching that collector's
schema. Short runs return rows inline. Long ones return a `run_id` — poll
`collector_run_status` with it, and note the parameter is `run_id` there.

```
run_collector  slug="google_maps_places"
               input={ keyword: "dentist", location: "Austin, TX", max_results: 20 }
```

Pass `format: "csv"` to `collector_run_status` when the user wants a file rather
than a table in chat.

## Choosing between a collector and scraping

If a collector covers the source, use it: it returns clean typed fields and keeps
working when the site's markup changes. Fall back to `scrape` only for a source
nothing covers — and if the user will need it repeatedly, see the
`durable-extraction` skill instead of scraping it again every time.

## Set the size before you run

`max_results` is the cost dial. A collector priced per row at 20 results is
cheap; the same one at 2,000 is not. When the user says "all of them", tell them
the per-row price from `list_collectors` and the number they are asking for, then
let them pick. Do not quietly cap it — a truncated answer presented as complete
is worse than an expensive one.

## Reporting

Give the rows, then the run's cost and count. If a run came back `partial`, say
which part is missing rather than presenting what arrived as the whole set.
