# Musk Fragrances — Homepage & Conversion Audit
_Prepared 2026-09-11, from live Shopify Admin data (30-day trailing window). Not from a visual crawl of the storefront — outbound access to muskfragrances.com and the Shopify CDN was blocked from this session's network, so every number below comes from the connected Shopify Admin and Klaviyo accounts rather than a screenshot._

Companion visual concept: [Musk Fragrances Redesign](https://claude.ai/code/artifact/225804bb-2d8e-4522-96e7-f9207e400edd)

## What the data actually says

| Metric (last 30 days) | Value |
|---|---|
| Sessions | 10,971 |
| Sessions with cart additions | 1,426 (13.0%) |
| Sessions that reached checkout | 853 (7.8%) |
| Sessions that completed checkout | 209 (1.9%) |
| **Checkout-start → completion rate** | **24.5%** (75.5% drop-off) |
| Mobile sessions | 8,748 (79.7%) — 2.18% conversion |
| Desktop sessions | 2,141 (19.5%) — **0.79% conversion** |
| Tablet sessions | 69 (0.6%) — 14.5% conversion (small sample) |
| Orders by referrer | Direct/unattributed 127 ($10,210) · Search 51 ($4,937) · Social 39 ($2,312) |
| Top sellers (gross sales) | Baccarat Rouge 540 Extrait ($830, 16 orders), Creed Absolu Aventus 2025 ($747), Shipping Protection add-on (176 orders — appears on most checkouts), Creed Aventus, Creed Wild Vetiver, Clive Christian Blonde Amber |
| Catalog | 1,302 products in "Shop All"; Men 1,043 · Women 751 · Niche 714 · Unisex 584 · New Arrivals 70 · **Gift Sets 69** (a second, larger gift-sets collection exists at handle `giftsets` beyond the 6-product "Gift Box Collection") |
| Collection sprawl | Dozens of per-brand collections with 1–3 products each (e.g. Andy Tauer: 1, Argos: 1, Ariana Grande: 1) |
| Lifecycle automations (Klaviyo) | Welcome, Browse Abandonment, Abandoned Cart, Abandoned Checkout, Post-Purchase, Replenishment, Winback — all **live**, email + SMS |

## Correction from the first pass

The initial version of this audit assumed the homepage lacked trust badges, a decant explainer, and gift-set merchandising, based on general fragrance-ecommerce patterns since the live storefront couldn't be fetched directly. After the user asked for changes to be made in a draft theme, the actual `templates/index.json` was inspected via the Shopify Admin GraphQL API. The real homepage already has: an authenticity hero, desktop + mobile "How It Works" size explainers (3/5/8/10ml), desktop + mobile trust-badge bars, a "What makes us unique" decant explainer, a Judge.me reviews carousel, a Men/Women/Unisex collection grid, Niche/Gift Sets product tabs, a featured-product spotlight, and a gift-sets promo banner. That part of this document is corrected here rather than left standing.

## Bugs found directly in the theme code (fixed in the draft)

1. **Duplicate chatbot script.** The homepage JSON template had the same Typebot chat-widget `custom-liquid` block twice (identical script, two section IDs). Likely two chat bubbles / double init. Removed the duplicate.
2. **Mislabeled size cards.** The "Shop by Size" cards (How It Works section) all read "3ml Spray (0.10 Oz) — From $15.99" regardless of size — the 5ml, 8ml, and 10ml cards used the wrong title and copied the same price, even though each card already had the correct product image for its size. Corrected the titles to match each card's image (3ml/5ml/8ml Travel Bottle/10ml). The price on each card was first replaced with "Priced by fragrance" (since a flat number was misleading), then, once the full catalog was scanned for real per-size minimums (see below), replaced again with the verified catalog-wide "from" price for that size.
3. **Featured-product spotlight didn't match sales data.** The homepage's single featured-product block spotlighted "Roja Parfums Espresso Aoud." Swapped it to **Maison Francis Kurkdjian Baccarat Rouge 540 Extrait** — the actual #1 seller by 30-day gross sales, and already the top product in the homepage's own reviews carousel, so the spotlight and the social proof now agree.

## Real per-size starting prices (full catalog scan)

To put accurate "from" prices on the size cards instead of guesses, every active product's variants (1,389 active products, paginated in full — confirmed reaching the end of the catalog) were scanned and grouped by size, excluding the non-fragrance "Shipping Protection" line item:

| Size | Real lowest live price | Sample size | Used on the size cards? |
|---|---|---|---|
| 3ml | **$5.99** (Geoffrey Beene Grey Flannel EDT) | 1,299 variants | Yes — "From $5.99" |
| 5ml | **$7.99** (Geoffrey Beene Grey Flannel EDT) | 1,299 variants | Yes — "From $7.99" |
| 8ml Travel Bottle | **$11.99** (Geoffrey Beene Grey Flannel EDT) | 1,274 variants | Yes — "From $11.99" |
| 10ml | **$14.99** (John Varvatos EDT) | 976 variants | Yes — "From $14.99" |
| 100ml full bottle | $79.99 (Montale Ristretto Intense Café) | 58 variants | Not used on homepage (no 100ml card) |
| 125ml full bottle | $149.00 (JPG Le Beau Le Parfum) | 6 variants | Not used on homepage (no 125ml card) |
| 50ml / 70ml / 90ml | $170 / $495 / $225 | 1 variant each | Not usable as a "from" price — single-listing, not representative |

The 3/5/8/10ml numbers are each backed by 900–1,300+ live variants, so they're solid for marketing. The homepage's "Shop by Size" cards (`how_it_works_V8cLQ7` in `templates/index.json`, draft theme) were updated to these verified figures, replacing the earlier "Priced by fragrance" placeholder and the original, wrong "$15.99 for every size" copy.

## Redesign applied to the draft theme (homepage)

The mockups were translated into the draft theme using sections the theme already ships, so no new Liquid was needed:

- **Hero copy** now leads with the decant proposition and the real entry price — "Try it before you commit to the bottle." / "…Sample sizes from $5.99 — no $300 blind buys."
- **Trust badge** "Fast & Reliable Shipping" → "Free Shipping Over $75", stating the real threshold up front instead of a vague claim (the truck icon still fits).
- **"Shop by Size" is now visible on mobile.** The `how-it-works` section had `hide_on_mobile: true`, so the four size cards — the single most important decision on this store — never rendered for the 79.7% of traffic that is mobile. The section's own CSS has explicit rules for phone widths (cards drop to 2 columns under 1023px) and the setting defaults to off, so it was authored to show there.
- **Duplicate steps disabled.** `mobile-how-it-works` repeated the same three steps that the now-visible section already shows; it is set to `disabled` rather than deleted, so it is one click to restore.
- **Section order rebuilt for mobile**: hero → trust → sizes → Best Sellers → New Arrivals → New Releases → … Previously the size cards sat seventh and Best Sellers ninth, below three filler sections.

### What still needs custom Liquid

The cart free-shipping progress bar and the per-size prices inside the product page's size selector are new components, not settings — they need section code written, which is a separate build from a homepage settings pass.

### Theme settings to flip by hand

Four switches in the draft theme's **Theme settings** would apply the rest of the cart mockup. They live in `config/settings_data.json`, which also holds every color scheme, font and app-embed block in the theme; that file could not be rewritten safely from here (a byte-exact reconstruction could not be verified against its checksum, and a near-miss would silently alter theme-wide configuration), so these are left as one-click changes in the theme editor:

| Setting | Current | Should be | Why |
|---|---|---|---|
| `cart_drawer_show_accelerated_button` | off | **on** | Shop Pay / PayPal / Apple Pay buttons in the cart drawer |
| `cart_estimate_shipping` | off | **on** | Shipping cost visible in cart instead of a surprise at checkout |
| `show_review_badge` | off | **on** | Verified ratings on product cards (Judge.me data already exists) |
| `pcard_show_lowest_prices` | off | **on** | "From $X" on product cards, matching how the catalog is priced |

## Where this was implemented

All three fixes were applied to `templates/index.json` in a new **unpublished (draft) theme — "CRO Redesign – Home"** (`gid://shopify/OnlineStoreTheme/163292446937`), duplicated from the live theme via the Shopify Admin API. Writes to the live/MAIN theme are blocked by that API, so the storefront customers see today is unchanged. To review:

- Preview (as a customer would see it): `https://musk-fragances.myshopify.com/?preview_theme_id=163292446937`
- Theme editor: `https://admin.shopify.com/store/musk-fragances/themes/163292446937/editor`
- Publish it live only from Shopify Admin → Online Store → Themes, when you're ready.

Three other unpublished drafts already existed in the account before this session ("CRO design," "Develop," "product update") — left untouched to avoid overwriting anyone else's in-progress work.

## Prioritized recommendations (structural / strategic — not yet implemented in theme code)

1. **Fix checkout friction first.** Only 24.5% of started checkouts complete. Add express-pay buttons (Shop Pay / PayPal / Apple Pay), default to guest checkout, and surface shipping cost before the final step. Largest revenue ceiling of anything on this list; Shopify's hosted checkout has limited native customization outside Shopify Plus checkout extensibility — worth a scoped follow-up to see what's editable on the current plan.
2. **Treat desktop as the underperforming surface.** 79.7% of sessions are mobile and mobile already converts better (2.18%) than desktop (0.79%) — audit the desktop template against whatever mobile is doing right.
3. **Replace brand-only navigation with scent-family browsing.** Dozens of one- and two-product brand collections are dead ends for shoppers and SEO. This needs product-tagging work (scent-family tags don't exist in the catalog yet) before a new "browse by family" section can be built — a bigger, separate project from a homepage-only theme edit.
4. **Build a discovery-set bundler.** The Gift Sets collection (69 products) is already promoted via the homepage banner and product tab — the opportunity now is a "pick 3, ship in one box" bundle price, not just a browse link.
5. **Confirm the Shipping Protection add-on is opt-in, not pre-checked.** It appears on the large majority of this month's orders — verify checkout discloses it clearly and lets customers remove it in one tap.
