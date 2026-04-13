# ePortfolio Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update diegourquiola.github.io with a growth-narrative progression timeline, Goalboard project page, placeholder screenshots, and LinkedIn section to meet the Individual ePortfolio assignment requirements.

**Architecture:** Pure static HTML/CSS site — no build system, no framework. All changes are direct edits to HTML and CSS files. New CSS classes are appended to `assets/css/style.css`. New pages mirror the exact structure of existing project detail pages. No JavaScript changes needed.

**Tech Stack:** HTML5, CSS3 (custom properties), existing `assets/js/main.js` (untouched)

---

## File Map

**Create:**
- `projects/goalboard.html` — Goalboard project detail page (new, full page)

**Modify:**
- `assets/css/style.css` — Append sections 16 and 17: journey timeline styles + screenshot placeholder styles + mobile overrides
- `index.html` — Hero badge/lead/meta-card reframe + new progression timeline section
- `about.html` — LinkedIn profile card block (after Education, before "View Full Resume" button)
- `projects/index.html` — Card screenshot thumbnails on all 4 cards + Goalboard as 4th card
- `projects/trevecca-pedia.html` — Placeholder screenshot + Goalboard link in sidebar
- `projects/huffman-encoding.html` — Placeholder screenshot + Goalboard link in sidebar
- `projects/arkanoid.html` — Placeholder screenshot + Goalboard link in sidebar

---

## Task 1: Add CSS classes

**Files:**
- Modify: `assets/css/style.css` (append after line 654, the end of the file)

- [ ] **Step 1: Append new CSS sections to `assets/css/style.css`**

Add the following block at the very end of the file (after the closing `}` of the last `@media` block):

```css

/* ---------------------------------------------------------------
   16. Journey / Progression Timeline (Home Page)
   --------------------------------------------------------------- */
.journey-track {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  position: relative;
  margin-top: 40px;
}
.journey-track::before {
  content: '';
  position: absolute;
  top: 21px;
  left: 12.5%;
  right: 12.5%;
  height: 2px;
  background: linear-gradient(90deg, var(--accent), var(--accent2));
  z-index: 0;
}
.journey-node {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 0 16px;
  position: relative;
  z-index: 1;
}
.journey-dot {
  width: 44px;
  height: 44px;
  border-radius: 999px;
  border: 2px solid var(--accent);
  background: var(--surface);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: 700;
  color: var(--accent);
  margin-bottom: 20px;
  box-shadow: 0 0 0 6px rgba(96,165,250,.1);
  flex-shrink: 0;
}
.journey-dot.current {
  border-color: var(--accent2);
  color: var(--accent2);
  box-shadow: 0 0 0 6px rgba(52,211,153,.12);
}
.journey-year {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .08em;
  color: var(--muted);
  margin-bottom: 6px;
}
.journey-node h3 { font-size: 16px; margin-bottom: 6px; }
.journey-theme {
  color: var(--text-secondary);
  font-size: 13px;
  margin-bottom: 10px;
  line-height: 1.5;
}
.journey-tags { justify-content: center; margin-bottom: 14px; }
.journey-link {
  font-size: 13px;
  font-weight: 600;
  color: var(--accent);
  transition: color var(--transition);
}
.journey-link:hover { color: var(--accent2); }

/* ---------------------------------------------------------------
   17. Project Screenshot Placeholder
   --------------------------------------------------------------- */
.project-screenshot {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: var(--radius);
  border: 1px dashed rgba(148,163,184,.28);
  background: rgba(17,24,39,.6);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-bottom: 32px;
  color: var(--muted);
  text-align: center;
}
.project-screenshot .ss-name {
  font-size: 16px;
  font-weight: 700;
  color: var(--text);
}
.project-screenshot .ss-sub { font-size: 13px; }

.card-screenshot {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 8px;
  background: rgba(15,23,42,.8);
  border: 1px dashed rgba(148,163,184,.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  color: var(--muted);
  margin-bottom: 4px;
}
```

- [ ] **Step 2: Add mobile overrides for journey timeline**

Inside the existing `@media (max-width: 900px)` block (around line 621), add these rules after the last existing rule in that block (before the closing `}`):

```css
  .journey-track {
    grid-template-columns: 1fr;
    gap: 32px;
  }
  .journey-track::before { display: none; }
  .journey-node {
    flex-direction: row;
    text-align: left;
    align-items: flex-start;
    gap: 20px;
  }
  .journey-dot { margin-bottom: 0; }
  .journey-tags { justify-content: flex-start; }
```

