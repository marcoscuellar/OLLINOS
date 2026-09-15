# HANDOFF — ŌLLIN Landing Page

**From:** Claude Code (session 3) · **To:** the next session · **Owner:** Marcos Cuellar (marcosmcuellar@gmail.com)
**Date:** 2026-08-18
**Repo:** `marcoscuellar/OLLINOS` · **Branch:** `main`

---

## SESSION 5 (2026-09-07) — the rebrand shipped. Read this before the older notes below.

`index.html` is now the **ŌLLIN OS rebrand** (graphite/yellow, "Stop the noise."), replacing the black/volt "not another AI sales tool" page. The previous page is in git history and in `builds/` (last snapshot `ollin-20260821-2048.html`). The older notes below describe that previous page — its section list, `.va-*` card, El Macron 3D stamp, and cut-in-half layout **no longer exist on the live page**; they survive in `builds/` and `parked/`.

What was done to the rebrand file before shipping (it failed the deploy gate as delivered):

1. **Sample data → approved roster.** Meridian Logistics → Aldervane Freight · Northstar Health → Verrida Health · ForgeWorks Manufacturing → Kestrelbrook Devices · Daniel Reyes → Omar Reyes · Rachel Okafor → Nadia Cole · Priya Natarajan → Priya Anand · Tom Lindqvist → Leo Fontaine · Marcus Bell → Marcus Vale · Amara Singh → Sofia Marin · Lucas Bennett → Dana Rivera. Sample emails moved to `.example`. Internal GO profile keys renamed to match. `./scripts/check-sample-data.sh` → PASS.
2. **`<head>` metadata ported** (El Macron favicon, description, OG/Twitter; `og:image` still the old cover — see README known issues).
3. **`#run` rebuilt as the Motion UI *CTA: signup celebrate* pattern** (Marcos's explicit ask, with the screenshot). One centred pill → POST `/api/contact` → on 200 the button morphs to **✓ Received** + particle burst + *"Received. A person replies within one business day at {email}."* On error: the direct address, visibly. The form script is a plain `<script>` so it works without the Motion CDN. Background stays graphite (deliberately not the example's pastel gradient).
4. **`api/contact.js`**: only the notification wording changed — subject `Bring an account — {email}`, first line "…asked to bring an account and see the four checks run live." Env vars, honeypot, reply-to, error handling untouched. **Every submission emails Marcos at `marcos@ollinos.com` immediately** (his ask).

Verified before shipping: gate PASS; Playwright at 390/768/1440 — no overflow, no JS errors, head meta present, pill stacks ≤520px; mocked 200 → Received state with the email in the line; mocked 500 → fallback + button re-enabled; invalid email → shake, no request.

Still true from below: hard rules 1–9, the env-var notes in §1b, the booking link (`OLLIN_BOOKING_URL`) still unset, Google Workspace still blocked on Marcos. **Deploy is by merging to `main`** — the ŌLLIN OS site project on Vercel is git-linked (that blocker from §1 is resolved).

---

## READ THIS FIRST — what changed in sessions 3–4 (previous page)

The repo was **completely empty** at the start of this session. The page existed only as a local file and an orphan Vercel upload. It is now in git, with history.

The page was also **restructured from a 14-section system tour into a short product page**, at Marcos's direction: *"it's a product page, product pages are supposed to be the ones connecting with the humans of like why you should care."*

**14 sections → 5 beats and an ask.** Everything removed was **parked, not deleted.**

### Current page order

| Rail | id | What it is |
|---|---|---|
| — | — | Hero — "Not another AI sales tool." |
| **00** | `#evidence` | The execution gap — 3 cited stat cards (number-scramble on scroll) |
| **01** | `#trust` | Safety/privacy — white verified report card + 3D El Macron stamp |
| **02** | `#signal` | The signal — console types a question, answers it (plays once) |
| **03** | `#vamos` | **The intelligence** — left half of the VAMOS card, built out |
| **04** | `#ovamos` | **The outreach** — right half + the provenance column |
| **→** | `#book` | Book 30 minutes |

### The centerpiece: the card, cut in half

Marcos's framing, verbatim: *"i was hoping you would really 'cut it in half' … show the intelligence built out and then the start of vamos."*

- **§03** = `.va-know` only, as **two groups side by side** (`01 The org` / `02 The contact`), so each fact and its engine attribution has room.
- **§04** = `.va-compose` only, narrower (620px), reading as the *start* of a flow.
- They are presented as **one physical card severed across the scroll**: §03 ends on a perforated edge with squared bottom corners, §04 opens on the mirrored perforation with squared top corners, and a dashed volt connector labelled *"the same card, cut in half"* bridges the section boundary.

