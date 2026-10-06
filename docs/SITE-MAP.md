# Little Craft Design — Site Map (Alan reference)

> Permanent inventory of every file that makes up the website, what it does, and what to touch when something needs fixing.  
> Source of truth for this doc: the live repo at `github.com/alanmeireles/littlecraftdesign` (`main` branch).  
> Local working copy: `/workspace/lcd-pages`.

---

## What this project is (plain English)

**Little Craft Design** is Manuela’s handmade cake-topper shop. This project is the shop’s **website** — a simple, static site built from ordinary web files:

- **HTML** — the pages and text visitors see  
- **CSS** — the colors, layout, and styling  
- **JavaScript** — the interactive bits (menu, gallery filters, shopping cart, checkout)

There is **no app server and nothing that has to “run” on a computer** for the site to work. The files live in **Alan’s GitHub repository** ([github.com/alanmeireles/littlecraftdesign](https://github.com/alanmeireles/littlecraftdesign)). **GitHub Pages** (GitHub’s free hosting) serves those files to anyone who opens the link. Visitors only download the files from GitHub’s servers; nothing runs on Alan’s or Manuela’s local machines.

**Live site:** [https://alanmeireles.github.io/littlecraftdesign/](https://alanmeireles.github.io/littlecraftdesign/)

**Payments:** The site does **not** take credit-card numbers. Checkout is meant to open **Stripe Payment Links** (Stripe’s own secure checkout pages). Until those links are filled in, the checkout button falls back to an **email order request** (`mailto:`) to `support@littlecraftdesign.com`. Card data never touches this website.

**How “pages” work:** There is only one real HTML file (`index.html`). Sections like Home, Gallery, Shop, and Checkout are shown/hidden with the URL hash (`#home`, `#shop`, etc.) — like tabs inside one page.

---

## Quick facts

| Item | Value |
|---|---|
| Live URL | https://alanmeireles.github.io/littlecraftdesign/ |
| Repo | https://github.com/alanmeireles/littlecraftdesign |
| Branch / deploy | `main` → GitHub Pages |
| Pages type | **Project site** (`username.github.io/repo/`) |
| Custom domain | **No** (no `CNAME` file) |
| Local source of truth | `/workspace/lcd-pages` |
| Local mirrors (not git) | `/workspace/lcd-site`, `/workspace/cake-toppers/website-mockup` |

---

## Architecture (read this first)

This is a **single-file SPA**, not a multi-page site with separate HTML/CSS/JS files.

| Reality | Detail |
|---|---|
| One HTML entry | `index.html` only (~1,797 lines) |
| CSS | **Inline** `<style>` block (~lines 11–671) — no `.css` files |
| JavaScript | **Inline** `<script>` block (~lines 1453–1794) — no `.js` files |
| “Pages” | Hash routes: `#home` `#gallery` `#how-it-works` `#shop` `#checkout` `#thank-you` |
| Hash aliases | `#pricing` and `#order` are remapped to `shop` in JS |
| Navigation | Links use `data-nav="…"`; JS `showPage()` / `hashchange` switches sections |
| Third-party scripts | **None** — no Stripe.js, no Google Fonts CDN, no analytics/gtag |
| Fonts | System stack only (`-apple-system` / Georgia) |
| Deploy helper | `.nojekyll` (empty file) — tells GitHub Pages to skip Jekyll processing |

---

## Full file tree

Paths are relative to the repo root. Only files that exist are listed (nothing invented).

```
littlecraftdesign/                 # repo root (= /workspace/lcd-pages locally)
├── .git/                          # git metadata; remotes → alanmeireles/littlecraftdesign
├── .nojekyll                      # empty; enables raw static Pages (no Jekyll)
├── index.html                     # THE entire site (HTML + CSS + JS)
├── STRIPE-SETUP.md                # owner how-to for Payment Links; ships on Pages, not linked in UI
├── docs/
│   └── SITE-MAP.md                # this file — permanent Alan reference
├── favicon.ico                    # browser tab icon (linked from <head>, ?v=2)
├── favicon-16x16.png
├── favicon-32x32.png
├── apple-touch-icon.png           # iOS home-screen icon
├── images/                        # all product / gallery / logo assets (38 files)
│   ├── logo-mark.png              # USED — nav, hero chip, footer (?v=2)
│   ├── logo.png                   # PRESENT but UNUSED (no src references)
│   └── gallery-*.jpg              # 36 gallery photos; also reused in hero / shop / occasions
└── screenshots/                   # QA captures; not referenced by index.html
    ├── [tracked] home / shop / gallery / checkout / thank-you / review (desktop + mobile)
    └── [may be local-only / untracked] header-hero, scrolled, footer, hero-bg-*, hero-crop
```

**What ships to GitHub Pages:** everything committed on `main` (including `STRIPE-SETUP.md`, tracked screenshots, and this `docs/` folder). The site UI never links to `screenshots/` or `STRIPE-SETUP.md`; they are reference/QA assets that happen to live in the same repo.

---

## File-by-file breakdown

### Root config & docs

| Path | What it does | How it connects |
|---|---|---|
| `index.html` | Sole site entry: all markup, styles, and behavior | GitHub Pages serves this at the site root / `#…` hashes |
| `.nojekyll` | Empty marker so Pages serves files as-is | Required for project Pages when you want no Jekyll processing |
| `STRIPE-SETUP.md` | Checklist of 14 Payment Links to create; paste URLs into `STRIPE_LINKS` | Documents keys that must match the JS config in `index.html` |
| `docs/SITE-MAP.md` | This inventory / “what to touch when” guide | Human reference only |
| `favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png` | Browser / device icons | Linked from `<head>` with `?v=2` cache-bust |

**Not present (confirmed):** `CNAME`, `_config.yml`, `robots.txt`, `package.json`, `README.md`, any `.css` or `.js` files, `node_modules`.

### `images/`

| Path | What it does | How it connects |
|---|---|---|
| `images/logo-mark.png` | Circular logo mark | Nav logo, hero logo chip, footer brand (`?v=2`) |
| `images/logo.png` | Alternate/full logo | **Unused** — safe to wire later or remove |
| `images/gallery-*.jpg` (36 files) | Real product photos | Gallery cards; also hero background/collage, home occasion tiles, most shop cards |

**Referencing rules**

- Paths are **relative** from repo root: `images/filename.jpg` (correct for project Pages base `/littlecraftdesign/`).
- Naming: `gallery-<theme>-<descriptor>.jpg`.
- Hero CSS background uses `images/gallery-baby-shower-gold-script-cake.jpg` plus a dark overlay.

### `screenshots/`

| Path | What it does | How it connects |
|---|---|---|
| Tracked PNGs (home, shop, gallery, checkout, thank-you, review — desktop 1280 & mobile 390) | Visual QA / design review captures | Not linked from the live site; available in the repo for Alan |
| Extra local PNGs (header-hero, scrolled, footer, hero-bg-*, hero-crop) | Local-only captures if untracked | Do not ship until committed |

---

## `index.html` — hash “pages” (SPA sections)

| Section ID | Hash | Role | Connects to |
|---|---|---|---|
| `page-home` | `#home` | Hero (photo BG + collage), occasion tiles, benefits, reviews CTA | Nav; CTAs → `#shop` / `#gallery`; occasion cards → gallery + filter |
| `page-gallery` | `#gallery` | 36 `.gallery-card`s + filter bar | Filters by `data-tags`; CTA → shop / Instagram |
| `page-how-it-works` | `#how-it-works` | Five steps + turnaround / rush copy | CTA → shop |
| `page-shop` | `#shop` | Six product cards + add-ons grid + FAQ | `.js-order` builds cart → `#checkout` |
| `page-checkout` | `#checkout` | Order summary + personalization form + pay button | Uses cart; opens Stripe or mailto |
| `page-thank-you` | `#thank-you` | Post-payment landing | Target for Stripe “after payment” redirect |

**Nav:** Home · Gallery · How It Works · Shop · Order (CTA, also `#shop`). Footer also links Checkout. Mobile: `#menuToggle` toggles `.nav-links.open`.

---

## CSS (all inside `index.html` `<style>`)

What the major blocks control:

- **Tokens / layout** — `:root` blush/gold palette, `.container`, `.section`, sticky `.site-header`, SPA `.page` / `.page.active`
- **Hero** — `#page-home .hero` full-bleed background image + overlay, `.hero-collage` / `.hero-photo-*`, glitter dots
- **Home grids** — `.occasions-grid`, `.benefits-grid`
- **Gallery** — `.filter-bar` / `.filter-btn`, `.gallery-grid` / `.gallery-card` / `.gallery-empty`
- **Shop** — `.shop-grid` / `.shop-card` / `.shop-field` / `.shop-price-from` / `.concept-art`
- **How it works** — `.steps` / `.step` / `.turnaround-note`
- **Shop extras** — `.addons` / `.addons-grid` / `.addon-item`, `.faq` accordion
- **Checkout** — `.checkout-layout` / `.summary-card` / `.checkout-form-card` / form controls / `.pay-status`
- **Shared frames** — `.photo-frame` (gallery, shop, occasion tiles)
- **Responsive** — `@media` breakpoints around 1000 / 900 / 640 / 560 px

### Leftover / unused CSS (old mockup — no matching HTML)

Safe to ignore or delete later:

- `.pricing-grid`, `.price-card`, `.price-badge`, `.price-tier`, `.price-amount`, `.price-features`, `.price-desc` (+ related mobile rules) — **old 3-tier pricing cards, not used**
- `.testimonials`, `.testimonial`, `.sample-tag`, `.stars` — reviews section is text-only now
- `.upload-zone` — site cannot upload files (photos via text/email)
- `.order-layout`, `.contact-sidebar` — superseded by `.checkout-layout`

**Still used (not leftover):** `.addons-grid` / `.addon-item` (rush / shipping / pickup / proof tiles on the shop page).

---

## JavaScript (all inside `index.html` `<script>`)

### Config block (primary edit zone — near top of `<script>`)

```js
var ORDER_EMAIL = 'support@littlecraftdesign.com';
var STRIPE_LINKS = {
  'photo-birthday-standard': '',
  'photo-birthday-large': '',
  'quince-standard': '',
  'quince-large': '',
  'quince-premium': '',
  'first-birthday-standard': '',
  'first-birthday-large': '',
  'name-age-standard': '',
  'name-age-large': '',
  'wedding-standard': '',
  'wedding-large': '',
  'wedding-premium': '',
  'soccer-standard': '',
  'soccer-large': ''
};
var SHIPPING_FLAT = 5.95;
var RUSH_FEE = 10;
var BUSINESS_PHONE = '(385) 208-1587';
```

**Current state:** all **14** `STRIPE_LINKS` values are empty → checkout uses the **mailto fallback**.

### What the script powers

| Feature | How |
|---|---|
| SPA routing | `showPage` / `navigate` / `hashchange`; page id list above |
| Mobile menu | `#menuToggle` |
| Gallery filters | `.filter-btn` + `data-tags` on cards; count badges; empty state |
| FAQ accordion | `.faq-q` toggles `.faq-item.open` |
| Cart | `readProductSelection` → in-memory `cart` + `sessionStorage['lcdCart']` |
| Checkout summary | `renderSummary` / `calcTotals` (base + addon + rush + shipping) |
| Pay button label | Link present → “Pay securely with Stripe”; else → “Send order request by email” |
| Checkout submit | If `STRIPE_LINKS[linkKey]` set → `window.open` Payment Link with `prefilled_email` + `client_reference_id`; else `mailto:ORDER_EMAIL` with full order body; if email also blank → clipboard + text phone |

**No Stripe.js** — the site only opens hosted Payment Link URLs when they are filled in.

---

## Shop: 6 products × sizes

| `data-product` | Sizes / prices | Premium? | Stripe link keys |
|---|---|---|---|
| `photo-birthday` | Standard $24 · Large $30 | No | `photo-birthday-standard` / `large` |
| `quince` | Standard $28 · Large $34 · Premium $45 | Yes | `quince-standard` / `large` / `premium` |
| `first-birthday` | Standard $22 · Large $28 | No | `first-birthday-standard` / `large` |
| `name-age` | Standard $14 · Large $18 | No | `name-age-standard` / `large` |
| `wedding` | Standard $28 · Large $34 · Premium $45 | Yes | `wedding-standard` / `large` / `premium` |
| `soccer` | Standard $24 · Large $30 | No | `soccer-standard` / `large` |

→ **14 Stripe Payment Link slots.**

Add-ons are per-product `<select class="js-addon">` (cupcakes, smash cake, picks). Rush (+$10) and delivery (local pickup free / US shipping $5.95) are on the **checkout form only** — not included in the base Stripe Payment Links (confirmed by text / follow-up payment per `STRIPE-SETUP.md`).

Photo Birthday shop card uses an inline SVG `.concept-art` placeholder (no photo yet). The other five products reuse gallery JPGs.

---

## Domain / DNS / GitHub Pages

| Check | Result |
|---|---|
| `CNAME` file | **Absent** → site is on the github.io project path, not a custom domain |
| `.nojekyll` | Present |
| `_config.yml` | Absent |
| Live confirmation | https://alanmeireles.github.io/littlecraftdesign/ serves this SPA |

When Stripe Payment Links are created, the after-payment redirect should be:

`https://alanmeireles.github.io/littlecraftdesign/#thank-you`

(If a custom domain is added later, update that redirect in Stripe and add a `CNAME` file.)

---

## External links & contact (as coded)

- Email: `support@littlecraftdesign.com` (`ORDER_EMAIL` + mailto links)
- Phone: `(385) 208-1587` / `+13852081587`
- Instagram: [@littlecraftdesign](https://www.instagram.com/littlecraftdesign/)
- Linktree: https://linktr.ee/littlecraftdesign
- Baker credits: [All Sweet Utah](https://www.instagram.com/allsweetutah/), [@dulzuradelamor](https://www.instagram.com/dulzuradelamor/)
- Facebook / Etsy footer slots: commented placeholders (no real URLs yet)

---

## Flags / unusual things

1. **Monolithic SPA** — almost every fix is in `index.html`.
2. **All 14 `STRIPE_LINKS` empty** — live checkout = mailto to `support@littlecraftdesign.com` (Stripe account not created yet per `STRIPE-SETUP.md`).
3. Constants present: `SHIPPING_FLAT = 5.95`, `RUSH_FEE = 10`, `ORDER_EMAIL = support@littlecraftdesign.com`, `BUSINESS_PHONE = (385) 208-1587`.
4. **Leftover CSS** for old pricing tiers, testimonials, and upload-zone.
5. **`logo.png` unused**; screenshots + `STRIPE-SETUP.md` ship publicly but aren’t in the UI.
6. Photos cannot upload on-site — customers text or email them after ordering.
7. No analytics, no font CDN, no Stripe.js.

---

## “What to touch when…” cheat sheet

| Task | Where |
|---|---|
| Change prices | Shop `<option data-price="…">` in `index.html` **and** matching rows in `STRIPE-SETUP.md` / Stripe Dashboard prices |
| Wire Stripe | Paste Payment Link URLs into `STRIPE_LINKS` near the top of the `<script>`; follow `STRIPE-SETUP.md` |
| Change shipping / rush fee | `SHIPPING_FLAT` / `RUSH_FEE` in JS config **plus** matching copy in shop FAQ / addons / checkout HTML |
| Change order email / phone | `ORDER_EMAIL` / `BUSINESS_PHONE` **plus** any hardcoded `mailto:` / `tel:` in the HTML |
| Fix cart / checkout / mailto | Inline `<script>` (cart, `renderSummary`, form submit) |
| Fix gallery filters | Filter button HTML (`data-filter`) + card `data-tags` + `applyFilter` JS |
| Add / replace a product photo | Drop JPG in `images/`; update `<img src="images/…">` in gallery and/or shop / hero |
| Change logo | Replace `images/logo-mark.png` (or update `src`); bump `?v=` |
| Change hero look | `#page-home .hero` CSS (background image / overlay) + `.hero-collage` HTML |
| Add a nav “page” | New `<section id="page-…">`, add id to `pages` array + nav `<li>` |
| Custom domain later | Add `CNAME` + DNS; update Stripe redirect URL in Dashboard / `STRIPE-SETUP.md` |
| Remove dead CSS | Delete unused `.pricing-grid` / `.price-*` / `.testimonials` / `.upload-zone` / `.order-layout` blocks |
| Deploy | Commit + push `main` (GitHub Pages deploys from `main`) |

---

## Local mirrors (workspace only)

These are **not** separate git remotes; they are local copies used during build/review:

- `/workspace/lcd-pages` — **canonical** git clone (push from here)
- `/workspace/lcd-site` — mirror of site-serving files (no `.git`)
- `/workspace/cake-toppers/website-mockup` — older mockup + extras (PDFs, design PNGs); also holds a synced `index.html` / images / favicons / `STRIPE-SETUP.md`

When updating the live site, change and push **`lcd-pages`**, then copy site files to the mirrors if you want local copies to stay in sync.

---

*Last inventoried from `/workspace/lcd-pages` against the live GitHub Pages URL. Do not invent files — only what exists in the repo is listed above.*
