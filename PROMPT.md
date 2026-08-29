# Master Prompt — "SIGNAL & SYSTEM" — The Sayed Mohammad Firdousi Portfolio

A copy-paste prompt for building the portfolio from scratch in a fresh implementation session.
Open a new Claude Code session **in this folder** (`MyPortfolio`) and paste everything between the
rules below. All facts come from `resume.txt` and the existing `index.html` — nothing is invented.

---

## THE PROMPT

> Build a single self-contained `index.html` — an advanced, graphics-led one-page portfolio for a
> backend / cloud engineer, executed at the level of a flagship agency site. Pure HTML, CSS and
> vanilla JS. No frameworks, no build step, no external libraries. Google Fonts is the only remote
> dependency; every image is a local asset from `images/`. It replaces the current `index.html`
> (keep a backup as `index.old.html`). It must score green on Lighthouse and feel engineered, not
> templated.

### 1. Art direction

**An engineering document that became beautiful.** Think: a system-design schematic plotted on
dark carbon paper, annotated in monospace by a careful hand, with one live signal running through
it. Swiss restraint + terminal heritage + editorial serif moments. It must feel like the person
who builds AWS serverless pipelines drew their own blueprints — precision, hairlines, measured
labels — never a "developer template". Restraint is the brief: one accent color, two accent
objects per section, generous black space.

The reference for *quality bar and spec discipline* is `../Architect Portfolio/index.html` and its
`PROMPT.md` — study how it achieves a printed feel (sheet frames, hairline rules, ghost type, one
signature generative graphic, a single rAF loop). Do **not** copy its red/Didone look; translate
its discipline into the dark engineering language below.

### 2. Palette — committed, no theme toggle

| Token | Hex | Role |
|---|---|---|
| `--carbon` | `#0B0D10` | page ground |
| `--panel` | `#12151A` | cards, elevated planes (1px hairline, no drop shadows) |
| `--bone` | `#EDEAE3` | primary type — warm ivory, never pure white |
| `--dim` | `#8A9099` | secondary type, captions |
| `--hairline` | `#262B33` | rules, frames, dividers |
| `--signal` | `#B6F09C` | THE accent — trace lines, active states, metrics, cursor |
| `--signal-dim` | `#5C7A4E` | spent/echo state of the signal |
| `--alert` | `#FF6B57` | used at most 3 times on the whole page (live badge, one metric, 404 of humor) |

One committed dark look. No light mode, no toggle — conviction reads as expensive. Ensure AAA
contrast for body (`--bone` on `--carbon` passes), AA minimum everywhere.

### 3. Typography

- **Display:** `Instrument Serif` (Google Fonts) — large editorial serif, used *sparingly*: hero
  headline, section numerals, pull-quotes. Italic for emphasis words.
- **Mono:** `IBM Plex Mono` — all labels, annotations, nav, metadata, buttons, code moments.
  Uppercase labels at `letter-spacing:.14em`, 11–12px.
- **Body:** `Inter` — paragraphs only, `line-height:1.75`, max measure `62ch`.
- Load with `preconnect` + `display=swap`, request only used weights (Serif 400/400i, Mono
  400/500, Inter 400/600).
- **Section headers as plates:** every section opens with a mono index (`01 / ABOUT`), a hairline
  that draws in from the left on reveal, and a display-serif title. Consistent everywhere.

### 4. The signature graphic — the live trace

A generative **circuit-trace field**: the page's identity, built in JS as inline SVG, appearing in
the hero (full-bleed behind content, right-weighted) and reused rotated in the contact section.

1. Generate ~28 orthogonal "traces": paths that run mostly horizontal with 45° corner bevels
   (PCB-routing style), spawned from a virtual left edge, jittered lanes, occasional vias
   (small circles, `r=2.5`, `fill:var(--carbon)`, `stroke:var(--signal)`).
2. Stroke `var(--hairline)` at rest — the board. Then 4–6 of them carry a **moving pulse**: a
   short bright `--signal` dash (`stroke-dasharray` window) traveling the path on the rAF clock at
   different speeds, easing through corners naturally because it follows path length.
3. Draw-on: all traces render with `pathLength=1`, `stroke-dashoffset 1→0`, 1.6s ease-out,
   staggered ~22ms, released by IntersectionObserver.