**Layout is locked** (Marcos confirmed against his screenshot of the real product): **intelligence LEFT, outreach RIGHT**. Do not mirror it.

### Light theme — how it works (don't duplicate rules)

The card renders on the real product's white surface via `.vp-light`, which **redefines the design tokens in scope** (`--raised`, `--surface`, `--black`, `--border`, `--white`, `--body`, `--slate`, `--dim`). Because every `.va-*` rule is written in custom properties, the whole component flips with no rule duplication and the JS keeps its ids.

Two traps already hit and fixed — do not reintroduce:
1. **`.vp-light{color:#0B0C10}` is required.** `body` inherited `#FFF` *before* the tokens were redefined, and inherited color is an absolute value — without this, `.va-name` is white on a white card and the contact's name is invisible.
2. **Volt as text is unreadable on white.** `.va-eng` and `.va-org` are swapped to `--olive: #6E7D1A`.

### Texture + depth (added last)

- **Paper grain** — fractal-noise SVG, `mix-blend-mode:multiply`, on `.vp-grain`.
- **Pointer tilt** — `perspective(1500px)` + `rotateX/rotateY` capped at **3.6°**, rAF-lerped, easing back to flat on leave.
- **Sheen** — volt-tinted highlight tracking the pointer + a directional light gradient.

**Critical stacking rule:** grain and sheen sit at `z-index:1`; `.va-head`, `.va-body`, `.vp-perf`, `.vp-cut` are lifted to `z-index:2`. An earlier pass had the sheen painting 60% white *over* the type and the whole card looked hazy. Texture treats the surface only.

The tilt **arms only after the `.rise` entrance settles** (MutationObserver + 1250ms), so the two transforms never fight. Pointer-only; no-ops on touch. Under `prefers-reduced-motion` the tilt is forced flat and the sheen is `display:none`.

### The CTA

Rebuilt on Marcos's own words: *"I'd love to show you how the engines actually work — let's book 30 minutes to go over them and learn how we can support you."*

- Eyebrow `30 MINUTES · THE ENGINES RUNNING LIVE`, headline *"Let me show you how the engines actually work."*, one button `Book 30 minutes →`.
- **Single primary action** — the old competing "See the system ↑" button is gone.
- `"Approval is not send"` was relocated here from the parked `#rules`, where it does the most work.
- The headline is now a genuine open loop: the engine carousel is parked, so the page never shows the engines.

**Layout (session 4) — centred signup, after the Motion UI `cta-signup-celebrate` pattern.** The section comes **off the `.sec` rail grid** entirely and runs as one centred column (`.cta-wrap`, 660px): pill badge → headline → one line of copy → **one field and one button sharing a single rounded pill** (`.cs-pill`) → fine print. Dropping the rail is safe: the scroll-lit observer only picks up sections that contain `.rail .no`.

- The five-field sheet is now **one field — work email — and nothing else**. Name, company, account and the message are **gone**, not hidden: an interim version parked them behind a disclosure and Marcos cut that too. The mailto body compensates by carrying the intent as a real editable draft ("I'd like 30 minutes to see the engines run…"), since the address alone says nothing. Subject is `30 minutes — <email>`.
- Below 520px the pill stacks (input over full-width button) — the two won't fit side by side without crushing the field.
- The confirmation is unchanged in behaviour but now **centred**, so the shard burst is thrown from `left:calc(50% - 3px)` instead of the old left-aligned origin. The reach is still clamped to the viewport — transformed shards count toward `scrollWidth`.

---

## IMMEDIATE TASKS

### 1. ⚠️ Deploy — BLOCKED, needs Marcos (one click)

**Nothing is live yet.** The Vercel MCP connector kept dropping out of the session and requires an OAuth authorization that a non-interactive session cannot complete. No Vercel CLI, no token, no `~/.vercel` credentials.

Two ways through, pick either:
- **Connect the repo (recommended, permanent):** Vercel → the ŌLLIN OS site project → Settings → Git → connect `marcoscuellar/OLLINOS`. Every push then auto-builds. Five commits are already waiting on the branch.
- **Authorize the Vercel connector** in claude.ai connector settings, then a session can deploy directly.

Project details: team `marcosmcuellar-3433s-projects` (`team_G7WBdov66WzlS910sSqwaJB9`), the ŌLLIN OS site project (`prj_hA9wYDj1a3D2BCKsReQJIQz4lMgb`). Deploy `index.html` as-is — no build step, zero config.

Do **not** try to inline the 99KB page into `deploy_to_vercel` — retyping 1,452 lines verbatim risks silent corruption.

### 1b. ⚠️ Contact form — needs two env vars in Vercel