- [ ] **Step 3: Verify CSS syntax**

Open `assets/css/style.css` and confirm:
- No unclosed `{` or `}` braces at the end of the file
- `.journey-track`, `.journey-node`, `.journey-dot`, `.project-screenshot`, `.card-screenshot` all appear

- [ ] **Step 4: Commit**

```bash
git add assets/css/style.css
git commit -m "style: add journey timeline, screenshot placeholder, and card thumbnail CSS"
```

---

## Task 2: Home page hero reframe

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Update hero badge**

In `index.html`, find:
```html
          <p class="badge">CS Student &mdash; Builder &amp; Problem Solver</p>
```
Replace with:
```html
          <p class="badge">CS Student &rarr; Software Engineer</p>
```

- [ ] **Step 2: Update hero lead paragraph**

Find:
```html
          <p class="lead">
            I'm Diego Urquiola, a Computer Science student at Trevecca Nazarene
            University. I design multi-service backends, implement core algorithms
            from scratch, and ship projects that are clean, functional, and ready
            to demo.
          </p>
```
Replace with:
```html
          <p class="lead">
            I'm Diego Urquiola, a Computer Science student at Trevecca Nazarene
            University. I design multi-service backends, implement core algorithms
            from scratch, and ship projects that are clean, functional, and ready
            to demo. I'm building toward a career as a software engineer with a focus
            on backend systems and full-stack mobile development.
          </p>
```

- [ ] **Step 3: Update "Focus" meta card**

Find:
```html
            <div class="meta-card">
              <div class="meta-label">Focus</div>
              <div class="meta-value">Systems &amp; Backends</div>
            </div>
```
Replace with:
```html
            <div class="meta-card">
              <div class="meta-label">Focus</div>
              <div class="meta-value">Backend &amp; Mobile</div>
            </div>
```

- [ ] **Step 4: Update hero panel list to include mobile development**

Find:
```html
            <li><span class="check" aria-hidden="true">&#10003;</span> Game development with OOP and collision systems</li>
```
Replace with:
```html
            <li><span class="check" aria-hidden="true">&#10003;</span> Game development with OOP and collision systems</li>
            <li><span class="check" aria-hidden="true">&#10003;</span> Full-stack mobile apps (React Native + FastAPI)</li>
```

- [ ] **Step 5: Verify in browser**

Open `index.html` in a browser and confirm:
- Badge reads "CS Student → Software Engineer"
- Lead paragraph ends with "…backend systems and full-stack mobile development."
- Focus meta card reads "Backend & Mobile"
- Hero panel has 7 bullet items, last one mentions React Native + FastAPI

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: reframe hero copy toward software engineer career goal"
```

---

## Task 3: Home page — progression timeline section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Insert journey section between Featured Projects and About Teaser**

In `index.html`, find the About Teaser section opening:
```html
    <!-- About Teaser -->
    <section class="section">
```
Insert the following block immediately before it:

```html
    <!-- Progression Timeline -->
    <section class="section alt">
      <div class="container">
        <p class="section-label">Growth</p>
        <h2>Four Years of Building</h2>
        <p class="lead">Not just what I built &mdash; how my thinking evolved from project to project.</p>

        <div class="journey-track">

          <div class="journey-node">
            <div class="journey-dot">1</div>
            <div class="journey-year">Sophomore</div>
            <h3>Arkanoid Remake</h3>
            <p class="journey-theme">Game Dev &amp; OOP</p>
            <div class="tag-row journey-tags">
              <span class="tag blue">Java</span>
              <span class="tag">OOP</span>
            </div>
            <a class="journey-link" href="./projects/arkanoid.html">View project &rarr;</a>
          </div>

          <div class="journey-node">
            <div class="journey-dot">2</div>
            <div class="journey-year">Junior</div>
            <h3>Huffman Encoding</h3>
            <p class="journey-theme">Algorithms &amp; Data Structures</p>
            <div class="tag-row journey-tags">
              <span class="tag blue">Java</span>
              <span class="tag">DS&amp;A</span>
            </div>
            <a class="journey-link" href="./projects/huffman-encoding.html">View project &rarr;</a>
          </div>

          <div class="journey-node">
            <div class="journey-dot">3</div>
            <div class="journey-year">Senior</div>
            <h3>Trevecca-pedia</h3>
            <p class="journey-theme">Backend Systems</p>
            <div class="tag-row journey-tags">
              <span class="tag blue">Go</span>
              <span class="tag">Docker</span>
            </div>
            <a class="journey-link" href="./projects/trevecca-pedia.html">View project &rarr;</a>
          </div>

          <div class="journey-node">
            <div class="journey-dot current">4</div>
            <div class="journey-year">Current</div>
            <h3>Goalboard</h3>
            <p class="journey-theme">Full-Stack Mobile</p>
            <div class="tag-row journey-tags">
              <span class="tag blue">Python</span>
              <span class="tag">React Native</span>
            </div>
            <a class="journey-link" href="./projects/goalboard.html">View project &rarr;</a>
          </div>

        </div>
      </div>
    </section>

