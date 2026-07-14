# YCCA Website Prototype — Implementation Plan

> **Status: COMPLETE** — All 9 tasks and 37 steps delivered. Prototype live at https://adampridmore.github.io/yorkshire-cx-website-prototype/index.html

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 7-page static HTML prototype of the improved YCCA website, ready for committee review.

**Architecture:** Plain HTML5 files sharing one CSS stylesheet. Nav and footer HTML are copy-pasted across pages (no build step). Images use placehold.co URLs as stand-ins for real race photos. One small inline script per page handles the mobile nav toggle.

**Tech Stack:** HTML5, CSS3 (custom properties, flexbox, grid), vanilla JS (mobile nav only), html-validate for HTML correctness checking.

## Global Constraints

- No CSS frameworks — one hand-written `css/styles.css`
- All internal links: relative (`href="page.html"`)
- Colour palette: primary `#1a3a5c`, accent `#e8a020`, bg `#ffffff`, text `#2d2d2d`
- Font: system font stack — no external font CDN
- Images: `https://placehold.co/WIDTHxHEIGHT/BGCOLOR/FGCOLOR?text=Label`
- All pages must pass `npx html-validate <file>` with zero errors
- Mobile-first; test at 375px, 768px, 1200px

---

## File Structure

```
website/
├── index.html               # Homepage — dual audience
├── new-to-cx.html           # What is CX & why try it
├── your-first-race.html     # Practical beginner guide
├── juniors.html             # Youth/junior info for parents & kids
├── current-season.html      # Race dates, venues, sign-up
├── results-archive.html     # Historical results by year
├── rules.html               # Technical rules & licensing
└── css/
    └── styles.css           # Full design system — shared by all pages
```

---

### Task 1: Project scaffold & CSS design system

**Files:**
- Create: `css/styles.css`

- [x] **Step 1: Check Node/npm is available**

```bash
node --version && npm --version
```

Expected: version numbers printed. If not installed, download from nodejs.org.

- [x] **Step 2: Install html-validate**

```bash
npm init -y && npm install --save-dev html-validate
```

Expected: `package.json` created, `node_modules/` populated.

- [x] **Step 3: Create `.gitignore`**

Create `.gitignore`:
```
node_modules/
package-lock.json
```

- [x] **Step 4: Create `css/styles.css`**

