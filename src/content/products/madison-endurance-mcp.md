---
title: Madison Endurance Sports MCP Server
type: digital
status: Shipped
price: Free · Open source
date: 2026-05-22
description: A FastMCP server for Madison-area endurance athletes — race discovery, NWS race-day weather, recurring group rides, and a /race_prep slash command, all in one Claude surface.
cover: /images/madison-endurance-mcp.jpg
link: https://github.com/andywoodruff6/mes-mcp-server
tags: ['mcp', 'fastmcp', 'building']
featured: false
---

> A Model Context Protocol server that joins race discovery, NWS race-day weather, and recurring group rides into one slash-command surface for Madison-area endurance athletes.

About 90 seconds of demo tells it faster than I can:

<iframe
  src="https://www.youtube.com/embed/G2FDquPLqWE"
  title="Madison Endurance Sports MCP Server — demo"
  loading="lazy"
  allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
  style="width:100%; aspect-ratio:16/9; border:0; border-radius:6px;"
></iframe>

## What it is

A [FastMCP](https://github.com/andywoodruff6/mes-mcp-server) server exposing four tools — race discovery, National Weather Service race-day weather, recurring group rides, and a `/race_prep` slash command — to any MCP-capable client. Clone it, `uv sync`, drop one line into your `.mcp.json`, and it's wired into Claude Code.

## Why it's here

This is a portfolio piece as much as a tool — I built it to learn FastMCP the only way that sticks, by shipping something real in it. Every response carries a mandatory `source_url` so the model always cites its provenance; the `@mcp.tool` / `@mcp.prompt` idioms are deliberate. The [full write-up](/ideas/madison-endurance-mcp-shipped/) covers the two design moves I want to be known for.

The [repo](https://github.com/andywoodruff6/mes-mcp-server) is the artifact, the video above is the proof.
