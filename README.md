# Andy Woodruff

Personal site for [andywoodruff.net](https://andywoodruff.net). Built with Astro, deployed to a Cloudflare Worker (static assets) via `wrangler` / the `/deploy` command.

## Local Development

```bash
bun install         # install deps (one-time)
bun run dev         # local dev server with hot reload at http://localhost:4321
bun run build       # production build to ./dist
bun run preview     # serve the production build locally
bun run typecheck   # astro check (type errors in .astro files + content schemas)
```

## Project Structure

```
src/
  content.config.ts          # Zod schemas for each content collection
  content/                   # markdown / mdx — one folder per collection
    projects/
    predictions/
    ideas/
    curation/
  layouts/
    Base.astro               # shell: head, nav, footer
    PostLayout.astro         # individual post wrapper
  pages/
    index.astro              # home
    about.astro
    archives.astro
    rss.xml.ts               # RSS feed for all collections
    {collection}/index.astro       # listing page
    {collection}/[...slug].astro   # dynamic single-post page
  styles/global.css
  components/
public/
  images/                    # static assets served at /images/*
```

## Adding Content

Drop a markdown file into `src/content/{collection}/`. Frontmatter is type-checked at build time per the schemas in `src/content.config.ts`. Required: `title`. Optional: `date`, `description`, `tags` (max 5 per [TAGS.md](TAGS.md)), `cover`, `draft`.

## Deploy (Cloudflare Worker — static assets)

**Deploy and source control are decoupled.** `git push` records source only — it does **not** deploy. Deploys are explicit, via `wrangler`:

```
bunx wrangler deploy    # builds are separate — run `bun run build` first
```

This uploads `dist/` as static assets to the `andywoodruff6` Cloudflare Worker (config in [`wrangler.jsonc`](wrangler.jsonc)); `andywoodruff.net` goes live within seconds. In practice, use the **`/deploy`** Claude command, which bundles version-bump → build → `wrangler deploy` → live-verify → commit → push.

> History: this was a git-connected Cloudflare **Pages** project until Cloudflare's Workers-Builds migration (July 2026) broke its auto-build. The project is now a Worker with static assets, deployed manually via `wrangler`; the GitHub build trigger is disconnected so pushes never auto-deploy.

## Content Guidelines

- YAML frontmatter (`---`) only, never TOML.
- See [TAGS.md](TAGS.md) for tagging rules and approved tags (max 5 per file).
- Anything smaller than an `h3` looks too small.
