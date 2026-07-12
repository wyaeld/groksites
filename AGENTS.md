# AGENTS.md — groksites

Instructions for AI agents working in this repository.

## What this repo is

A **monorepo of independent Hugo websites**, each deployed on Netlify as its own Netlify site. The Git root is not a single Hugo project.

```
groksites/                 # Git root only
  <site>/                  # one Hugo site per directory
    hugo.toml
    netlify.toml
    package.json           # optional; per-site Node/Tailwind
    content/ layouts/ assets/ static/ …
```

Current sites: see root `README.md`.

## Hard rules

1. **Scope changes to one site** unless the user explicitly asks for monorepo-wide work (root docs, `.gitignore`, multi-site refactors).
2. **Do not invent a shared theme, root `package.json`, or cross-site imports** unless requested. Sites stay independent.
3. **Do not commit secrets.** Never commit `.env`, keys, or Netlify tokens. Prefer Netlify UI / env vars for deploy secrets.
4. **Do not commit build output.** `public/`, `resources/`, `node_modules/`, and built `static/css/main.css` are gitignored — regenerate via site scripts.
5. **Prefer surgical edits.** Match existing layout/CSS patterns in that site; no drive-by refactors of sibling sites.
6. **Netlify base directory is the site folder.** Build and publish paths in `netlify.toml` are relative to that folder, not the monorepo root.

## Where to work

| Task | Location |
|------|----------|
| Content, copy, pages | `<site>/content/`, sometimes layouts for single-page sites |
| Markup / templates | `<site>/layouts/` |
| Site params, menus, baseURL | `<site>/hugo.toml` |
| Tailwind / CSS source | `<site>/assets/css/` (build → `static/css/`, gitignored) |
| Deploy build, headers, redirects | `<site>/netlify.toml` |
| npm scripts & Tailwind deps | `<site>/package.json` |
| Shared ignore / monorepo docs | Root `.gitignore`, `README.md`, this file |

## Commands (run from the site directory)

```bash
cd <site>
npm install          # if package.json exists
npm run build:css    # Tailwind sites
hugo server          # local preview
npm run build        # production: CSS + hugo (see package.json)
```

Pin Hugo/Node versions to what that site’s `netlify.toml` declares when debugging deploy mismatches.

## Continuous deployment

- One Git repo (`wyaeld/groksites`) → many Netlify projects (one per site folder).
- Each Netlify project: **Base directory** = that site’s top-level folder (e.g. `mrwaterbed`). Package directory should not be a nested path when it equals the base.
- Push to `main` triggers a build per linked project; each site’s `netlify.toml` `ignore` should skip builds when only other monorepo paths changed.
- Prefer Git CD over manual `netlify deploy --prod` once a site is linked.

## Adding a new site

Follow root `README.md` → **Adding a new site**. Minimum:

- Top-level directory name = site id (lowercase, no spaces)
- Own `hugo.toml` + `netlify.toml`
- Own `package.json` only if the site needs Node tooling
- Update the **Sites** table in root `README.md`
- Create a **new** Netlify project with base directory = that folder (same repo, branch `main`)

## Stack notes (typical site)

Pattern used by `mrwaterbed/` (not mandatory for every future site):

- Hugo static site
- Tailwind CSS v4 via `@tailwindcss/cli` (input `assets/css/main.css` → output `static/css/main.css`)
- Netlify: `npm run build`, publish `public`
- Single-page marketing layouts possible; redirects for old multi-page URLs may live in `netlify.toml`

## Official Netlify skills

