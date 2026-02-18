# Diego Urquiola — Portfolio Site

A static portfolio site built with plain HTML, CSS, and JavaScript — no build tools, no frameworks. Hosted on GitHub Pages.

---

## Project Website

**Live URL:** `https://diegourquiola08.github.io/diegourquiola.github.io/`
*(or the root URL if this is your user pages repo: `https://diegourquiola08.github.io/`)*

The site includes:

| Page | Path |
|---|---|
| Landing / Home | `index.html` |
| Projects listing | `projects/index.html` |
| Trevecca-pedia writeup | `projects/trevecca-pedia.html` |
| Huffman Encoding writeup | `projects/huffman-encoding.html` |
| Arkanoid Remake writeup | `projects/arkanoid.html` |
| About + Skills | `about.html` |
| Resume | `resume.html` |
| Contact | `contact.html` |

---

## Enable GitHub Pages

1. Go to your repo on GitHub.
2. Click **Settings** → **Pages** (left sidebar).
3. Under **Branch**, select **`main`** and folder **`/ (root)`**.
4. Click **Save**.
5. GitHub will give you the live URL within a minute or two.

> If this is a **project repo** (not `username.github.io`), the site will be at
> `https://diegourquiola08.github.io/<repo-name>/`
> All internal links use relative paths, so they work in both cases.

---

## File Structure

```
.
├── index.html               # Landing page
├── about.html               # About + Skills + Experience
├── resume.html              # Resume page (add assets/resume.pdf)
├── contact.html             # Contact page
├── projects/
│   ├── index.html           # Projects listing
│   ├── trevecca-pedia.html  # Project detail
│   ├── huffman-encoding.html
│   └── arkanoid.html
├── assets/
│   ├── css/
│   │   └── style.css        # All styles (single shared stylesheet)
│   ├── js/
│   │   └── main.js          # Mobile nav, smooth scroll, year
│   ├── img/
│   │   └── favicon.svg      # SVG favicon
│   └── resume.pdf           # ← Add your resume here (see below)
└── style.css                # Legacy file — no longer used, safe to delete
```

---

## How to Add Your Resume

1. Export your resume as a PDF.
2. Name it `resume.pdf` and place it at `assets/resume.pdf`.
3. Open `resume.html` and:
   - Remove the `.resume-placeholder` `<div>`.
   - Uncomment the `<iframe src="./assets/resume.pdf" ...>` line.
4. The PDF will embed inline and the "Download PDF" button will work automatically.

---

## How to Edit Pages and Styles

### Add a new project

1. Copy `projects/huffman-encoding.html` to `projects/<slug>.html`.
2. Update the title, description, tech stack, and content sections.
3. Add a card for it in `projects/index.html` and `index.html` (Featured Projects).

### Change colors or typography

All design tokens are CSS variables at the top of `assets/css/style.css`:

```css
:root {
  --bg:      #0b0f17;   /* page background */
  --accent:  #60a5fa;   /* primary blue    */
  --accent2: #34d399;   /* green           */
  --text:    #e5e7eb;   /* body text       */
  /* ... */
}
```

Change a variable and it updates everywhere.

### Update contact info

- **Email:** search `resume.html` and `contact.html` for `your.email@example.com` and replace.
- **LinkedIn:** search all HTML files for `linkedin.com/in/diegourquiola0806` and update the URL.
- **GitHub:** already set to `github.com/DeLsonJabberwo` — update if your username changes.

---

## Tech

- Pure HTML5, CSS3, vanilla JavaScript
- [Inter](https://fonts.google.com/specimen/Inter) from Google Fonts
- No npm, no build step — edit and push directly