```css
:root {
  --color-primary:      #1a3a5c;
  --color-primary-dark: #0f2236;
  --color-accent:       #e8a020;
  --color-accent-dark:  #c8871a;
  --color-text:         #2d2d2d;
  --color-text-light:   #5a5a5a;
  --color-bg:           #ffffff;
  --color-bg-alt:       #f5f7fa;
  --color-border:       #dde2e8;
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, sans-serif;
  --space-xs:  0.5rem;
  --space-sm:  1rem;
  --space-md:  1.5rem;
  --space-lg:  2.5rem;
  --space-xl:  4rem;
  --radius:    6px;
  --shadow:    0 2px 8px rgba(0,0,0,0.10);
  --max-width: 1100px;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { font-size: 16px; scroll-behavior: smooth; }
body { font-family: var(--font-family); color: var(--color-text); background: var(--color-bg); line-height: 1.6; }
img { max-width: 100%; height: auto; display: block; }
a { color: var(--color-primary); text-decoration: none; }
a:hover { text-decoration: underline; }
ul { list-style: none; }

.container { width: 100%; max-width: var(--max-width); margin: 0 auto; padding: 0 var(--space-sm); }
.section { padding: var(--space-xl) 0; }
.section--alt { background: var(--color-bg-alt); }

h1, h2, h3, h4 { color: var(--color-primary); line-height: 1.2; margin-bottom: var(--space-sm); }
h1 { font-size: clamp(1.8rem, 4vw, 2.8rem); }
h2 { font-size: clamp(1.4rem, 3vw, 2rem); }
h3 { font-size: clamp(1.1rem, 2.5vw, 1.4rem); }
p { margin-bottom: var(--space-sm); }
p:last-child { margin-bottom: 0; }
.lead { font-size: 1.15rem; color: var(--color-text-light); margin-bottom: var(--space-md); }

.btn {
  display: inline-block;
  padding: 0.75rem 1.75rem;
  border-radius: var(--radius);
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
  text-decoration: none;
  border: 2px solid transparent;
}
.btn:hover { text-decoration: none; transform: translateY(-1px); }
.btn--primary { background: var(--color-accent); color: var(--color-primary-dark); }
.btn--primary:hover { background: var(--color-accent-dark); }
.btn--outline { background: transparent; color: var(--color-primary); border-color: var(--color-primary); }
.btn--outline:hover { background: var(--color-primary); color: white; }
.btn--white { background: white; color: var(--color-primary); }
.btn--white:hover { background: var(--color-bg-alt); }

.site-header { background: var(--color-primary); position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
.site-header .container { display: flex; align-items: center; justify-content: space-between; padding-top: 0.75rem; padding-bottom: 0.75rem; }
.site-logo { color: white; font-size: 1.15rem; font-weight: 700; text-decoration: none; line-height: 1.1; }
.site-logo span { display: block; font-size: 0.7rem; font-weight: 400; letter-spacing: 0.1em; text-transform: uppercase; color: var(--color-accent); }
.site-nav ul { display: flex; gap: 0.25rem; align-items: center; }
.site-nav a { color: rgba(255,255,255,0.85); font-size: 0.9rem; font-weight: 500; padding: 0.4rem 0.65rem; border-radius: var(--radius); transition: color 0.2s, background 0.2s; }
.site-nav a:hover, .site-nav a.active { color: white; background: rgba(255,255,255,0.1); text-decoration: none; }
.site-nav .nav-cta { background: var(--color-accent); color: var(--color-primary-dark); font-weight: 700; padding: 0.4rem 1rem; }
.site-nav .nav-cta:hover { background: var(--color-accent-dark); }
.nav-toggle { display: none; background: none; border: 2px solid rgba(255,255,255,0.4); border-radius: var(--radius); color: white; font-size: 1.2rem; cursor: pointer; padding: 0.3rem 0.6rem; line-height: 1; }

@media (max-width: 768px) {
  .nav-toggle { display: block; }
  .site-nav { display: none; position: absolute; top: 100%; left: 0; right: 0; background: var(--color-primary-dark); padding: var(--space-sm); box-shadow: 0 4px 8px rgba(0,0,0,0.2); }
  .site-nav.open { display: block; }
  .site-nav ul { flex-direction: column; align-items: stretch; gap: 0; }
  .site-nav a { display: block; padding: 0.75rem var(--space-sm); border-radius: 0; border-bottom: 1px solid rgba(255,255,255,0.08); }
}

.hero { background: var(--color-primary); color: white; position: relative; overflow: hidden; min-height: 440px; display: flex; align-items: center; }
.hero__bg { position: absolute; inset: 0; background-size: cover; background-position: center; opacity: 0.3; }
.hero__content { position: relative; z-index: 1; max-width: 620px; padding: var(--space-xl) 0; }
.hero__label { display: inline-block; background: var(--color-accent); color: var(--color-primary-dark); font-size: 0.7rem; font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; padding: 0.3rem 0.75rem; border-radius: var(--radius); margin-bottom: var(--space-sm); }
.hero h1 { color: white; }
.hero p { color: rgba(255,255,255,0.85); font-size: 1.1rem; margin-bottom: var(--space-md); }
.hero__actions { display: flex; gap: var(--space-sm); flex-wrap: wrap; }

@media (max-width: 600px) {
  .hero { min-height: 340px; }
  .hero__actions { flex-direction: column; }
  .hero__actions .btn { text-align: center; }
}

.page-header { background: var(--color-primary); color: white; padding: var(--space-lg) 0; }
.page-header h1 { color: white; margin-bottom: var(--space-xs); }
.page-header p { color: rgba(255,255,255,0.8); font-size: 1.05rem; margin: 0; }

.section-header { text-align: center; max-width: 640px; margin: 0 auto var(--space-lg); }
.section-header p { color: var(--color-text-light); font-size: 1.05rem; margin: 0; }

.card-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: var(--space-md); }
.card { background: white; border: 1px solid var(--color-border); border-radius: var(--radius); overflow: hidden; box-shadow: var(--shadow); transition: transform 0.2s, box-shadow 0.2s; display: flex; flex-direction: column; }
.card:hover { transform: translateY(-3px); box-shadow: 0 6px 20px rgba(0,0,0,0.12); }
.card__image { width: 100%; height: 200px; object-fit: cover; }
.card__body { padding: var(--space-md); flex: 1; display: flex; flex-direction: column; }
.card__body h3 { margin-bottom: var(--space-xs); }
.card__body p { color: var(--color-text-light); font-size: 0.95rem; flex: 1; }
.card__body .btn { margin-top: var(--space-sm); align-self: flex-start; }

.info-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: var(--space-md); }
.info-item { display: flex; gap: var(--space-sm); align-items: flex-start; }
.info-item__icon { font-size: 1.8rem; flex-shrink: 0; width: 2.5rem; text-align: center; }
.info-item__text h3 { font-size: 1rem; margin-bottom: 0.25rem; }
.info-item__text p { font-size: 0.9rem; color: var(--color-text-light); margin: 0; }

.accordion { border: 1px solid var(--color-border); border-radius: var(--radius); overflow: hidden; }
.accordion details { border-bottom: 1px solid var(--color-border); }
.accordion details:last-child { border-bottom: none; }
.accordion details summary { padding: var(--space-sm) var(--space-md); font-weight: 600; color: var(--color-primary); cursor: pointer; list-style: none; display: flex; justify-content: space-between; align-items: center; transition: background 0.15s; }
.accordion details summary::-webkit-details-marker { display: none; }
.accordion details summary::after { content: '+'; font-size: 1.3rem; font-weight: 300; }
.accordion details[open] summary::after { content: '−'; }
.accordion details summary:hover { background: var(--color-bg-alt); }
.accordion details > p, .accordion details > div { padding: 0 var(--space-md) var(--space-sm); color: var(--color-text-light); }

.step-list { counter-reset: steps; margin-top: var(--space-md); }
.step-list li { counter-increment: steps; display: flex; gap: var(--space-md); align-items: flex-start; padding: var(--space-md) 0; border-bottom: 1px solid var(--color-border); }
.step-list li:last-child { border-bottom: none; }
.step-list li::before { content: counter(steps); background: var(--color-primary); color: white; font-weight: 700; width: 2rem; height: 2rem; border-radius: 50%; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.step-list h3 { font-size: 1rem; margin-bottom: 0.25rem; }
.step-list p { font-size: 0.9rem; color: var(--color-text-light); margin: 0; }

.data-table { width: 100%; border-collapse: collapse; font-size: 0.95rem; margin-top: var(--space-md); }
.data-table th { background: var(--color-primary); color: white; text-align: left; padding: 0.75rem var(--space-sm); font-weight: 600; }
.data-table td { padding: 0.65rem var(--space-sm); border-bottom: 1px solid var(--color-border); }
.data-table tr:last-child td { border-bottom: none; }
.data-table tr:nth-child(even) td { background: var(--color-bg-alt); }
.data-table a { color: var(--color-accent-dark); font-weight: 500; }

.badge { display: inline-block; background: var(--color-accent); color: var(--color-primary-dark); font-size: 0.7rem; font-weight: 700; padding: 0.2rem 0.55rem; border-radius: 20px; text-transform: uppercase; letter-spacing: 0.06em; }
.badge--green { background: #d1fae5; color: #065f46; }
.badge--grey { background: var(--color-bg-alt); color: var(--color-text-light); }

.cta-banner { background: var(--color-primary); text-align: center; padding: var(--space-xl) 0; }
.cta-banner h2 { color: white; }
.cta-banner p { color: rgba(255,255,255,0.8); margin-bottom: var(--space-md); }
.cta-banner .btn + .btn { margin-left: var(--space-xs); }

.site-footer { background: var(--color-primary-dark); color: rgba(255,255,255,0.65); padding: var(--space-lg) 0 var(--space-md); }
.footer__grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: var(--space-lg); margin-bottom: var(--space-lg); }
.footer__col h4 { color: white; font-size: 0.78rem; letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: var(--space-sm); }
.footer__col ul { display: flex; flex-direction: column; gap: 0.5rem; }
.footer__col a { color: rgba(255,255,255,0.65); font-size: 0.9rem; transition: color 0.2s; }
.footer__col a:hover { color: white; text-decoration: none; }
.footer__bottom { border-top: 1px solid rgba(255,255,255,0.1); padding-top: var(--space-sm); font-size: 0.82rem; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: var(--space-xs); }

.mt-sm { margin-top: var(--space-sm); }
.mt-md { margin-top: var(--space-md); }
.mt-lg { margin-top: var(--space-lg); }
.text-center { text-align: center; }
```