```

- [ ] **Step 2: Verify in browser**

Open `index.html` in a browser and confirm:
- "Four Years of Building" section appears between Featured Projects and the About Teaser
- Four nodes appear horizontally: Arkanoid → Huffman → Trevecca-pedia → Goalboard
- A gradient connecting line runs between nodes
- Node 4 (Goalboard) has a green dot
- All "View project →" links are present
- On a narrow window (< 900px), nodes stack vertically

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Four Years of Building progression timeline to home page"
```

---

## Task 4: Screenshot placeholders on existing project detail pages

**Files:**
- Modify: `projects/trevecca-pedia.html`
- Modify: `projects/huffman-encoding.html`
- Modify: `projects/arkanoid.html`

### trevecca-pedia.html

- [ ] **Step 1: Add screenshot placeholder before Overview**

Find the opening of the article's first content section in `projects/trevecca-pedia.html`:
```html
          <!-- Overview -->
          <div class="content-section">
            <h2>Overview</h2>
```
Insert the following block immediately before it:

```html
          <!-- Screenshot Placeholder -->
          <div class="project-screenshot">
            <div class="ss-name">Trevecca-pedia</div>
            <div class="ss-sub">Screenshot coming soon</div>
          </div>

```

- [ ] **Step 2: Add Goalboard to sidebar "Other Projects"**

Find in `projects/trevecca-pedia.html`:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./huffman-encoding.html">&#9670; Huffman Encoding</a>
              <a href="./arkanoid.html">&#9670; Arkanoid Remake</a>
            </div>
          </div>
```
Replace with:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./huffman-encoding.html">&#9670; Huffman Encoding</a>
              <a href="./arkanoid.html">&#9670; Arkanoid Remake</a>
              <a href="./goalboard.html">&#9670; Goalboard</a>
            </div>
          </div>
```

### huffman-encoding.html

- [ ] **Step 3: Add screenshot placeholder before Overview**

Find in `projects/huffman-encoding.html`:
```html
          <!-- Overview -->
          <div class="content-section">
            <h2>Overview</h2>
```
Insert immediately before it:

```html
          <!-- Screenshot Placeholder -->
          <div class="project-screenshot">
            <div class="ss-name">Huffman Encoding</div>
            <div class="ss-sub">Screenshot coming soon</div>
          </div>

```

- [ ] **Step 4: Add Goalboard to sidebar "Other Projects"**

Find in `projects/huffman-encoding.html`:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./trevecca-pedia.html">&#9670; Trevecca-pedia</a>
              <a href="./arkanoid.html">&#9670; Arkanoid Remake</a>
            </div>
          </div>
```
Replace with:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./trevecca-pedia.html">&#9670; Trevecca-pedia</a>
              <a href="./arkanoid.html">&#9670; Arkanoid Remake</a>
              <a href="./goalboard.html">&#9670; Goalboard</a>
            </div>
          </div>
```

### arkanoid.html

- [ ] **Step 5: Add screenshot placeholder before Overview**

Find in `projects/arkanoid.html`:
```html
          <!-- Overview -->
          <div class="content-section">
            <h2>Overview</h2>
```
Insert immediately before it:

```html
          <!-- Screenshot Placeholder -->
          <div class="project-screenshot">
            <div class="ss-name">Arkanoid Remake</div>
            <div class="ss-sub">Screenshot coming soon</div>
          </div>

```

- [ ] **Step 6: Add Goalboard to sidebar "Other Projects"**

Find in `projects/arkanoid.html`:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./trevecca-pedia.html">&#9670; Trevecca-pedia</a>
              <a href="./huffman-encoding.html">&#9670; Huffman Encoding</a>
            </div>
          </div>
