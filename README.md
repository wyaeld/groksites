# groksites

Monorepo for multiple independent [Hugo](https://gohugo.io/) websites, each deployed on [Netlify](https://www.netlify.com/).

## Layout

```
groksites/
├── README.md
├── AGENTS.md
├── .gitignore          # shared ignore rules for all sites
├── mrwaterbed/         # one site = one top-level directory
│   ├── hugo.toml
│   ├── netlify.toml
│   ├── package.json    # site-local deps (e.g. Tailwind)
│   ├── content/
│   ├── layouts/
│   ├── assets/
│   └── static/
├── geekpress/          # same pattern (may use hugo.yaml)
└── <another-site>/
```

Each site is a **standalone Hugo project**: its own config, content, theme/layouts, Node deps, and Netlify settings. Sites do not share build output or `node_modules`.

## Sites

| Directory     | Netlify project | Base directory | URL |
|---------------|-----------------|----------------|-----|
| `mrwaterbed/` | `waterbeds` | `mrwaterbed` | https://www.mrwaterbed.co.nz |
| `geekpress/`  | `geekpress` | `geekpress` | https://www.geekpress.me |

## Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended if a site needs it; see that site’s `netlify.toml` for the pinned version)
- [Node.js](https://nodejs.org/) (for sites that build CSS with Tailwind; see each `package.json`)

## Local development

Work inside the site directory:

```bash
cd mrwaterbed
npm install
npm run build:css   # if the site uses Tailwind
hugo server
```

Typical dual-terminal CSS + Hugo workflow (when the site defines these scripts):

```bash
npm run dev:css     # terminal 1 — watch CSS
hugo server         # terminal 2
```

Or a combined build/serve script if present:

```bash
npm run dev
```

## Production build

```bash
cd <site>
npm run build       # usually: CSS + hugo --minify
```

Output goes to that site’s `public/` directory (gitignored).

## Netlify continuous deploy (one Netlify site per subdir)

This monorepo uses **Git continuous deployment**: a push to `main` rebuilds each linked Netlify project. Each site folder maps to **its own** Netlify project with **Base directory** set to that folder.

### How sites are wired today

Each Netlify project builds one monorepo subdirectory (base directory = that folder):

| Setting | `mrwaterbed/` → `waterbeds` | `geekpress/` → `geekpress` |
|---------|-----------------------------|----------------------------|
| Site ID | `67f4b9e6-b2bb-478e-907c-cace62c46ed2` | `58511c40-0545-421f-bab7-84d25c0203e1` |
| Custom domain | **www.mrwaterbed.co.nz** | **www.geekpress.me** |
| Repo | `wyaeld/groksites` | `wyaeld/groksites` |
| Branch | `main` | `main` |
| Base directory | `mrwaterbed` | `geekpress` |
| Build command | `npm ci && npm run build` (or site `netlify.toml`) | `npm ci && hugo --gc --minify` (or site `netlify.toml`) |
| Publish directory | `public` (relative to base) | `public` (relative to base) |
| Path filter | `ignore` in that site’s `netlify.toml` | same pattern |

Do **not** set the monorepo root as the base directory unless you intentionally only build one site from root. Do **not** set Netlify **Package directory** to a nested path when it is the same as the base directory (that broke the first CD builds).

Suggested env (usually already in each `netlify.toml`):

- `HUGO_VERSION` — match local Hugo when possible  
- `NODE_VERSION` — match the site’s Node needs  
- `HUGO_ENV=production`  
- `HUGO_ENABLEGITINFO=true` (optional; needs Git available at build)

### Adding a new site (code + Netlify CD)

1. Create a new top-level directory: `mkdir newsite && cd newsite`
2. Scaffold Hugo: `hugo new site . --force` (or copy a sibling site as a template)
3. Add `netlify.toml` with build command, publish dir, Hugo/Node versions, headers/redirects, and an `ignore` path filter (copy from `mrwaterbed/netlify.toml`)
4. If using Tailwind (or other Node tooling), add `package.json` and scripts; keep deps **per site**
5. Document the site in this README’s **Sites** table
6. Create a **new** Netlify project (do not reuse another site’s base dir):
   - Connect repo `wyaeld/groksites`, production branch `main`
   - **Base directory** = `newsite`
   - Leave package directory empty / same as base
   - Build command / publish from that folder’s `netlify.toml`
## Shared vs per-site

| Concern | Where it lives |
|--------|----------------|
| Git ignore for Hugo/Node/Netlify | Root `.gitignore` |
| Repo overview & agent rules | Root `README.md`, `AGENTS.md` |
| Official Netlify agent skills | `.agents/skills/netlify-*/` (from [netlify/context-and-tools](https://github.com/netlify/context-and-tools)) |
| Site content, layouts, assets | `<site>/` |
| Hugo config | `<site>/hugo.toml` |
| Netlify config | `<site>/netlify.toml` |
| npm / Tailwind | `<site>/package.json` |

There is no shared Hugo theme or root `package.json` unless you intentionally add one later.

### Agent skills (Netlify)

Official Netlify skills are installed project-locally for agents (Claude, Grok, Codex, etc.):

```bash
npx skills add netlify/context-and-tools --skill '*' --yes
```

See `AGENTS.md` for which skills matter in this monorepo.
