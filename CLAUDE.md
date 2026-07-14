# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static HTML prototype for the Yorkshire Cyclo-Cross Association website, built for committee review before porting to the live WordPress site at yorkshirecyclocross.com. No build step, no framework, no server.

## Commands

Validate all HTML pages:
```bash
npx html-validate index.html new-to-cx.html your-first-race.html juniors.html current-season.html results-archive.html rules.html
```

Validate a single page:
```bash
npx html-validate <filename>.html
```

Open the prototype in a browser:
```bash
open index.html
```

## Architecture

**One CSS file, seven HTML files.** All styling lives in `css/styles.css` using CSS custom properties (`--color-primary`, `--space-md`, etc.). Every HTML page is self-contained — nav and footer HTML are copy-pasted across all pages rather than included dynamically.

**Nav active state** is set per-page by adding `class="active"` to the matching `<a>` in the nav. `your-first-race.html` marks `new-to-cx.html` as active (it's a child of that section).

**Accordions** use native `<details>`/`<summary>` elements styled via `.accordion` in the CSS — no JavaScript required. The first `<details>` in an accordion gets the `open` attribute to be expanded by default.

**Mobile nav** is the only JavaScript on each page — a single inline `<script>` at the bottom toggles `.open` on `#main-nav` and updates `aria-expanded` on the hamburger button.

**Placeholder images** use `placehold.co` URLs throughout. The hero background image on `index.html` is set as a CSS background in `styles.css` (`.hero__bg`) rather than an inline style.

**Stub links** — results and entry links use `href="javascript:void(0)"` as placeholders in `current-season.html` and `results-archive.html`.

## Key content to keep accurate

The committee will verify these before any WordPress handoff:
- Entry fees: Adults £12, U18 £6, U16 £5, U12 £4 (in `current-season.html`)
- Licence thresholds: Under 12s need no licence; Under 14+ recommended (in `juniors.html` and `rules.html`)
- Season framing: both Summer Series (May–Aug) and Winter Series (Oct–Feb) exist — do not describe CX as a "winter only" sport
