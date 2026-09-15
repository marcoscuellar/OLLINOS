# ŌLLIN — landing page

A self-contained landing page for **ŌLLIN OS** — sales intelligence, verified before it reaches you.

- **`index.html` is canonical.** Open it in a browser and it works.
- **No build step. No dependencies.** External requests: the Google Fonts CDN (Archivo + IBM Plex Mono) and the Motion library (`motion@13.2.0` from jsdelivr) for reveals. The contact pill degrades gracefully if Motion never loads.
- All interactions are vanilla JS in two `<script>` blocks at the end of `<body>`. No frameworks.
- Deployed as a static site on Vercel — a root `index.html` needs zero config.

## Layout

| Path | What it is |
|---|---|
| `index.html` | The live page. Edit this. |
| `builds/` | Date-stamped snapshots (`ollin-YYYYMMDD-HHMM.html`). Never edited — these are the retrieval trail. |
| `parked/` | Sections lifted off the landing page but preserved whole, ready to become their own pages. Not referenced by `index.html`. |

`builds/ollin-20260817-1211.html` is the **complete original 14-section build** the current page was reduced from. If a section needs to come back, it's in there and in git history.

## The product story — do not dilute

- **ŌLLIN AI** — conversational front door / orchestrator
- **ŌLLIN** — the intelligence-to-execution system (8 engines)

> An earlier working name was retired from all branding and infrastructure in 2026-09. If an old identifier surfaces anywhere, replace it — do not reintroduce it.
- **Tracker** — connective tissue / shared record
- **VAMOS** — puts prepared actions in front of the human

The human reviews, approves, sends. Narrative arc:
`QUESTION → SIGNAL → EVIDENCE → DECISION → ACTION → RECORD → NEXT ACTION`

## HARD RULES — do not violate

These have been enforced repeatedly. If a spec document asks for something on this list, **flag it and confirm** rather than doing it.

1. **No letter-scramble animation. Ever.** It has been added twice from spec docs and rejected both times. (The *number*-scramble on the stat figures in §00 is different and is original, intended behavior — that one stays.)
2. **No real client or person names in sample data.**
   - Approved fictional set: **Verrida Health, Kestrelbrook Devices, Marrowfield Platforms, Aldervane Freight**, Dana Rivera, Marcus Vale, Priya Anand, Leo Fontaine, Nadia Cole, Omar Reyes, Sofia Marin, Grace Kim.
   - Must **never** appear: Mediaocean, Satish Mandalika, Hallmark, Crissi Matthews, 24 Seven, Procom, Northwind, **Brightpath, Cobalt, Halcyon, Northgate**.
   - **A plausible company name is not a safe one.** Brightpath, Cobalt and Halcyon shipped here by mistake and each collides with a real business (BrightPath Health is a live telehealth company; Cobalt.io and several Cobalt logistics firms exist; Halcyon Freight Ltd is registered). Every current name was web-searched and returns zero company results — do the same before introducing a new one.
   - Cards also carry a visible **"Sample data · fictional"** marker, because name-checking alone can never be a guarantee.
   - This applies to req numbers, salary bands, and company names copied from real screenshots too.
3. **ŌLLIN always carries the macron (Ō)** in all visible text.
4. **No hype language.** Never "revolutionary", "game-changing", "effortless", "next level", or unverifiable absolutes like "the only".
5. **Preserve verbatim:** all stats and their citations (Salesforce State of Sales, Bullhorn GRID 2026), the receipt quotes and their anonymized attributions, the lines "Evidence or not at all", "Missing beats fabricated", "Approval is not send", and every "sample data / nothing sent" disclaimer.
6. **Ship a date-stamped copy into `builds/` on every delivery** so a specific version can always be retrieved.
7. **El Macron in 3D is fine small** (a badge or stamp on a card) but **not** as a large page-header showcase.
8. The verified stamp reads **VERIFIED** — not "VALIDATED".

## Design system — do not redesign

Rebranded 2026-09-07 (session 5) from black/volt to graphite/yellow. The file is the source of truth; these are the tokens in use.

**Colors**

| Token | Value |
|---|---|
| `--graphite` / `--graphite-2` | `#181A1E` / `#1F2227` (dark surfaces: nav, hero, Mushroom, CTA, footer) |
| `--slate` / `--slate-2` | `#2F343D` / `#3A404A` |
| `--yellow` / `--yellow-2` / `--yellow-tint` | `#FFE14D` / `#F5D63A` / `#FFF8D6` (the accent; buttons, highlights, verified ticks) |
| `--white` / `--off` / `--light` | `#FFFFFF` / `#F6F7F9` / `#E9EDF2` (light sections) |
| `--ink` / `--ink-2` / `--gray` / `--gray-2` | `#181A1E` / `#3A404A` / `#6B7280` / `#9CA3AF` |
| `--on-dark` / `--on-dark-2` / `--on-dark-3` | `#F3F4F6` / `#B8BEC7` / `#7C8390` (text on graphite) |
| `--line` / `--line-dark` | `#D9DEE5` / `#2C3037` |

