---
title: Madison Endurance Sports
date: 2026-05-01
description: A FastMCP server for Madison-area endurance athletes — race discovery, NWS race-day weather, and recurring group rides, behind a fuzzy /race_prep slash command.
status: Shipped · v0.1
stack: Python — FastMCP 3.x on Prefect Horizon
repo: https://github.com/andywoodruff6/mes-mcp-server
tags: [AI, mcp, tech]
order: 2
---

A FastMCP server for Madison-area endurance athletes — race discovery, NWS race-day
weather, and recurring group rides — wrapped in a `/race_prep` slash command
that resolves fuzzy race references (`"madtown half"` → the right race) and
returns a focused pre-race read: how long you'll be out there, what conditions to
expect, and basic nutrition matched to both.

<p><iframe
  src="https://www.youtube.com/embed/G2FDquPLqWE"
  title="Madison Endurance Sports MCP Server — 90-second demo"
  loading="lazy"
  style="display:block;width:100%;aspect-ratio:16/9;height:auto;border:0;border-radius:6px;"
  allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe></p>

### Details

- **Stack** — Python, FastMCP 3.x on Prefect Horizon
- **Surface** — `find_races`, `race_weather`, `find_group_rides`, and the `/race_prep` prompt
- **Data** — Hand-curated Madison race + group-ride seed (Ironman Wisconsin, Run Madtown, Horribly
  Hilly, Wisconsin Triathlon Series, Trek Bicycle Madison rides, MadCity Runners…); NWS
  for inside-7-days forecasts; bundled 30-year NOAA climate normals beyond. Optional
  RunSignup enrichment when an API key is provided.
- **Repo** — [github.com/andywoodruff6/mes-mcp-server](https://github.com/andywoodruff6/mes-mcp-server)

### Why it matters

Two design moves I want to be known for. *Dual-channel weather* — the
server refuses to invent race-day forecasts beyond NWS's honest 7-day horizon, and
falls back to a labeled climate normal rather than confidently presenting next
week's weather as race day's. *Fuzzy slash-command UX* — slash commands
that demand UUIDs are a portfolio piece, not a product; resolving
`"madtown half"` server-side is the difference. The piece collapses six
websites of Madison endurance planning into one MCP conversation built on FastMCP
idioms a Prefect engineer can read in under five minutes.
