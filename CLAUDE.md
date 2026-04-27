# DfX Lab — Project Guidelines

## What This Is

A one-page scrolling lab website for a robotics lab at Hanbat National University.
The goal is a custom-designed static site that a non-technical professor can update without touching code.

## Core Constraints

- **Free hosting only** — Netlify or Vercel (static hosting, no server)
- **Zero server maintenance** — no backend, no database, no CMS service
- **Minimum tools** — everything must work with just the files in this folder
- **Non-technical editor** — the professor edits content via `admin.html`, never touches code

## File Structure

```
dfx-lab/
├── index.html    — the actual website (reads from data.js)
├── data.js       — all site content as a JS object (siteData)
├── admin.html    — local content editor for the professor
└── images/       — all images and videos referenced by the site
```

## How the Content Flow Works

1. Professor opens `admin.html` locally in a browser
2. Edits text, uploads images, adds/removes cards
3. Clicks "Download data.js" → replaces the old `data.js`
4. Uploads new image files to Netlify dashboard
5. Drags `data.js` to Netlify → site updates instantly

`index.html` loads `data.js` via `<script src="data.js"></script>` and renders everything from `window.siteData`.

## siteData Shape

```js
const siteData = {
  lab:      { name, fullName, subtitle },
  hero:     { videoFile, imageFile },           // paths like "images/hero.mp4"
  research: [{ id, tags[], mediaFile, keywords[], description }],
  projects: [{ id, name, year, imageFile, bullets[] }],
  alumni:   [{ id, name, company, photoFile }],
  contact:  { professorName, title, email, github, scholar, photoFile },
  footer:   string
}
```

## Design Spec (from layout-mock.html)

- **Color:** dark base `#0f0f0f`, white, accent orange `#ff4d1c`
- **Font:** geometric/grotesque — Geist, DM Sans, or similar. Bold weights. No serif.
- **Motion:** subtle scroll-triggered fade-ins. Hero has looping video background.
- **Language toggle:** EN/KR — body copy switches, labels/names stay English.

### Sections

| # | Section | Layout |
|---|---------|--------|
| 01 | Nav bar | Sticky, full-width, blurs background on scroll. Lab name left, links right, EN\|KR far right. |
| 02 | Hero | 100vh full-bleed. Giant "Design for X" type over video/image. |
| 03 | Research | Pill tag filters → 2-col media grid. Keywords + one sentence per card. |
| 04 | Projects | Alternating full-bleed image + text rows. Name, year, 2–3 bullet points only. |
| 05 | Alumni | Horizontal scroll strip. Portrait photo, name, company. |
| 06 | Contact | Full-width dark section. Professor photo, name, title, email, links, CTA button. |
| 07 | Footer | One-liner. |

## What admin.html Does

- Runs entirely in the browser — no server needed
- Auto-saves all edits to `localStorage`
- Image uploads: shows preview + filename, stores `"images/filename.ext"` in data
- Generates and downloads `data.js` on demand
- Has a file checklist showing which images need to be uploaded to Netlify

## What NOT to Do

- Do not add a backend or database
- Do not require npm, Node.js, or any build step to run the site
- Do not use a framework that requires a dev server (React, Vue, etc.) — vanilla JS only
- Do not store image base64 in data.js — always store filename paths only
- Do not break the admin → data.js → index.html flow
