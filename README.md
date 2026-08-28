# QuanticData for Claude

Give Claude live web data: read any page as clean Markdown, search and actually
read the results, and pull typed rows from 74 ready-made collectors.

This plugin bundles the [QuanticData](https://quanticdata.io) remote MCP server
with five skills that teach Claude when to reach for which tool — and, just as
importantly, what each one costs before it runs.

## Install

```
/plugin install quanticdata
```

Then set your API key in the environment Claude Code runs in:

```bash
export QUANTICDATA_API_KEY="qd_live_..."
```

Keys are at <https://app.quanticdata.io/api-keys>. Signing up is free; running
tools draws on a prepaid wallet balance. Browsing the tool list needs no key —
only calling a tool does, so a populated tool list is not proof the key works.
`list_collectors` is free and is the quickest way to confirm it.

## What you get

**25 tools** over the QuanticData API — 19 read-only, 6 that write. The only one
marked destructive is `whitelist_ip`, because removing an entry takes a machine's
access to the proxies away.

| Skill | What it is for |
| --- | --- |
| `web-research` | Answer a question from the live web, reading the actual pages |
| `structured-data` | Typed rows from a named source instead of hand-rolled scraping |
| `local-leads` | Businesses for a place and category, plus contacts from their sites |
| `site-inventory` | Map a site's URLs, crawl a section, audit what renders without JS |
| `durable-extraction` | Extraction that repairs itself when the site changes |

## Examples

```
Read https://example.com/pricing and tell me what changed since their
cached version.

Find 20 dentists in Austin, TX with their websites, then get me contact
emails for the ones rating above 4.5.

How many pages are under docs.stripe.com/api, grouped by path?

Set up a weekly extraction of price and stock from this product page
that keeps working when they redesign.
```

## What it costs

Usage is metered and prepaid: per request for `scrape`, `search`, `map` and
`crawl`, per delivered row for the collectors — failures are never charged.
`list_collectors` reports every collector's per-row price, and the skills are
written to state the cost of a large run before starting it rather than
after.

## How the connector works

`.mcp.json` points at `https://api.quanticdata.io/mcp` over streamable HTTP, and
sends your key as `Authorization: Bearer`. The server is stateless: each request
carries its own key, and nothing is shared between them.

## Privacy

When you use these tools **you** decide what public web data to collect and you
are the controller of that data. QuanticData processes it only to fulfil your
request and does not retain page content longer than needed to return your
result. Requests are logged for billing, abuse prevention and support — target
URL or query, timestamp, status, bytes and the API key used — not page content.

Full policy: <https://quanticdata.io/privacy/>

You are responsible for using the service lawfully, for respecting the terms of
the sites you access, and for any personal data you collect through it.

## Support

- Docs: <https://quanticdata.io/mcp-server/>
- Email: <hello@quanticdata.io>

## Licence

MIT — see [LICENSE](LICENSE).
