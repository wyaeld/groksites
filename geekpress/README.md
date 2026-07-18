# Geek Press

Marketing site for Geek Press — technical partner for the AI era (vibe coding, security, applications, marketing video).

**Stack:** Hugo · Tailwind CSS v4 (via `css.TailwindCSS`) · Netlify  
**Live:** https://www.geekpress.me  
**Netlify project:** `geekpress` (base directory `geekpress` in this monorepo)

## Brand

| | |
|--|--|
| Name | Geek Press |
| Tagline | Build with AI. Ship software that's secure, scalable, and seen. |
| Site | https://www.geekpress.me |
| Email | hello@geekpress.me |
| Font | Inter |
| Accent | brand blue (`#2563eb`) + amber accent (`#f59e0b`) |

## Local development

```bash
cd geekpress
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

- Page copy → front matter in `content/_index.md` and `content/*.md`
- Site chrome (brand, header CTA, footer) → `hugo.yaml` `params`
- Templates: structure only — avoid hardcoding marketing copy in layouts

## Deploy

Git continuous deploy on Netlify project **`geekpress`**:

| Setting | Value |
|---------|--------|
| Repo | `wyaeld/groksites` |
| Branch | `main` |
| Base directory | `geekpress` |
| Build | `npm ci && hugo --gc --minify` |
| Publish | `public` |