- [x] **Step 5: Commit**

```bash
git add css/styles.css .gitignore package.json
git commit -m "feat: add CSS design system and project scaffold"
```

---

### Task 2: Homepage (`index.html`)

**Files:**
- Create: `index.html`

- [x] **Step 1: Create `index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Yorkshire Cyclo-Cross Association</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <section class="hero">
    <div class="hero__bg" style="background-image:url('https://placehold.co/1400x500/1a3a5c/336699?text=Race+Action+Photo');"></div>
    <div class="container">
      <div class="hero__content">
        <span class="hero__label">Yorkshire&#39;s Home of Cyclo-Cross Racing</span>
        <h1>Mud, Speed &amp; Community Racing</h1>
        <p>One-hour races across Yorkshire every Sunday &#8212; for every age and ability, from complete beginners to seasoned racers.</p>
        <div class="hero__actions">
          <a href="new-to-cx.html" class="btn btn--primary">New to cyclo-cross?</a>
          <a href="current-season.html" class="btn btn--white">This season&#39;s races</a>
        </div>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="section-header">
        <h2>Why cyclo-cross?</h2>
        <p>A sport that welcomes everyone &#8212; whatever your bike, your fitness, or your experience.</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <div class="info-item__icon">&#9201;</div>
          <div class="info-item__text">
            <h3>Under an hour</h3>
            <p>Short, sharp races that fit into a Sunday morning. No all-day commitment needed.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128101;</div>
          <div class="info-item__text">
            <h3>All ages, all abilities</h3>
            <p>Under 8s to veterans &#8212; age-graded categories mean you always have people to race.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128690;</div>
          <div class="info-item__text">
            <h3>Any bike</h3>
            <p>Road bike, mountain bike or dedicated CX machine &#8212; start with what you have.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#127881;</div>
          <div class="info-item__text">
            <h3>Real race atmosphere</h3>
            <p>Mass starts, crowd support, proper results. It feels like proper sport from race one.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <div class="section-header">
        <h2>Where do you want to start?</h2>
      </div>
      <div class="card-grid">
        <div class="card">
          <img src="https://placehold.co/600x300/2d5a8e/ffffff?text=Adult+Racing" alt="Adult cyclocross racing" class="card__image">
          <div class="card__body">
            <h3>New to cyclo-cross?</h3>
            <p>Never raced before? We walk you through what to bring, what to expect, and how to enter your first event.</p>
            <a href="new-to-cx.html" class="btn btn--primary">Get started</a>
          </div>
        </div>
        <div class="card">
          <img src="https://placehold.co/600x300/1a3a5c/e8a020?text=Junior+Racing" alt="Junior cyclocross racing" class="card__image">
          <div class="card__body">
            <h3>Junior &amp; youth riders</h3>
            <p>Under 16? We have dedicated youth categories and a junior racing programme across Yorkshire.</p>
            <a href="juniors.html" class="btn btn--primary">Junior info</a>
          </div>
        </div>
        <div class="card">
          <img src="https://placehold.co/600x300/0f2236/ffffff?text=Race+Venue" alt="A cyclocross race venue" class="card__image">
          <div class="card__body">
            <h3>Current season</h3>
            <p>Already a member? Jump to this season&#39;s race dates, venues, sign-up, and results.</p>
            <a href="current-season.html" class="btn btn--primary">This season</a>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="cta-banner">
    <div class="container">
      <h2>Ready to give it a go?</h2>
      <p>Entries are open online. Sign up in advance for any 2026 Summer Series round.</p>
      <a href="current-season.html" class="btn btn--primary">Enter a race</a>
      <a href="new-to-cx.html" class="btn btn--white">Find out more first</a>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open index.html
```

