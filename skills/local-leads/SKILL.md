---
name: local-leads
description: Build a prospect list for a place and a category — businesses with address, phone, site and rating, then contact details pulled from their websites. Use for "find me [category] in [city]", "build a lead list", "who are the [trade] near [area]", "get contacts for these companies", or any local prospecting or market-mapping request.
---

# Local lead lists

Two collectors do the whole job: one finds the businesses, the other finds who to
write to.

## The pipeline

1. **Find the businesses.** `run_collector` with `google_maps_places` (keyword +
   location) returns name, address, phone, website, rating, review count,
   opening hours, coordinates, and the place ids. For directory-style coverage in
   Italy, `business_directory` covers Pagine Gialle/Bianche.
2. **Get the contacts.** Feed the websites to `site_contacts`, which crawls each
   one for emails, phone numbers, socials and the pages they came from.
3. **Qualify, if it helps.** `place_reviews` on a place id tells you what
   customers actually complain about — which is usually the opening line of a good
   outreach email.

Run step 1, show the user what came back, and confirm before step 2: contacts are
priced per row and the list is only worth enriching once they agree the
businesses are the right ones.

## Do the arithmetic out loud

Both steps are billed per delivered row. "Every restaurant in Milan" is a
different number from "20 restaurants in Milan". Before a large run, state the
per-row price (from `list_collectors`), the row count, and the total — then let
the user choose. Never scale a request down on your own to keep it cheap; say
what it costs and let them decide.

## Handle it as personal data

The contact details you are collecting belong to real people, and the user is the
controller of that data — not QuanticData, and not you. If the request looks like
bulk unsolicited email, say once, plainly, that recipients in the EU and UK have
consent and opt-out rights under GDPR and PECR and that a business address is not
automatically fair game. Then do the work they asked for. Do not lecture twice,
and do not refuse ordinary B2B prospecting.

## Output

A table is the deliverable. Include the source URL for each contact so the user
can verify one before writing to a hundred, and flag rows where the email is
pattern-inferred rather than found on the page — those bounce.