```
Replace with:
```html
          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./trevecca-pedia.html">&#9670; Trevecca-pedia</a>
              <a href="./huffman-encoding.html">&#9670; Huffman Encoding</a>
              <a href="./goalboard.html">&#9670; Goalboard</a>
            </div>
          </div>
```

- [ ] **Step 7: Verify in browser**

Open each of the three project pages and confirm:
- A dark 16:9 placeholder block labeled "[Project Name] / Screenshot coming soon" appears above the Overview section
- "Other Projects" sidebar in each page now lists Goalboard as a third link

- [ ] **Step 8: Commit**

```bash
git add projects/trevecca-pedia.html projects/huffman-encoding.html projects/arkanoid.html
git commit -m "feat: add screenshot placeholders and Goalboard sidebar links to existing project pages"
```

---

## Task 5: Create Goalboard project detail page

**Files:**
- Create: `projects/goalboard.html`

- [ ] **Step 1: Create `projects/goalboard.html`**

Create the file with the following content:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Goalboard — Diego Urquiola</title>
  <meta name="description" content="Goalboard: a soccer analytics mobile app built with React Native (Expo) and Python FastAPI, displaying league standings, match results, and team trends." />
  <link rel="icon" type="image/svg+xml" href="../assets/img/favicon.svg" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="../assets/css/style.css" />
</head>
<body>

  <header class="site-header">
    <div class="container header-inner">
      <a class="brand" href="../">
        <span class="dot" aria-hidden="true"></span>
        <span>Diego Urquiola</span>
      </a>
      <button class="nav-toggle" aria-label="Toggle navigation" aria-expanded="false">
        <span></span><span></span><span></span>
      </button>
      <nav class="nav" id="main-nav" aria-label="Primary navigation">
        <a href="../">Home</a>
        <a href="./index.html" class="active">Projects</a>
        <a href="../about.html">About</a>
        <a href="../resume.html">Resume</a>
        <a href="../contact.html">Contact</a>
      </nav>
    </div>
  </header>

  <main>

    <!-- Page Hero -->
    <section class="page-hero">
      <div class="container">
        <nav class="breadcrumb" aria-label="Breadcrumb">
          <a href="../">Home</a>
          <span aria-hidden="true">/</span>
          <a href="./index.html">Projects</a>
          <span aria-hidden="true">/</span>
          <span>Goalboard</span>
        </nav>
        <div class="tag-row" style="margin-bottom:14px">
          <span class="tag blue">Python</span>
          <span class="tag">React Native</span>
          <span class="tag">FastAPI</span>
          <span class="tag">Expo</span>
          <span class="tag green">In Progress</span>
        </div>
        <h1>Goalboard</h1>
        <p class="lead">
          A soccer analytics mobile app that surfaces live league standings, match
          results, and team performance trends &mdash; built with a React Native (Expo)
          front-end and a Python FastAPI back-end.
        </p>
      </div>
    </section>

    <!-- Content -->
    <section class="project-detail">
      <div class="container project-layout">

        <article class="project-main">

          <!-- Screenshot Placeholder -->
          <div class="project-screenshot">
            <div class="ss-name">Goalboard</div>
            <div class="ss-sub">Screenshot coming soon</div>
          </div>

          <!-- Overview -->
          <div class="content-section">
            <h2>Overview</h2>
            <p style="color:var(--text-secondary); line-height:1.75">
              Goalboard is a cross-platform mobile app for soccer fans who want real data
              without the noise. It pulls league standings, recent match results, and team
              performance trends from the Soccerdata API and presents them in a clean,
              navigable interface. The project was motivated by wanting to build something
              full-stack that I actually use &mdash; and by the challenge of connecting a
              Python data layer to a mobile front-end.
            </p>
            <p style="color:var(--text-secondary); line-height:1.75">
              This is my current project and the most technically broad thing I've built:
              it involves API design, data normalization, async mobile state management,
              and cross-platform UI &mdash; all wired together from scratch.
            </p>

            <div class="grid-3" style="margin-top:20px">
              <div class="card">
                <h3>Problem</h3>
                <p>Existing soccer apps are cluttered. I wanted a focused view: standings, results, and trends &mdash; nothing else.</p>
              </div>
              <div class="card">
                <h3>Solution</h3>
                <p>A FastAPI backend that normalizes Soccerdata output into clean REST endpoints, consumed by a React Native app.</p>
              </div>
              <div class="card">
                <h3>My Role</h3>
                <p>Sole developer. Responsible for backend API design, data modeling, and the full mobile UI.</p>
              </div>
            </div>
          </div>

          <!-- Tech Stack -->
          <div class="content-section">
            <h2>Tech Stack</h2>
            <div class="tech-grid">
              <span class="tech-badge">Python</span>
              <span class="tech-badge">FastAPI</span>
              <span class="tech-badge">Soccerdata API</span>
              <span class="tech-badge">React Native</span>
              <span class="tech-badge">Expo</span>
              <span class="tech-badge">REST APIs</span>
            </div>
          </div>

          <!-- What I Built -->
          <div class="content-section">
            <h2>What I Built</h2>
            <ul class="content-list">
              <li><strong>FastAPI REST backend</strong> &mdash; designed and implemented endpoints for league standings, recent matches, and team stats. Each endpoint normalizes raw Soccerdata output into a consistent response shape the app can rely on.</li>
              <li><strong>Soccerdata integration</strong> &mdash; wrapped the Soccerdata Python library to fetch live football data, handling rate limits and data normalization before passing results to the API layer.</li>
              <li><strong>React Native screens</strong> &mdash; built three core screens: Standings (tabular league table), Matches (recent results with scores), and Team Detail (aggregated performance stats for a selected team).</li>
              <li><strong>Expo configuration</strong> &mdash; set up the project for cross-platform deployment targeting both iOS and Android through Expo's managed workflow.</li>
              <li><strong>Async data fetching</strong> &mdash; managed API calls and loading states in React Native using async/await with proper error boundaries to handle network failures gracefully.</li>
              <li><strong>Data modeling</strong> &mdash; designed Pydantic models in FastAPI to validate and shape all API responses, making the front-end integration predictable.</li>
            </ul>
          </div>

          <!-- Architecture -->
          <div class="content-section">
            <h2>Architecture</h2>
            <p style="color:var(--text-secondary); margin-bottom:16px">How data flows from the source API to the mobile screen:</p>

            <div class="diagram">
              <div class="diagram-flow">
                <div class="dnode">Soccerdata API</div>
                <div class="darrow">&rarr;</div>
                <div class="dnode hi">FastAPI Backend</div>
                <div class="darrow">&rarr;</div>
                <div class="dnode">REST Endpoints</div>
                <div class="darrow">&rarr;</div>
                <div class="dnode green">React Native App</div>
              </div>
            </div>

            <div class="callout" style="margin-top:16px">
              <strong>Data flow:</strong> The React Native app makes HTTP requests to the FastAPI
              backend. The backend fetches raw data from the Soccerdata Python library,
              normalizes it into Pydantic-validated response models, and returns clean JSON.
              The front-end never touches raw Soccerdata output &mdash; the backend owns the
              data contract.
            </div>
          </div>

          <!-- Challenges + Learnings -->
          <div class="content-section">
            <h2>Challenges &amp; What I Learned</h2>
            <ul class="content-list">
              <li><strong>Soccerdata normalization</strong> &mdash; the Soccerdata library returns data in different shapes depending on the league and season. I learned to write defensive normalization code that handles missing fields rather than assuming consistent structure.</li>
              <li><strong>React Native async state</strong> &mdash; managing loading, error, and success states for multiple concurrent API calls requires clear patterns. I moved to a consistent fetch-on-mount with loading flag approach that's easy to reason about.</li>
              <li><strong>Pydantic as a contract</strong> &mdash; defining Pydantic models upfront forced me to think about the API contract before writing handler logic. This made the front-end integration much smoother because the shape was locked in early.</li>
              <li><strong>Cross-platform layout</strong> &mdash; React Native's Flexbox layout is close to CSS but not identical. I learned where the differences are (no grid, different default axis) through trial and error on the standings table layout.</li>
            </ul>
          </div>

        </article>

        <!-- Sidebar -->
        <aside class="project-sidebar">

          <div class="sidebar-card">
            <h4>Links</h4>
            <div class="sidebar-links">
              <a href="https://github.com/diegourquiola/goalboard" target="_blank" rel="noreferrer">
                &#9670; Repository
              </a>
            </div>
          </div>

          <div class="sidebar-card">
            <h4>Project Info</h4>
            <div style="display:grid; gap:10px">
              <div>
                <div style="font-size:11px; text-transform:uppercase; letter-spacing:.06em; color:var(--muted); margin-bottom:4px">Type</div>
                <div style="font-size:14px; font-weight:600">Solo / Current</div>
              </div>
              <div>
                <div style="font-size:11px; text-transform:uppercase; letter-spacing:.06em; color:var(--muted); margin-bottom:4px">Stack</div>
                <div style="font-size:14px; font-weight:600">Python + React Native</div>
              </div>
              <div>
                <div style="font-size:11px; text-transform:uppercase; letter-spacing:.06em; color:var(--muted); margin-bottom:4px">Status</div>
                <div style="font-size:14px; font-weight:600; color:var(--accent2)">In Progress</div>
              </div>
            </div>
          </div>

          <div class="sidebar-card">
            <h4>Other Projects</h4>
            <div class="sidebar-links">
              <a href="./trevecca-pedia.html">&#9670; Trevecca-pedia</a>
              <a href="./huffman-encoding.html">&#9670; Huffman Encoding</a>
              <a href="./arkanoid.html">&#9670; Arkanoid Remake</a>
            </div>
          </div>

        </aside>

      </div>
    </section>

  </main>

  <footer class="footer">
    <div class="container footer-inner">
      <p>&copy; <span id="year"></span> Diego Urquiola</p>
      <div class="footer-links">
        <a href="https://github.com/diegourquiola" target="_blank" rel="noreferrer">GitHub</a>
        <a href="https://www.linkedin.com/in/diegourquiola0806/" target="_blank" rel="noreferrer">LinkedIn</a>
        <a href="../contact.html">Contact</a>
      </div>
    </div>
  </footer>

  <script src="../assets/js/main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `projects/goalboard.html` in a browser and confirm:
- Page header: breadcrumb shows Home / Projects / Goalboard
- Tags: Python, React Native, FastAPI, Expo, In Progress (green)
- Screenshot placeholder appears above Overview
- Architecture diagram renders (4 nodes with arrows)
- Sidebar shows real GitHub link to `github.com/diegourquiola/goalboard`
- "Other Projects" sidebar shows all three other projects
- Navigation and footer render correctly

- [ ] **Step 3: Commit**

```bash
git add projects/goalboard.html
git commit -m "feat: add Goalboard project detail page"
```

---

## Task 6: Update projects index

**Files:**
- Modify: `projects/index.html`

- [ ] **Step 1: Add card screenshot thumbnail to Trevecca-pedia card**

In `projects/index.html`, find the opening of the Trevecca-pedia card's inner div:
```html
          <a href="./trevecca-pedia.html" class="project-card">
            <div>
              <div class="tag-row" style="margin-bottom:12px">
