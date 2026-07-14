# YCCA Website Prototype — Design Spec

**Date:** 2026-07-14
**Project:** Yorkshire Cyclocross Association (yorkshirecyclocross.com)
**Goal:** A static HTML prototype of an improved website to address falling rider numbers and attract younger riders (juniors and young adults). Prototype will be reviewed by the committee and later merged into the existing WordPress site.

---

## Problem

- Rider numbers have been falling over recent years
- Not enough new (especially younger) riders joining the league
- Current website is results/schedule-focused and not welcoming to newcomers
- "Get Into Cyclo-Cross" page lacks practical detail for absolute beginners
- No dedicated youth/junior content
- Homepage does not serve first-time visitors well

---

## Solution

Build a 7-page static HTML prototype with:
1. Improved content for newcomers and younger riders
2. Clean, modern, mobile-friendly design
3. Stock cyclocross photography as placeholders (to be replaced with real club photos)
4. Well-organised existing-member pages (current season, archive, rules)

This prototype is a standalone static site — no WordPress required to view or iterate. Content and layout can be ported into WordPress once approved.

---

## Channels Context

The club currently communicates via:
- Website (yorkshirecyclocross.com — WordPress)
- Facebook group page
- Email newsletter ("Stakes & Chips")
- Instagram (@yorkshirecyclocross)

The website prototype should serve as the content hub that all channels link back to.

---

## Pages

### 1. Homepage (`index.html`)
- Hero image (action cyclocross shot, stock)
- Headline: *"Yorkshire's home of cyclo-cross racing"*
- 2-sentence jargon-free intro to the sport
- Two clear CTAs:
  - "New to cyclo-cross? Start here →" (links to new-to-cx.html)
  - "This season's races →" (links to current-season.html)
- Sponsor logos in footer

### 2. New to Cyclo-Cross (`new-to-cx.html`)
- What is cyclocross — short, punchy, no jargon
- Why it's fun and accessible (all ages, all fitness levels, 1-hour races)
- Photo of mixed-age group racing
- Two CTAs: "Your first race" and "Junior & youth riders"

### 3. Your First Race (`your-first-race.html`)
- What to bring and what to wear
- Entry costs and how to register online in advance
- What happens on the day: sign-on, course walk, warm-up, race, results
- Common concerns addressed: mud, fitness, obstacles, getting lapped
- Link to current season sign-up

### 4. Juniors & Youth (`juniors.html`)
- Age categories explained (Under 8 through Under 16/Junior)
- What a junior race day looks like
- Coaching sessions: times, venues, costs
- What parents need to know (supervision, licence, equipment)
- Photos of junior racing (stock, replaced with real later)

### 5. Current Season (`current-season.html`)
- This season's race dates and venues
- Sign-up links (MyRaceResults or equivalent)
- Live/recent results
- Practical info: parking, jet wash policy, sign-on times

### 6. Results Archive (`results-archive.html`)
- Results organised by year and series (Winter/Summer)
- Clean, scannable layout — less cluttered than current site

### 7. Rules & Regulations (`rules.html`)
- Technical rules for racing
- Licence requirements by category
- Links to British Cycling resources

---

## Visual Style

- Clean, modern layout
- Bold action photography (stock Unsplash/Pexels for prototype)
- Mobile-friendly (responsive CSS)
- Energetic but not over-designed — volunteer-maintainable
- Consistent header/nav and footer across all pages

---

## Navigation

All pages share a top nav:
```
Home | New to CX | Juniors | Current Season | Results | Rules | Contact
```

---

## Out of Scope (for this prototype)

- WordPress integration (handled later)
- Social media content strategy
- Paid advertising
- Email newsletter redesign
- Backend/dynamic content

---

## Success Criteria

- Committee can review and give feedback on content and layout
- Newcomer pages clearly answer: "What is this? Can I do it? How do I start?"
- Junior page clearly answers parents' key questions
- Existing members can find current season info in 1 click from homepage
- Design feels modern enough to share on Instagram/Facebook as a preview
