---
name: site-inventory
description: Take stock of a whole site rather than one page — list its URLs, crawl a section into Markdown, or audit what a page serves before JavaScript runs. Use for "how many pages does this site have", "crawl their docs", "what's under /blog", "why doesn't Google see this page", "check the SEO on this URL", or when migrating or auditing a site.
---

# Site inventory

Three tools, three different questions.

## "What is on this site?" → `map`

Returns the URL tree — up to 5,000 URLs plus a site-wide total and per-prefix
counts. One call, no page fetching, so it is the cheap way to answer "how big is
this" before committing to anything.

- `search` narrows to URLs containing a substring — use it before raising `limit`.
- `group_by: "path"` returns the path tree with counts instead of a flat list.
  This is what you want when the user asks how a site is organised.

Start here. Mapping first and crawling second is almost always cheaper than
crawling blind.

## "Read this section" → `crawl`

A BFS crawl from a seed URL, each page converted to Markdown. It returns a job id
immediately — poll `crawl_status` with `jobId` (not `job_id`; the async pollers
all take `jobId`).

Set `limit` and `depth` deliberately. `depth: 3` on a large site is thousands of
pages. Use `map` to find out how many there are, tell the user, then crawl.

Pass `include_content: false` while polling to check progress without pulling
every page into context; fetch the content once the job is done.

## "Why does this page look broken to crawlers?" → `seo_audit`

Fetches the page both without JavaScript and with a real browser, then reports
both views plus the diff: what only exists after rendering, title and meta,
word counts, status. This is the tool for "Google shows the wrong description",
"our page is not indexed", or "the SPA serves an empty shell".

`scrape` with `format: "html"` gives you the same no-JS view when you want to
read the raw markup yourself.

## Many URLs at once

`batch` scrapes a list of URLs in parallel and returns a job id — poll
`batch_status` with `jobId`. Use it when you already know the URLs; use `crawl`
when you need to discover them.

## Cost

`map` is one request regardless of how many URLs it returns, which is why it
belongs first. `crawl` and `batch` are billed per page fetched. Say the page
count out loud before starting anything over a few dozen.
