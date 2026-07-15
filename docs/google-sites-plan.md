# Porting the prototype to Google Sites

## Honest assessment

Google Sites (the current version at sites.google.com) is a drag-and-drop
builder with no support for custom CSS or JavaScript. This makes it a poor
fit for the prototype as designed.

| Feature | WordPress | Google Sites |
|---|---|---|
| Custom CSS (brand colours, layout) | ✅ Full control | ❌ Not supported |
| Custom fonts | ✅ | ⚠️ Google Fonts only, limited selection |
| Banner image at top of every page | ✅ | ✅ (as a header image per page) |
| Branded nav bar | ✅ | ❌ Google's own nav only |
| Card grids / info grids | ✅ | ⚠️ Approximate with columns |
| Accordions (details/summary) | ✅ | ⚠️ Google Sites has "Expandable text" |
| Data tables | ✅ | ⚠️ Basic table support |
| Mobile-responsive | ✅ | ✅ Automatic |
| Custom JavaScript (mobile nav) | ✅ | ❌ Not supported (not needed — Google handles mobile) |
| Free to host | ✅ (with existing hosting) | ✅ |
| No login required to edit | ❌ Needs WP access | ✅ Google account only |
| Difficulty to set up | Medium | Low |

**Bottom line:** Google Sites is easier to get *something* live, but the result
will look like a generic Google site, not the branded prototype. Use it if
ease of editing matters more than design fidelity.

---

## What you would lose

- All brand colours (pink `#ff66c4` / navy `#07294e`) — replaced by a Google theme
- The banner image as a full-width strip (can be added per page but won't be consistent)
- Card hover effects, step-list counters, badge styling
- Exact accordion styling
- The sticky branded nav bar

---

## What you would keep

- All page content (text, tables, fees, rules)
- The 7-page structure
- Mobile responsiveness (handled automatically by Google Sites)
- Free hosting under a `sites.google.com/...` URL or a custom domain

---

## Option A — Native Google Sites (content only)

Best for: getting content live quickly if design fidelity is not a priority.

### Step 1 — Create the site

Go to [sites.google.com](https://sites.google.com) → **+ Blank** → name it
"Yorkshire Cyclo-Cross Association".

### Step 2 — Apply the closest theme

Click **Themes** (right panel) and choose the darkest/cleanest option
available. Set the accent colour to `#ff66c4` (Google Sites allows a custom
hex for the theme accent).

### Step 3 — Add the banner image to every page

On each page, set a full-width header image using `images/banner.png`
(upload from the prototype). In Google Sites, go to the page header area →
**Change image** → upload the file.

### Step 4 — Create the 7 pages

In **Pages** (right panel), add:

| Google Sites page | Source |
|---|---|
| Home | `index.html` |
| New to CX? | `new-to-cx.html` |
| Your First Race | `your-first-race.html` |
| Juniors | `juniors.html` |
| Current Season | `current-season.html` |
| Results Archive | `results-archive.html` |
| Rules | `rules.html` |

### Step 5 — Add content to each page

For each page, use Google Sites' layout blocks:

| Prototype element | Google Sites equivalent |
|---|---|
| Hero text + CTA buttons | Text block + Button block |
| Info grid (4 items) | 4-column layout with text |
| Card grid | Image + text columns |
| Accordion (FAQ) | "Expandable text" block |
| Data table | Table block |
| Step list | Numbered text / text block |
| CTA banner | Full-width coloured section |

Copy the text content from the prototype HTML pages. Formatting will be
approximate — no custom classes, no CSS counters, no hover effects.

### Step 6 — Set up navigation

Google Sites automatically builds a nav bar from your page list. Reorder
pages in the **Pages** panel to match the prototype nav order.

### Step 7 — Add the race schedule

For the Current Season page, consider embedding a **Google Calendar** instead
of the static table — it's easier to keep up to date and integrates natively
with Google Sites.

### Step 8 — Publish

Click **Publish** → choose a URL:
- Free: `sites.google.com/view/yorkshirecyclocross`
- Custom domain (e.g. `yorkshirecyclocross.com`): set up in **Manage →
  Custom domains** — requires access to the domain's DNS settings.

---

## Option B — Embed the GitHub Pages prototype (recommended if using Google Sites)

Rather than recreating everything in Google Sites' limited editor, embed the
prototype directly. Google Sites supports HTML embeds via iframe.

### How it works

The prototype is already live at:
`https://adampridmore.github.io/yorkshire-cx-website-prototype/index.html`

You can embed any page of it into a Google Sites page as a full-height iframe.

### Steps

1. Create a Google Site with the same 7 pages (just titles and nav, no content)
2. On each page, insert an **Embed** block (Insert → Embed → By URL)
3. Paste the corresponding GitHub Pages URL for that page
4. Set the embed to full width

**Result:** Visitors see the fully styled prototype inside Google Sites' shell.
The Google nav provides the top-level navigation; the embedded frame shows the
full branded page.

**Limitations of this approach:**
- The nav will appear twice (Google Sites nav + the prototype's nav inside the frame)
- Clicking links inside the frame won't update the Google Sites URL
- Not suitable as a permanent solution — only useful as a demo wrapper

---

## Recommendation

| Goal | Best option |
|---|---|
| Demo to committee quickly | **GitHub Pages** (already done — share the link directly) |
| Permanent home, full design | **WordPress** (see `docs/wordpress-plan.md`) |
| Permanent home, easy editing, basic design | **Google Sites Option A** |
| Temporary wrapper around the prototype | **Google Sites Option B** |

For yorkshirecyclocross.com, WordPress is the right long-term answer.
Google Sites is only worth considering if the committee wants to manage
content themselves without any WordPress access and can accept a more
generic look.
