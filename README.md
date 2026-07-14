# YCCA Website Prototype

A 7-page static HTML prototype for the Yorkshire Cyclo-Cross Association website, built for committee review. No server, build step, or framework required.

## Viewing the prototype

Open any HTML file directly in your browser:

```bash
open index.html
```

Or double-click `index.html` in Finder. All pages link to each other via the navigation bar.

## Pages

| File | Page |
|---|---|
| `index.html` | Homepage |
| `new-to-cx.html` | New to Cyclo-Cross? |
| `your-first-race.html` | Your First Race — practical guide |
| `juniors.html` | Junior & Youth Riders |
| `current-season.html` | Current Season (race schedule) |
| `results-archive.html` | Results Archive |
| `rules.html` | Rules & Regulations |

## Validating HTML

Requires [Node.js](https://nodejs.org). Install dependencies once:

```bash
npm install
```

Then validate all pages:

```bash
npx html-validate index.html new-to-cx.html your-first-race.html juniors.html current-season.html results-archive.html rules.html
```

## Placeholder content to replace before going live

- **Photos** — all images use [placehold.co](https://placehold.co) placeholders. Replace with real club race photos.
- **Entry fees** — verify current prices with the committee before publishing.
- **Race venues and dates** — update `current-season.html` with confirmed 2026 fixtures.
- **Results links** — `href="javascript:void(0)"` placeholders throughout `current-season.html` and `results-archive.html`. Link to real MyRaceResults pages.
- **Contact email** — confirm `info@yorkshirecyclocross.com` is monitored.
- **Social media URLs** — confirm Facebook group and Instagram account URLs are correct.
- **Coaching sessions** — `juniors.html` directs users to Facebook for current session details. Add specifics once confirmed.

## Porting to WordPress

Each page maps to a WordPress page. Copy the main content (between `<div class="page-header">` and `<footer>`) into the WordPress page editor, and apply the CSS from `css/styles.css` as a custom stylesheet or child theme addition.

## Project structure

```
website/
├── index.html
├── new-to-cx.html
├── your-first-race.html
├── juniors.html
├── current-season.html
├── results-archive.html
├── rules.html
├── css/
│   └── styles.css          # Shared design system
└── docs/
    └── superpowers/
        ├── specs/           # Design spec
        └── plans/           # Implementation plan
```
