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
└── <another-site>/     # same pattern
```

Each site is a **standalone Hugo project**: its own config, content, theme/layouts, Node deps, and Netlify settings. Sites do not share build output or `node_modules`.

## Sites

| Directory    | Description                          | Production URL              |
|-------------|--------------------------------------|-----------------------------|
| `mrwaterbed/` | Mr Waterbed NZ marketing site      | https://www.mrwaterbed.co.nz/ |

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

## Netlify deploy (one site per Netlify site)

Create a **separate Netlify site** for each folder you want live.

1. Connect this Git repository.
2. Set **Base directory** to the site folder (e.g. `mrwaterbed`).
3. Leave build/publish settings to that folder’s `netlify.toml` (or set them explicitly):
   - **Build command:** typically `npm run build`
   - **Publish directory:** `public` (relative to the base directory)

Do **not** set the monorepo root as the base directory unless you intentionally only build one site from root.

Suggested Netlify environment (often already in each `netlify.toml`):

- `HUGO_VERSION` — match local Hugo when possible  
- `NODE_VERSION` — match the site’s Node needs  
- `HUGO_ENV=production`  
- `HUGO_ENABLEGITINFO=true` (optional; needs Git available at build)

### Monorepo tip

With base directory set, Netlify only runs that site’s build. Other folders in the repo are ignored for that deploy. You can still use [Netlify monorepo / path filters](https://docs.netlify.com/configure-builds/file-based-configuration/) or ignore builds when unrelated paths change, if you want faster CI.

## Adding a new site

1. Create a new top-level directory: `mkdir newsite && cd newsite`
2. Scaffold Hugo: `hugo new site . --force` (or copy a sibling site as a template)
3. Add `netlify.toml` with build command, publish dir, Hugo/Node versions, and any headers/redirects
4. If using Tailwind (or other Node tooling), add `package.json` and scripts; keep deps **per site**
5. Document the site in this README’s **Sites** table
6. On Netlify: new site → same repo → **Base directory** = `newsite`

## Shared vs per-site

| Concern | Where it lives |
|--------|----------------|
| Git ignore for Hugo/Node/Netlify | Root `.gitignore` |
| Repo overview & agent rules | Root `README.md`, `AGENTS.md` |
| Site content, layouts, assets | `<site>/` |
| Hugo config | `<site>/hugo.toml` |
| Netlify config | `<site>/netlify.toml` |
| npm / Tailwind | `<site>/package.json` |

There is no shared Hugo theme or root `package.json` unless you intentionally add one later.
