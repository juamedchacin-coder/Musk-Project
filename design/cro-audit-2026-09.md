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
2. **Mislabeled size cards.** The "Shop by Size" cards (How It Works section) all read "3ml Spray (0.10 Oz) — From $15.99" regardless of size — the 5ml, 8ml, and 10ml cards used the wrong title and copied the same price, even though each card already had the correct product image for its size. Corrected the titles to match each card's image (3ml/5ml/8ml Travel Bottle/10ml) and removed the copy-pasted price from the 5/8/10ml cards (replaced with "Priced by fragrance," since price varies by product and a flat number was misleading — showing one wrong price next to every size is the kind of shipping-cost surprise that already drags down checkout completion).
3. **Featured-product spotlight didn't match sales data.** The homepage's single featured-product block spotlighted "Roja Parfums Espresso Aoud." Swapped it to **Maison Francis Kurkdjian Baccarat Rouge 540 Extrait** — the actual #1 seller by 30-day gross sales, and already the top product in the homepage's own reviews carousel, so the spotlight and the social proof now agree.

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