```
Replace with:
```html
          <a href="./trevecca-pedia.html" class="project-card">
            <div>
              <div class="card-screenshot">Screenshot coming soon</div>
              <div class="tag-row" style="margin-bottom:12px">
```

- [ ] **Step 2: Add card screenshot thumbnail to Huffman card**

Find:
```html
          <a href="./huffman-encoding.html" class="project-card">
            <div>
              <div class="tag-row" style="margin-bottom:12px">
```
Replace with:
```html
          <a href="./huffman-encoding.html" class="project-card">
            <div>
              <div class="card-screenshot">Screenshot coming soon</div>
              <div class="tag-row" style="margin-bottom:12px">
```

- [ ] **Step 3: Add card screenshot thumbnail to Arkanoid card**

Find:
```html
          <a href="./arkanoid.html" class="project-card">
            <div>
              <div class="tag-row" style="margin-bottom:12px">
```
Replace with:
```html
          <a href="./arkanoid.html" class="project-card">
            <div>
              <div class="card-screenshot">Screenshot coming soon</div>
              <div class="tag-row" style="margin-bottom:12px">
```

- [ ] **Step 4: Add Goalboard as fourth card**

Find the closing of the grid div (after the Arkanoid card closes):
```html
        </div>

      </div>
    </section>
```
Replace with:
```html
          <!-- Goalboard -->
          <a href="./goalboard.html" class="project-card">
            <div>
              <div class="card-screenshot">Screenshot coming soon</div>
              <div class="tag-row" style="margin-bottom:12px">
                <span class="tag blue">Python</span>
                <span class="tag">React Native</span>
                <span class="tag">FastAPI</span>
                <span class="tag">Expo</span>
              </div>
              <h3>Goalboard</h3>
              <p class="desc">
                A soccer analytics mobile app displaying live league standings, match
                results, and team performance trends. FastAPI backend consumes the
                Soccerdata API; React Native (Expo) front-end renders the data.
              </p>
            </div>
            <div class="project-card-footer">
              <div class="tag-row">
                <span class="tag">Mobile</span>
                <span class="tag">Full-Stack</span>
                <span class="tag green">In Progress</span>
              </div>
              <span class="project-link">Read more &rarr;</span>
            </div>
          </a>

        </div>

      </div>
    </section>
