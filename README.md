# KayRich Insurance Site Demo

Static demo of the redesigned kayrichinsure.com, deployed on Vercel.

**Live demo:** https://kayrich-site-demo.vercel.app (`/` redirects to `/Home.dc.html`). The KayRich reporting hub (`KayRich-Insurance-Reporting`) links to this URL. If the Vercel project is renamed or gets a different domain, update the card in that repo's `public/index.html`.

## Deploy on Vercel

1. In Vercel, choose **Add New → Project** and import `TomsTools11/kayrich-site-demo`.
2. Keep the project name **`kayrich-site-demo`**. That name produces the `kayrich-site-demo.vercel.app` URL the reporting hub links to.
3. Use these settings (most are picked up from `vercel.json` automatically):

| Setting | Value |
| --- | --- |
| Framework Preset | **Other** |
| Build Command | *(leave empty)* |
| Output Directory | `public` (already set by `vercel.json`) |
| Install Command | *(leave empty)* |
| Root Directory | `./` |

`vercel.json` pins `"framework": null` (the **Other** preset), `"installCommand": ""` and `"buildCommand": ""`. The deploy stays a plain static upload even if the import screen shows different defaults. There are no environment variables, no build step and no package manager. Every push to `main` redeploys.

Checked with `vercel build` (CLI 48.10.5). The output is exactly the files in `public/`; nothing from the repo root is included. `/` returns a 307 to `/Home.dc.html`, and unknown paths get `public/404.html`.

## Repository layout

```
public/                      # everything here is deployed and publicly reachable
  index.html                 # fallback redirect to Home.dc.html (for local preview / other hosts)
  404.html                   # served by Vercel for unknown paths
  Home.dc.html               # homepage
  Auto-Insurance.dc.html     # ...one file per page, see docs/routes.md
  Site-Nav.dc.html           # shared components, fetched at runtime by support.js
  Site-Footer.dc.html
  Quote-Band.dc.html
  support.js                 # page runtime (loads React 18 + Babel from unpkg.com)
  image-slot.js              # <image-slot> image placeholder component
  .image-slots.state.json    # image-slot state; empty, so slots show their placeholder
  assets/                    # logos and owner photography
  robots.txt                 # blocks crawlers
docs/
  routes.md                  # planned production routes, redirects, and NAP
vercel.json                  # Vercel output, redirect, and header config
```

## How the pages work

Each `*.dc.html` file is a component document. `support.js` boots the page and loads React, ReactDOM and Babel from `unpkg.com`. It then fetches the shared components (`Site-Nav`, `Site-Footer`, `Quote-Band`) from the same folder. Icons come from Phosphor on `unpkg.com`, and fonts (Outfit, Geist) come from Google Fonts. The site needs JavaScript and those CDNs to render.

Pages link to each other by filename, e.g. `href="Auto-Insurance.dc.html"`. Do not turn on `cleanUrls`. `support.js` works out which page to render from a URL that ends in `.dc.html`, so `/Auto-Insurance` would break. For the same reason, `/` is a **redirect** to `/Home.dc.html`, not a rewrite.

There is deliberately no Content-Security-Policy header. The runtime needs `unpkg.com` scripts and Babel's in-browser compilation, which requires `unsafe-eval`. A strict policy like the one on the reporting site would stop the pages from rendering.

The Auto, Home and Business coverage pages each have one `<image-slot>` with no image yet, so it renders as a captioned placeholder tile. To fill one, drop an image into the slot in the design tool, then copy the generated `.image-slots.state.json` into `public/`.

## What `vercel.json` does

- `framework: null`, `installCommand: ""`, `buildCommand: ""`: Other preset, no install, no build.
- `outputDirectory: "public"`: only `public/` is served.
- `cleanUrls: false`: pages keep their `.dc.html` URLs, which `support.js` needs.
- `redirects`: `/` → `/Home.dc.html` (temporary, 307).
- `Cache-Control: public, max-age=0, must-revalidate`: design updates show up immediately.
- `X-Robots-Tag: noindex, nofollow` (plus `robots.txt`): keeps the demo out of search results. Canonical and `og:url` tags already point at `https://www.kayrichinsure.com`.
- `Strict-Transport-Security`, `X-Content-Type-Options`, `Referrer-Policy`: standard hardening.

## Updating the demo

The source of truth is the design project export (`kayrich-site-redesign/KayRich Insurance Demo Site/`). To publish a new version, copy the `*.dc.html` files, `support.js`, `image-slot.js` and `assets/` into `public/`, then commit and push.

**Keep the logo SVGs from `kayrich-site-redesign/assets/`, not from the export.** The export's `alt-logo-1.svg` and `main-logo-1.svg` (about 9 KB each) have their embedded image data stripped and render blank. That hides the logo in the quote form on every page and on the 404 page. The originals (139 KB and 90 KB) have the same dimensions and render correctly. After copying an export, run `cp ../assets/alt-logo-1.svg ../assets/main-logo-1.svg public/assets/`. Check with `grep -c 'data:image' public/assets/*.svg`, which should print 2 for each file. `audit.md` and the single-file `kayrich-demo-standalone.html` export are intentionally left out. The audit is internal, and the standalone export only contains the homepage.

## Access note

Anyone with a Vercel deployment URL can open it. `robots.txt` and `X-Robots-Tag` keep it out of search results but don't restrict access. To restrict it, turn on Vercel Deployment Protection under **Project Settings → Deployment Protection**.

## Local preview

```bash
python3 -m http.server 4322 -d public
# then open http://localhost:4322/
```
