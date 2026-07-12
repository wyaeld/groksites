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

## Adding a new site

Follow root `README.md` → **Adding a new site**. Minimum:

- Top-level directory name = site id (lowercase, no spaces)
- Own `hugo.toml` + `netlify.toml`
- Own `package.json` only if the site needs Node tooling
- Update the **Sites** table in root `README.md`

## Stack notes (typical site)

Pattern used by `mrwaterbed/` (not mandatory for every future site):

- Hugo static site
- Tailwind CSS v4 via `@tailwindcss/cli` (input `assets/css/main.css` → output `static/css/main.css`)
- Netlify: `npm run build`, publish `public`
- Single-page marketing layouts possible; redirects for old multi-page URLs may live in `netlify.toml`

## Git

- Repo root: `groksites/` (this directory).
- One remote can back many Netlify sites via different base directories.
- Do not nest a second `.git` inside a site directory.
- Commit only when the user asks.

## Agent checklist before finishing

- [ ] Edits are under the intended site (or intentionally root docs/ignore)
- [ ] No `public/` or `node_modules` staged
- [ ] Built CSS not reintroduced into git if the site regenerates it on build
- [ ] `baseURL` and contact/SEO params only changed when content/deploy target requires it
- [ ] Netlify redirects/headers still make sense after URL/structure changes
- [ ] Root README site table updated if a site was added/removed/renamed
