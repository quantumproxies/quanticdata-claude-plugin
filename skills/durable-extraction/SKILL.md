---
name: durable-extraction
description: Set up an extraction that keeps working after the site changes its markup — learn a page's structure once, store it as a versioned parser, and repair it automatically when selectors break. Use when the user wants the same fields from the same site repeatedly ("track this every week", "pull these fields from all their product pages", "my scraper broke"), rather than a one-off read.
---

# Durable extraction

A scraper written by hand breaks the first time the site ships a redesign. A
parser preset re-learns instead.

## The three-step setup

1. **Learn it.** `generate_parser` on a representative URL, with `fields` as an
   object mapping the field name to a plain-English description of what it is:
   `{ price: "the current selling price", title: "the product name" }`. It returns
   the selectors it found and a per-field report of what actually matched.
2. **Check the report before storing anything.** A field with no selector will
   never fill. Fix the description and re-run rather than saving a parser that is
   already half-broken.
3. **Store it.** `save_parser_preset` with `name`, `parser`, and — this one
   matters — **`source_url`**. Without `source_url` the preset saves fine but
   `heal_parser_preset` later answers *"no sourceUrl to relearn from"*, because
   there is no page for it to re-learn against. Set `auto_heal: true` so repair
   happens without anyone watching.

## Living with it

- `list_parser_presets` — what exists, with version and changelog.
- `parser_preset_stats` — success rate per field and mean coverage over recent
  runs, plus whether the preset now counts as decayed. A field whose success rate
  has dropped is the site changing under you; check it before the data silently
  goes empty.
- `heal_parser_preset` — regenerate selectors now. It adopts new ones **only** if
  they extract more than the current ones, so a heal that finds nothing better
  leaves the preset alone and is not billed. `force: true` bypasses the cooldown.

## When to use a preset instead of a collector

If a collector covers the source, use the collector — it is already maintained
for you (see the `structured-data` skill). Presets are for the sources nothing
covers and the user needs repeatedly. For a one-off read, plain `scrape` is
cheaper than setting up a preset that will never run twice.

## Datasets, when the shape is not known yet

`create_dataset` takes a plain-English brief ("the 200 largest logistics
companies in Germany with revenue and headcount") and assembles the table itself,
picking its own sources. It returns a job id — poll `dataset_status` with
**`jobId`**. Set `limits.max_rows` and `limits.max_cost_usd` up front: this is
the tool that can spend real money without anyone noticing, so agree the ceiling
with the user before starting it.
