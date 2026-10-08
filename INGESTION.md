# Bridge Hedges nightly ingestion  -  architecture and operational reference

## Analytics, fonts and cookies (do not undo)

- This site has NO Google Analytics, NO gtag or dataLayer snippet, NO Google Fonts links and NO cookie banner or cookie-consent script. They were removed deliberately on 8 October 2026. Do not add any of them back, on any page, template, generator or sample snippet, and do not add any other third-party script, font, image host or tracker.
- Visit counting is NordAnalytics (first-party, cookieless). Every page loads `<script src="/assets/js/analytics.js?v=20261008" defer></script>` as the last element before `</body>`. That one file adds the footer links ("How we count visits", "Don't count my visits") and loads the counter. Copy the head, the footer and that script line from an existing page unchanged. Every new page must include it (redirect stubs with a meta refresh excepted).
- Fonts are self-hosted: pages link `<link rel="stylesheet" href="/assets/fonts.css">` where a Google Fonts link used to be. Copy it from an existing page.
- Audits: do NOT flag the absence of Google Analytics, a GA4 ID, gtag, consent mode or a cookie banner, and do NOT restore any of them. Older reports and notes that mention GA4 are out of date. DO flag and remove any page containing googletagmanager.com, gtag(, dataLayer, fonts.googleapis.com, fonts.gstatic.com or cookie-consent, and flag any non-redirect page missing the analytics.js script line.
- `/privacy.html` carries the approved "Website statistics" section (id `website-statistics`). Keep its wording unchanged.

**Site:** bridgehedges.co.uk
**Pipeline shape:** SE-shape  -  Anthropic cloud routine (claude.ai/code scheduled agent), same shape as the rest of the Kent hedge-trimming ring.
**Owner:** Richard (claude.ai/code account holder)
**Wired:** 2026-09-20 by Jet, at site launch.

## What this pipeline is

An Anthropic-hosted scheduled Claude Code routine that runs nightly on
Anthropic's cloud infrastructure. Each run: discovers an uncovered
Bridge/Nailbourne-valley hedge-care topic + a real, verifiable UK YouTube
video (or other web source), generates a full **video-companion-led**
how-to article matching `TEMPLATES/how-to.md`, commits it to this repo, and
pushes to `main`. The repo's own `.github/workflows/deploy.yml` GH Action
then FTPS-deploys to Krystal, no deploy-pipeline work needed alongside this
content scaffold.

## How it works  -  end-to-end flow (per the SE-shape pattern)

### Stage 1  -  trigger
- Scheduled cron on Richard's claude.ai/code account fires the routine.
- The routine spins up a fresh Claude Code session on Anthropic's infra.

### Stage 2  -  topic discovery (in-session)
- Session reads the existing `how-to/` folder to identify covered vs
  uncovered topics against the category list in `config/ingestion.json`.
- Uses web search / WebFetch to find a real, existing UK hedge-care video
  (or written source) on a plausible uncovered topic. The video is the spine
  of the article on this site, so it must genuinely exist and resolve, no
  fabricated video IDs or titles.
- Constraints: UK-based creator/source where possible, region UK, qualifying
  content type (not shorts, not compilations). Channel-authority used as the
  heuristic since the YouTube Data API isn't used (egress to youtube.com is
  often blocked from Anthropic infra, falls back to search-result
  cross-referencing).

### Stage 3  -  verification
- Session confirms the video exists (resolves to a real watch page) and the
  creator/channel is a plausible, genuine source via web search
  cross-references.
- Records a note about verifiability in the run report.

### Stage 4  -  article generation
- Session writes a full HTML article per `TEMPLATES/how-to.md`, the
  authoritative contract for this repo. Key shape:
  1. Hero + lede introducing the video and why it's relevant.
  2. `.callout`-styled video embed block (single `<iframe>`,
     `youtube-nocookie.com/embed/<id>`, no autoplay) with a direct link to
     the original video for attribution.
  3. "What the video shows" section, real technique/steps.
  4. "What it doesn't cover: Bridge and the Nailbourne valley" section,
     the local payoff: North Downs chalk vs valley-floor frost pocket,
     Bifrons Park Conservation Area, nesting law.
  5. CTA callout back to `contact.html` / phone / email.
  6. Sources line naming the video (title, creator, URL) and any Acts
     referenced.
  7. JSON-LD matching this site's existing per-article pattern exactly
     (flat `Article`).

### Stage 5  -  site updates
- Session updates `how-to/index.html`, adds a new card/entry matching the
  existing grid layout.
- Adds 1-3 contextual in-prose links on genuinely relevant existing
  articles or service pages back to the new one.
- Adds the new URL to `sitemap.xml`.
- Footer's static local-guides list is site-wide and not rotated
  automatically, leave it alone.

