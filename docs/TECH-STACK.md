# Little Craft Design — Tech stack

> Plain-English inventory of **every technology actually used** in this repo, what it does here, and which files use it.  
> Source of truth: `/workspace/lcd-pages` (= [github.com/alanmeireles/littlecraftdesign](https://github.com/alanmeireles/littlecraftdesign) `main`).  
> Companion doc: [SITE-MAP.md](./SITE-MAP.md) (file-by-file site inventory).  
> Do not invent tech — only what exists in the repo is listed.

---

## One-sentence summary

A **static single-page website**: one `index.html` with **inline CSS** and **vanilla JavaScript**, product photos as **JPEG/PNG**, hosted on **GitHub Pages**, with checkout intended for **Stripe Payment Links** (currently falling back to **mailto** because all link slots are empty).

---

## Architecture

| Fact | Detail |
|---|---|
| Shape | Single-file SPA (`index.html` only for the live UI) |
| CSS | Inline `<style>` block (~lines 11–671) — **no** separate `.css` files |
| JavaScript | Inline `<script>` block (~lines 1453–1794) — **no** separate `.js` files |
| Routing | URL hash (`#home`, `#gallery`, `#shop`, …) show/hide `<section class="page">` blocks |
| Third-party scripts | **None** loaded on the page |
| Build step | **None** — edit files, commit, push `main` |

---

## Technologies in use

### 1. HTML5

**What it does here:** Markup for the whole shop — header/nav, six hash “pages” (home, gallery, how-it-works, shop, checkout, thank-you), forms, footer, favicon links, image tags.

**Key files:**
- `index.html` — sole HTML document (`<!DOCTYPE html>`, `<meta charset>`, viewport meta, semantic sections)

---

### 2. CSS3 (inline)

**What it does here:** All visual design — blush/gold palette via CSS custom properties (`:root`), layout (flex/grid), sticky header, hero background image + overlay, gallery/shop cards, checkout form, responsive `@media` breakpoints, leftover unused rules from an older mockup (pricing cards, testimonials, upload-zone).

**Key files:**
- `index.html` — the `<style>` block only (no `*.css` anywhere in the repo)

**Fonts (CSS only, no downloads):** system stacks declared as `--font` and `--font-display`:
- UI: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`
- Display headings: `Georgia, "Times New Roman", Times, serif`

---

### 3. Vanilla JavaScript (inline, no frameworks)

**What it does here:** SPA navigation (`showPage` / `hashchange` / `history.replaceState`), mobile menu, gallery filters, FAQ accordion, in-memory cart + `sessionStorage['lcdCart']`, checkout totals, pay-button label, checkout submit (Stripe Payment Link **or** mailto **or** clipboard fallback).

**Key files:**
- `index.html` — the `<script>` IIFE (~lines 1453–1794), including the `ORDER_EMAIL` / `STRIPE_LINKS` / `SHIPPING_FLAT` / `RUSH_FEE` / `BUSINESS_PHONE` config block

**Browser APIs used (built into the browser — not libraries):** DOM (`querySelector`, `classList`, …), History API, `sessionStorage`, `window.open`, `mailto:` via `location.href`, optional `navigator.clipboard`, form `checkValidity` / `reportValidity`.

**Not used:** React, Vue, Angular, Svelte, jQuery, TypeScript, modules (`import`/`require`), `fetch` / XHR, service workers.

---

### 4. Stripe Payment Links (config + hosted checkout — **not** Stripe.js)

**What it does here:** When a URL is pasted into `STRIPE_LINKS[product-size]`, checkout opens that Stripe-hosted Payment Link in a new tab (with optional `prefilled_email` and `client_reference_id` query params). Card numbers never touch this site.

**Current state:** all **14** `STRIPE_LINKS` values are empty strings → live checkout uses the **mailto** fallback instead.

**Key files:**
- `index.html` — `STRIPE_LINKS` object + submit handler that `window.open`s a filled link
- `STRIPE-SETUP.md` — owner checklist of the 14 links to create and paste (ships on Pages; not linked in the UI)

**Not used:** Stripe.js, Stripe Elements, Stripe Checkout JS SDK, any `js.stripe.com` script tag.

---

### 5. `mailto:` (email order fallback)

**What it does here:** While Payment Links are empty, the pay button opens the visitor’s email app to `support@littlecraftdesign.com` with a pre-filled subject/body built from the cart and form.

**Key files:**
- `index.html` — `ORDER_EMAIL`, checkout submit mailto branch, plus various `mailto:` links in the HTML body

---

### 6. Inline SVG

**What it does here:** Icons (hamburger menu, benefit icons, FAQ chevrons, turnaround clock) and one shop “design concept” illustration (Photo Birthday card) drawn as SVG markup inside the HTML — no external `.svg` files.

**Key files:**
- `index.html` only (~12 `<svg>` occurrences)

---

### 7. JPEG images

**What it does here:** Product / gallery / hero photos.

**Key files:**
- `images/gallery-*.jpg` — **36** photos; referenced from gallery cards, home occasion tiles, hero collage/background CSS, and most shop cards

---

### 8. PNG images

**What it does here:** Logos, favicons / touch icon, and QA screenshots.

**Key files:**
- `images/logo-mark.png` — **used** (nav, hero chip, footer; `?v=2` cache-bust)
- `images/logo.png` — present but **unused** (no `src` references)
- `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png` — linked from `<head>`
- `screenshots/*.png` — visual QA captures; **not** referenced by `index.html`

---

### 9. ICO favicon

**What it does here:** Classic browser tab icon.

**Key files:**
- `favicon.ico` — linked from `<head>` as `favicon.ico?v=2`

---

### 10. Markdown

**What it does here:** Human documentation only (not rendered as site pages by this project).

**Key files:**
- `docs/SITE-MAP.md` — permanent file inventory / “what to touch when”
- `docs/TECH-STACK.md` — this file
- `STRIPE-SETUP.md` — Stripe Payment Links setup checklist

---

### 11. GitHub Pages (static hosting)

**What it does here:** Serves the committed files on `main` at  
https://alanmeireles.github.io/littlecraftdesign/  
(project site: `username.github.io/repo/`). No app server.

**Related repo files:**
- Entire repo root as published (including docs/screenshots that the UI does not link to)
- **No** `CNAME` → no custom domain wired in-repo
- **No** `_config.yml` → not a Jekyll site config

---

### 12. `.nojekyll` (GitHub Pages marker)

**What it does here:** Empty file that tells GitHub Pages to **skip Jekyll** and serve files as raw static assets.

**Key files:**
- `.nojekyll` (empty)

---

### 13. Git

**What it does here:** Version control and deploy trigger (push to `main` → Pages rebuild).

**Key paths:**
- `.git/` (local); remote `origin` → `https://github.com/alanmeireles/littlecraftdesign.git`

---

## Browser features worth naming (not separate “libraries”)

These are standard browser capabilities the inline JS/HTML rely on — listed so they are not mistaken for third-party packages:

| Feature | Role in this project |
|---|---|
| URL hash + `hashchange` | SPA “pages” |
| History `replaceState` | Update `#…` without a full reload |
| `sessionStorage` | Persist cart across hash navigations (`lcdCart`) |
| HTML form validation | `required` fields + `checkValidity` / `reportValidity` |
| `loading="lazy"` / `decoding="async"` on `<img>` | Performance hints on gallery/shop photos |
| `tel:` links | Click-to-call / text prompt for `(385) 208-1587` |

---

## Explicitly **not** used (confirmed absent)

| Technology | Evidence |
|---|---|
| Python / PHP / Ruby / Go / etc. | No source files of those types |
| React / Vue / Angular / Svelte | No framework markers or build config |
| Node.js / npm / yarn | No `package.json`, no `node_modules` |
| Separate `.css` or `.js` files | Repo has none |
| Stripe.js / Elements | No Stripe script tags; Payment Links only |
| Google Fonts / any font CDN | No `fonts.googleapis.com` / `@font-face` downloads; system fonts only |
| jQuery / Bootstrap / Tailwind / etc. | Not present |
| Analytics (gtag, GA, Plausible, …) | No third-party scripts |
| Jekyll | `.nojekyll` present; no `_config.yml` |
| Service worker / PWA manifest | None |
| Custom domain in-repo | No `CNAME` |
| Image upload on-site | No upload UI wired; leftover `.upload-zone` CSS only |

---

## File ↔ technology map (quick)

| Path | Technologies |
|---|---|
| `index.html` | HTML5 + inline CSS3 + inline vanilla JS + inline SVG; references JPEG/PNG/ICO; mailto; Stripe Payment Link URLs (config) |
| `.nojekyll` | GitHub Pages (skip Jekyll) |
| `STRIPE-SETUP.md` | Markdown; documents Stripe Payment Links |
| `docs/SITE-MAP.md` | Markdown |
| `docs/TECH-STACK.md` | Markdown (this file) |
| `images/gallery-*.jpg` | JPEG |
| `images/logo-mark.png` | PNG (used) |
| `images/logo.png` | PNG (unused) |
| `favicon.ico` | ICO |
| `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png` | PNG icons |
| `screenshots/*.png` | PNG (QA only; not used by the live UI) |
| `.git/` | Git |

---

## Local mirrors (workspace only — not separate remotes)

When updating docs, keep these in sync if convenient:

- `/workspace/lcd-pages` — **canonical** git clone (push from here)
- `/workspace/lcd-site` — local mirror of site-serving files (no `.git`)
- `/workspace/cake-toppers/website-mockup` — older mockup tree; may hold synced `docs/`

---

*Inventoried from `/workspace/lcd-pages`. If a technology is not listed above, it is not part of this project.*