Check: hero with two CTA buttons, four info items, three cards, CTA banner, four-column footer. At 375px the hamburger button appears and opens the nav.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate index.html
```

Expected: `index.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add homepage with dual-audience hero and card grid"
```

---

### Task 3: New to CX (`new-to-cx.html`)

**Files:**
- Create: `new-to-cx.html`

- [x] **Step 1: Create `new-to-cx.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>New to Cyclo-Cross &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html" class="active">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>New to Cyclo-Cross?</h1>
      <p>Everything you need to know about getting started with Yorkshire&#39;s favourite winter sport.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <h2>What is cyclo-cross?</h2>
      <p class="lead">Cyclo-cross is off-road bike racing on short circuits &#8212; parks, fields, and woodland &#8212; that mix grass, mud, sand, and small obstacles. Races last under an hour. There is no long-distance suffering &#8212; just intense, exciting racing with a great community around it.</p>
      <img src="https://placehold.co/1100x420/2d5a8e/ffffff?text=Cyclocross+Race+Action+Photo" alt="Cyclocross race in action" style="border-radius:6px; margin-top:1.5rem;">
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <div class="section-header">
        <h2>Why you&#39;ll love it</h2>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <div class="info-item__icon">&#9201;</div>
          <div class="info-item__text">
            <h3>Short, sharp races</h3>
            <p>Under an hour. You&#39;ll be home for lunch and wondering when the next race is.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#127774;</div>
          <div class="info-item__text">
            <h3>Winter fun</h3>
            <p>The season runs October through February &#8212; something brilliant to train for all winter.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128101;</div>
          <div class="info-item__text">
            <h3>Race at your own level</h3>
            <p>Age-graded categories from Under 8 to Veteran 60+. You&#39;re always racing people at a similar level.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128690;</div>
          <div class="info-item__text">
            <h3>Any bike will do</h3>
            <p>Road, mountain, gravel or CX &#8212; beginners regularly turn up on whatever they have and have a great time.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#127881;</div>
          <div class="info-item__text">
            <h3>Real race atmosphere</h3>
            <p>Mass starts, a hooter, a crowd, and your name called at the finish. It feels like proper sport.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128578;</div>
          <div class="info-item__text">
            <h3>Genuinely welcoming</h3>
            <p>The YCCA has been running races for decades. Everyone remembers their first race &#8212; you&#39;ll be looked after.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <div class="section-header">
        <h2>Where do you want to go next?</h2>
      </div>
      <div class="card-grid">
        <div class="card">
          <img src="https://placehold.co/600x300/1a3a5c/ffffff?text=Your+First+Race" alt="First race guide" class="card__image">
          <div class="card__body">
            <h3>Your first race &#8212; a practical guide</h3>
            <p>What to wear, what to bring, how to sign up, and exactly what happens on the day.</p>
            <a href="your-first-race.html" class="btn btn--primary">Read the guide</a>
          </div>
        </div>
        <div class="card">
          <img src="https://placehold.co/600x300/0f2236/e8a020?text=Junior+Riders" alt="Junior and youth cycling" class="card__image">
          <div class="card__body">
            <h3>Bringing a young rider?</h3>
            <p>We have dedicated junior categories from Under 8 upwards. Find out what to expect for young riders.</p>
            <a href="juniors.html" class="btn btn--primary">Junior &amp; youth info</a>
          </div>
        </div>
      </div>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open new-to-cx.html
```

Check: "New to CX?" highlighted in nav, wide action photo, 6-item info grid, two cards at bottom.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate new-to-cx.html
```

Expected: `new-to-cx.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add new-to-cx.html
git commit -m "feat: add New to CX introductory page"
```

---

### Task 4: Your First Race (`your-first-race.html`)

**Files:**
- Create: `your-first-race.html`

- [x] **Step 1: Create `your-first-race.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your First Race &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>Your First Race &#8212; A Practical Guide</h1>
      <p>Everything you need to know before race day, so you can focus on enjoying it.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <h2>Entering a race</h2>
      <p>All YCCA races require <strong>online pre-registration</strong> &#8212; you cannot enter on the day. Entry typically costs <strong>&#163;10&#8211;&#163;15 for adults</strong> and <strong>&#163;5&#8211;&#163;8 for juniors</strong>.</p>
      <p>Sign up via the <a href="current-season.html">Current Season</a> page. Entries usually open 2&#8211;3 weeks before each race and close the evening before.</p>
      <p>You do not need a racing licence for the Novice category &#8212; just enter and turn up.</p>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>What to bring</h2>
      <div class="info-grid mt-md">
        <div class="info-item">
          <div class="info-item__icon">&#128690;</div>
          <div class="info-item__text">
            <h3>Your bike</h3>
            <p>Any bike is fine for a first race. Check the brakes work and tyres are inflated.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#9940;</div>
          <div class="info-item__text">
            <h3>A helmet</h3>
            <p>Compulsory. Must be worn at all times on the course, including warm-up laps.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128084;</div>
          <div class="info-item__text">
            <h3>Cycling kit</h3>
            <p>Padded shorts help. Wear layers you don&#39;t mind getting muddy &#8212; you will get muddy.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128167;</div>
          <div class="info-item__text">
            <h3>Water &amp; a snack</h3>
            <p>The race is under an hour, but have something for after.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128716;</div>
          <div class="info-item__text">
            <h3>Cash for parking</h3>
            <p>Many venues charge &#163;2&#8211;&#163;3 for parking. Check the event page for details.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128139;</div>
          <div class="info-item__text">
            <h3>Spare dry kit</h3>
            <p>Leave a full change of clothes in the car. You&#39;ll want them after.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <h2>What happens on the day</h2>
      <ol class="step-list mt-md">
        <li>
          <div>
            <h3>Arrive &amp; sign on</h3>
            <p>Sign-on opens 90 minutes before your race. Find the sign-on tent, give your name, collect your race number and timing chip. Pin the number to your jersey or shorts.</p>
          </div>
        </li>
        <li>
          <div>
            <h3>Walk or ride the course</h3>
            <p>The course is open for pre-ride before the race starts. Walk or ride it slowly &#8212; look at the obstacles, the off-camber sections, where to dismount. One walk-through is enough.</p>
          </div>
        </li>
        <li>
          <div>
            <h3>Warm up &amp; get to the start</h3>
            <p>A 10&#8211;15 minute gentle warm-up ride will help. Get to the start area 5 minutes before your race time. Novices typically start from the back &#8212; that is fine, it keeps the start safe.</p>
          </div>
        </li>
        <li>
          <div>
            <h3>Race</h3>
            <p>The hooter sounds and you&#39;re off. Race for as many laps as the time allows. You will likely be lapped &#8212; pull slightly right when a faster rider comes through, then carry on. Marshals will pull you when your time is up.</p>
          </div>
        </li>
        <li>
          <div>
            <h3>Finish &amp; results</h3>
            <p>Ride through the finish area. Results are usually published within an hour. Return your timing chip at the sign-on tent if required.</p>
          </div>
        </li>
        <li>
          <div>
            <h3>Wash your bike</h3>
            <p>Most venues have a jet washer (bring &#163;1&#8211;&#163;2 coins). Wash your bike before leaving &#8212; it is a condition of entry and keeps the venues available to us.</p>
          </div>
        </li>
      </ol>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>Common questions</h2>
      <div class="accordion mt-md">
        <details>
          <summary>I&#39;m not very fit &#8212; will I be able to finish?</summary>
          <p>Yes. Cyclo-cross categories are time-based, not lap-based. You race until the time runs out, whatever pace that is. Novice and beginner categories are specifically for people new to the sport. Finishing a race feels great at any pace.</p>
        </details>
        <details>
          <summary>What if I get lapped?</summary>
          <p>Getting lapped is completely normal in your first few races. Experienced riders will call out as they approach. Pull slightly to the right, let them pass, and carry on. There is no shame in it &#8212; everyone starts here.</p>
        </details>
        <details>
          <summary>Do I need to dismount for the obstacles?</summary>
          <p>The course includes barriers you generally need to dismount and carry your bike over. Most courses also have steep run-ups where carrying is faster than riding. Marshals will be nearby. Watch what others do on your course walk-through.</p>
        </details>
        <details>
          <summary>What if it&#39;s really muddy?</summary>
          <p>Mud is a feature, not a bug &#8212; cyclo-cross was invented for exactly this. Lower your tyre pressure slightly (around 30&#8211;35 psi for a road bike, less for wider tyres). Ride through mud rather than around it. Bring a towel.</p>
        </details>
        <details>
          <summary>Do I need a racing licence?</summary>
          <p>No &#8212; not for the Novice or Newcomer categories. You can enter and race without a British Cycling licence. If you continue racing and want to ride at higher categories, a day licence (around &#163;5) or annual licence becomes useful.</p>
        </details>
      </div>
    </div>
  </section>

  <section class="cta-banner">
    <div class="container">
      <h2>Ready? Entries are open.</h2>
      <p>Sign up online for any round of the 2026 Summer Series.</p>
      <a href="current-season.html" class="btn btn--primary">See this season&#39;s races</a>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open your-first-race.html
```

