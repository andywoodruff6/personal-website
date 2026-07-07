---
description: Bump version, build, deploy to production via wrangler, then commit + push source
---

Deploy the andywoodruff.net site. **Deploy and source-control are decoupled:** `wrangler deploy` pushes the built site to production (the `andywoodruff6` Cloudflare Worker, static-assets); `git push` only records source. A plain `git push` does NOT deploy — this command is the only thing that ships to prod.

> Background: Cloudflare's Workers-Builds migration broke the old git-connected Pages auto-deploy (its `npx wrangler deploy` step fails on the existing `andywoodruff6` Worker, and it tries to SSR-convert this static site). The push-triggered build is disabled/ignored; `wrangler deploy` from `wrangler.jsonc` is the sanctioned deploy path. See ISA `site-sell-the-hire-sprint` (2026-07-07) for the full diagnosis.

## Prereqs (verify once, cheap)

- wrangler authed as `andywoodruff6@gmail.com` (`bunx wrangler whoami` → that email; if not, `bunx wrangler login`).
- `wrangler.jsonc` present at repo root (`name: "andywoodruff6"`, `assets.directory: "./dist"`).

## Steps

1. **Check for uncommitted changes**: `git status` + `git diff` to gather all real changes for the commit message.

2. **Bump the patch version**: Read `package.json`, parse `version` (`major.minor.patch`), increment patch by 1, write back. `0.1.0` → `0.1.1`.

3. **Build**: `bun run build`. Abort if it fails — never deploy or commit broken code.

4. **Deploy to production** (the go-live step): `bunx wrangler deploy`. This uploads `dist/` static assets to the `andywoodruff6` Worker; `andywoodruff.net` (and `theshiftingcurrent.com`) go live within seconds. Capture the printed Version ID. If the deploy fails, STOP and report — do not proceed to commit.
   - The `andywoodruff6@gmail.com` wrangler token is single-account, so no `CLOUDFLARE_ACCOUNT_ID` is needed. If wrangler ever errors with "more than one account," grab the id from `bunx wrangler whoami` and prefix `CLOUDFLARE_ACCOUNT_ID=<id> bunx wrangler deploy` (don't hardcode the id in this tracked file — the repo is public).

5. **Verify live** (mandatory): `curl -sL -o /dev/null -w "%{http_code}" https://andywoodruff.net/` → expect 200, plus one route touched by this change (remember trailing slash: `/work` 307→`/work/` 200). Prefer the Interceptor skill for a visual check when the change is visual.

6. **Commit** (source control): `git add -A` (include `wrangler.jsonc` if new). Analyze `git diff --staged`, then compose:
   - **Title**: conventional prefix (`feat:`/`fix:`/`refactor:`/`chore:`/`style:`/`docs:`) from the actual work. Never mention the version bump in the title.
   - **Body**: tight bulleted summary (3–8 bullets) of what changed and why — impact, not file-by-file.
   - **Trailer**: `Co-Authored-By: Ekko <noreply@anthropic.com>`.
   - Version-bump-only change → `chore: bump version to <new_version>`, no body.

7. **Push** (source control only — does NOT deploy): this repo's `origin` needs the `andywoodruff6` gh account (per CLAUDE.md). 
   ```
   gh auth switch --user andywoodruff6 && git push origin main ; gh auth switch --user andy-ocme
   ```

8. **Report**: version change (`0.1.0 → 0.1.1`), commit SHA, wrangler Version ID, and the live 200 confirmation.

## Important

- **`wrangler deploy` is the deploy; `git push` is not.** Never claim "it'll deploy from the push."
- Never skip the version bump.
- Never run `npm` / `npx`. Always `bun` / `bunx`.
- Never `--no-verify` or `--force` push.
- If the build or the wrangler deploy fails, STOP. Do not commit/push broken code.