### Stage 6  -  self-audit report
- Session writes `reports/YYYY-MM-DD-content.md` documenting: URL shipped,
  video source used, creator/authority verification, topic-gap check
  evidence, Bridge/Nailbourne-valley-specificity notes, confirmation the
  video-companion structure was followed, files modified.

### Stage 7  -  commit + push
- Session commits with a `Nightly: add N how-to articles (D Month YYYY)`
  message body listing the shipped articles.
- Includes a `Claude-Session:` trailer for provenance.
- Pushes to `main`.

### Stage 8  -  deploy
- Repo's `.github/workflows/deploy.yml` fires on push, FTPS-uploads
  changed files to Krystal via `SamKirkland/FTP-Deploy-Action@v4.3.5`. No
  changes needed to this workflow as part of content scaffolding.

## Morning audit (second scheduled run)

A second nightly/morning routine audits the full site: broken links, dead
video embeds (a video going private/deleted after publication is the
biggest single risk on this site's structural angle, since the video is the
spine of every article), SEO completeness (title/description/canonical/OG/
JSON-LD present on every page), video-companion structural compliance on
how-to articles, and general relevance. Writes `reports/YYYY-MM-DD-audit.md`.

## Configuration surface

| What | Where |
|---|---|
| Discovery + verification prompt | Routine prompt on claude.ai/code (opaque from repo) |
| Article template rules (incl. video-companion-led angle) | `TEMPLATES/how-to.md`, routine should read at run start |
| Category list, cadence, schedule | `config/ingestion.json` |
| Site palette + brand voice | `AGENTS.md` + rendered examples in `how-to/` |
| Deploy target | `.github/workflows/deploy.yml` (already working) + FTPS repo secrets |
| Cron schedule | claude.ai/code routine schedule |

## Cost model

- **Claude usage:** counts against Richard's claude.ai/code allowance.
- **Web search / WebFetch:** included in claude.ai/code.
- **YouTube:** used only via WebFetch (no Data API key).
- **FTPS / hosting:** covered by Krystal LiteSpeed hosting plan.

## Failure modes and resilience

| Symptom | Root cause | Resilience |
|---|---|---|
| YouTube egress blocked (HTTP 403) | Anthropic infra egress policy on youtube.com | Routine falls back to search-result cross-referencing, documented in every run report |
| No verifiable video this run | Candidate not verifiable within session | Routine ships nothing rather than an unverified claim, reports the shortfall |
| A previously-embedded video goes private/deleted | Creator removes or privates the source video | Morning audit flags the dead embed; fix is either finding a replacement video for that article or, if no equivalent exists, noting it in the audit for manual review |
| Anthropic infra outage | Anthropic side | Manual re-run via routine dashboard, or wait for next scheduled run |
| New article drifts away from video-companion structure | Model defaults to a non-video structure | Morning audit checks structural compliance; fix is a structural edit, flag in report if it recurs |

## Ownership

- **Routine config (prompt, cron, model):** Richard (his claude.ai/code account)
- **Template contract file:** Jet (site fleet lane), PRs land in this repo
- **Site content review / audits:** Jet
- **Infra (deploy pipeline, hosting):** Webster
- **Owner-of-record for the site:** Richard

## Related files

- Site repo: `E:/Ai/Codex/bridgehedges.co.uk/`
- Template contract: `TEMPLATES/how-to.md`
- Config: `config/ingestion.json`
- Ring reference implementation: `E:/Ai/Codex/chathamhedges.co.uk/INGESTION.md`
- Nightly self-audits (once live): `reports/YYYY-MM-DD-content.md` /
  `reports/YYYY-MM-DD-audit.md` in this repo

## YouTube videos: click-to-play only (do not undo)

Wherever this document mentions a YouTube `<iframe>` or embed, use the click-to-play button instead. An iframe loads YouTube (Google) before the visitor clicks, which sets cookies and shares their IP address. Put this exact markup inside the existing video wrapper element (keep the wrapper class unchanged):

```html
<button type="button" class="yt-play" data-yt-id="VIDEO_ID" data-yt-title="VIDEO TITLE"><span class="yt-play-icon" aria-hidden="true"></span><span class="yt-play-text">Play video<small>Playing this video loads it from YouTube (Google), which may store cookies on your device and receive your IP address.</small></span></button>
```

Use the real 11-character video id and the video title (HTML-escaped). The click handler is already in the site's `analytics.js`, which is on every page. Never write `<iframe ... youtube ...>` into a page. Any audit or check that finds a YouTube iframe in a page must convert it to this button. The video credit paragraph stays as it is.

## Source lines: no process wording (do not undo)

Published source credits must read like an honest reference list, never like a description of how the page was produced. Never write any of these on a public page: "egress", "proxy", "blocked", "generation environment", "INGESTION.md", "AGENTS.md", "web search", "cross-referenced via", "confirmed to exist via". If a source could not be opened, simply leave it out of the list or cite a source you did read. The only acceptable wording is plain, e.g. "checked against current sources". Audits must flag and rewrite any page that contains these phrases.
