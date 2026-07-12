# Mr Waterbed NZ

Mobile-first marketing site for Colin’s soft-sided waterbed business.

**Stack:** Hugo · Tailwind CSS v4 · Netlify

## Local development

```bash
cd mrwaterbed
npm install
npm run build:css
hugo server
```

Or run CSS watch in one terminal and Hugo in another:

```bash
npm run dev:css   # terminal 1
hugo server       # terminal 2
```

## Production build

```bash
npm run build
```

Output is written to `public/`. Netlify runs the same command (`netlify.toml`).

## Content

Single-page site: home content is in `layouts/home.html`. Site title/description and contact details are in `hugo.toml` and `content/_index.md`.

## Deploy

Connect the `mrwaterbed` directory (or monorepo base) to Netlify. Build command and publish directory are already set in `netlify.toml`.
