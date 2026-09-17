# AI in Humanities Research: A Map of the Interpretive Cycle

An open, interactive map of how humanities research actually moves — and where
**human judgment**, **agentic AI** and the **academic library** each carry the load.

One file. No build step. No tracking, no accounts, no backend.

**Live:** _(GitHub Pages not enabled yet — see [Deploying](#deploying))_

It is a sibling to [The Research Lifecycle](https://vladimirbrusov83-hub.github.io/research_lifecycle/),
which maps *scientific* research. This version keeps that page's visual identity but
changes the model underneath it. Scientific research can be drawn as a line: hypothesis,
method, data, result. Humanities research cannot. The question changes when the archive
answers back. So this map is a spiral with arrows running **backward**, and those arrows
are the argument.

---

## Contents

- [What it covers](#what-it-covers)
- [Interactive features](#interactive-features)
- [Tech](#tech)
- [Running it locally](#running-it-locally)
- [Deploying](#deploying)
- [How to update the content](#how-to-update-the-content)
  - [Phases](#phases)
  - [Contested zones](#contested-zones)
  - [Traffic-light thresholds](#traffic-light-thresholds)
  - [The spiral's shape](#the-spirals-shape)
  - [Field guide links](#field-guide-links)
- [Accessibility](#accessibility)
- [Editorial commitments](#editorial-commitments)
- [Known checks](#known-checks)
- [Credits](#credits)

---

## What it covers

Ten phases, in rough order but explicitly **not** in a line:

| # | Phase | Notes |
|---|-------|-------|
| 01 | Research Question & Interpretive Framing | tagged *Revisable* — not a fixed hypothesis |
| 02 | Historiography & Critical Literature Review | contested |
| 03 | Primary Source & Archival Engagement | heaviest library lane on the page |
| 04 | Close Reading & Interpretation | contested · **loops back to 01** |
| 05 | Citation Verification & Historical Contextualization | contested |
| 06 | Writing & Argument Construction | contested · monograph and single-author norms |
| 07 | Reflexivity & Positionality | contested |
| 08 | Scholarly Dialogue & Peer Response | **loops back to 03** |
| 09 | Publication & Dissemination | |
| 10 | Afterlife: Reuse, Teaching & Reinterpretation | **loops back to 01** |

Plus:

- **Four contested zones** — AI-generated interpretive arguments · fabricated quotes and
  citations · authorial voice and originality · positionality and situatedness. Each one
  gives two legitimate scholarly positions rather than a verdict, a traffic-light rating
  for specific common uses, and an "Interrogate the Output" exercise built on a real
  AI-generated sample.
- **Four library-only floors** — archival and special-collections access · primary-source
  discovery and authentication · citation and historiographical expertise · research
  consultation and instruction. Framed as capabilities no model has by construction, not
  as gaps that close with a better model.
- **A field guide** of ten open resources (HathiTrust, DPLA, Europeana, Internet Archive,
  ArchiveGrid, Zotero, Chicago, COPE, ORCID, DOAJ).

## Interactive features

- **The spiral** — ten nodes on a two-turn spiral, with gold arrows for the three backward
  loops. Click or keyboard-activate any node to jump to its card. Hover a node to light up
  the loops attached to it. Below 640px the spiral is replaced by a text statement of the
  loops, because ten nodes collide on a phone.
- **Load-shift curve** — researcher / AI / library effort across all ten phases. Hover for
  detail, click to jump, use the hero chips to isolate a single line.
- **Expandable phase cards** — three lanes each, with a nested "more" layer for decisions,
  tools, and what to ask a librarian.
- **Contested zones** — keyboard-accessible panels, each with a collapsed "what to notice"
  answer to its exercise.
- **Library Lens** — a toggle that dims everything except the institutional layer.
- **AI Disclosure Statement Generator** — four fields in, a formatted 2–3 sentence
  statement out, suitable for a manuscript, thesis, syllabus or grant application.
- **Stress-test this map** — flag which phases and zones don't fit your discipline, add a
  note, and copy the result or open it in email. Nothing is transmitted; selections are
  kept in `localStorage` for that browser only.
- **Deep links** — `#p1`–`#p10` open a phase card; `#zone-interpretation`,
  `#zone-fabrication`, `#zone-voice`, `#zone-positionality` open a contested zone.
- Respects `prefers-reduced-motion`. Print stylesheet included (the spiral and the two
  tools are dropped; every panel prints expanded).

## Tech

A single, dependency-free `index.html` — vanilla HTML, CSS and JavaScript, plus Google
Fonts (Fraunces, Spectral, JetBrains Mono). Roughly 1,500 lines of CSS and 1,300 of JS,
all inline. No framework, no bundler, no package.json, nothing to install.

```
.
├── index.html     everything — markup, styles, content data, behaviour
├── README.md      this file
├── PRODUCT.md     audience, purpose, design principles, anti-references
└── .gitignore
```

## Running it locally

```bash
git clone https://github.com/vladimirbrusov83-hub/Ai_in_humanities.git
cd Ai_in_humanities
open index.html          # macOS; or just double-click the file
```

That's it. There is no server, build or install step. Edit `index.html`, save, refresh.

If you prefer a local server (useful for testing `localStorage` behaviour, which is
per-origin):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploying

Any static host works, because there is nothing to build.

**GitHub Pages** (what the sibling project uses):

1. Settings → Pages
2. Source: *Deploy from a branch*
3. Branch: `main`, folder: `/ (root)` → Save

The page appears at `https://vladimirbrusov83-hub.github.io/Ai_in_humanities/` after a
minute or two. Update the **Live** link at the top of this file once it is up.

**Netlify / Vercel / Cloudflare Pages:** point them at the repo, leave the build command
empty and the publish directory as the repo root.

---

## How to update the content

Everything a reader sees is **data at the top of the `<script>` block** in `index.html`,
under a comment header that repeats these instructions in place. You do not need to touch
the markup or the CSS to change what the page says.

### Phases

Edit the `phases` array. The phase cards, the spiral nodes, the load curve, the map strip
and the stress-test checkboxes all rebuild themselves from it — add or remove an entry and
all five update.

```js
{
  num:"04 / 10",                    // the label on the card; renumber the rest if you insert one
  short:"Close Reading",            // the label beside the spiral node — keep under ~14 characters
  title:"Close Reading & Interpretation",
  tags:["Contested","Deep-dive"],   // also: "Revisable". Tags drive the filter chips.
  load:{r:"full",a:"light",l:"light"},   // "full" | "partial" | "light"
                                         // r = researcher, a = agentic AI, l = library
  lead:"Researcher",                // shown as "Support lead:"
  loopsTo:[1],                      // 1-based phase numbers this phase sends you back to.
                                    // Draws a gold arrow on the spiral AND the card badge.
                                    // [] means no loop.
  researcher:"…",                   // the three lane summaries
  aiToday:"…",
  aiEmerging:"…",
  library:"…",
  verdict:"…",                      // closing line with the "The tension" badge —
                                    // OR use `note:"…"` instead for a plain closing line
  rMore:{decisions:["…"], cost:"…"},
  aMore:{tools:["…"],     watch:"…"},
  lMore:{where:["…"],     ask:"…"}
}
```

Two things worth knowing:

- `loopsTo` is the only place the recursion is defined. The spiral arrows, the card badges,
  the `Recursive` filter chip and the mobile text summary all read from it.
- The `Library-led` filter selects on `load.l === "full"`, not on the `lead` string, so a
  phase led jointly still shows up there.

### Contested zones

Append to the `zones` array. The panel, the two positions, the traffic lights and the
exercise all render automatically.

```js
{
  id:"fabrication",                 // used for the deep link: #zone-fabrication
  phase:"Phase 05 · Citation Verification",
  title:"Fabricated quotes and citations",
  frame:"…",                        // one or two sentences framing the disagreement
  positions:[                       // exactly two, both stated in good faith
    {name:"Manageable with verification", body:"…"},
    {name:"Structurally hazardous",       body:"…"}
  ],
  lights:[                          // 2–4 specific uses, each rated
    {c:"green", use:"…"},
    {c:"amber", use:"…"},
    {c:"red",   use:"…"}
  ],
  exercise:{
    label:"…",                      // what the sample is
    sample:"…",                     // the AI-generated text to interrogate (\n is respected)
    questions:["…","…","…"],        // 2–3 guided questions
    notice:"…"                      // the collapsed answer behind "What to notice"
  }
}
```

### Traffic-light thresholds

The ratings are **editorial, not computed**. The rule used throughout:

| | Meaning |
|---|---|
| 🟢 `green` | Verifiable by the reader, and it does not touch interpretation. |
| 🟡 `amber` | Acceptable with disclosure and independent checking. |
| 🔴 `red` | Substitutes for the scholarly labour itself, or cannot be verified at all. |

To change a rating, edit `c:` in that zone's `lights` array. If you change the *thresholds*
rather than a single rating, update this table and the comment header in `index.html` so
the two don't drift apart.

### The spiral's shape

Constants at the top of `buildSpiral()` in `index.html`:

| Constant | Does |
|---|---|
| `R0`, `R1` | inner and outer radius — the gap between turns |
| `SWEEP` | total degrees travelled (720 = two full turns) |
| `START` | where phase 01 sits, in degrees from 3 o'clock |
| `BOW` | how far each backward arrow bows off its chord; alternating signs fan them apart |

Labels dodge automatically: if a backward arrow arrives from the outward side of a node,
that node's label flips to the inward side so the arrowhead stays legible.

### Field guide links

The `#shelf` section is plain HTML near the bottom of the body. Edit it directly. Each card
is an `<a class="shelf-card">` with a phase tag, a title, a one-line description and the
bare domain.

---

## Accessibility

- Every interactive element — spiral nodes, contested-zone panels, phase accordions, filter
  chips, both tools — is reachable by keyboard and operable with <kbd>Enter</kbd> or
  <kbd>Space</kbd>, with a visible focus ring.
- Body and secondary text meet WCAG AA contrast against the page ground (verified across
  26 text styles).
- `prefers-reduced-motion: reduce` stops the spiral draw-in, the drifting hero field and
  the reveal-on-scroll transitions.
- The spiral carries a descriptive `aria-label`, and each node announces its phase number
  and title.
- `localStorage` is wrapped in `try`/`catch` throughout, so private windows and blocked
  site data degrade quietly instead of throwing.

## Editorial commitments

These are deliberate and worth preserving if you fork or extend the page:

- **AI use is presented as a spectrum**, from *reject* through *bounded and skeptical* and
  *integrated tool* to *AI-first* — never as a binary. Every point on it is treated as a
  defensible position.
- **Contested zones give two positions, not a verdict.** The sibling project's
  `Verdict · a record` badge was deliberately replaced with `The tension`.
- **The fabricated-citation example is fabricated on purpose and labelled twice.** The
  scholar, the article and the journal are all invented, and both were checked against the
  real record before shipping. Do not replace them with a real name — a convincing fake
  quote attributed to a real person, in a library instruction resource, is the exact harm
  this page warns about.
- **Phase 07 is admitted to be a distortion.** Reflexivity is continuous, not a step you
  finish, and the card says so rather than smoothing it over.

## Known checks

The `#shelf` links all point at real, current services, but several sit behind Cloudflare
and return `403` to automated requests. They work in a browser. Worth a manual click
through before sharing the page widely — especially **ArchiveGrid**, which OCLC has
relocated before.

## Credits

Adapted from [The Research Lifecycle](https://github.com/vladimirbrusov83-hub/research_lifecycle),
which maps the scientific research pipeline and supplied this page's visual system.

Type: [Fraunces](https://fonts.google.com/specimen/Fraunces),
[Spectral](https://fonts.google.com/specimen/Spectral),
[JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono).