This repo vendors Netlify’s official agent skills from
[`netlify/context-and-tools`](https://github.com/netlify/context-and-tools):

- Path: `.agents/skills/netlify-*/`
- Lockfile: root `skills-lock.json` (source + content hashes)
- Most relevant for this monorepo: **netlify-config**, **netlify-deploy**, **netlify-forms**, **netlify-caching**, **netlify-image-cdn**

When changing `netlify.toml`, deploys, domains, redirects, headers, env vars, forms, or Netlify primitives, **read the matching skill’s `SKILL.md` first** (and any `references/` files it points to). Prefer those facts over inventing Netlify APIs.

Refresh / reinstall:

```bash
npx skills add netlify/context-and-tools --skill '*' --yes
```

(Also available as the **netlify** plugin from the xAI Grok marketplace, or Claude `/plugin install netlify-skills@netlify-context-and-tools`.)

## Git

- Repo root: `groksites/` (this directory).
- One remote can back many Netlify sites via different base directories.
- Do not nest a second `.git` inside a site directory.
- Commit only when the user asks.
- Tracked agent content: `.agents/skills/` only. Other `.agents/`, `.claude/`, `.grok/` dirs stay gitignored.

## QA checks

Run these before calling a site “done” or shipping a content/layout/image change. Scope to the site you touched. Fail open: if something is missing, fix it or flag it to the user — do not silently skip.

### Images

- [ ] Every `<img>` (and CSS background image that ships a file) maps to a real asset under that site’s `static/` (or build pipeline).
- [ ] **Display size vs file size:** intrinsic dimensions are appropriate for how the image is used in layout (CSS `max-width` / fixed sizes). Target roughly **2× the largest CSS display size** for retina; do not ship multi‑MB sources used at a few hundred pixels.
- [ ] **Width/height attributes** on `<img>` match the file’s real aspect ratio (correct intrinsic size after resize).
- [ ] **WebP (or better):** convert raster photos/illustrations to WebP when alpha/quality allow; keep PNG only when needed (e.g. tiny favicon). Prefer JPEG for OG/social cards if scrapers need it.
- [ ] Masters may live in `assets/`; **only optimized files** should be what Hugo serves from `static/images/` (or equivalent).

### Analytics & tags

- [ ] **Google Analytics** is configured when the site should track traffic: measurement ID in site config (e.g. `params.googleAnalytics` in `hugo.toml`), partial included in the head, snippet present in production HTML.
- [ ] GA (and similar third-party tags) **do not fire on local `hugo server`** if that is the site’s pattern; they **do** appear in production builds.
- [ ] **Meta / head tags** are present and sensible: `<title>`, meta description, canonical, Open Graph (`og:title`, `og:description`, `og:image`, `og:url`), Twitter card tags, favicon, `theme-color` where used.
- [ ] Forms that should collect leads use **Netlify Forms** (or the site’s chosen provider) with correct `name` / field names; honeypot or equivalent spam protection when applicable.

### Copy review

- [ ] **Second-agent copy review:** before treating marketing/landing copy as final, have **another agent** (or a dedicated review pass) read the visible copy for tone, clarity, NZ spelling if relevant, broken claims, and consistency with brand voice. Do not rely on a single authoring pass alone.
- [ ] Headings, CTAs, contact details, and legal-ish claims match what is in site params / README brand notes.

### Brand & site docs

- [ ] The site has a **`README.md`** under `<site>/` with **brand details**: name, positioning/tagline, primary colours, fonts, contact (phone/email), region/service area, and any voice/tone notes an agent needs to stay on-brand.
- [ ] Contact and brand facts in layouts/`hugo.toml` match that README (no drift).

### Deploy / monorepo sanity

- [ ] Site builds cleanly (`npm run build` or site equivalent).
- [ ] Netlify base directory / `HUGO_VERSION` still make sense for that site after config changes.

## Agent checklist before finishing

- [ ] Edits are under the intended site (or intentionally root docs/ignore)
- [ ] No `public/` or `node_modules` staged
- [ ] Built CSS not reintroduced into git if the site regenerates it on build
- [ ] `baseURL` and contact/SEO params only changed when content/deploy target requires it
- [ ] Netlify redirects/headers still make sense after URL/structure changes
- [ ] Root README site table updated if a site was added/removed/renamed
- [ ] **QA checks** above completed (or gaps called out to the user) for the work just done
