# ePortfolio Update — Design Spec
**Date:** 2026-04-13
**Goal:** Update `diegourquiola.github.io` to meet the Individual ePortfolio assignment requirements, targeting a potential employer audience with a "wow factor."

---

## Context

The existing site has five pages: Home, Projects (3 project detail pages), About, Resume, and Contact. It uses a clean dark design system with Inter font, consistent card/tag components, and a single shared CSS file. The site is deployed via GitHub Pages.

The assignment requires: resume, LinkedIn profile showcase, 3+ major projects with visuals and GitHub links, and a strong growth narrative with wow factor. Supplemental components (certifications, awards, etc.) are out of scope for this iteration.

---

## Pages Modified

| Page | Change |
|---|---|
| `index.html` | Hero copy reframe + new progression timeline section |
| `about.html` | Add LinkedIn profile card block |
| `projects/index.html` | Add Goalboard card, add placeholder screenshots to all cards |
| `projects/trevecca-pedia.html` | Add placeholder screenshot |
| `projects/huffman-encoding.html` | Add placeholder screenshot |
| `projects/arkanoid.html` | Add placeholder screenshot |

## Pages Created

| Page | Purpose |
|---|---|
| `projects/goalboard.html` | Full project detail page for Goalboard |

---

## Section Designs

### 1. Home Page — Hero Reframe

- Badge: change from `"CS Student — Builder & Problem Solver"` to `"CS Student → Software Engineer"`
- Lead paragraph: add one sentence making the career direction explicit — e.g., "I'm building toward a career as a software engineer, with a focus on backend systems and full-stack mobile development."
- Meta cards: update "Focus" card from `"Systems & Backends"` to `"Software Engineer"` or `"Backend & Mobile"`

### 2. Home Page — Progression Timeline (new section)

A new section placed between "Featured Projects" and the "About Teaser" section.

**Title:** "Four Years of Building" (or "My Journey")
**Subtitle:** "Not just what I built — how my thinking evolved."

Four milestone nodes displayed horizontally (stacked vertically on mobile), each linking to the project page:

| Node | Project | Theme | Year | Stack |
|---|---|---|---|---|
| 1 | Arkanoid Remake | Game Dev / OOP | Sophomore | Java, OOP |
| 2 | Huffman Encoding | Algorithms & DS | Junior | Java, Data Structures |
| 3 | Trevecca-pedia | Backend Systems | Senior | Go, Docker, PostgreSQL, JWT |
| 4 | Goalboard | Full-Stack Mobile | Current | React Native, FastAPI, Python |

Each node shows: project name, one-line theme, year label, stack tags, and a "View →" link.
A connecting line or arrow runs between nodes to visually convey progression.

### 3. Projects — Goalboard Detail Page (`projects/goalboard.html`)

Follows the same layout as existing project detail pages.

**Header tags:** `Python`, `React Native`, `FastAPI`, `Expo`, `Soccerdata API`
**Status badge:** `In Progress`
**Placeholder screenshot:** styled dark rectangle with project name and "Screenshot coming soon" label

**Sections:**
- **Overview:** Soccer analytics mobile app. Displays league standings, match results, and team performance trends. Built as a current/active senior project.
- **Architecture:** FastAPI backend consumes the Soccerdata API and exposes REST endpoints. React Native (Expo) front-end fetches and renders data. Clear separation between data layer and UI layer.
- **My Role:** Sole developer. Responsible for API design, data modeling, and mobile UI.
- **GitHub:** Link to `https://github.com/diegourquiola/goalboard`

### 4. Projects Index — Goalboard Card

Added as the fourth card in the grid. Uses the same `project-card` component. Tags: `Python`, `React Native`, `FastAPI`. Status badge: green `In Progress`. Brief description matching the project page overview.

### 5. Project Detail Pages — Placeholder Screenshots

Each of the four project detail pages (Arkanoid, Huffman, Trevecca-pedia, Goalboard) gets a styled placeholder image block inserted near the top of the writeup section. Styled as a dark rounded rectangle with the project name centered — consistent with the existing design system. Implemented in pure HTML/CSS, no external images needed.

### 6. About Page — LinkedIn Section

Added below the Education timeline block, above the "View Full Resume" button.

**Content:**
- Section heading: "LinkedIn"
- URL displayed: `linkedin.com/in/diegourquiola0806`
- One-line description: "Connect with me or view my full professional profile."
- CTA button: "View LinkedIn Profile →" (opens in new tab)
- Checklist of profile completeness signals: photo, headline, experience descriptions, skills, projects

---

## Design Constraints

- Use only existing CSS classes and variables — no new stylesheet additions unless strictly necessary
- All placeholder screenshots implemented in HTML/CSS (no image files)
- All external links open in `target="_blank" rel="noreferrer"`
- Mobile responsive — timeline stacks vertically on small screens
- No JavaScript additions beyond what already exists in `main.js`

---

## Out of Scope

- Supplemental components (certifications, awards, community involvement)
- Real project screenshots (placeholders only — easy to swap later)
- LinkedIn profile content editing (that's done on LinkedIn directly)
- Contact page changes
- Resume PDF update
