# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Two audiences:
- **Primary — Albuquerque-area diners** (couples, families, roommates, groups) stuck on "where should we eat," who land here from an app store listing, word of mouth, or local marketing (e.g. r/Albuquerque) to decide whether to download the Eat ABQ Android/iOS app.
- **Secondary — local restaurant owners/managers** considering paying for a Featured promotion inside the app, who need to understand that option exists and how to pursue it. Not yet built into this site; see Capabilities and Constraints.

## Product Purpose

This is the marketing/landing site for **Eat ABQ**, a mobile app (Android live, iOS in Apple review as of this writing) that helps people decide where to eat in Albuquerque, NM by spinning a 12-category food wheel and getting 3–5 real, curated local restaurant picks. The site's job is to explain the wheel mechanic, build enough trust to convert a visitor into an app download (via Play Store / App Store links), and host the required legal pages (Privacy Policy, Terms of Use). Success = app installs and the visitor understanding what the app does and that it's genuinely local before they leave the page.

## Positioning

Not a generic restaurant-finder skin reused across every city — Eat ABQ is built and maintained specifically for Albuquerque: every restaurant is a real ABQ spot, checked against Google's live data monthly (closed places drop off, new ones appear), rated 3.7★ and up. The wheel-spin mechanic turns "I don't know, what do you want to eat?" into a fast decision rather than another list to scroll. Free, no account required, works mostly offline.

## Operating Context

- Static site (plain HTML/CSS/vanilla JS, no framework, no build step — edit the HTML files directly and push to deploy) served via GitHub Pages at the custom domain `eatabq.app` (see `CNAME`).
- Sibling repo `Eat ABQ` (the Android/KMP app itself, plus `Marketing/`, `Notes/`) is the source of truth for app features, copy, and screenshots — this site should stay consistent with it rather than drift into its own claims.
- Visitors arrive from app store listings, local subreddit/word-of-mouth marketing, or a direct download link shared by an existing user.

## Capabilities and Constraints

- Must link to the real Google Play listing (`com.connorosterberg.eatabq`); the App Store listing (`com.osterbergapps.eatabq`, App Store ID `6792827854`) is submitted and awaiting Apple's review decision as of this writing (confirmed `apps.apple.com/app/id6792827854` still 404s as of 2026-08-08) — **not yet public**. The hero has a real "Coming Soon on the App Store" badge (`.btn-soon`, non-clickable, next to the Google Play button) ready for the moment Apple approves — an HTML comment right above `.store-badges` in `index.html` has the exact swap (turn the `<span>` into an `<a href="https://apps.apple.com/app/id6792827854">`). Don't flip it live before confirming the listing actually resolves.
- Footer must keep working links to `/privacy` and `/terms` (both already exist as separate static pages on this site and must stay untouched by unrelated redesign work) and a contact path (`mailto:feedback@eatabq.app`).
- No backend, no forms, no account system on this site — anything needing a server (e.g. a real promotion-inquiry form for restaurant owners) is out of scope until explicitly requested; for now, a business audience can only be pointed at a contact email/method, not a submission flow.
- Real app screenshots and the user's own Zia sun-symbol artwork live in `assets/` and should be reused rather than re-generated or replaced with generic icon art. `assets/screenshot-featured.png` (the Featured tab, added this session) came from the sibling repo's `Play Store Screenshots/2_featured.png`.
- The nav bar lists "How it works," "Categories," "Featured," and "FAQ" as anchors (Featured added this session, pointing at the new `#featured` section). The old FAQ item claiming iOS was "still in the works" was removed once a real App Store badge existed on the page — don't re-add "in the works" language.
- The hero background (inline SVG) animates a full day-to-sunset color cycle via SMIL `<animate>` on the sky gradient stops and the two colored mountain path fills, synced to the existing 42s sun-arc animation (`.sun{animation:sunArcH 42s linear infinite}`): sunset tones (warm orange/red sky, maroon/red-orange mountains) at the arc's low points (0%/100%, sunrise/sunset), blue sky + brown/green mountains at the arc's peak (50%, midday). The old filter-based `dayNightShift` hue-rotate hack was removed in favor of this literal color animation.

## Brand Commitments

- Product name: **Eat ABQ**. Legal entity: Osterberg Apps LLC (Albuquerque, NM). Tagline: "505 forever."
- Brand mark: a New Mexico Zia sun symbol — the user's own artwork (not a generic icon or AI-generated substitute), used as flanking art beside the spin wheel. The nav badge (top-left corner) uses the real app icon (`apple-touch-icon.png`, crossed red/green chiles) instead, per the user's explicit request — this deliberately supersedes the earlier "nav badge = Zia" convention; don't revert without asking.
- Contact: `feedback@eatabq.app` (site footer). App-store/marketing docs elsewhere in the sibling `Eat ABQ` repo also reference `osterberg@eatabq.app` as a support address — this site currently only uses `feedback@eatabq.app`; don't silently swap it without asking.

## Evidence on Hand

- Real Android app screenshots (Wheel/Browse/Favorites) copied into `assets/` from `Eat ABQ/Play Store Screenshots/`.
- Approved Play Store listing copy at `Eat ABQ/Marketing/store_listing.md` — the source used for this site's FAQ content (how the wheel works, the 12 categories, favorites/notes/filters, Featured promotions, offline behavior, privacy stance, monthly data refresh). Treat that file as the canonical wording to draw from for future copy, not this site's own paraphrase of it.
- `Eat ABQ/CLAUDE.md` (sibling repo) has the full technical/product history if deeper detail is ever needed (e.g. exact category list/colors, monetization mechanics) — durable facts worth promoting into this file should be pulled in deliberately, not assumed current without a quick check (it's a fast-moving, session-log-style doc).
- No testimonials, press mentions, or usage-count claims exist yet — don't fabricate any.

## Product Principles

1. Hyper-local, not generic — every claim on the page should read as genuinely Albuquerque-specific, not a template reused for any city.
2. Free and low-friction for the diner — no account, no login, minimal data collection; the copy should keep saying so plainly, not just imply it.
3. The wheel is the product's identity — visual and copy work should keep it as the centerpiece, not bury it under generic app-marketing patterns.
4. Say only what's true and live right now (e.g. don't claim iOS availability before Apple approves it) — prefer copy that ages gracefully over copy that needs to be walked back.
5. This site is consumer-first; a restaurant/advertiser audience is a real secondary consideration for future work, but nothing here should crowd out or complicate the primary "decide where to eat, download the app" path.