**Type** — Archivo (800, tight tracking, `font-stretch:105%`, all headlines) + IBM Plex Mono (eyebrows, labels, system voice). Hero `h1` is `clamp(46px,6.6vw,84px)`.

**Convention:** mono = the system talking, Archivo = humans talking.

**Logo** — "El Macron": in this build it is the wordmark `ŌLLIN | OS` in the masthead and the small `.omark` (yellow bar over an O) inside the VERIFIED chips. The favicon is the inline-SVG El Macron carried over from the previous build.

**Signature devices** — the yellow highlight block (`.mark`) behind a key phrase, the animated yellow underline on the hero's key word, and the yellow verified tick that lands on each row of the account card.

**Motion** — Motion.dev (`animate`, `inView`, `scroll`, `stagger`): hero staggers in, the account card's rows verify one by one, the four-check rail fills on scroll, the Mushroom org map grows out from the seed contact, ŌLLIN GO rotates through three sample accounts. `prefers-reduced-motion` disables all of it and every piece of content stays visible.

## Brand voice

Concise, confident, specific, human. Editorial black and volt. The system shows its work; restraint is the differentiator. Anti-lead-gen positioning: everyone else sells volume — ŌLLIN verifies, protects, and tracks the person.

## Page structure

| id | Section | What it does |
|---|---|---|
| — | Hero | "Find the right account, the right person, and the reason to reach out now." + the example verified account card (four rows: why now, right person, right message, direct line — the last one **withheld**, because unverified) |
| — | Four checks | Source found → cross-checked → live-confirmed → engine QA; a rail that fills on scroll |
| `#why` | The haystack | Traditional sales intelligence vs ŌLLIN OS, side by side |
| `#product` | Pillars | Verified Insights · Whole Account Mapping · It prepares, you decide |
| `#mushroom` | Mushroom | Map the whole account from one seed contact; confirmed vs inferred lines clearly marked |
| `#execute` | ŌLLIN GO | The execution workspace (`go.ollinos.com`): one card per contact, grounded draft, audited, *Let's go* |
| — | Stats | Salesforce State of Sales, Bullhorn GRID 2026, and the Bullhorn receipt quote — verbatim |
| `#rules` | The rules | Never invents · never softens · never sends on its own |
| — | Core positioning | Verification is the mechanism. Better decisions are the product. |
| `#run` | Bring an account | The contact pill (see below) |

Every "book / meet / bring an account" link on the page points to `#run`.

## Contact and booking

`#run` is a single centred signup pill, after the Motion UI *CTA: signup celebrate* pattern: one field (work email) and one button sharing a rounded pill. On submit it POSTs `{email, source:'ollin', website}` to **`/api/contact`** (`api/contact.js`, a zero-config Vercel Node function). The function sends through Resend to `CONTACT_TO` (default **`marcos@ollinos.com`**) with `reply_to` set to the visitor, and only then returns 200 — so the button morphing to **✓ Received** and the line *"Received. A person replies within one business day at …"* is honest. Any other outcome shows the direct address instead of failing silently. The `website` field is an off-screen honeypot.

Env vars (Vercel → the ŌLLIN OS site project → Settings → Environment Variables): `RESEND_API_KEY` (required), `CONTACT_TO`, `CONTACT_FROM` — see the header comment in `api/contact.js`. The form script is a plain `<script>`, independent of the Motion module, so it works even if the CDN is blocked; Motion only adds the particle burst.

Three constants sit at the top of that script:

```js
var OLLIN_BOOKING_URL   = '';               // optional scheduler link; when set, the masthead button points at it
var OLLIN_FORM_ENDPOINT = '/api/contact';   // where the pill posts
var OLLIN_CONTACT_EMAIL = 'marcos@ollinos.com';
```

`scripts/check-sample-data.sh` allowlists `marcos@ollinos.com` and the placeholder `you@company.com`; every other email on the page must use a reserved TLD.

## Known issues

- **`CONTACT_FROM` defaults to Resend's shared `onboarding@resend.dev`**, which only delivers to the Resend account's own address. Verify `ollinos.com` in Resend and set `CONTACT_FROM` to lift that (see `api/contact.js`).
- **`assets/og-cover.png` is the previous (black/volt) design.** Social previews work but don't match the rebrand — regenerate it at 1200×630 in graphite/yellow.
- The stats quote attributed to a Bullhorn executive is marked *published with permission* — keep that confirmable.
- The 30-minute duration in the CTA copy should match whatever a scheduler actually books, if `OLLIN_BOOKING_URL` is ever set.
