---
title: Todoist + Telemetry Logging
date: 2025-09-01
description: A remote, GitHub-OAuth-protected MCP server that gives any agent secure read/write access to my Todoist — and logs every tool call so the AI can answer "how much time did you save me?"
status: Production
stack: Cloudflare Workers + Durable Objects + D1 + KV
tags: [AI, mcp, tech]
order: 1
---

A remote, GitHub-OAuth-protected MCP server that gives any agent secure read/write access
to my Todoist — and quietly logs every tool call so the AI can answer "how much time did
you save me?" Two surfaces in one server: a Todoist control plane and an AI-usage
observability plane. Live on Cloudflare's edge since September 2025.

### Details

- **Live** — Cloudflare Workers
- **Stack** — Cloudflare Workers + Durable Objects + D1 + KV
- **Auth** — GitHub OAuth (per-user approval, not a shared API key)
- **SDK** — `@modelcontextprotocol/sdk`, `@cloudflare/workers-oauth-provider`

### Why it matters

Quantifies the AI's actual impact — every tool call lands in D1 with a configurable
time-saved weight, so the server can answer *"how much time did you save me this
month?"* with real numbers. Most MCP servers expose data; this one also measures
itself.
