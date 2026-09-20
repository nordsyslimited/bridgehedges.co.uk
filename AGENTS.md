# Agent / Contributor Notes

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
- Fonts: Libre Caslon Text (headings/display) + Source Sans 3 (body/UI), Google Fonts.
- Logo: inline SVG favicon data URI (navy square, gold hedge-arc mark), reuse the existing one, do not invent a new one.
- GA4: measurement ID set in every page's gtag snippet, see `index.html` for the current value.
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
- No tracking scripts beyond the existing GA4 gtag snippet without asking first.
- No third-party chat widgets.
- Do not clone another ring site's how-to structure onto this one, see "Repo family" above.
- Do not add per-article named-author variation beyond the established `Richard Lim` / `Bridge Hedges & Tree Services` JSON-LD pattern, no fabricated real-person schema beyond what's already in use.
- Do not invent specific statistics (council fees, exact conservation-area boundaries) that haven't been verified against a real source. Reference the Act/guidance by name instead, or say "check the council's current figure."
- Do not revive the Broadstairs-derived coastal content this site was scaffolded from (salt-wind species, herring gulls, Thanet Coast SSSI, Viking Bay etc.), Bridge is an inland Nailbourne-valley/North Downs village, not a coastal town.

## Related files

- Site repo: `E:/Ai/Codex/bridgehedges.co.uk/`
- Template contract: `TEMPLATES/how-to.md`
- Config: `config/secrets.php` (git-ignored, real key present in the deployed working copy), `config/secrets.example.php` (committed template)
- Sister sites (reference pattern, not to be copied verbatim): `E:/Ai/Codex/chathamhedges.co.uk/AGENTS.md`, `E:/Ai/Codex/littlebournehedges.co.uk/AGENTS.md` (Littlebourne is the geographically nearest sister site to Bridge)
