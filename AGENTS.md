# Agent / Contributor Notes

## Analytics, fonts and cookies (do not undo)

- This site has NO Google Analytics, NO gtag or dataLayer snippet, NO Google Fonts links and NO cookie banner or cookie-consent script. They were removed deliberately on 8 October 2026. Do not add any of them back, on any page, template, generator or sample snippet, and do not add any other third-party script, font, image host or tracker.
- Visit counting is NordAnalytics (first-party, cookieless). Every page loads `<script src="/assets/js/analytics.js?v=20261008" defer></script>` as the last element before `</body>`. That one file adds the footer links ("How we count visits", "Don't count my visits") and loads the counter. Copy the head, the footer and that script line from an existing page unchanged. Every new page must include it (redirect stubs with a meta refresh excepted).
- Fonts are self-hosted: pages link `<link rel="stylesheet" href="/assets/fonts.css">` where a Google Fonts link used to be. Copy it from an existing page.
- Audits: do NOT flag the absence of Google Analytics, a GA4 ID, gtag, consent mode or a cookie banner, and do NOT restore any of them. Older reports and notes that mention GA4 are out of date. DO flag and remove any page containing googletagmanager.com, gtag(, dataLayer, fonts.googleapis.com, fonts.gstatic.com or cookie-consent, and flag any non-redirect page missing the analytics.js script line.
- `/privacy.html` carries the approved "Website statistics" section (id `website-statistics`). Keep its wording unchanged.

Ground rules for any future AI assistant or human contributor working on this site.

## Stack

- Plain static HTML, one shared `assets/css/styles.css`. No build step, no shared JS file beyond the small inline scripts already in each page (year stamp, jobs-preview fetch, form status message).
- Deployed via GitHub Actions FTPS to Krystal shared hosting, `.github/workflows/deploy.yml` fires on push to `main`, uses `SamKirkland/FTP-Deploy-Action@v4.3.5` against `p125.lon.krystal.io`, repo secrets `FTP_USER` / `FTP_PASS`.
- Manual FTPS fallback creds (if the GH Actions path is ever unavailable): `~/.claude/secrets/bridgehedges-ftps.env` on Jet/Chef's chassis, holds `FTPS_HOST`, `FTPS_PORT`, `FTPS_USER`, `FTPS_PASSWORD`, `FTPS_TARGET_DIR`.
- Contact form (`contact-submit.php`) uses Resend via `config/secrets.php` (git-ignored, generated from `config/secrets.example.php`, real key present in the deployed working copy, do not commit it). `RESEND_FROM` uses the fleet's one verified Resend sending domain (`noreply@sandwichlawnmowing.co.uk`, display name set to this site's brand), not `onboarding@resend.dev`, so mail actually delivers to a non-account-owner recipient. `RESEND_TO` is `hello@bridgehedges.co.uk`, which forwards to `nordsyslimited@gmail.com` via a cPanel email forwarder.
- The bridgehedges.co.uk cPanel account is an addon domain under the 3dbee.co.uk hosting account (`akhiuynskl` on `p125.lon.krystal.io`), same account as the rest of the fleet. Self-serve UAPI access documented fleet-wide, see Jet/Chef memory `reference_3dbee_cpanel_uapi.md`.

## Repo family

This is one of roughly 30 sister sites in a Kent hedge-trimming ring (broadstairs / canterbury / chatham / deal / dover / margate / ramsgate / thanet / wingham / sandwich / littlebourne and many others), same business model and layout conventions, different towns. Each ring site has its own distinct macro-structure for its how-to content so the ring doesn't read as templated duplicate content to Google. **This site's assigned angle is video-companion-led**, see `TEMPLATES/how-to.md`. Do not copy another ring site's content pattern verbatim onto this one (checklist-first, procedural-first, comparison-table, FAQ-led, story-led, data/stats-led, myth-busting, hyper-local, timeline/seasonal-calendar-led, and others are all other sites' assigned angles, not this one's), even though the underlying HTML/CSS conventions (fonts, nav, footer) are legitimately shared/similar within the ring.

## Branding

- Palette: navy `#1e2f45` (headers/hero/footer), navy-deep tones `#3e5766` / `#2b3f4d`, gold/brass accent `#b58a44`, cream/sand `#f6f2ea` / `#ecdfc7`. Shared with several sibling sites in the ring, this is the "Kent chalk downland" palette variant, not a coastal one.
- Fonts: Libre Caslon Text (headings/display) + Source Sans 3 (body/UI), self-hosted fonts (/assets/fonts.css).
- Logo: inline SVG favicon data URI (navy square, gold hedge-arc mark), reuse the existing one, do not invent a new one.
- Analytics: none from Google. NordAnalytics (first-party, cookieless) is loaded by `/assets/js/analytics.js`, included at the end of every page (see the rule at the top of this file).
- Phone/WhatsApp: `07763 100 477` (shared NordSys contact number). Email: `hello@bridgehedges.co.uk`. Area: Bridge CT4, covering the Nailbourne valley villages (Patrixbourne, Bekesbourne, Bishopsbourne, Lower Hardres, Barham, Kingston).
- Topbar tagline: "Considered hedge work on the old Roman road." (Bridge sits on the old Watling Street, the London-Dover Roman road; this is this site's own line, do not reuse a sibling site's tagline).

