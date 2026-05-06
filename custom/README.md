# Super Green Kutt customizations

This directory holds Super Green's customizations of Kutt. Per Kutt's docs, files
placed here override or augment Kutt's defaults at runtime — **without modifying
Kutt's source code**, which keeps `git merge upstream/main` cheap.

## Layout

```
custom/
├─ css/
│  └─ super-green.css   ← brand color + typography overrides (additive)
├─ images/              ← favicon, logo.png, card.png replacements (filename-matched)
└─ views/               ← per-template overrides; mirror server/views/ structure
```

## How loading works

- **`css/`** — every `.css` file is auto-loaded on every page after Kutt's default
  `styles.css`. Naming a file `styles.css` REPLACES the default entirely; any
  other name is **additive**. We use additive (`super-green.css`) so we keep
  Kutt's component styles and only re-skin via CSS variables.
- **`images/`** — drop a file with the same name as one in `static/images/` and
  it replaces the original. Files become available at `/images/<name>`.
- **`views/`** — drop an `.hbs` file matching `server/views/<name>.hbs` and Kutt
  renders yours instead. **Caveat**: when upstream renames blocks/partials, your
  override can desync silently — re-check after `git merge upstream/main`.

## Branding source of truth

Brand tokens (colors, type, radius) come from
[`sg-ui-kit`](https://github.com/Super-Green/sg-ui-kit) — see
`lib/brand-tokens.ts`. The current overrides map Kutt's CSS variables to:

- Magenta `#FB2185` — primary buttons, focus rings, accents
- Light Cream `#F9F3EB` — page background
- Cream `#F0E0CC` — table surfaces
- Purple `#1A1A33` — body text and secondary buttons
- Neon `#BCF400` — success states
- Orange `#F9562B` — danger / error states
- 14px (`0.875rem`) border radius

## TODOs

- [ ] Replace `static/images/logo.png` with a Super Green wordmark (need a 200x60 PNG)
- [ ] Replace `static/images/favicon-*.png` and `favicon.ico` (need 16/32/196 PNG + ICO)
- [ ] Replace `static/images/card.png` (OG share card, 1200x630)
- [ ] Bundle `PPFragment-Glare` woff2 files and serve from `/fonts/` for true brand display type
- [ ] Override `views/homepage.hbs` for a branded landing page
- [ ] Override `views/login.hbs` and `views/create_admin.hbs` for branded auth flows

## Workflow

```bash
# Edit a file in custom/, then:
git add custom/
git commit -m "branding: <what you changed>"
git push origin main
# Render auto-deploys.
```

## Pulling upstream Kutt updates

```bash
git fetch upstream
git merge upstream/main
# Resolve any conflicts (rare — should only touch custom/ here).
git push origin main
```
