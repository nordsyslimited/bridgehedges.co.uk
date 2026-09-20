# bridgehedges.co.uk

Local hedge-cutting and tree-work site for Bridge, Kent, and the Nailbourne valley villages. Part of a ~30-site Kent hedge-trimming ring.

## Stack
Plain static HTML. No framework. No build step.

- `index.html`  -  homepage
- `services/`  -  hedge cutting and hedge reduction, plus a `tree/` section (pruning, crown thinning, crown raising, crown reduction, deadwooding)
- `areas/`  -  coverage index across Bridge village and the surrounding Nailbourne-valley parishes
- `how-to/`  -  hand-authored, video-companion-led guide articles
- `jobs/`  -  JSON-driven recent-jobs feed (`jobs.json` is the source of truth; append new entries to the top)
- `assets/css/styles.css`  -  chalk-white / deep-navy / brass palette + Libre Caslon Text serif typography
- `assets/partials.html`  -  reusable HTML fragments (header / footer / topbar) for hand-copying into new pages

## Identity
- **Palette:** chalk-white (`#f6f2ea`), warmer cream (`#fbf7ef`), deep navy (`#1e2f45`), brass (`#b58a44`), sea-slate (`#3e5766`).
- **Typography:** Libre Caslon Text (headlines, brand), Source Sans 3 (body, UI).
- **Tone:** considered, working-contractor voice. Bridge's identity is Roman road (Watling Street), Nailbourne winterbourne stream, North Downs chalk, Bifrons Park Conservation Area, not a coastal town.

## Deploy
Static HTML deploy, addon domain on Krystal 3dbee cPanel, FTPS via UAPI-provisioned account. GitHub Actions (`SamKirkland/FTP-Deploy-Action@v4.3.5`) deploys on push to `main`. Contact form uses `contact-submit.php` to the Resend API.

## GA4
See any page's gtag snippet for the current measurement ID (own property, provisioned at launch, not a placeholder).

## Content sources
See `AGENTS.md` for branding/voice rules and `INGESTION.md` + `TEMPLATES/how-to.md` for the video-companion-led content pipeline. Real Bridge/Nailbourne-valley facts (Nailbourne winterbourne, Watling Street, Bifrons Park CA, Canterbury City Council, neighbouring villages) were verified via web search at build time, 20 September 2026.