```

- [ ] **Step 5: Verify in browser**

Open `projects/index.html` in a browser and confirm:
- All four project cards appear in the grid
- Each card has a dark 16:9 thumbnail placeholder above the tag row
- Goalboard is the fourth card with "In Progress" green badge
- Clicking the Goalboard card navigates to `goalboard.html`

- [ ] **Step 6: Commit**

```bash
git add projects/index.html
git commit -m "feat: add Goalboard card and screenshot thumbnails to projects index"
```

---

## Task 7: LinkedIn section on about page

**Files:**
- Modify: `about.html`

- [ ] **Step 1: Add LinkedIn section after Education timeline**

In `about.html`, find:
```html
          <div style="margin-top:40px">
            <a class="btn primary" href="./resume.html">View Full Resume</a>
          </div>
```
Replace with:
```html
          <h2 style="margin:40px 0 24px">LinkedIn</h2>
          <div class="card" style="margin-bottom:24px">
            <div style="margin-bottom:16px">
              <div style="font-size:11px; text-transform:uppercase; letter-spacing:.06em; color:var(--muted); margin-bottom:4px">Profile URL</div>
              <div style="font-size:14px; font-weight:600; color:var(--accent)">linkedin.com/in/diegourquiola0806</div>
            </div>
            <p style="color:var(--text-secondary); font-size:14px; margin-bottom:16px">
              Connect with me or view my full professional profile.
            </p>
            <ul style="list-style:none; padding:0; margin:0 0 16px; display:grid; gap:6px; font-size:13px; color:var(--text-secondary)">
              <li style="display:flex; align-items:center; gap:8px"><span style="color:var(--accent2)">&#10003;</span> Professional photo &amp; headline</li>
              <li style="display:flex; align-items:center; gap:8px"><span style="color:var(--accent2)">&#10003;</span> Detailed experience descriptions</li>
              <li style="display:flex; align-items:center; gap:8px"><span style="color:var(--accent2)">&#10003;</span> Technical skills listed</li>
              <li style="display:flex; align-items:center; gap:8px"><span style="color:var(--accent2)">&#10003;</span> Projects referenced</li>
              <li style="display:flex; align-items:center; gap:8px"><span style="color:var(--accent2)">&#10003;</span> Customized URL</li>
            </ul>
            <a class="btn primary" href="https://www.linkedin.com/in/diegourquiola0806/" target="_blank" rel="noreferrer">View LinkedIn Profile &rarr;</a>
          </div>

          <div style="margin-top:0">
            <a class="btn primary" href="./resume.html">View Full Resume</a>
          </div>