`api/contact.js` is a zero-config Vercel Node function at `/api/contact`. The page
POSTs `{email, source, website}` to it and it sends through Resend, replying-to the
visitor. `OLLIN_FORM_ENDPOINT` is now set, so the confirmation says **Received** only
once the function returns 200; if it errors the page falls back to the direct address.

**Until `RESEND_API_KEY` is set in Vercel, every submission fails visibly.** Set it at
Vercel → the ŌLLIN OS site project → Settings → Environment Variables (all three environments),
then redeploy. `CONTACT_TO` defaults to `marcos@ollinos.com` and `CONTACT_FROM` to
Resend's shared `onboarding@resend.dev`, which **only delivers to the Resend account's
own address** — verify `ollinos.com` in Resend and set `CONTACT_FROM` to lift that.

A honeypot field (`name="website"`, off-screen) is submitted with every request; the
function answers 200 and sends nothing when it is filled.

### 2. ⚠️ Wire the booking link

`OLLIN_BOOKING_URL` is a **single empty constant** at the top of the second `<script>` block. Set it and both CTAs (masthead `#bookStreak`, final `#bookBtn`) pick it up automatically:

```js
var OLLIN_BOOKING_URL = 'https://...';
```

While empty, both fall back to jumping to `#book`. Marcos said the real link *"lives on another brand of his"* — he still owes it. Also confirm the scheduler actually books **30** minutes, to match the copy.

### 3. ⚠️ Verify the Forrester stat before production

§01 footnote: *"4.3 hours a week … $14,200 per employee, per year. Forrester · via Tandem.ai."* Flagged **unverified** in the previous handoff and still unverified. It is a public claim with a named source. Confirm or cut it.

### 4. Legacy domain

A `.com` on the retired working name is still attached to the ŌLLIN OS site project. Remove it, or keep it only as a redirect to ollinos.com if old links must survive. Reassigning a live domain is outward-facing — confirm with Marcos first.

### 5. Google Workspace (still blocked on him)

Signup says *"domain already in use."* Path A: `admin.google.com` with his gmail or a mailbox on the legacy domain. Path B: the "here" link → domain-reclaim → TXT record in Vercel DNS.
**DNS gotcha he already hit:** for Google verification it is Name `@`/blank, Type **TXT**, Value the full `google-site-verification=...` string — *not* an A record. Gmail MX: Name blank, Type MX, `smtp.google.com`, priority 1.

---

## HARD RULES (enforced repeatedly — do not violate)

1. **NO letter-scramble animation. Ever.** Even if a spec asks — flag and confirm instead. It was added twice before and rejected both times. **The dead `scrambleOne()` implementation has now been deleted outright** so it cannot be re-enabled by accident; `.hero .scr[data-final]` returns 0 elements. The **number**-scramble on the §00 stat figures is different, is original behavior, and stays.
2. **No real client or person names in sample data.** Approved fictional set: Brightpath Health Systems, Halcyon Devices, Cobalt Platforms, Northgate Freight, Dana Rivera, Marcus Vale, Priya Anand, Leo Fontaine, Nadia Cole, Omar Reyes, Sofia Marin, Grace Kim. Must **never** appear: Mediaocean, Satish Mandalika, Hallmark, Crissi Matthews, 24 Seven, Procom, Northwind — **and now also Spyglass Partners, Prisma, and req numbers/salary bands**, which appeared in a product screenshot Marcos shared. Sample emails must use a reserved TLD (`.example`).
   → **`./scripts/check-sample-data.sh` enforces all of this** and exits non-zero on any violation. Run it before shipping; it is deploy-gate ready.
3. **ŌLLIN always with the macron (Ō)** in all visible text.
4. **No hype language:** never "revolutionary", "game-changing", "effortless", "next level", or unverifiable absolutes like "the only".
5. **Preserve verbatim:** stats + citations (Salesforce State of Sales, Bullhorn GRID 2026), receipt quotes + anonymized attributions, "Evidence or not at all", "Missing beats fabricated", "Approval is not send", and all "sample data / nothing sent" disclaimers. Parking a section does not alter its text; reinstating one must not reword it.
6. **Ship a date-stamped copy into `builds/` on every delivery** (`ollin-YYYYMMDD-HHMM.html`).
7. **El Macron in 3D is fine small** (badge/stamp on a card) but **never** as a big page-header showcase.
8. Verified stamp reads **VERIFIED**, not "VALIDATED".
9. **He works section-by-section, fast, and has ADHD.** TLDR first, bullets, short replies. Small edits: apply and ship. Visual changes: one screenshot check.

---

## DESIGN SYSTEM — do not redesign

