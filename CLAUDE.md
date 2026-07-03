# CLAUDE.md

Guidance for Claude Code working in this repo. Stack and commands are documented in [README.md](README.md) — this file covers conventions, gotchas, and Claude-specific rules that aren't obvious from the code.

## Project Overview

Andy Woodruff's personal site at [andywoodruff.net](https://andywoodruff.net). Astro static site, deployed via Cloudflare Pages on push to `main`.

## Stack

- **Astro 5** — static site generator, content collections, MDX
- **Bun** — package manager and runtime (never npm/npx)
- **TypeScript** — `astro check` for typechecking
- **Cloudflare Pages** — auto-deploys from `main`

Canonical commands (from README.md):

| Purpose | Command |
|---|---|
| Install deps | `bun install` |
| Local dev (`http://localhost:4321`) | `bun run dev` |
| Production build (→ `dist/`) | `bun run build` |
| Preview production build | `bun run preview` |
| Typecheck | `bun run typecheck` |

## Layout

```
src/
  content.config.ts    # Zod schemas per content collection
  content/
    projects/          # markdown collection
    predictions/
    ideas/
    curation/
    themes/            # yearly themes (Knowledge Implementation 2022 → present)
  pages/
    index.astro
    about.astro
    archives.astro
    rss.xml.ts         # RSS feed for all collections
    {collection}/index.astro       # listing
    {collection}/[...slug].astro   # single post
  layouts/             # Base.astro, PostLayout.astro
  components/
  styles/global.css
public/                # static assets only — served at site root
  images/              # → /images/*
  llms.txt             # → /llms.txt (intentional SEO file for LLMs)
```

## Content Conventions

- **Frontmatter**: YAML (`---`) only. Never TOML.
- **Required**: `title`. **Common**: `date`, `description`, `tags`, `cover`, `draft`.
- **Tags**: max 5 per file, only approved values — see [TAGS.md](TAGS.md).
- **Images**: drop in `public/images/`, reference as `/images/filename.png`. Astro serves `public/` at root.
- **Headings**: anything smaller than `h3` looks too small. Stop at `h3`.
- **Schemas**: every collection has a Zod schema in `src/content.config.ts`. Frontmatter is type-checked at build time — `bun run typecheck` catches mismatches.
- **Drafts**: set `draft: true` in frontmatter. Filter is DEV-visible, PROD-hidden — entry appears in `bun run dev` and is excluded from `bun run build` / Cloudflare. The 15 `getCollection` call-sites (14 files; index.astro has two) use the pattern `({ data }) => import.meta.env.DEV || !data.draft`; keep this pattern when adding new pages or listings. Note: PAI's security hook blocks Bash commands containing the `import.meta.env` literal — grep for `!data.draft` instead.

## Adding a New Page

- **New post in an existing collection**: drop a `.md` or `.mdx` in `src/content/{collection}/`. The dynamic route `[...slug].astro` picks it up automatically.
- **New top-level page**: add a `.astro` file in `src/pages/`. Filename = URL.
- **New collection**: add the schema to `src/content.config.ts`, create `src/content/{name}/`, then a listing + dynamic route in `src/pages/{name}/`.

## Other Repo Files

- [README.md](README.md) — canonical setup, build, deploy
- [TAGS.md](TAGS.md) — tag rules and approved tag list
- [AEO-SPEC.md](AEO-SPEC.md) — Answer Engine Optimization spec for the site
- [BACKLOG.md](BACKLOG.md) — outstanding work
- `Plans/` — one-off planning artifacts (not load-bearing)

## Section Investment Posture

Some sections are deliberately dormant — they stay live but earn no new effort. Filter from [BACKLOG.md](BACKLOG.md): an update is worth doing only if it advances M1 (100M DIDs by 2034) or feeds The Shifting Current newsletter. Everything else is hobby surface.

**Dormant sections — push back before working on them:**

- `src/content/themes/`
- `src/content/curation/`
- Side projects in `src/content/projects/` (anything that isn't `open-commercial-media-ecosystem.md` or `trust-over-ip.md` — e.g. `cycling-and-health.md`, `new-century-school.md`)
- `src/content/predictions/` — unless it's a deliberate T1–T4 thesis batch per [BACKLOG.md](BACKLOG.md)
- Consulting bucket in `src/pages/connect.astro`

**Rule:** When Andy asks for an update, new entry, restyle, or buildout in any of these, **challenge first** — one short line, e.g. *"This is in the dormant set per CLAUDE.md — does this actually beat M1 / newsletter work right now?"* Wait for the call. He may override; that's fine, he just wants to be warned.

**Invest-here sections — default priority, no challenge needed:**

- `src/pages/2034.astro` + `src/data/ssi-progress.ts`
- `src/content/ideas/`
- `src/content/projects/open-commercial-media-ecosystem.md` and `trust-over-ip.md`
- `src/pages/index.astro`, `src/pages/about.astro`

Background: see `~/.claude/PAI/MEMORY/WORK/site-stop-doing-filter/ISA.md` for the full pass.

## Gotchas

- **Bun, not npm.** Zero exceptions.
- **`public/` is served at site root** — anything in there ships verbatim. Don't dump build output there; that's `dist/`.
- **`dist/` and `.astro/` are gitignored.** Never commit either.