```

- [ ] **Step 2: Verify in browser**

Open `about.html` in a browser and confirm:
- "LinkedIn" heading appears below the Education timeline
- Profile URL `linkedin.com/in/diegourquiola0806` shown in accent blue
- Five green checkmarks listing profile completeness signals
- "View LinkedIn Profile →" primary button appears and links to the correct URL
- "View Full Resume" button still appears below

- [ ] **Step 3: Commit**

```bash
git add about.html
git commit -m "feat: add LinkedIn profile card to about page"
```

---

## Self-Review Checklist

After all tasks are complete:

- [ ] Open `index.html` — hero badge, lead, meta card, and timeline all correct
- [ ] Open `about.html` — LinkedIn section visible, checklist and button present
- [ ] Open `projects/index.html` — 4 cards, all with thumbnails, Goalboard links correctly
- [ ] Open `projects/goalboard.html` — all sections present, GitHub link correct, nav/footer render
- [ ] Open `projects/trevecca-pedia.html` — screenshot placeholder visible, Goalboard in sidebar
- [ ] Open `projects/huffman-encoding.html` — screenshot placeholder visible, Goalboard in sidebar
- [ ] Open `projects/arkanoid.html` — screenshot placeholder visible, Goalboard in sidebar
- [ ] Narrow browser to < 900px — journey timeline stacks vertically on home page
