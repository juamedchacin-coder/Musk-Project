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
| Catalog | 1,302 products in "Shop All"; Men 1,043 · Women 751 · Niche 714 · Unisex 584 · New Arrivals 70 · Gift Box **6** |
| Collection sprawl | Dozens of per-brand collections with 1–3 products each (e.g. Andy Tauer: 1, Argos: 1, Ariana Grande: 1) |
| Lifecycle automations (Klaviyo) | Welcome, Browse Abandonment, Abandoned Cart, Abandoned Checkout, Post-Purchase, Replenishment, Winback — all **live**, email + SMS |

## Prioritized recommendations

1. **Fix checkout, not traffic.** Only 24.5% of started checkouts finish — worse than the cart→checkout drop-off. Add Shop Pay / PayPal / Apple Pay express buttons above the manual form, default to guest checkout, and show shipping cost (and the free-shipping threshold) in the cart before checkout starts, not on the last step. This is the single highest-leverage fix available — it doesn't require more visitors, it stops losing the ones already there.
2. **Treat desktop as the underperforming surface.** 79.7% of sessions are mobile and mobile already converts better (2.18%) than desktop (0.79%). That's the reverse of most stores. Audit the desktop template against whatever mobile is doing right, instead of assuming mobile needs the most work.
3. **Replace brand-only navigation with scent-family browsing.** A shopper who doesn't already know "Areej Le Doré" or "Boadicea The Victorious" has no way into a 1,300-product catalog today. Keep the ~90 per-brand collections as filters, not top-level nav; lead with scent family (amber/oud, fresh/citrus, woody/musk, sweet/gourmand) and occasion instead.
4. **Put a real, live bestsellers rail on the homepage**, pulled from `FROM sales SHOW gross_sales GROUP BY product_title` rather than a manually curated "featured" block. Baccarat Rouge 540 Extrait, Santal 33, Creed Royal Oud, Parfums de Marly Layton are already proven sellers — homepage social proof should say so.
5. **Build a discovery-set bundler.** The Gift Box collection has 6 products against a catalog of 1,300+. A "pick 3 decants, ship in one box" bundle turns catalog depth — the store's real asset — into a merchandising offer and lifts AOV. The Shipping Protection add-on already proves customers accept a well-placed add-on (it rides on most checkouts this month); a curated bundle should convert at a much higher ticket.
6. **Confirm the Shipping Protection add-on is opt-in, not pre-checked.** It appears on the large majority of this month's orders. That's either a strong upsell win or a dark-pattern risk — verify checkout discloses it clearly and lets customers remove it in one tap. This protects the trust the rest of the funnel depends on.
7. **Lifecycle email/SMS is already solid** — Welcome, Browse/Cart/Checkout abandonment, Post-Purchase, Replenishment and Winback are all live in Klaviyo for both email and SMS. This is not a gap; don't rebuild it. Direct/unattributed traffic is the largest order source (127 of 217 orders), which is consistent with a healthy repeat/lifecycle program — the leak is on-site conversion, not retention marketing.

## What this audit could not check directly

Outbound fetches to `muskfragrances.com` and `cdn.shopify.com` were blocked by this session's network policy, so the actual rendered homepage, images, and checkout screens were not inspected visually. The visual redesign concept therefore illustrates the *structure and copy* changes above using the store's real product names, prices, and sales figures — it is a concept to build from, not a pixel-accurate mockup of the current site. Recommend a follow-up pass with real screenshots (e.g. via the Shopify theme editor or a session with storefront access) to validate hero imagery, current checkout steps, and mobile layout specifics before implementation.
