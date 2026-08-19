# SEO work log

Last updated: 2026-08-19.

## Done

### Technical
- `robots.txt` + `sitemap.xml` added and live.
- `<link rel="canonical">` on both pages.
- Open Graph + Twitter Card meta tags (`index.html`).
- `theme-color`, `apple-touch-icon`.
- `JSON-LD` `MobileApplication` on `index.html` — reflects the real pricing
  model: free app + one-time `€2.99` Premium unlock (`quiltmath_premium_lifetime`,
  see `google-play/BILLING-SETUP.md` in the app repo). Not a subscription.
- `JSON-LD` `FAQPage` on `index.html`, matching the visible "How the numbers
  work" section content (Google requires FAQ schema to match on-page text).
- Screenshots converted to WebP (~59% smaller) and served via `<picture>`
  with PNG fallback; `width`/`height` set on all images to avoid layout
  shift; below-the-fold screenshots use `loading="lazy"`.
- CSS cache-busting via `?v=` query string (see DEPLOY.md) — needed because
  nginx serves `assets/style.css` with a 7-day `cache-control`.

### Content
- Premium feature block split from one card into five individual cards
  (Borders & sashing, Flying Geese, Quarter-square triangles, Continuous
  bias binding, Saved projects & PDF export) — more keyword surface, matches
  what people actually search for.
- Added "How the numbers work" section — six real Q&A pairs sourced from
  the app's `docs/CALCULATION_ASSUMPTIONS.md` (binding seam allowance,
  HST seam allowance, Flying Geese ratio validation, backing layout logic,
  rounding rule, "measure before you cut" caveat). Doubles as long-tail SEO
  content and backs the FAQPage schema above.
- Added "What's new" section summarizing the 1.3.0–1.3.2 release notes in
  plain language — signals an actively maintained product.

### Google Search Console
- Property `https://brokenmanstudios.com/` added and verified (HTML file
  method — `google851ba87864f359aa.html`, must stay published or
  verification is revoked).
- `sitemap.xml` submitted; both URLs discovered.
- Manual indexing requested for the homepage.

## Explicitly not done — and why

- **Google Analytics / any tracking script on the site.** The site's own
  copy promises "no ads, no analytics" for the app, and the studio's whole
  pitch is privacy. Adding GA here would contradict that message. If
  traffic data is ever needed, use Search Console's own performance report
  (impressions/clicks/queries, no visitor tracking) instead.
- **Play Console → Ссылки на контент → Добавить домен** (Digital Asset
  Links / `assetlinks.json` domain verification for Android App Links).
  Investigated 2026-08-19: the app currently has **no deep-link intent
  filters** in its manifest ("Ссылки не найдены" in that Play Console
  screen), so verifying the domain now would have no visible effect until
  the app itself is changed to handle `https://brokenmanstudios.com/...`
  links. Postponed until deep links are actually implemented app-side.
  When that happens: generate the JSON in Play Console, publish it at
  `/.well-known/assetlinks.json` on this site (SSH deploy, same as any
  other file here), then click "Установить связь с сайтом".

## Open / next

- Google Ads account — set up for free (targeting/keyword research only,
  no spend, no payment method entered by the agent). Payment setup and
  actually launching a paid campaign is the owner's call and requires
  entering billing details directly in Google's UI.
- Consider adding `hreflang`/localized copy if QuiltMath ever ships a
  non-English store listing.
- Once QuiltMath leaves closed testing, simplify the "How to try QuiltMath"
  section to a single Play Store link (already flagged in DEPLOY.md).
