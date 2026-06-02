# Ada Lovelace — Portfolio

> A professional portfolio website built with [Quarto](https://quarto.org) and published on GitHub Pages.

![Built with Quarto](https://img.shields.io/badge/Built_with-Quarto-2B3A67)
![Hosted on GitHub Pages](https://img.shields.io/badge/Hosted_on-GitHub_Pages-B56A4B)
![License: MIT](https://img.shields.io/badge/License-MIT-1B1C22)

**Live site:** `https://<username>.github.io/<repo>/`

---

## What this repository is

This repo holds the source for Ada Lovelace's portfolio site. It is intentionally *source-first*: everything you see on the live site is generated from plain-text `.qmd` files and a single stylesheet, so the repository is readable, reviewable, and easy to maintain.

This README has two jobs:

1. **Orient a reviewer.** Anyone landing on the repo can understand the structure, build it locally, and find their way around in a couple of minutes.
2. **Be the style contract.** It documents the design system — *"Poetical Science"* — so that every contributor (seasoned coder or vibe-coder) produces pages that look like they belong to the same site. **If a styling decision isn't covered here, match the spirit of what is, and prefer a native Quarto feature over new CSS.**

---

## Design system: *Poetical Science*

Ada Lovelace called her way of thinking "poetical science" — rigor with imagination. The site's look follows the same idea: an **editorial, archival, precise** aesthetic. Think fine letterpress meeting computational clarity. It is deliberately *not* steampunk, and deliberately *not* the default Quarto Cosmo look.

### Guiding principles

- **Restraint over decoration.** Whitespace, a tight type scale, and hairline rules do the heavy lifting. One confident accent beats five timid ones.
- **Native Quarto first.** Almost everything below is achieved by overriding Sass *variables*, not by writing CSS rules. Custom rules are reserved for a handful of signature touches.
- **Low technical debt is a feature.** Keep all styling in one `theme.scss`. No JavaScript, no extra CSS frameworks, no per-page `<style>` blocks.
- **Cohesion is the goal.** Every page should feel like the same hand made it.

### Color — "Ink & Copper"

A warm paper background, near-black ink, a deep lapis blue for structure, and a single copper accent for emphasis. Define these once at the top of `theme.scss` and reference them everywhere.

**Light mode**

| Role | Token | Hex |
|---|---|---|
| Paper (background) | `$paper` | `#FAF8F3` |
| Ink (body text) | `$ink` | `#1B1C22` |
| Surface (cards) | `$surface` | `#FFFFFF` |
| Lapis (primary / links / structure) | `$lapis` | `#2B3A67` |
| Copper (accent / emphasis / hover) | `$copper` | `#B56A4B` |
| Muted (captions, metadata) | `$muted` | `#6B6A64` |
| Hairline (borders, rules) | `$hairline` | `#E3DED3` |

**Dark mode**

| Role | Hex |
|---|---|
| Background | `#15161C` |
| Surface | `#1E2029` |
| Text | `#ECEAE3` |
| Lapis (lightened for contrast) | `#8FA6D6` |
| Copper | `#D98E63` |
| Muted | `#9A988F` |
| Hairline | `#2C2E38` |

> **Rule:** Copper is for *emphasis only* — links on hover, the H1 underline, a card's reveal border. If copper starts appearing on large surfaces, you've overused it. Lapis carries structural color; copper is the spark.

### Typography

Distinctive but readable. All three faces are free and load cleanly from Google Fonts.

| Use | Typeface | Notes |
|---|---|---|
| Display / headings | **Fraunces** | A soft-modern serif with real character. Use weights 400–600. |
| Body | **Hanken Grotesk** | A humanist sans — warmer and less generic than the usual defaults. |
| Code / labels / "eyebrows" | **IBM Plex Mono** | Doubles as code font and as the uppercase kicker labels. |

**Scale** — base `1.0625rem` (≈17px), major-third ratio (~1.25):

- `h1` ≈ 2.75rem · `h2` ≈ 2rem · `h3` ≈ 1.5rem · `h4` ≈ 1.2rem
- Body line-height **1.65**, measure (`max-width`) ~**68ch** for comfortable reading.
- **Eyebrow labels:** IBM Plex Mono, uppercase, `0.78rem`, `letter-spacing: 0.12em`, copper. Used as a small kicker above a section heading.

> Avoid the generic stack (Arial, Roboto, Inter, system-ui). The pairing above *is* the brand — don't substitute fonts per page.

### Layout & rhythm

- **Slim sticky navbar** on a paper background with a single bottom hairline. No drop shadow.
- **Reading column** capped at ~720–760px; **listings** may use the full content grid.
- **Hairline dividers** (`<hr>`) instead of boxes and heavy shadows.
- **Generous vertical space** between sections — let the page breathe.
- **About / landing** uses the Quarto `trestles` about-template (portrait left, intro right) — it reads like a refined CV header.
- **Projects** use a `grid` listing of cards.

### Signature touches (the memorable bits)

These are the *only* things that should live in your custom CSS rules. Keep them subtle.

1. **Mono eyebrow kickers** above major section headings.
2. **Copper underline** that animates in under links on hover (and a static one under the page H1).
3. **Card hover:** a small lift + a thin copper top-border reveal.
4. **Perforated rule** — an optional `<hr>` styled as a row of dots, a quiet nod to punched cards. Use sparingly as a section break.

---

## How the styling is implemented

### CSS budget (read this before adding styles)

- **~90% of the look comes from `scss:defaults`** (variable overrides). Set a variable before you write a rule.
- **`scss:rules` is capped at ~80 lines.** If you're writing more, you've probably missed a native variable — check the [Quarto Sass variables list](https://quarto.org/docs/output-formats/html-themes.html#sass-variables).
- **One stylesheet only:** `theme.scss`. No `styles.css`, no inline styles, no `<style>` blocks in `.qmd` files.
- **No JavaScript.** If a thing needs JS, reconsider whether the site needs the thing.

### `_quarto.yml` (skeleton)

```yaml
project:
  type: website
  output-dir: docs          # lets GitHub Pages serve from /docs (no Action required)

website:
  title: "Ada Lovelace"
  site-url: "https://<username>.github.io/<repo>/"
  navbar:
    background: light        # paper-colored navbar; fine-tuned in theme.scss
    pinned: true
    left:
      - text: "Work"
        href: projects.qmd
      - text: "About"
        href: about.qmd
    right:
      - icon: github
        href: https://github.com/<username>
      - icon: linkedin
        href: https://www.linkedin.com/in/<handle>
  page-footer:
    border: true
    center: "© 2026 Ada Lovelace · Built with Quarto"

format:
  html:
    theme:
      light: theme.scss      # inherits Bootstrap defaults, then our overrides
      dark: [theme.scss, theme-dark.scss]
    toc: true
    toc-location: right
    code-copy: true
    code-overflow: wrap
    highlight-style: github  # swap for a custom .theme file if desired
    smooth-scroll: true
    link-external-icon: true
    link-external-newwindow: true
```

### `theme.scss` (starting point)

This single file encodes the whole design system. Copy it as-is and adjust values, not structure.

```scss
/*-- scss:defaults --*/

// 1. Brand tokens — defined once, reused below
$paper:     #FAF8F3;
$ink:       #1B1C22;
$surface:   #FFFFFF;
$lapis:     #2B3A67;
$copper:    #B56A4B;
$muted:     #6B6A64;
$hairline:  #E3DED3;

// 2. Fonts (loaded from Google Fonts)
@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400..600&family=Hanken+Grotesk:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap');

$font-family-sans-serif: "Hanken Grotesk", sans-serif;
$font-family-monospace:  "IBM Plex Mono", monospace;
$headings-font-family:   "Fraunces", serif;
$headings-font-weight:   500;

// 3. Map brand tokens onto Bootstrap/Quarto variables
$body-bg:        $paper;
$body-color:     $ink;
$primary:        $lapis;
$link-color:     $lapis;
$link-hover-color: $copper;
$border-color:   $hairline;
$card-bg:        $surface;

// 4. Type scale (major third)
$font-size-root: 17px;
$line-height-base: 1.65;
$h1-font-size: 2.75rem;
$h2-font-size: 2rem;
$h3-font-size: 1.5rem;

/*-- scss:rules --*/

// Reading measure
main { max-width: 760px; }

// Signature 1 — mono eyebrow kicker (add class .eyebrow to a paragraph)
.eyebrow {
  font-family: "IBM Plex Mono", monospace;
  text-transform: uppercase;
  font-size: 0.78rem;
  letter-spacing: 0.12em;
  color: $copper;
  margin-bottom: 0.25rem;
}

// Signature 2 — animated copper underline on links
main a {
  text-decoration: none;
  background-image: linear-gradient($copper, $copper);
  background-size: 0% 1.5px;
  background-position: 0 100%;
  background-repeat: no-repeat;
  transition: background-size 0.25s ease;
}
main a:hover { background-size: 100% 1.5px; }

// Static underline under the page H1
h1.title { border-bottom: 2px solid $copper; padding-bottom: 0.4rem; }

// Signature 3 — card hover for grid listings
.card {
  border: 1px solid $hairline;
  border-top: 3px solid transparent;
  transition: transform 0.2s ease, border-top-color 0.2s ease;
}
.card:hover {
  transform: translateY(-3px);
  border-top-color: $copper;
}

// Signature 4 — perforated rule (punched-card nod). Use <hr class="punch">
hr.punch {
  border: none;
  height: 6px;
  background-image: radial-gradient($hairline 1.5px, transparent 1.6px);
  background-size: 14px 6px;
}
```

`theme-dark.scss` only needs to re-declare the tokens that change for dark mode (background, ink, lapis, copper, hairline) — everything else inherits.

---

## Repository structure

```
.
├── _quarto.yml            # site config: navbar, theme, formats
├── index.qmd              # landing page — the front door
├── about.qmd              # longer bio, portrait image
├── resume.qmd             # full resume
├── projects.qmd           # grid listing of all work
├── theme.scss             # stylesheet — light mode + signature rules
├── theme-dark.scss        # dark-mode token overrides only
├── assets/                # portrait, project images (with alt text!)
├── docs/                  # rendered site (served by GitHub Pages) — committed
├── .gitignore
└── README.md              # you are here
```

---

## Quick start (local development)

1. **Install Quarto** — download from [quarto.org/docs/get-started](https://quarto.org/docs/get-started/). Verify:
   ```bash
   quarto --version
   ```
2. **Clone and preview** — live-reload as you edit:
   ```bash
   git clone https://github.com/<username>/<repo>.git
   cd <repo>
   quarto preview
   ```
3. **Render the full site** to `docs/`:
   ```bash
   quarto render
   ```

A browser tab opens automatically with `quarto preview`; edits to any `.qmd` or to `theme.scss` reload instantly.

---

## Adding a project

Create a new file in `projects/`, e.g. `projects/my-project.qmd`, with this front matter. The fields are what the grid listing displays — keep them filled in.

```yaml
---
title: "Project title"
description: "One sentence. What it is and why it mattered."
date: 2026-05-01
image: ../assets/img/my-project.jpg
image-alt: "Describe the image for screen readers — required."
categories: [analysis, writing]   # used as filters / tags
---
```

**Content conventions**

- Open with an eyebrow + a short framing paragraph, then detail.
  ```markdown
  [SELECTED WORK]{.eyebrow}

  ## Notes on the Analytical Engine
  ```
- Keep prose tight. The reading column is narrow on purpose.
- **Every image needs `image-alt` / alt text.** No exceptions.
- Use the `punch` rule (`<hr class="punch">`) sparingly between major sections — not on every page.
- Don't introduce new colors or fonts. The palette and the three faces are the whole kit.

---

## Build & deployment (GitHub Pages)

Two supported paths — pick one and stick with it.

### Option A — serve from `/docs` (simplest, no CI)

1. Confirm `output-dir: docs` in `_quarto.yml`.
2. `quarto render`, then commit the `docs/` folder.
3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/docs`.

The site goes live at `https://<username>.github.io/<repo>/`.

### Option B — render on push (GitHub Action)

Keeps `docs/` out of your commits; CI renders and deploys. Add `.github/workflows/publish.yml`:

```yaml
on:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: quarto-dev/quarto-actions/setup@v2
      - uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Then set **Settings → Pages → Source: Deploy from a branch → `gh-pages`**.

---

## Style quick-reference (do / don't)

| Do | Don't |
|---|---|
| Override a Sass **variable** in `scss:defaults` | Write a CSS rule when a variable exists |
| Keep all styling in `theme.scss` | Add `styles.css`, inline styles, or `<style>` blocks |
| Use copper for emphasis only | Fill large areas with copper |
| Use the eyebrow + heading pattern | Invent new section-header styles |
| Reuse the three brand fonts | Swap in Arial / Roboto / Inter / system fonts |
| Add alt text to every image | Ship images with no `image-alt` |
| Let whitespace and hairlines structure the page | Reach for boxes, heavy shadows, or borders everywhere |
| Prefer a native Quarto feature (about, listing, callouts, columns) | Hand-roll components that Quarto already ships |

---

## Accessibility & performance notes

- Maintain readable contrast — the Ink/Paper and lightened dark-mode tokens are chosen with this in mind; check any new pairing.
- Don't remove focus outlines.
- Images: provide `image-alt`, and keep file sizes reasonable.
- No client-side JavaScript means fast loads and graceful degradation by default — keep it that way.

---

## License

Content © 2026 Ada Lovelace. Code released under the [MIT License](LICENSE).
