# Bridge Hedges & Tree Services - how-to article template contract

Contract for every future `how-to/*.html` article shipped on this repo.

Owner of this contract: Jet (website fleet).

Version: 1.0 - 2026-09-20 (scaffolded fresh for this site at launch).

## Why this exists

bridgehedges.co.uk launched 2026-09-20 with 4 hand-authored `how-to/*.html`
articles. This contract exists so any future article, whether hand-authored
or produced by a nightly content-ingestion routine, matches that established
markup exactly, and so future template changes are PR-able without
ambiguity.

If this file conflicts with any routine's prompt, this file wins, update the
prompt to match.

## Why video-companion-led (this site's structural angle)

bridgehedges.co.uk is one of roughly 30 sister sites in a Kent
hedge-trimming ring (broadstairs/canterbury/chatham/deal/dover/margate/
ramsgate/thanet/wingham/sandwich/littlebourne and many others), same
business model, same layout conventions, different towns. Near-identical
article sets across sister domains is a known SEO/AIO risk, so each ring
site has been assigned a distinct macro-structure to avoid the ring reading
as templated duplicate content to Google.

**This site's assigned angle is video-companion-led.** Every how-to article
here is built around one specific, real, publicly-available UK hedge-care
YouTube video. The article is not a transcript and not a generic rewrite of
the video's content, it is a companion: it credits the video and creator up
front, summarises what the video actually shows (with approximate
timestamps where useful), then adds what the video does not and cannot
cover, namely Bridge/Nailbourne-valley specificity (North Downs chalk,
frost-pocket valley floor, Bifrons Park Conservation Area, the relevant
legislation). The video is the spine; the local knowledge is the payoff for
reading past the embed. This is a firm house style for this site
specifically, not a rotation. Do not revert to a plain hero → prose →
callout skeleton with no video anchor, and do not borrow another sister
site's assigned angle (procedural-first, checklist-first, comparison-table,
FAQ-led, story-led/case-study, data/stats-led, myth-busting, hyper-local,
timeline/seasonal-calendar-led, tool-and-technique-led,
expert-QA/interview-led, cost/pricing-led, problem-diagnosis-led,
coastal/weather-led, before-and-after-led, staged-progress-led,
decision-tree-led, regulation-first, quick-answer/TL;DR-first,
glossary/definitions-led, heritage/period-garden-led, numbered-rules-led).

## Hard requirements (P1 - must be in every article)

### 1. Match the existing page anatomy exactly
Copy the head/body shape from an existing hand-authored article, not from a
generic template. That means:
- `<title>` ending ` | Bridge Hedges & Tree Services`
- `meta description`, `canonical`, OG (`og:type=article`, title, description,
  url, image `https://bridgehedges.co.uk/assets/img/og-default.png`
  1200x630), `robots content="index,follow,max-image-preview:large"`
- `<html lang="en-GB">`
- Fonts: `Libre+Caslon+Text:wght@400;700` + `Source+Sans+3:wght@400;500;600`
  (same `<link>` block as existing pages)
- Favicon: the inline SVG data URI already in use (navy square, gold hedge
  arc with mast/anchor flourish), do not invent a new one
