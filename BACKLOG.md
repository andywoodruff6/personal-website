# Backlog

Running list of things to work on in the future. Items live here until they
graduate into a session, get punted indefinitely, or are killed outright.

Format: one item per bullet. Add a `(YYYY-MM-DD)` date when adding so we can
tell what's been waiting longest. Promote to a session when picking up.

---

## Predictions

- **Brier scoring for resolved predictions.** Add a `Brier` column to the predictions table, auto-computed from `confidence` × `status`: `(1−p)²` if Correct, `p²` if Incorrect, blank for Open. Show the running average Brier as a single number near the top — that's the "I keep score" signal. Decided 2026-05-15 to defer; the page reads fine without it for now. (2026-05-15)
- **Group predictions under T1–T4 theses** from SELF TELOS — AI vs Blockchain · Information Overload · Knowledge Worker Displacement · In-Person Trumps Digital. Add 2–4 specific time-bound predictions per thesis (currently 2 total, none thesis-tagged). (2026-05-15)

## Content pipelines

- **Pipe previously written articles into `/ideas/` or `/curation/`.** 


## Strategic surfaces

- **Q1 2026 review (sanitized).** Constraint-Build Seesaw, Autonomy Ladder, Relationship Gap, Journaling Canary. Quarterly cadence — most repeatable long-form post. (2026-05-15)
- **`/connect/` page with structured intent.** Three buckets: Partnership (OCME), Consulting (Shifting Current Consulting Ops), Press / Speaking. (2026-05-15)


## Products

- **"Register interest" capture on product pages.** Let a visitor signal interest in buying a physical product (AI Token Counter, Cycling Charging Station) by sharing their email. First pass used a `mailto:` CTA — rejected because it opens the visitor's email client instead of capturing on-page. Real version: an on-page form posting to a Cloudflare Pages Function with KV (or D1) storage + Turnstile spam protection, scoped per product. Shelved until a product is closer to real. (2026-06-10)

## Operational

- **Stop Doing list against the site.** Apply the $150/hr filter to every section. Anything that doesn't either advance M1 (100M DIDs) or feed the newsletter is a candidate for cut. (2026-05-15)