Check: entry cost section, 6-item what-to-bring grid, numbered step list (1–6), accordion FAQ opens/closes, CTA banner.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate your-first-race.html
```

Expected: `your-first-race.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add your-first-race.html
git commit -m "feat: add Your First Race practical guide with accordion FAQ"
```

---

### Task 5: Juniors & Youth (`juniors.html`)

**Files:**
- Create: `juniors.html`

- [x] **Step 1: Create `juniors.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Junior &amp; Youth Riders &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html" class="active">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>Junior &amp; Youth Riders</h1>
      <p>Cyclo-cross is one of the best sports for young riders &#8212; short, safe, exciting, and great fun in all weathers.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <img src="https://placehold.co/1100x400/1a3a5c/e8a020?text=Junior+Race+Photo" alt="Junior cyclocross racing in Yorkshire" style="border-radius:6px; margin-bottom:var(--space-lg);">
      <h2>Why cyclo-cross for young riders?</h2>
      <div class="info-grid mt-md">
        <div class="info-item">
          <div class="info-item__icon">&#127919;</div>
          <div class="info-item__text">
            <h3>Skill development</h3>
            <p>Balancing, dismounting, remounting, carrying a bike &#8212; CX builds bike-handling skills no road race can.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#9201;</div>
          <div class="info-item__text">
            <h3>Short races</h3>
            <p>Junior races are 20&#8211;45 minutes depending on age. Exciting enough to engage young riders, short enough to leave them wanting more.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128101;</div>
          <div class="info-item__text">
            <h3>Age-matched competition</h3>
            <p>Separate categories for every age group mean young riders race against peers, not adults.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#127774;</div>
          <div class="info-item__text">
            <h3>All-weather adventure</h3>
            <p>Mud, grass and obstacles make CX genuinely exciting for kids who love being outdoors.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>Age categories</h2>
      <p>Junior races are organised by age as of 1 January of the current year.</p>
      <table class="data-table mt-md">
        <thead>
          <tr>
            <th>Category</th>
            <th>Age</th>
            <th>Typical race duration</th>
            <th>Licence required?</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>Under 8</td><td>6&#8211;7</td><td>10&#8211;15 min</td><td>No</td></tr>
          <tr><td>Under 10</td><td>8&#8211;9</td><td>15&#8211;20 min</td><td>No</td></tr>
          <tr><td>Under 12</td><td>10&#8211;11</td><td>20&#8211;25 min</td><td>No</td></tr>
          <tr><td>Under 14</td><td>12&#8211;13</td><td>25&#8211;30 min</td><td>Recommended</td></tr>
          <tr><td>Under 16</td><td>14&#8211;15</td><td>30&#8211;40 min</td><td>Recommended</td></tr>
          <tr><td>Junior (U18)</td><td>16&#8211;17</td><td>40&#8211;45 min</td><td>Yes</td></tr>
        </tbody>
      </table>
      <p class="mt-sm" style="font-size:0.9rem; color:var(--color-text-light);">Ages are as of 1 January of the current year. Contact us if you&#39;re unsure which category your child belongs in.</p>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <h2>Coaching sessions</h2>
      <p>New to the sport? Our coaching sessions are a great, low-pressure way for young riders to get started before racing.</p>
      <div class="info-grid mt-md">
        <div class="info-item">
          <div class="info-item__icon">&#128205;</div>
          <div class="info-item__text">
            <h3>Leeds &amp; Bradford area</h3>
            <p><strong>Saturdays, 10:00&#8211;11:30am</strong><br>Roundhay Park, Leeds<br>&#163;5 per session, no booking needed</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128205;</div>
          <div class="info-item__text">
            <h3>Sheffield area</h3>
            <p><strong>Sundays (off-season), 10:00&#8211;11:30am</strong><br>Graves Park, Sheffield<br>&#163;5 per session, no booking needed</p>
          </div>
        </div>
      </div>
      <p class="mt-md" style="font-size:0.9rem; color:var(--color-text-light);">Coaching locations vary seasonally. Check the <a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook group</a> for the latest schedule or <a href="mailto:info@yorkshirecyclocross.com">email us</a>.</p>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>For parents</h2>
      <div class="accordion mt-md">
        <details>
          <summary>Does my child need a racing licence?</summary>
          <p>Under 14s can race without a licence at YCCA events. For Under 14 and above, a British Cycling day licence (around &#163;5) is recommended. Annual junior memberships from British Cycling start at around &#163;25 and include racing and insurance benefits.</p>
        </details>
        <details>
          <summary>What equipment does my child need?</summary>
          <p>A helmet (compulsory), any bike, and suitable clothing for the weather. Mountain bikes work well for juniors as they&#39;re robust and handle mud well. You don&#39;t need to buy a dedicated cyclo-cross bike to get started.</p>
        </details>
        <details>
          <summary>Do parents need to stay at the venue?</summary>
          <p>Yes &#8212; a parent or responsible adult must remain at the event for all junior riders (Under 18). You don&#39;t need to be trackside for the whole race, but you should be on site and reachable.</p>
        </details>
        <details>
          <summary>Is cyclo-cross safe for children?</summary>
          <p>Cyclo-cross is one of the safer cycling disciplines &#8212; speeds are low, courses are short-circuit and well-marshalled, and age-graded categories keep young riders racing with peers. Courses are designed to be challenging but not dangerous.</p>
        </details>
        <details>
          <summary>How do I enter my child in a race?</summary>
          <p>Visit the <a href="current-season.html">Current Season</a> page for entry links. Pre-registration is required &#8212; you cannot enter on the day. Entry costs for juniors are typically &#163;5&#8211;&#163;8 per race.</p>
        </details>
      </div>
    </div>
  </section>

  <section class="cta-banner">
    <div class="container">
      <h2>Get your young rider on the start line</h2>
      <p>Junior entries are open for the 2026 Summer Series. Enter online in advance.</p>
      <a href="current-season.html" class="btn btn--primary">See this season&#39;s races</a>
      <a href="mailto:info@yorkshirecyclocross.com" class="btn btn--white">Ask us a question</a>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open juniors.html
```

Check: hero image, 4-item why-CX grid, age categories table with 6 rows, 2 coaching items, 5-item accordion FAQ.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate juniors.html
```

Expected: `juniors.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add juniors.html
git commit -m "feat: add Juniors & Youth page with age table and parent FAQ"
```

---

### Task 6: Current Season (`current-season.html`)

**Files:**
- Create: `current-season.html`

- [x] **Step 1: Create `current-season.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Current Season &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html" class="active">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>2026 Summer Series</h1>
      <p>Race dates, venues, sign-up links and results for the current season.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <h2>Race schedule</h2>
      <p>All races start at <strong>10:30am</strong> (sign-on from 9:00am). Course practice open from 9:00am. Race entry closes online the evening before each event.</p>
      <table class="data-table mt-md">
        <thead>
          <tr>
            <th>Round</th><th>Date</th><th>Venue</th><th>Status</th><th>Entry / Results</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>Round 1</td><td>Sun 18 May 2026</td><td>Roundhay Park, Leeds</td><td><span class="badge badge--grey">Results available</span></td><td><a href="#">View results</a></td></tr>
          <tr><td>Round 2</td><td>Sun 1 Jun 2026</td><td>Graves Park, Sheffield</td><td><span class="badge badge--grey">Results available</span></td><td><a href="#">View results</a></td></tr>
          <tr><td>Round 3</td><td>Sun 15 Jun 2026</td><td>East Park, Hull</td><td><span class="badge">Entries open</span></td><td><a href="#">Enter now</a></td></tr>
          <tr><td>Round 4</td><td>Sun 6 Jul 2026</td><td>Harrogate</td><td><span class="badge">Entries open</span></td><td><a href="#">Enter now</a></td></tr>
          <tr><td>Round 5</td><td>Sun 20 Jul 2026</td><td>Doncaster</td><td><span class="badge badge--grey">Coming soon</span></td><td>&#8212;</td></tr>
          <tr><td>Round 6</td><td>Sun 3 Aug 2026</td><td>Bradford</td><td><span class="badge badge--grey">Coming soon</span></td><td>&#8212;</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>Practical information</h2>
      <div class="info-grid mt-md">
        <div class="info-item">
          <div class="info-item__icon">&#8987;</div>
          <div class="info-item__text">
            <h3>Sign-on times</h3>
            <p>Sign-on opens 90 minutes before your race. Arrive with time to spare &#8212; queues build up close to race time.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128716;</div>
          <div class="info-item__text">
            <h3>Parking</h3>
            <p>Most venues charge &#163;2&#8211;&#163;3 for parking. Check the individual event details for your venue.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#128167;</div>
          <div class="info-item__text">
            <h3>Jet washer</h3>
            <p>Available at all venues (&#163;1&#8211;&#163;2 coins). Please wash your bike before leaving. This is a condition of entry.</p>
          </div>
        </div>
        <div class="info-item">
          <div class="info-item__icon">&#127828;</div>
          <div class="info-item__text">
            <h3>Catering</h3>
            <p>Hot drinks, cake and sandwiches available at most venues. All proceeds support the club.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <h2>Entry fees</h2>
      <table class="data-table mt-md">
        <thead>
          <tr><th>Category</th><th>Per race entry</th></tr>
        </thead>
        <tbody>
          <tr><td>Adults (Senior / Vet)</td><td>&#163;12</td></tr>
          <tr><td>Juniors (U18)</td><td>&#163;6</td></tr>
          <tr><td>Under 16</td><td>&#163;5</td></tr>
          <tr><td>Under 12 &amp; below</td><td>&#163;4</td></tr>
        </tbody>
      </table>
      <p class="mt-sm" style="font-size:0.9rem; color:var(--color-text-light);">All entries via online pre-registration only. No entry on the day.</p>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open current-season.html
```

Check: race schedule table with 6 rounds and colour-coded badges, practical info grid, entry fee table.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate current-season.html
```

Expected: `current-season.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add current-season.html
git commit -m "feat: add Current Season page with race schedule and practical info"
```

---

### Task 7: Results Archive (`results-archive.html`)

**Files:**
- Create: `results-archive.html`

- [x] **Step 1: Create `results-archive.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Results Archive &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html" class="active">Results</a></li>
          <li><a href="rules.html">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>Results Archive</h1>
      <p>Results from all YCCA Summer and Winter Series races since 2012.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <p>Current season results are on the <a href="current-season.html">Current Season</a> page. Previous years are listed below, most recent first.</p>
      <div class="accordion mt-md">
        <details open>
          <summary>2025 &#8212; Winter Series</summary>
          <div>
            <table class="data-table">
              <thead><tr><th>Round</th><th>Date</th><th>Venue</th><th>Results</th></tr></thead>
              <tbody>
                <tr><td>Round 1</td><td>19 Oct 2025</td><td>Roundhay Park, Leeds</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 2</td><td>2 Nov 2025</td><td>Graves Park, Sheffield</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 3</td><td>16 Nov 2025</td><td>East Park, Hull</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 4</td><td>30 Nov 2025</td><td>Harrogate</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 5</td><td>14 Dec 2025</td><td>Bradford</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 6</td><td>11 Jan 2026</td><td>Doncaster</td><td><a href="#">View results</a></td></tr>
              </tbody>
            </table>
          </div>
        </details>
        <details>
          <summary>2025 &#8212; Summer Series</summary>
          <div>
            <table class="data-table">
              <thead><tr><th>Round</th><th>Date</th><th>Venue</th><th>Results</th></tr></thead>
              <tbody>
                <tr><td>Round 1</td><td>18 May 2025</td><td>Roundhay Park, Leeds</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 2</td><td>1 Jun 2025</td><td>Graves Park, Sheffield</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 3</td><td>15 Jun 2025</td><td>East Park, Hull</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 4</td><td>6 Jul 2025</td><td>Harrogate</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 5</td><td>20 Jul 2025</td><td>Doncaster</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 6</td><td>3 Aug 2025</td><td>Bradford</td><td><a href="#">View results</a></td></tr>
              </tbody>
            </table>
          </div>
        </details>
        <details>
          <summary>2024 &#8212; Winter Series</summary>
          <div>
            <table class="data-table">
              <thead><tr><th>Round</th><th>Date</th><th>Venue</th><th>Results</th></tr></thead>
              <tbody>
                <tr><td>Round 1</td><td>20 Oct 2024</td><td>Roundhay Park, Leeds</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 2</td><td>3 Nov 2024</td><td>Graves Park, Sheffield</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 3</td><td>17 Nov 2024</td><td>East Park, Hull</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 4</td><td>1 Dec 2024</td><td>Harrogate</td><td><a href="#">View results</a></td></tr>
                <tr><td>Round 5</td><td>15 Dec 2024</td><td>Bradford</td><td><a href="#">View results</a></td></tr>
              </tbody>
            </table>
          </div>
        </details>
        <details>
          <summary>Earlier seasons (2012&#8211;2023)</summary>
          <div>
            <p>Results for seasons prior to 2024 are held on our main website at <a href="https://yorkshirecyclocross.com" target="_blank" rel="noopener">yorkshirecyclocross.com</a>.</p>
          </div>
        </details>
      </div>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open results-archive.html
```

Check: 2025 Winter Series open by default, other seasons collapsed, clicking expands the table inside.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate results-archive.html
```

Expected: `results-archive.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add results-archive.html
git commit -m "feat: add Results Archive with collapsible year sections"
```

---

### Task 8: Rules & Regulations (`rules.html`)

**Files:**
- Create: `rules.html`

- [x] **Step 1: Create `rules.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rules &amp; Regulations &#8212; Yorkshire CX</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="index.html" class="site-logo"><span>Yorkshire</span>Cyclo-Cross</a>
      <nav class="site-nav" id="main-nav">
        <ul>
          <li><a href="new-to-cx.html">New to CX?</a></li>
          <li><a href="juniors.html">Juniors</a></li>
          <li><a href="current-season.html">Current Season</a></li>
          <li><a href="results-archive.html">Results</a></li>
          <li><a href="rules.html" class="active">Rules</a></li>
          <li><a href="current-season.html" class="nav-cta">Enter a Race</a></li>
        </ul>
      </nav>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-controls="main-nav" aria-expanded="false">&#9776;</button>
    </div>
  </header>

  <div class="page-header">
    <div class="container">
      <h1>Rules &amp; Regulations</h1>
      <p>YCCA technical and conduct rules for all competitors.</p>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <h2>Licensing</h2>
      <table class="data-table mt-md">
        <thead>
          <tr><th>Category</th><th>Licence required</th><th>Notes</th></tr>
        </thead>
        <tbody>
          <tr><td>Novice / Newcomer</td><td>No</td><td>Open to riders in their first or second season</td></tr>
          <tr><td>Under 8 &#8211; Under 12</td><td>No</td><td>Parent/guardian must be present on site</td></tr>
          <tr><td>Under 14 &#8211; Under 16</td><td>Recommended</td><td>Day licence available at sign-on (&#163;5)</td></tr>
          <tr><td>Junior (U18)</td><td>Yes</td><td>British Cycling junior licence required</td></tr>
          <tr><td>Senior / Vet</td><td>Yes (or day licence)</td><td>Annual or day licence from British Cycling</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section class="section section--alt">
    <div class="container">
      <h2>Technical rules</h2>
      <div class="accordion mt-md">
        <details open>
          <summary>Equipment &#8212; bikes</summary>
          <p>Any bike may be used in Novice and junior categories. For open categories, bikes must be pedal-powered and comply with British Cycling technical regulations. Handlebars must be plugged. No aerobars. Minimum tyre width 28mm.</p>
        </details>
        <details>
          <summary>Equipment &#8212; clothing &amp; helmets</summary>
          <p>A hard-shell cycling helmet meeting EN1078, ASTM F1447, or equivalent standard is compulsory for all riders at all times on the course including warm-up. Shorts and jersey or equivalent must be worn.</p>
        </details>
        <details>
          <summary>Course conduct</summary>
          <p>Riders must follow the marked course at all times. Cutting corners or leaving the course results in disqualification. Riders being lapped must yield by moving to the right of the course. Dangerous riding or deliberate obstruction will result in disqualification and may result in exclusion from future events.</p>
        </details>
        <details>
          <summary>Sign-on and race numbers</summary>
          <p>All riders must sign on before racing. Race numbers must be visible on the back of the jersey or on shorts. Numbers must not be folded or obscured. Riders who have not signed on will not receive results.</p>
        </details>
        <details>
          <summary>Protests and appeals</summary>
          <p>Protests must be made to the chief commissaire within 15 minutes of provisional results being posted. A &#163;10 deposit is required (refunded if the protest is upheld). The commissaire&#39;s decision is final.</p>
        </details>
        <details>
          <summary>Jet washing requirement</summary>
          <p>All riders must wash their bikes using the jet washer before leaving the venue. This is a condition of entry and helps prevent the spread of plant diseases. Jet washers are available at all venues (&#163;1&#8211;&#163;2 coins).</p>
        </details>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="container">
      <h2>British Cycling resources</h2>
      <p>YCCA races are run under British Cycling regulations. The following are useful for competitive riders:</p>
      <ul style="margin-top:var(--space-sm); padding-left:var(--space-md); list-style:disc;">
        <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling &#8212; Cyclo-Cross Technical Regulations</a></li>
        <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">Licence types and how to purchase</a></li>
        <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">Commissaires and race officials guide</a></li>
      </ul>
    </div>
  </section>

  <footer class="site-footer">
    <div class="container">
      <div class="footer__grid">
        <div class="footer__col">
          <h4>Yorkshire Cyclo-Cross</h4>
          <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="new-to-cx.html">New to CX?</a></li>
            <li><a href="juniors.html">Junior &amp; Youth Riders</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Racing</h4>
          <ul>
            <li><a href="current-season.html">Current Season</a></li>
            <li><a href="results-archive.html">Results Archive</a></li>
            <li><a href="rules.html">Rules &amp; Regulations</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Follow Us</h4>
          <ul>
            <li><a href="https://www.facebook.com/groups/yorkshirecyclocross" target="_blank" rel="noopener">Facebook</a></li>
            <li><a href="https://www.instagram.com/yorkshirecyclocross/" target="_blank" rel="noopener">Instagram</a></li>
          </ul>
        </div>
        <div class="footer__col">
          <h4>Get In Touch</h4>
          <ul>
            <li><a href="mailto:info@yorkshirecyclocross.com">Email Us</a></li>
            <li><a href="https://www.britishcycling.org.uk" target="_blank" rel="noopener">British Cycling</a></li>
          </ul>
        </div>
      </div>
      <div class="footer__bottom">
        <span>&#169; 2026 Yorkshire Cyclo-Cross Association</span>
        <span>Prototype &#8212; not the live website</span>
      </div>
    </div>
  </footer>

  <script>
    document.querySelector('.nav-toggle').addEventListener('click', function () {
      var nav = document.getElementById('main-nav');
      var open = nav.classList.toggle('open');
      this.setAttribute('aria-expanded', open ? 'true' : 'false');
    });
  </script>
</body>
</html>
```

- [x] **Step 2: Open in browser and verify**

```bash
open rules.html
```

Check: licensing table, 6-item accordion (bikes open by default), British Cycling links list.

- [x] **Step 3: Validate HTML**

```bash
npx html-validate rules.html
```

Expected: `rules.html: no errors found`

- [x] **Step 4: Commit**

```bash
git add rules.html
git commit -m "feat: add Rules & Regulations page"
```

---

### Task 9: Final review pass

- [x] **Step 1: Click every nav link across all 7 pages**

```bash
open index.html
```

Walk every page → every nav link. All 7 pages must be reachable; active page must highlight correctly in nav.

- [x] **Step 2: Test mobile nav at 375px**

In browser dev tools, set viewport to 375px on each page. Confirm: hamburger visible, tap opens nav, nav link navigates correctly.

- [x] **Step 3: Run html-validate across all pages**

```bash
npx html-validate index.html new-to-cx.html your-first-race.html juniors.html current-season.html results-archive.html rules.html
```

Expected: all files report `no errors found`

- [x] **Step 4: Final commit**

```bash
git add -A
git commit -m "feat: complete 7-page YCCA website prototype"
```

---

## Spec coverage

| Spec requirement | Task |
|---|---|
| Homepage with hero, dual CTA, intro | Task 2 |
| New to CX page | Task 3 |
| Your First Race practical guide | Task 4 |
| Juniors & Youth page with age table, coaching, parent FAQ | Task 5 |
| Current Season with race schedule, practical info, entry fees | Task 6 |
| Results Archive, organised by year | Task 7 |
| Rules & Regulations with licensing table | Task 8 |
| Shared nav and footer on all pages | Tasks 2–8 |
| Mobile-responsive design | Task 1 CSS |
| Stock placeholder images | Tasks 2–8 |
| Prototype banner in footer | All pages |
| HTML validation passing | Tasks 2–9 |