- **Colors:** bg `#0B0C10`, surface `#14161F`, raised `#0E1016`, border `#222634`, volt `#D4FF00` (`--em`, rgb `212,255,0`), on-volt `#14180A`, white `#F8FAFC`, slate `#94A3B8`, body `#C9D3DE`, dim `#77879A`. Volt applies via `<html data-accent="volt">`; the bare `:root` orange is the alternate theme.
- **Light-card palette** (`.sp-card` and `.vp-light`): `#fff`, `#E4E7EC`, `#EEF0F3`, `#98A0AD`, olive `#6E7D1A`, `#39414E`, `#4A5462`, `#C4CAD2`.
- **Type:** Archivo (900, uppercase, tight tracking, all headlines) + IBM Plex Mono (labels, eyebrows, system voice). Hero `h1`: `clamp(52px,8.6vw,126px)`, line-height `.88`, tracking `-.055em`.
- **Convention:** mono = the system talking, Archivo = humans talking.
- **Logo:** "El Macron" — volt bar over white ring. Masthead 20px, hero 34px (scroll-fades by ~420px), footer 40px, favicon inline SVG data URI.
- **Signature devices:** the volt highlight block (`.hl`) behind a key word; volt periods ending headlines; **and now the cut** — the perforated severed edge between §03 and §04.
- **Motion:** Motion.dev hero-terminal feel — blur + fade + rise + slight scale, staggered children, IntersectionObserver `rootMargin:'0px 0px -42% 0px'` so reveals play on arrival, not on first peek. `prefers-reduced-motion` handled throughout.

---

## REPO LAYOUT

| Path | What |
|---|---|
| `index.html` | The live page. Edit this. ~1,452 lines, self-contained. |
| `builds/ollin-20260817-1211.html` | The **complete original 14-section build**, byte-identical to what Marcos delivered (md5 `c0b64afa1b26ac2d6839b2b48baa28fe`). Never edit. |
| `parked/pages/` | Standalone **runnable** extractions: `engines.html` (8-card 3D carousel), `pathways.html` (4 routes), `vamos-dark.html`, `ai.html`. All render-verified. |
| `parked/sections/` | Static markup snippets: `output`, `tracker`, `rules`, `receipts` — each annotated with original line range + CSS/JS deps. |
| `parked/README.md` | Why each section came off and how to reinstate it. |
| `scripts/check-sample-data.sh` | Hard-rule-#2 guard. |
| `README.md` | Rules, design system, page structure, known issues. |

**Marcos wants the parked engine cards back later as a marketing tool** — *"i can use the cards later as marketing tool"* — and said *"bring out the pathways and the engines later."* `parked/pages/engines.html` and `pathways.html` are the seeds; they already work standalone.

---

## TECH NOTES

- Two inline `<script>` blocks at the end of `<body>`. No frameworks, no build. Only external request is the Google Fonts CDN.
- **Dead code removed this session:** `scrambleOne()`, the unreachable `armMagnet()` body, the orphaned `ANGLES` switcher + `.vc-*` CSS, unused `SEGS`/`mk()`, and the engine/pathway/console modules whose markup is parked. `GROUPS` trimmed to live selectors. 1710 → ~1452 lines.
- **Bugs fixed:** `.va-q:hover` referenced an undefined `--line-2`; grid children had default `min-width:auto` and punched 487px out of a 324px card on mobile (silently clipped by `body{overflow-x:clip}`) — fixed at the base rule with `.va-body>*{min-width:0}`, so the parked dark version inherits it.
- The intro overlay plays on **every** load (~7s: types the greeting, then ignites). Consider `localStorage` once-per-visitor gating for production.
- Footer reveal: `.shell{margin-bottom:360px}` + `footer{position:fixed}` — keep the two in sync if footer height changes (mobile 320px).
- **Verification method:** Playwright, Chromium at `/opt/pw-browsers/chromium`, `PLAYWRIGHT_BROWSERS_PATH=/opt/pw-browsers`. **Never run `playwright install`.** Google Fonts is blocked in the sandbox, so `ERR_CONNECTION_RESET` in console is expected and not a page fault.
- Last verified: 390/768/1440 — no overflow, no clipping, no dead anchors, no JS errors; chips retype the draft; `Let's go` types to 414 chars; reduced-motion disables tilt and sheen.

---

## BRAND VOICE

Concise, confident, specific, human. Editorial black and volt. The system shows its work; **restraint is the differentiator**. Anti-lead-gen: everyone else sells volume — ŌLLIN verifies, protects, and tracks the person.

The strongest thing on the page is that it **volunteers what it does not know**: two intel rows read `not tracked yet`, and the copy points at them — *"missing beats fabricated"*, *"no fact, no sentence."* Protect that. It is the whole differentiator.
