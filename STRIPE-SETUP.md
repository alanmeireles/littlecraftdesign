# Stripe Payment Links setup — Little Craft Design

Owner Stripe account is **not created yet**. When it is ready, create the Payment Links below and paste each URL into `STRIPE_LINKS` in `index.html` (near the top of the `<script>` block).

Also set `ORDER_EMAIL` in that same config block to the owner’s real order inbox (do not invent one). Until then it stays empty and the checkout button uses the “coming soon” path (text/email order details to the phone).

Live site: https://alanmeireles.github.io/littlecraftdesign/

---

## Shared settings for every Payment Link

In Stripe Dashboard → **Payment Links** → Create:

| Setting | Value |
|---|---|
| Product name | Use the **Stripe product name** column below |
| Price | Exact USD amount below (one-time) |
| After payment → Redirect customers to | `https://alanmeireles.github.io/littlecraftdesign/#thank-you` |
| Collect phone number | **On** |
| Collect shipping address | **On** (buyers who pick up locally can still leave N/A / shop address notes; or turn off later if you prefer pickup-only links) |
| Custom fields | Add two text fields: **“Name / wording for topper”** and **“Age / number”** (optional for age) |
| Quantity | Allow customers to adjust quantity: **Off** (one topper per link; add-ons handled separately) |
| Allow promotion codes | Optional |

**Add-ons & rush:** These Payment Links cover **product + size only**. Cupcake toppers, smash-cake add-ons, picks, and rush (+$10) are selected on the website and confirmed by text; you can send a second Payment Link or a custom invoice for add-ons, or create optional “add-on” Payment Links later.

**Shipping:** Website offers free local pickup (American Fork / Utah County) or US shipping at a suggested **$5.95** flat (from the Etsy launch kit). You can add a $5.95 shipping amount in Stripe Checkout, or keep shipping off-link and settle by text.

---

## Payment Links to create (14)

Paste each resulting URL into `STRIPE_LINKS['…']` with the matching key.

| Config key (`STRIPE_LINKS`) | Stripe product name | Price (USD) |
|---|---|---|
| `photo-birthday-standard` | Custom Photo Birthday Topper — Standard (~6 in) | **24.00** |
| `photo-birthday-large` | Custom Photo Birthday Topper — Large (~8 in) | **30.00** |
| `quince-standard` | Quinceañera / 15 Anos Topper — Standard (~6 in) | **28.00** |
| `quince-large` | Quinceañera / 15 Anos Topper — Large (~8 in) | **34.00** |
| `quince-premium` | Quinceañera / 15 Anos Topper — Premium layered (~8 in) | **45.00** |
| `first-birthday-standard` | First Birthday Topper — Standard (~6 in) | **22.00** |
| `first-birthday-large` | First Birthday Topper — Large (~8 in) | **28.00** |
| `name-age-standard` | Name & Age Topper — Standard (~6 in) | **14.00** |
| `name-age-large` | Name & Age Topper — Large (~8 in) | **18.00** |
| `wedding-standard` | Mr & Mrs Photo Wedding Topper — Standard (~6 in) | **28.00** |
| `wedding-large` | Mr & Mrs Photo Wedding Topper — Large (~8 in) | **34.00** |
| `wedding-premium` | Mr & Mrs Photo Wedding Topper — Premium layered (~8 in) | **45.00** |
| `soccer-standard` | Soccer / Sports Topper — Standard (~6 in) | **24.00** |
| `soccer-large` | Soccer / Sports Topper — Large (~8 in) | **30.00** |

---

## Where to paste URLs

In `/workspace/lcd-pages/index.html` (and the synced copies):

```js
var ORDER_EMAIL = ''; // TODO: set owner order email when known
var STRIPE_LINKS = {
  'photo-birthday-standard': 'https://buy.stripe.com/...',
  'photo-birthday-large': '',
  // …etc
};
```

When a key’s URL is non-empty, checkout opens that link with:

- `?prefilled_email=` (customer email from the form)
- `&client_reference_id=` (generated order reference)

When a key is empty, the button label becomes:

> Online payment coming soon — we'll text you a payment link

…and the order details are sent via `mailto:` to `ORDER_EMAIL` (if set) or copied for the customer to text to **(385) 208-1587**.

---

## Optional later: add-on Payment Links

| Suggested name | Price |
|---|---|
| Rush production (2–3 business days after proof) | 10.00 |
| 12 matching cupcake toppers (photo/quince/wedding/soccer) | 10.00 |
| 24 matching cupcake toppers (photo/quince/wedding) | 18.00 |
| 12 matching cupcake toppers (name & age) | 8.00 |
| 24 matching cupcake toppers (name & age) | 14.00 |
| Mini smash-cake topper (first birthday) | 6.00 |
| Smash-cake + 12 cupcake toppers (first birthday) | 15.00 |
| Ball & flag picks set (soccer) | 8.00 |
| Picks set + 12 cupcake toppers (soccer) | 16.00 |
| US shipping (flat) | 5.95 |

---

## Checklist after creating links

- [ ] All 14 URLs pasted into `STRIPE_LINKS`
- [ ] `ORDER_EMAIL` set to the real owner inbox
- [ ] After-payment redirect points to `#thank-you`
- [ ] Test one link end-to-end (Shop → Checkout → Stripe → thank-you)
- [ ] Commit and push to `main` to deploy on GitHub Pages