## Writing style

See `TEMPLATES/how-to.md` "Voice and language" for the full contract. In short: British English, working-contractor first-person voice, no em-dashes, no corporate filler, real cited figures over vague or invented claims. Real Bridge/Nailbourne-valley geography only, no fabricated street names, no invented specific statistics (council fees, exact CA boundaries) where the real figure isn't confirmed, real figures where they are (national legal thresholds, dates).

## SEO and AI search baked in

Every page must carry:

- Unique `<title>`, `<meta name="description">`, `<link rel="canonical">`.
- `og:type`, `og:image` (`https://bridgehedges.co.uk/assets/img/og-default.png`, 1200x630), `og:title`, `og:description`, `og:url`.
- `<html lang="en-GB">`, `<meta name="robots" content="index,follow,max-image-preview:large">` on content pages.
- JSON-LD:
  - Individual how-to articles: flat `Article` type (no `@graph`, no `BreadcrumbList`, no `FAQPage`).
  - `how-to/index.html` (the hub): `@graph` with `CollectionPage` + `BreadcrumbList`, this is the one page on the site using `@graph` besides the home page.
  - Home page: `@graph` with `LocalBusiness`/`HomeAndConstructionBusiness`, `FAQPage`, `WebSite`.
- `robots.txt` is a minimal `Allow: /` (no explicit AI-crawler allow/deny list).
- `sitemap.xml` updated whenever a page is added or removed, this repo has no build step, nothing does this automatically.

## Images

- `assets/img/og-default.png` (and the hero photo wired into `assets/css/styles.css`'s `.hero` background) is a real generated photo matching Bridge's Nailbourne-valley/Roman-road setting, produced via the gpt-image-bridge skill. `assets/img/og-default.svg` is a text-based fallback source, kept in sync in spirit but not pixel-identical.
- Contact page WhatsApp QR (`assets/img/wa-qr.png`) is generated locally via the Python `qrcode` library, pointed at the site's `wa.me` link with prefilled text naming this site. Regenerate it any time the WhatsApp pre-fill text changes.

## How-to pattern

See `TEMPLATES/how-to.md` for the full contract. This site's assigned structural angle is **video-companion-led**, every how-to article is built around one real, verified UK hedge-care YouTube video, credited and embedded, then extended with Bridge/Nailbourne-valley specificity the video itself doesn't cover. This is a firm house style, not a rotation.

## Contact form rules

- Posts to `contact-submit.php`, which sends via Resend (`config/secrets.php`, git-ignored). Redirects to `thanks.html` on success, `contact.html?status=invalid` or `?status=error` otherwise.
- Do not switch this to FormSubmit.co or any other provider as part of unrelated content work.

## What not to do

- No frameworks (React, Vue, Tailwind, Next, etc.).
- No build step. No npm dependencies.
- No tracking scripts and no third-party fonts or images. Visit counting is the NordAnalytics script already on every page; do not add anything else.
- No third-party chat widgets.
- Do not clone another ring site's how-to structure onto this one, see "Repo family" above.
- Do not add per-article named-author variation beyond the established site-name Organization JSON-LD author pattern, no fabricated real-person schema beyond what's already in use.
- Do not invent specific statistics (council fees, exact conservation-area boundaries) that haven't been verified against a real source. Reference the Act/guidance by name instead, or say "check the council's current figure."
- Do not revive the Broadstairs-derived coastal content this site was scaffolded from (salt-wind species, herring gulls, Thanet Coast SSSI, Viking Bay etc.), Bridge is an inland Nailbourne-valley/North Downs village, not a coastal town.

## Related files

- Site repo: `E:/Ai/Codex/bridgehedges.co.uk/`
- Template contract: `TEMPLATES/how-to.md`
- Config: `config/secrets.php` (git-ignored, real key present in the deployed working copy), `config/secrets.example.php` (committed template)
- Sister sites (reference pattern, not to be copied verbatim): `E:/Ai/Codex/chathamhedges.co.uk/AGENTS.md`, `E:/Ai/Codex/littlebournehedges.co.uk/AGENTS.md` (Littlebourne is the geographically nearest sister site to Bridge)

## Published-copy rule (Richard 2026-10-05)

Never write the parent company name, owner/founder/operator/"run by"/"owned by" statements, company or registration lines, or any reference to a network, ring, sister sites or common ownership into any public page (footer, about, areas, how-to, llms.txt, JSON-LD). Footer = site name, copyright year, privacy and sitemap links, contact. JSON-LD author = the site Organization, never a named person. Editorial links to a neighbouring town's site are fine if they do not describe a network.

## Source lines: no process wording (do not undo)

Published source credits must read like an honest reference list, never like a description of how the page was produced. Never write any of these on a public page: "egress", "proxy", "blocked", "generation environment", "INGESTION.md", "AGENTS.md", "web search", "cross-referenced via", "confirmed to exist via". If a source could not be opened, simply leave it out of the list or cite a source you did read. The only acceptable wording is plain, e.g. "checked against current sources". Audits must flag and rewrite any page that contains these phrases.
