# Avalon Service Centre site

Hugo static site in the **groksites** monorepo at `avalonservicecentre/`.

## Stack
- Hugo extended (see `netlify.toml` for pinned `HUGO_VERSION`)
- Tailwind CSS v4 via Hugo `css.TailwindCSS` (`assets/css/main.css`)
- Template structure: `layouts/baseof.html`, `layouts/home.html`, `layouts/_partials/`

## Copy
- Page-level copy → front matter in `content/_index.md`
- Site chrome and contact details → `hugo.yaml` `params`
- Prefer not hardcoding marketing copy in layouts

## Build
From this directory:

```bash
npm install
hugo server          # local
npm run build        # production
```

## Deploy
Netlify project **`avalonservicecentre`**, base directory `avalonservicecentre` on repo `wyaeld/groksites`.
