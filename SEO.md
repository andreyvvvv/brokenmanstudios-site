# SEO work log

Last updated: 2026-08-20.

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

## Verification pass (2026-08-20)

- **PageSpeed Insights (mobile), brokenmanstudios.com:** Performance 100,
  Accessibility 97 → 100 after the fix below, Best Practices 100, SEO 100.
- Only accessibility finding: "Document doesn't have a `main` landmark" on
  both pages. Fixed by wrapping page content in `<main>` — deployed.
- **Rich Results Test:** `MobileApplication` structured data detected,
  0 errors. One optional-field notice: missing `aggregateRating`. Do not
  add a fabricated rating — QuiltMath is still in closed testing with no
  public Play Store reviews yet. Revisit once real ratings exist.
- `FAQPage` JSON-LD did not show up as a separate eligible type in the
  Rich Results Test. Google restricted FAQ rich results to
  government/health sites in an August 2023 policy change, so a visible
  FAQ snippet in search results is unlikely regardless of markup
  correctness. The schema is still valid and harmless to keep (helps
  Google's semantic understanding of the page even without a rich result).

## In progress — Google Ads (paused 2026-08-20, resume here)

Goal: set up a Google Ads account/campaign structure for QuiltMath at
**zero cost** — targeting, keyword research, ad copy drafts only. No
payment method is to be entered by the agent; actually activating spend
is the owner's own action in Google's billing UI.

Status when paused:
- Signed into Google Ads (ads.google.com) with the account owner's Google
  login. There is already an **existing Ads account, ID 390-623-4129**,
  but it is **deactivated/blocked** ("Ваш аккаунт неактивен... аккаунт
  был заблокирован") with zero campaigns in it — reason for the block
  was not investigated.
- Owner decided (2026-08-20): do **not** try to reactivate 390-623-4129
  (unknown block history, could be an old unrelated account). Instead,
  **create a brand-new Ads account dedicated to QuiltMath / Broken Man
  Studios.**
- Was in the middle of locating the "create new account" flow in the
  Ads UI when the session ended (account-switcher isn't the "Все
  кампании" campaign filter dropdown at top-left — that's a decoy;
  `.../aw/accountchooser` is a dead URL, 404). Next attempt: use the
  small account/profile control near the top-right avatar, or
  Google's guided "Jetzt loslegen" / "Fortfahren" flow from
  ads.google.com's landing page again and choose "create a new account"
  explicitly rather than continuing into the existing blocked one.
- Nothing financial was touched: no payment method entered, no card
  details viewed or typed, no campaign launched or budget set.

Plan once the new account exists (still free, still paused):
1. Keyword research via Keyword Planner for terms like "quilt binding
   calculator", "how much fabric for quilt backing", "half square
   triangle calculator", "flying geese calculator quilt".
2. Draft a Search campaign structure (ad groups per calculator: Binding,
   Backing, HST, Flying Geese/Premium) with headlines/descriptions
   pulled from the site copy already written in `index.html`.
3. Set geographic/language targeting per owner's target market.
4. Leave the campaign in **paused / draft** status. Do not add billing
   or click anything that activates spend — that step needs the owner
   present to enter their own payment details.
5. Log the finished draft here with account ID and campaign name so the
   owner can review and decide whether/when to fund and launch it.

## Open / next

- Finish Google Ads setup per the plan above.
- Consider adding `hreflang`/localized copy if QuiltMath ever ships a
  non-English store listing.
- Once QuiltMath leaves closed testing, simplify the "How to try QuiltMath"
  section to a single Play Store link (already flagged in DEPLOY.md).
