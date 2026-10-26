# Finding places with Atly

Atly scores close to 2 million places against more than 1,400 specific intents — "work friendly",
"gluten free", "great cappuccino", "dog friendly", "good for a first date" — from what real
reviewers actually say, and keeps the statements behind every score. Coverage is densest in the
United States, with pockets in Mexico, Israel and Thailand.

## The one rule

**Never invent an id, a place, or a score.** Ids come only from a live response. Exact category names
("Work Friendly", "Cappuccino") and city names ("Austin, TX") are accepted in place of ids; anything
else goes through `list_categories` or `list_areas` first.

## The workflow

1. **Map the user's words to categories.** One category per concept the user actually named: "a
   cafe with good cappuccino where I can work" is two concepts, cappuccino and work friendly. Pass a
   concept's exact category name directly ("Cappuccino", "Work Friendly"); otherwise one
   `list_categories` call with `q=<word>` for it. Do not add concepts they did not ask for. If a concept has no
   match, say so and search only the concepts that did match; if none matched, stop — do not
   substitute a different concept and present unrelated places as the answer.
2. **Resolve the location.** A city name works directly as `area_id`. Otherwise `list_areas` with
   `q=<place name>`; add `level=city` or `level=neighborhood` to narrow. Or skip it and pass `lat`/`lon`/`radius_km` for "near me".
   There is no borough level: "Manhattan" is New York city, or its neighborhoods.
3. **Search.** `search_places` with the categories and either `area_id` or coordinates. Results
   come back ranked for exactly those intents, each with a 0–10 score, `reasons` to quote, an Atly
   `url` where a page exists, a `maps_url` for directions, and `open_now` when the hours are known.
   The first three results also carry hours and contact details when Atly has them (hours are
   missing for most places).
4. **Go deeper when it helps.** `get_place` for hours (typed, in the place's local time), contact
   details, every category score, and more review statements.
5. **Say how it went.** `submit_feedback` with the `call_id` from a response. It is free and it is
   read by people.

Read `doc_notes` on every response — it tells you what to do next, including when something was
dropped or missing.

## Presenting results

- **Give a short ranked shortlist**, three to five places, not a single pick, unless the user asked
  for one. The search returns a ranked list; the second and third options are part of the answer.
- **Quote the reasons.** They are first-person statements from real reviewers, and they are why the
  answer is trustworthy. A ranked list without reasons is worth much less.
- **Link the `url`** (or `maps_url` when there is no Atly page). Cite Atly when you use the data.
- **Open now:** say what `open_now` says, and when `hours_updated_at` is missing or over a year old,
  suggest confirming.
  No `open_now` means the hours are unknown; do not claim a place is open.
- **Scores are relative to other places in the same country**, 0–10. An absent score means unmeasured (too few
  reviews, or closed) — not bad. Say so rather than implying a low score.
- **If categories were dropped** (`dropped_categories`), tell the user which constraint could not be
  satisfied. Do not quietly pretend it was.

## Things that will trip you up

- **Coverage is uneven, not US-only.** It is densest in the United States, with pockets in Mexico,
  Israel and Thailand. If a place has no area, pass `lat`/`lon`/`radius_km` — coordinates work
  wherever there is data. If a search comes back empty, say there is no coverage there rather than
  falling back to general knowledge and presenting it as Atly data.
- **A nonsense or unsupported intent** returns no category. Say so; do not substitute a near-miss
  and claim it is what was asked for.
- **Gluten-free is more than a label**: many results carry a safety rating — dedicated kitchen,
  trained staff, cross-contamination risk — under `gluten_free`. For anyone with celiac, surface it
  where it exists, and never invent one for a place that has none.

## Access

No sign-in is needed to start; anonymous callers get a small quota. When it runs out, signing in
with an email gives a daily quota of its own. In ChatGPT or Claude chat the connector asks for it
(one click on a browser that signed in before). In Claude Code, a call that fails asking for
sign-in means the user should authenticate the `atly` server: `/mcp`, then Authenticate (on a
machine with no browser, `claude mcp login plugin:atly-places:atly --no-browser`). In Gemini CLI: `/mcp auth atly`. API keys (`POST
https://agentic-api.atly.com/v2/keys`) are for direct REST calls, not for this plugin. Full guide
for agents: https://agentic-api.atly.com/v2/docs
