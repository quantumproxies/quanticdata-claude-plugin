# Setup

The QuanticData connector needs one thing: an API key. Everything else is already
configured in `.mcp.json`.

## Walk the user through it

1. **Check whether a key is already set.** If `QUANTICDATA_API_KEY` is in the
   environment, the connector is ready — say so and stop. Do not ask for a key
   that already exists.

2. **Send them to get one.** Keys live at
   <https://app.quanticdata.io/api-keys>. Creating an account is free; running
   tools is prepaid from a wallet balance, so a new account needs credit before
   the paid tools return data.

3. **Have them set it themselves.** Ask the user to put the key in their own
   shell profile or their Claude Code settings:

   ```bash
   export QUANTICDATA_API_KEY="qd_live_..."
   ```

   **Never ask the user to paste the key into the chat, and never write it into a
   file in their repository.** It is a bearer credential for a wallet that spends
   real money. If they paste it anyway, tell them to rotate it at
   <https://app.quanticdata.io/api-keys>.

4. **Verify.** After they restart Claude Code, call `list_collectors` — it is
   free, needs no arguments, and returns the catalogue. If it comes back with a
   list, the connector works. If it answers "No API key on this request", the
   variable did not reach the process: it must be exported in the environment
   Claude Code itself was started from.

## What "no key" looks like

Browsing the tool list works without a key — you will see all 25 tools whether or
not one is set, so a populated tool list is *not* proof that the key works. Only
a successful tool call is.

## Costs, so nobody is surprised

Usage is metered and prepaid: per request for `scrape`, `search`, `map` and
`crawl`, and per delivered row for the collectors. Failures are never charged.
Before running anything large — a crawl over a whole site, a collector with a
high `max_results`, a `create_dataset` brief — tell the user roughly what it will
cost and let them decide. `list_collectors` reports the per-row price of every
collector, so use it to answer the question instead of guessing.
