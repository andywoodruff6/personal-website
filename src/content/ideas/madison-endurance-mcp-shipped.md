---
title: "I Shipped an MCP Server for Madison Endurance Athletes"
date: "2026-05-22T20:00:00.000Z"
draft: false
description: "A FastMCP server that joins race discovery, NWS weather, and group rides into one slash-command surface — built as a portfolio piece for the FastMCP team, with two design moves I want to be known for."
tags: ['mcp', 'fastmcp', 'building']
---

I shipped a small Model Context Protocol server this week — [`madison-endurance-sports-mcp-server`](https://github.com/andywoodruff6/mes-mcp-server) — a FastMCP server for Madison-area endurance athletes. Race discovery, NWS race-day weather, recurring group rides, and simple `/race_prep` slash command. About 90 seconds of demo will tell you faster than I can:

<iframe
  src="https://www.youtube.com/embed/G2FDquPLqWE"
  title="Madison Endurance Sports MCP Server — demo"
  loading="lazy"
  allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
  style="width:100%; aspect-ratio:16/9; border:0; border-radius:6px;"
></iframe>

It exists for two reasons: 
1. I actually want this — race planning for endurance atheletes means six tabs for registration, a tab for the forecast, and a half-remembered "is the Tuesday Trek ride still happening this week?"
2. Jeremy Adams' <a href="https://www.youtube.com/watch?v=96G7FLab8xc" target="_blank" rel="noopener noreferrer">FastMCP talk</a> on the AI Engineer YouTube channel sent me down the FastMCP rabbit hole — and the fastest way to actually learn a framework is to ship something in it.

What follows is what I built, and the two design moves I'd point at in a code review.

## The shipped surface

Three tools and one prompt. Deliberately small.

- **`find_races`** — filters a curated Madison-area race seed by sport, date, distance, and radius from the Capitol. About 30 events in the seed — Ironman Wisconsin, Run Madtown Half, Horribly Hilly Hundreds, the full Wisconsin Triathlon Series, IRONMAN 70.3 Wisconsin, Door County Triathlon.
- **`race_weather`** — dual-channel weather (more on this below).
- **`find_group_rides`** — curated set of recurring rides, runs, and open-water swims; Trek Bicycle Madison, Saris Cycling Group, MadCity Runners, Madison Triathlon Club, Planet Trek.
- **`/race_prep`** — the slash command that joins them. Accepts `"madtown half"` and resolves it server-side. Returns three plain-prose paragraphs: how long you'll likely be out there, what conditions to expect, and basic nutrition + hydration matched to both.

## Design move #1 — Dual-channel weather

The first version of `race_weather` led to some misleading results. It did the obvious thing: hit NWS, pick the forecast period closest to race day, return it. Fine for races inside 7 days. Disastrous for anything farther out.

The failure mode: NWS publishes hourly periods for a 7 forecast. Ask for a forecast 30 days out and it'll return the *last available period* — which is just next week's weather — and the tool would label it with the race's date. The `confidence_band: "low"` field signaled the horizon problem, but the temperature and precip *values* were already misleading. The model would dutifully relay "78°F and sunny" for a race in November, it's more likely to be 38°F. False precision dressed up as data.

The fix is structural rather than cosmetic. `WeatherForecast` now carries two channels:

```python
class WeatherForecast(BaseModel):
    race_date: date
    today: date
    horizon_days: int
    nws_forecast: NwsForecast | None     # inside 7 days only
    climate_normal: ClimateNormal        # always populated
    primary_signal: Literal["nws_forecast", "climate_normal"]
    advisory: str                        # human-readable framing
```

If the race is more than 7 days out, NWS isn't queried at all — we don't even let the response exist. The `climate_normal` channel carries the signal, sourced from a bundled 30-year NOAA dataset (1991–2020 normals for the KMSN station). The `advisory` string tells the model how to frame it: *"Race is 42 days out — historical climate normal, NOT a forecast. Re-check ≤7 days out for an actual NWS forecast."*

The takeaway I'd point at in a code review: **tools should refuse to lie cleanly, rather than answer dirtily.** The model can't tell the difference between a real forecast and a fake one wearing the same schema — so it falls on the tool to not present the fake one at all. A `None` channel with an explicit `primary_signal` does work that an `is_low_confidence: true` flag never could.

## Design move #2 — Fuzzy slash-command UX

The first version of `/race_prep` required a race ID:

```
/race_prep run-madtown-half-marathon-2026
```

That's not a product. That's a portfolio piece for the model, not the athlete. Real usage on day one went:

```
/race_prep Madtown half marathon
→ Unknown race_id: 'Madtown'. Call `find_races` first…
```

Bad UX, and the kind of bad UX that's easy to ship because the server-side tests all pass with the literal ID strings.

I Fixed it server-side with a 4-layer match ladder: exact ID → exact name → all-tokens-present in ID-or-name → substring. Returns either a unique race (proceed) or a candidate list (clarify). The arg renamed from `race_id` to `race` — small thing, but it tells the client *"you can put a name here."*

The takeaway: **slash commands that demand UUIDs aren't slash commands, they're function calls with extra steps.** If you're going to give the user a typeable surface, the surface should be typeable. MCP's `completion/complete` is the right long-term answer; a fuzzy resolver is the right immediate one.

(Bonus design move that came out of the same impulse: `find_group_rides` now returns a wrapper with a top-level `advisory` field — *"Group rides are informal and frequently change week-to-week. Before showing up, confirm with the ride coordinator."* Structurally present on every response, so it can't be summarized away. Group ride aggregators that don't say this lie by omission.)

## What's next

If you want to read the [code](https://github.com/andywoodruff6/mes-mcp-server) — the FastMCP idioms are deliberate. `@mcp.tool` and `@mcp.prompt` decorators register against a single server instance; `Literal` enums on tool args generate completion schemas automatically; `fastmcp.json` declares the entrypoint; Pydantic models carry mandatory `source_url` fields so every response cites its provenance.

Three things I want to add to the seed before the next push: automatically pulling races from sites like BikeReg, expanded group-ride coverage (currently 6 entries — Machinery Row, Cronometro, Bombay Bicycle Club, masters swims at Goodman should all be there), and a `/group_ride_tonight` prompt that crosses tonight's day-of-week against the user's effort target. Those are evening-and-weekend additions, not architectural ones.

If you're a Madison endurance athlete and want this hooked into your Claude Code, the [README](https://github.com/andywoodruff6/mes-mcp-server) has the connection instructions. It's a one-line `.mcp.json` entry once you `git clone` and `uv sync`.

If you're curious about the build itself — the [repo](https://github.com/andywoodruff6/mes-mcp-server) is the artifact, the video above is the proof, and I'd love to talk about what I'd build with FastMCP next.
