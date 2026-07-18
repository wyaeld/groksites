# Geek Press site

Hugo static site in the **groksites** monorepo at `geekpress/`.

## Stack
- Hugo extended (see `netlify.toml` for pinned `HUGO_VERSION`)
- Tailwind CSS v4 via Hugo `css.TailwindCSS` (`assets/css/main.css`)
- New (0.146+) template structure: `layouts/baseof.html`, `layouts/home.html`, `layouts/page.html`, `layouts/_partials/`. No `_default/`.

## Copy lives in data, not templates
- Page-level copy → front matter in `content/_index.md` and `content/*.md`.
- Site-wide chrome (brand, header CTA, footer headings, signup form strings) → `hugo.yaml` `params`.
- Templates contain only structure and Go template directives — no hardcoded English.

## Build
From this directory:

```bash
npm install
hugo server          # local
npm run build        # production
```

## Deploy
Netlify project **`geekpress`**, base directory `geekpress` on repo `wyaeld/groksites`. Domain: https://www.geekpress.me
