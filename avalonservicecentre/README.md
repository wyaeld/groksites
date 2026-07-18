# Avalon Service Centre

Workshop marketing site for Avalon Service Centre (Avalon, Lower Hutt) — servicing, repairs, and WOFs.

**Stack:** Hugo · Tailwind CSS v4 (via `css.TailwindCSS`) · Netlify  
**Live:** https://avalonservicecentre.netlify.app (baseURL target: https://www.avalonservicecentre.co.nz/)  
**Netlify project:** `avalonservicecentre` (base directory `avalonservicecentre` in this monorepo)

## Brand

| | |
|--|--|
| Name | Avalon Service Centre |
| Tagline | Your one-stop auto workshop in Avalon, Lower Hutt. |
| Phone | 04 567 7429 |
| Mobile | 027 813 3040 |
| Email | avalonservicecentre@xtra.co.nz |
| Address | 170–174 Taita Drive, Avalon, Lower Hutt 5011 |
| Hours | Mon–Fri, 8am–5:30pm |
| Credentials | MTA Assured · AA Licensed Repairer |
| Region | Lower Hutt / Avalon, NZ |

## Local development

```bash
cd avalonservicecentre
npm install
hugo server
```

CSS is compiled by Hugo’s Tailwind pipe (`layouts/baseof.html` + `assets/css/main.css`). Node deps are still required so Tailwind is available to Hugo.

## Production build

```bash
npm run build
# or: npm ci && hugo --gc --minify
```

Output goes to `public/` (gitignored). Netlify runs the same via `netlify.toml`.

## Content

- Home copy → front matter in `content/_index.md`
- Contact / brand chrome → `hugo.yaml` `params`
- Templates: structure only where practical

## Deploy

Git continuous deploy on Netlify project **`avalonservicecentre`**:

| Setting | Value |
|---------|--------|
| Repo | `wyaeld/groksites` |
| Branch | `main` |
| Base directory | `avalonservicecentre` |
| Build | `npm ci && hugo --gc --minify` |
| Publish | `public` |