4. Pointer interaction: traces within 140px of the cursor brighten toward `--signal-dim` with a
   quadratic falloff; the nearest via emits a one-shot 400ms radar ring. Subtle — a board waking
   under a probe, not a light show.
5. Scroll gain: accumulate `|Δscroll|/200` into a decaying `gain` (×0.92/frame) that multiplies
   pulse speed — scroll hard and the system visibly works harder, then settles.
6. Set the viewBox from the true bounds of generated geometry; `vector-effect:
   non-scaling-stroke`; the whole layer `aria-hidden` and `pointer-events:none` (hit-testing done
   from cursor math, not DOM events per path).

### 5. Boot sequence

Full-viewport `--carbon` overlay, gated behind a `.js` class set by an inline `<head>` script.
A mono terminal line types in: `$ firdousi --init` → three status lines appear
(`▸ loading systems … ok`, `▸ mounting projects … ok`, `▸ signal established`), each with a
120–300ms stagger, while a hairline progress rule fills `--signal` left-to-right over ~1.6s with a
`000→100` counter at its right end. On completion the overlay exits with
`clip-path:inset(0 0 100% 0)`. Drive from rAF timestamps entered via `raf(tick)` (never call
`tick()` directly — first frame becomes `NaN`). Arm a ~4s `setTimeout` safety net that force-clears
the overlay. Play it once per session (`sessionStorage`), skip entirely on revisit.

### 6. Persistent chrome

- **Top-left:** mono wordmark `SMF://` — clicking it scrolls home.
- **Top-right nav:** mono uppercase links (About · Stack · Work · Field · Contact) with a
  left-to-right `--signal` underline wipe on hover and a magnetic pull within 80px. Active section
  tracked via IntersectionObserver, marked with a leading `▸`.
- **Bottom-left rail** (`writing-mode:vertical-rl`, ≥1000px only): `MUMBAI, IN — <live UTC+5:30
  clock, updating each minute>`.
- **Bottom-right:** section counter `01 / 09` plus a 1px scroll-progress rule filling in
  `--signal`.
- Mobile: nav collapses to a full-screen overlay menu (mono list, staggered rise-in, body scroll
  locked, closes on link tap / Esc).
- All chrome gets a `--carbon` text-shadow halo so the trace field never fights it.

### 7. Sheet 01 — Hero

- The live trace field across the background, density biased right.
- `images/first.png` (watercolor splash profile, transparent PNG) large on the right, entering
  with a soft rise + settle; a faint `--signal` rim-light glow behind it
  (`radial-gradient`, 12% opacity). Parallax at 0.03× pointer.
- Mono kicker types out of character noise: `BACKEND DEVELOPER — CLOUD ENGINEER — MUMBAI`.
- Headline in Instrument Serif, ~`clamp(44px, 7vw, 96px)`:
  **"I build systems that** *stay up***."** — "stay up" italic in `--signal`. Per-word rise
  reveal (translateY + clip), staggered 60ms.
- Sub-line (Inter, `--dim`, ≤2 lines): "1.5+ years shipping production CRMs, SaaS platforms and
  serverless AWS workflows — from schema to deploy."
- Two buttons, mono: `[ VIEW RESUME ↗ ]` (Google Drive link) and `[ GET IN TOUCH ]` — 1px
  hairline borders that fill `--signal` (text flips to `--carbon`) on hover; both magnetic.
- Bottom of viewport: a scroll cue — mono `SCROLL` + a 24px vertical rule with a descending
  `--signal` dot on loop.
- Availability badge top of hero content: pulsing 6px `--signal` dot + mono
  `OPEN TO OPPORTUNITIES`.

### 8. Marquee

Between hero and About, a full-bleed strip ruled top and bottom (hairlines), carrying
`Node.js ● PostgreSQL ● AWS Lambda ● TypeScript ● Python ● React ● Redis ● Docker ● Next.js ●
Supabase` in outline display type — `color:transparent; -webkit-text-stroke:1px var(--hairline)`
at `clamp(28px,5vw,72px)` — with every fourth item solid `--signal-dim`. Two identical runs in a
flex track translated by modulo of one run's measured width for a seamless loop; base drift
~0.5px/frame plus the scroll `gain`. Pause on hover.

### 9. Sheet 02 — About

