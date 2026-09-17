# Eat ABQ Website — CLAUDE.md

## Project overview
Marketing/landing site for **Eat ABQ**, a mobile app that helps people decide where to eat in
Albuquerque, NM by spinning a food-category wheel. Static site, no build step, no backend —
deployed via GitHub Pages at `eatabq.app` (repo `github.com/connor5165/eatabq-website`). See
`PRODUCT.md` for the full product brief (audience, positioning, brand rules, constraints) — read
that first for any design/copy work, it's kept up to date separately from this file.

Sibling repo `C:\Users\oster\Projects\Eat ABQ` (the Android/KMP app itself) is the source of
truth for app features/categories/copy — this site should stay consistent with it, not drift
into its own claims. That repo's `CLAUDE.md` has the full technical history if deeper detail is
ever needed.

## Tech stack
Plain HTML/CSS/vanilla JS — everything lives in `index.html` (inline `<style>`/`<script>`, no
build tooling, no framework, no dependencies to install). `privacy.html`/`terms.html` are
separate static pages. `assets/` holds screenshots, the Zia artwork, and the store badges.

## Current status (as of 2026-09-16)
- Hero, wheel, category chips, "How it works", Featured tab preview, screenshots, FAQ, and
  footer are all in place and current.
- The site's **13-category wheel/chip list is generated from one JS array (`CATEGORIES`)**
  defined near the bottom of `index.html`, kept in exact sync with the app's real
  `shared/commonMain/.../model/FoodCategory.kt` enum (dbKey, label, emoji, wheel color). Do
  **not** hand-edit wheel `<span>`s or category `<span class="chip">`s directly in the HTML —
  edit the `CATEGORIES` array instead, everything else (wheel conic-gradient, wheel labels, chip
  list, the "N Food Categories" heading count) derives from it. This was a deliberate fix after
  a real drift bug was found (site still showed a standalone "BBQ" chip and was missing
  "Dessert" months after the app merged/added those categories).
- The hero's mountain SVG has real snow-cap triangles that must sit exactly on the taller back
  ridge's peak vertices (`path` with fill `#8C2A45`) — if the mountains are ever redrawn, check
  that a snow-cap's apex isn't sitting behind/below where the OTHER mountain layer's silhouette
  pokes up higher at that x-coordinate (this exact bug existed before 2026-09-16 and made snow
  look "cut into" the ridge instead of tipping it).
- The hero stats strip's restaurant count (`#statRestaurants`) is fetched **live** at page load
  from the app's own public Firestore doc:
  `https://firestore.googleapis.com/v1/projects/eat-abq/databases/(default)/documents/meta/restaurants_version`
  (no API key needed — Firestore's REST API respects security rules, and `meta` is publicly
  readable per the app repo's `firestore.rules`). This doc's `count` field is already updated by
  the app's existing monthly `sync_firestore.py` refresh, so the website needs **zero pipeline
  changes** to stay current — it just reads what's already there. Falls back to a static "800+"
  if the fetch fails (offline dev, CORS issue, etc.).
- The "Spin to Decide" wheel is a real interactive demo, not just decorative: clicking it spins,
  computes which category actually landed under the pointer, and reveals a small result card
  with 3 real sample restaurants for that category (from the `SAMPLES` object, right below
  `CATEGORIES` in the script). **This data is a hand-picked static snapshot of real, current,
  well-rated restaurants per category** (pulled from `restaurants.txt` on 2026-09-16) — it will
  drift stale over time as the real catalog changes monthly. Worth refreshing every few months by
  re-running the same top-rated-per-category pull against the app repo's current
  `restaurants.txt`/`master_restaurants.json` (see that repo's `DataRefresh/` folder) — not
  automated, a manual touch-up.
- Scroll-triggered reveal animations (`.reveal` class + `IntersectionObserver`) are wired on every
  major section (stats strip, wheel, categories, how-it-works, featured, screenshots, FAQ, duke
  banner). Category chips fade in individually once their parent section reveals. Respects
  `prefers-reduced-motion`.
- "How it works" is 6 steps (Spin, Discover, Pick One, Save, Customize, Offline) — Customize
  includes a 3-dot Red/Green/Christmas theme swatch, matching the app's real theme picker.

## Key files
- `index.html` — the entire site. Structure top to bottom: nav, hero (SVG mountain scene), stats
  strip, wheel section (+ spin demo), categories, how-it-works, featured preview, screenshots,
  FAQ, duke banner, footer. All JS is one `<script>` block at the end of `<body>`.
- `privacy.html` / `terms.html` — legal pages, footer-linked, don't touch casually.
- `PRODUCT.md` — durable product context (audience, positioning, brand rules, what's true/live
  right now). Read before any content change; update it if a brand/positioning fact changes.
- `assets/` — screenshots (`screenshot-*.png`), Zia artwork (`zia-symbol.png`), official store
  badge art (`google-play-badge.png`, `app-store-badge.svg` — see PRODUCT.md for the padding/
  sizing gotchas if these are ever resized).

## Last session summary (2026-09-16)
Connor asked for the website to catch up with recent app changes and get more visual polish.
Fixed a real bug first (mountain snow-caps sitting below the true peak tips due to two
overlapping mountain silhouettes), then:
1. **13-category sync, site-wide**: added the real `NEW_MEXICAN` category (turquoise, 🌶️,
   split off from Mexican in the app back in September) and fixed the category chip list, which
   had drifted (still showed standalone BBQ, was missing Dessert). Rebuilt the whole
   wheel/chip-generation as one data-driven `CATEGORIES` array to prevent this class of drift
   recurring.
2. **Content parity additions**: added a live theme-picker callout (Red/Green Chile/Christmas
   dots), a "Pick One" step describing the app's pick-history feature, and a "Customize" step
   describing the wheel's filters (price/distance/open now).
3. **Pizzazz**: made the wheel demo interactive (real spin → real landing category → 3 real
   sample restaurants, mirroring the app's own "You landed on..." moment), added scroll-in
   reveal animations across the page, and added a live-fetched restaurant-count stat plus a
   static category-count stat near the hero.
Verified everything live in a real browser (desktop + mobile width) before finishing — confirmed
the wheel colors/labels, the chip list contents, the spin-to-result flow across multiple
categories, the live count (pulled live: 844 restaurants), and the scroll reveals all work.

## Next steps
- Nothing blocking — this was a self-contained polish/content pass. Not committed to git yet
  (Connor hasn't asked); the working tree has `index.html` changes only.
- Worth periodically refreshing `SAMPLES` (the spin-demo restaurant data) against the app's
  current `restaurants.txt` every few months so it doesn't feel stale.
- If Connor wants "New Mexican" to also get its own dedicated marketing callout beyond the
  wheel/chips (it's arguably the single most on-brand category this app has), that's a real,
  bigger idea floated but not built this session — see the conversation this session came from.
