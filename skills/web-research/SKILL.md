---
name: web-research
description: Answer a question from the live web — search, read the actual pages, and cite what you used. Use when the user asks about something current, niche, or behind a site that ordinary fetching cannot reach ("what does X cost now", "research this company", "read this page and summarise it", "what changed on their pricing page"). Also use when a plain fetch returned a login wall, an empty shell, or a bot challenge.
---

# Web research

Reading a page is not the same as guessing what it probably says. This skill goes
and reads it.

## Pick the right entry point

| The user gives you | Use |
| --- | --- |
| One URL | `scrape` |
| A question | `search_and_read` — searches and reads the top results in one call |
| A question where you want to choose the sources yourself | `search`, then `scrape` on the ones worth reading |
| A whole site to inventory | see the `site-inventory` skill |

`search_and_read` is one billed call instead of several, so prefer it when the
user just wants an answer. Reach for `search` + `scrape` when the ranking matters
— comparing what different sources claim, or when the top result is a
content farm.

## Reading pages well

`scrape` returns Markdown by default and keeps the whole page: tables survive as
GFM tables, links are absolutised, and navigation/footer/cookie chrome is
stripped. That default (`content_mode: "smart"`) is what you want almost always.

- `content_mode: "article"` when the page is a blog post buried in a busy layout
  and you only want the body.
- `format: "html"` when you need to inspect what the page serves *before*
  JavaScript — the no-JS/SEO fallback. Useful for diagnosing why a page looks
  empty to other tools.
- Pass `prompt` to have fields pulled out during the scrape rather than reading
  the whole page yourself, when you know exactly what you want from it.

If a site blocks you, say so plainly and name the site. Do not silently substitute
a different source and present it as the answer.

## Citing

Every source you read has a URL — give it. When two sources disagree, say which
one you are following and why, rather than blending them into one confident
paragraph. When a page is dated, carry the date into your answer: "as of their
March pricing page" is a different claim from "their price is".

## Cost

Each `scrape`, `search` and `search_and_read` is a billed request against the
user's prepaid balance. Reading five pages to answer a one-line question is
wasteful — read what you need. For anything over a handful of pages, say what you
are about to do first.