- GA4 tag (current measurement ID, see any live page's gtag snippet), same
  snippet verbatim
- Full topbar ("Considered hedge work on the old Roman road." + pensioner-
  discount badge + phone + WhatsApp button + email)
- Full site nav (Home / Services / Areas / Guides / Recent jobs / About /
  Contact)
- Full footer (Services / Local guides / Contact columns), identical to
  every other page, do not shorten it. The "Local guides" column is a fixed
  3-link list (All guides / Areas covered / About), it does not grow per
  article.
- `<script>document.getElementById('yr')...</script>` year script at the
  bottom

### 2. JSON-LD - plain `Article`, not `@graph`, not `HowTo`
A single `<script type="application/ld+json">` block with a flat `Article`
object: `headline`, `description`, `author: {"@type": "Person", "name":
"Richard Lim"}`, `publisher: {"@type": "Organization", "name": "Bridge
Hedges & Tree Services"}`, `datePublished`, `inLanguage`,
`mainEntityOfPage`. **No `@graph` wrapper, no `BreadcrumbList`, no
`FAQPage`**, this site's hub page (`how-to/index.html`) is the only page
using `@graph` (`CollectionPage` + `BreadcrumbList`). Individual articles
stay flat `Article`.

### 3. datePublished + dateModified
```json
"datePublished": "YYYY-MM-DD"
```
Add `dateModified` only if you are editing a previously-published article.

### 4. The video-companion opening (this site's house structure)
Every article MUST open with:

1. A short intro paragraph naming the video, its creator/channel, and why
   it is a genuine, useful, real source (not a fabricated citation).
2. A `.callout`-styled "Watch first" block containing the embedded video
   (single `<iframe>`, no autoplay, `youtube-nocookie.com/embed/<id>`) and a
   direct link to the original `youtube.com/watch?v=<id>` for attribution.
3. A `<h2>What the video shows</h2>` section: a short bullet list of the
   real technique/steps demonstrated, with approximate timestamps
   (`0:45`-style) where they add value. Do not invent timestamps you have
   not verified, use "early in the video" / "toward the end" if unsure.
4. A `<h2>What it doesn't cover: Bridge and the Nailbourne valley</h2>`
   section, the genuinely local payoff: chalk vs valley-floor species
   choice, frost-pocket timing, Bifrons Park CA rules, nesting law, whatever
   is genuinely relevant to the specific video's topic.

Use the existing `.callout` class for the video-embed block, do not invent a
new CSS component for this site.

### 5. The nesting-season / legal reference (this site's established pattern)
Any article touching cutting/trimming timing MUST reference the **UK
Wildlife and Countryside Act 1981, section 1** (nesting season, offence to
damage/destroy an active nest, **1 March to 31 August**) by name and
section, in ordinary prose. If the topic is a neighbour/legal one, also
reference the **Anti-social Behaviour Act 2003, Part 8** (High Hedges, 2m
evergreen threshold, Canterbury City Council administers for the Bridge
parish). If the topic touches the conservation area, reference **s.211 Town
and Country Planning Act 1990** (six-week notice, Bifrons Park CA, 75mm stem
threshold).

### 6. Open Graph image
Every article uses `https://bridgehedges.co.uk/assets/img/og-default.png`
(1200x630) unless a more specific image already exists for the topic, this
site does not have a per-article CDN image library.

### 7. CTA callout near the end
A `.callout` box near the end of the article with a "Want a slot?" / "Get a
quote" style heading, a short instruction to email
`hello@bridgehedges.co.uk` or call/WhatsApp `07763 100 477` with postcode
and relevant detail, and a one-line promise of what they'll get back.

### 8. Sources line
Every article ends with a `<p class="muted">Sources: ...</p>` line naming
the video (title, creator, URL) and any Acts/guidance referenced. Do not
cite a specific figure (council fee, exact CA boundary) that hasn't been
verified, say "check the council's current figure" instead of inventing a
number.

### 9. Related-article linking
No `JET-RELATED-GUIDES` marker block, no growing footer list. Contextual
in-prose links only: 1-3 inline links to genuinely relevant existing
articles or service pages.

### 10. Hub page update
`how-to/index.html` uses a plain `.grid.grid-2` card list. Add a new
`.card` block with a `section-eyebrow` category label, an `<h3>` link, a
one-sentence teaser naming the video source, and a "Read the guide" link.

## Strong recommendations (P2 - should be in most articles)

### 11. Bridge/Nailbourne-valley specificity
Every article should reference real local detail: Bridge village, the
Nailbourne (a winterbourne chalk stream), the old Roman Watling Street,
Bifrons Park Conservation Area, the Kent Downs Area of Outstanding Natural
Beauty, neighbouring villages (Patrixbourne, Bekesbourne, Bishopsbourne,
Lower Hardres, Barham, Kingston), the North Downs chalk-vs-valley-floor
frost-pocket split, and Canterbury City Council as the relevant authority.
Do not invent specific street addresses or fabricate exact statistics.

### 12. Video credit
Credit the creator by name/channel in the opening paragraph and again in
the closing Sources line. Only use videos that genuinely exist and that you
have verified resolve to a real page, do not fabricate a video ID or title.

### 13. Author/publisher attribution
Named-person author (`Richard Lim`) in JSON-LD, and prose voice reads as a
single working contractor ("I check this against the address before
quoting"), not a team byline.

## Voice and language

- British English (colour, organisation, whilst). Postcode, pavement, tyre.
- Working-contractor voice, first person singular ("I check the address",
  "I'd survey this before touching it").
- **No em-dashes.** Commas, full stops, colons or parentheses instead.
  Hyphens in compound words and en-dashes in ranges are fine.
- No corporate filler: utilise, leverage, seamlessly, best-in-class,
  synergy, robust, cutting-edge.
- Dates as "20 September 2026" or "late September", matching existing
  articles' "Updated Month YYYY" meta line format.
- Real figures where known (national legal thresholds, penalties, dates),
  flag anything unverifiable rather than inventing a number.

## Nice-to-haves (P3)

- A short "What the video doesn't show" caveat where the video's technique
  or region-of-origin differs from Kent Downs practice.
- A one-line "why this video" note if the creator has notable relevant
  expertise (e.g. a specialist nursery, a professional hedgelaying society).

## Non-goals

- No cookie banners.
- No `<script>` tags beyond the GA4 gtag snippet and the single video
  `<iframe>`.
- No autoplaying video, no more than one embed per article.
- Do not switch this site's flat `Article` JSON-LD to `@graph` or `HowTo`.
- Do not import another sister site's structural angle onto this site.
  This site's structure is video-companion-led, full stop.

## Adopting this contract

Every new article must satisfy every P1 rule from first ship. The 4 existing
hand-authored articles are the reference implementation for exact markup
shape.