Two-column: `images/image.png` (watercolor blazer portrait) left inside a hairline plate with a
mono caption bar beneath (`FIG. 01 — S.M. FIRDOUSI`), text right:

- Lead pull-quote, display serif: *"Full-stack developer and cloud engineer with proven
  end-to-end product ownership — from database design to deployment."*
- Two short Inter paragraphs (source them from the current About copy: AgentLens, Rowan Rose,
  Electrolab, freelance — tighten, don't pad).
- **Stats row** with rolling count-up on reveal (mono numerals):
  `1.5+ YRS EXPERIENCE · 50+ PROJECTS DELIVERED · 2 AWS CERTIFICATIONS · 32 API ENDPOINTS
  (AGENTLENS)`.

### 10. Sheet 03 — Experience: the git log

The career timeline rendered as an **annotated git graph**: a vertical `--hairline` line with
`--signal` commit nodes, drawn on by scroll progress (line grows as you scroll through the
section). Each entry is a commit:

```
● a3f9e21 — Jan 2026 → present
  ROWAN ROSE SOLICITORS — Sole Full-Stack Developer & Cloud Engineer · Remote, UK
```

with 2–3 bullet lines from the resume beneath. Entries: Rowan Rose (Jan 2026–present),
Electrolab Pvt Ltd, Business Analyst (Jul 2024–Apr 2025), CloudWhiz Freelance, Backend & Cloud
(Feb 2023–Jun 2024). Hash strings are decorative — generate stable fake hex, never label them as
real. Entry cards rise in as their node lights up.

### 11. Sheet 04 — Stack

Not tag-soup. A **systems board**: six mono-labeled groups (Backend / Cloud & DevOps / Databases /
Frontend / AI & Automation / Tools & Integrations — items from the current site) laid out as a
responsive grid of hairline plates. Each item is a mono chip; on hover its chip border charges
`--signal` and a one-line mono footnote appears in the plate's caption bar stating *where it was
used in production* (e.g. `LAMBDA → document pipelines @ Rowan Rose`; map honestly from the resume,
reuse entries where needed). One plate carries a small live embellishment: a 12-bar equalizer of
1px `--signal` rules idling on the rAF clock.

### 12. Sheet 05 — Work: case-study plates

Projects as numbered editorial plates, not cards in a grid. Each plate: full-width, hairline-framed,
mono header row (`PLATE 01 — PRODUCTION`), display-serif title, one tight paragraph, mono tech
list, links. Alternate text/visual alignment. The "visual" for each is a **generated schematic**,
not a screenshot: a small inline-SVG diagram per project (boxes + labeled arrows in mono 10px)
drawn in the same trace language — e.g. AgentLens: `AGENT → INTERCEPT → RISK 0-100 → HUMAN
APPROVAL → EXECUTE`. Diagrams draw on when revealed.

Order and content (from the current site — keep links exactly):
1. **AgentLens — AI Agent Safety Platform** · live `https://agent-lens-gamma.vercel.app/` ·
   metrics row: `32 API ROUTES · 20 DB MIGRATIONS · 3 LLM PROVIDERS`
2. **Legal & Sales CRM Platform** (Rowan Rose) · `PRIVATE — IN PRODUCTION` badge ·
   `5 USER ROLES · E-SIGN · VOIP ANALYTICS`
3. **Weird — Luxury Fashion Website** · live `https://weird-nine.vercel.app/`
4. **S.S. Classic Mould & Dies — B2B Manufacturing** · live `https://ss-8cs.pages.dev/`
5. **Payment Processing System (MT940)** · private
6. **Serverless Data Processing on AWS** · `https://github.com/Momoeka`
7. **FoReal — CodeCraft App** · `https://github.com/Momoeka/FoReal`

Give plates 1–2 the full treatment; 3–7 may share a tighter two-column plate format. Every
external link `target="_blank" rel="noopener"`.

### 13. Sheet 06 — The Field (Beyond Code)

The differentiator no other dev portfolio has. A **monochrome chapter** on NCC cadet years and
discipline — treat it like a film insert: background shifts to near-black, all photos
`filter:grayscale(1) contrast(1.05)`, mono captions like contact-sheet frames.

- Chapter intro, display serif: *"Discipline was the first system I built."* One short honest
  paragraph connecting cadet training (parade precision, early mornings, responsibility) to how
  they engineer today. Write it restrained — no invented ranks, units or awards; the images may
  be described only by what is visible.
- A horizontal **film-strip scroller** (scroll-snap, drag-to-scroll on desktop) of 4–5 frames:
  `army00.jpg` (uniform, full parade dress), `army4.jpg` (planche calisthenics — strength frame),
  `army9.png` (the uniform on its hanger — use as the closing, most cinematic frame), plus 1–2
  best of the remaining `army*` images *after* optimization (§16). Each frame: hairline border,
  mono caption (`FRAME 03 — NCC YEARS`), slight scale-settle on entry.
- Keep this section quiet: no signal-green here except one thin rule. Grayscale is the accent.

### 14. Sheet 07 — Credentials · Education · Words

Three compact sub-blocks in one section:
- **Credentials:** two hairline plates — AWS Certified Cloud Practitioner (Jun 2024) with
  `images/AWS Certified Cloud Practitioner.jpg` as a small thumbnail opening a lightbox
  (self-built: dialog element, Esc/backdrop close), and AWS Knowledge: Amazon EKS (Jul 2024).
  `images/KodeKloud.png` as a third plate if it is a legible certificate.
- **Education:** three mono rows — M.Tech CS, RGPV Bhopal (2025–2027) · B.Tech CS, RGPV
  (2022–2025) · Diploma Electronics, Jamia Millia Islamia (2019–2022).
- **Words:** port the existing six testimonials as a quiet auto-advancing quote rotator
  (one visible, cross-fade every 7s, pause on hover, dots hidden — mono `01/06` counter instead).
  Port the quotes verbatim from `index.old.html`.

### 15. Sheet 08 — Contact + Colophon

- Display-serif headline: **"Let's build something that** *lasts***."**
- The trace field reused, rotated 180°, faint, behind.
- A **terminal contact block** instead of a form (no backend exists — a form that silently fails
  is worse than none): a mono panel styled like a shell —
  `$ contact --email` → `mohammadsayed722@gmail.com` (click-to-copy with a `copied ✓` tick),
  `$ contact --phone` → `+91 89290 23900`, plus link rows for GitHub (`github.com/Momoeka`),
  LinkedIn, Resume. Each row hover = `--signal` charge.
- Colophon strip at the very bottom: oversized outline wordmark `FIRDOUSI` bleeding off-canvas at
  8% opacity, and a mono line: `DESIGNED & BUILT BY S.M. FIRDOUSI — HTML/CSS/JS, NO FRAMEWORKS —
  © <year auto>` plus `▸ back to top` (smooth scroll).

### 16. Assets — mandatory preprocessing before wiring anything

The `images/` folder contains multi-megabyte files that would destroy load performance
(`army3.png` 28MB, `army5.png` 26MB, `army6.png` 19MB, `army8.png` 19MB, `army7.png` 11MB,
`1.jpg` 6MB, `army2.JPG` 4MB, `army10.JPG` 3MB). **First step of the build:** create
`images/opt/` and re-encode every image you actually use — max 1600px on the long edge (800px for
thumbnails), JPEG/WebP quality ~80, grayscale conversion baked into the Field images. Use any
locally available tool (`magick`, or a quick Node `sharp` script — installing sharp locally for a
one-off script is fine; the *site* stays dependency-free). Reference only `images/opt/*` from the
page. Add `width`/`height` attributes on every `<img>` (no CLS), `loading="lazy"` on everything
below the fold, `fetchpriority="high"` + `<link rel="preload">` on `first.png` only.

Use: `first.png` (hero) · `image.png` (about) · `army00.jpg`, `army4.jpg`, `army9.png` + 1–2 more
army frames (Field) · AWS cert jpg, `KodeKloud.png` (credentials). Do **not** use:
`developer-pic-1.png` (stock art of someone else), `bg_1.jpg`, `g1.jpg`, `1.jpg`, template
leftovers. `headline2.png` / `coatwa.png` (blazer portraits) are reserves if a section needs a
second portrait — prefer the watercolor pair for consistency.

### 17. Finish — the last 10%

- **Grain:** fixed full-viewport `feTurbulence` data-URI, `fractalNoise`, `baseFrequency .8`,
  `opacity:.06`, `mix-blend-mode:overlay`, stepped keyframe flicker. Subtler than the architect
  site — carbon, not film.
- **Cursor** (`(pointer:fine)` only): native cursor hidden; a 5px `--signal` dot + trailing 28px
  hairline ring lerping at 0.18/frame. Ring expands + tints on links; becomes a mono `VIEW` chip
  over project plates; a `+` reticle over Field frames.
- **Reveals:** one system — `.rise` (opacity 0, translateY 24px → settled, 0.9s
  `cubic-bezier(.22,1,.36,1)`, staggered ≤0.5s) for content; hairlines scale-x in; serif
  headlines per-word clip-wipe. Nothing animates twice.
- **Scramble:** section mono-labels resolve out of character noise on first reveal (30ms ticks) —
  keep it to labels, never body text.
- **Selection:** `::selection { background:var(--signal); color:var(--carbon) }`. Matching
  `--signal` focus-visible outlines. Styled thin scrollbar.
- **Easter egg:** Konami code or triple-click on the wordmark logs a mono ASCII banner +
  `hire me: mohammadsayed722@gmail.com` to the console; also ship a one-line styled
  `console.log` on load.

### 18. Engineering non-negotiables

- **One `requestAnimationFrame` loop** for everything (traces, pulses, marquee, cursor, gain
  decay, equalizer). Scroll/pointer handlers rAF-throttled and `{passive:true}`. Loop pauses on
  `document.hidden`.
- Full **`prefers-reduced-motion`** branch: boot skipped, traces drawn complete with pulses
  frozen, marquee static, cursor native, reveals instant, grain static, `scroll-behavior:auto`.
  The rAF loop must not start at all.
- Responsive at 1200 / 1000 / 760 / 480px. `scrollWidth === clientWidth` at every size — zero
  horizontal overflow. Film-strip and marquee scroll inside their own overflow containers. Test
  at 360px width too.
- Accessibility: semantic landmarks (`header/nav/main/section/footer`), one `h1`, real `alt` text
  on every photo (`aria-hidden` on decoration), skip-to-content link, full keyboard nav, visible
  focus, lightbox focus-trapped.
- **SEO head:** title `Sayed Mohammad Firdousi — Backend Developer & Cloud Engineer`, meta
  description, canonical `https://momoeka.github.io/MyPortfolio/`, Open Graph + Twitter card
  (generate a 1200×630 `images/opt/og.jpg` from the hero composition), JSON-LD `Person` schema
  (name, jobTitle, sameAs: GitHub/LinkedIn, alumniOf), favicon: inline SVG `S` monogram in
  `--signal` on `--carbon`.
- Performance budget: total transfer < 1.2MB excluding lazy Field images; Lighthouse ≥ 95
  Performance / 100 Accessibility / 100 SEO. Fonts preconnected; no other remote requests.
- Never show working-file names (`army4`, `coatwa`) in visible captions; caption in the plate
  language (`FRAME 02`, `FIG. 01`).

### 19. Definition of done

Serve locally (`python -m http.server` or any static server), then verify: boot plays once and
never traps a throttled tab · every section reveals correctly on a full slow scroll · traces
respond to pointer and scroll · zero console errors · zero horizontal overflow at 360/760/1200 ·
reduced-motion pass with DevTools emulation · keyboard-only pass · Lighthouse run meets budget ·
click every link. Screenshot hero, Work, and Field at 1440px and 390px and review them yourself
against §1 before calling it finished: if a section reads "template", redesign it.

---

## Build notes for the implementing session

- Current repo state: `index.html` at the root is the live site (a self-built modern page — its
  copy is the content source of truth alongside `resume.txt`). `css/`, `js/`, `scss/`, `fonts/`,
  `lib/` are leftovers of an old Colorlib template and are **unreferenced by the new build** — do
  not import from them; leave deletion to a separate cleanup commit at the end
  (`git rm -r css js scss fonts lib prepros-6.config shot.cfm changes.patch`).
- Facts discipline: every date, employer, metric and cert above is from `resume.txt`. If copy
  needs expanding, tighten phrasing — never invent numbers, clients, or NCC details.
- The site deploys via GitHub Pages from this repo (`momoeka.github.io/MyPortfolio`) — keep
  everything relative-pathed, no absolute `/` URLs.
- Work sheet-by-sheet in the order of this prompt; get the trace field and type system right
  first — everything else inherits from them.
