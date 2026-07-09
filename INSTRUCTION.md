# Site Maintenance Guide — theuglyhaxor.github.io

Everything you need to update your site and add write-ups. No build tools, no
frameworks — just plain HTML/CSS/JS files you edit and push to GitHub.

---

## 1. What's in this repo

| File / folder | What it is |
|---|---|
| `index.html` | **Home page = your Write-ups.** A minimal header (name, job title, Portfolio + social links) followed by the grid of all posts with category filters. **Posts are stored in a small JavaScript array inside this file.** |
| `portfolio.html` | Your full **portfolio / CV** page (Experience, Education, Core Expertise, Projects, Certifications). Linked from the header. |
| `blog.html` | Legacy address — now just **redirects to `index.html`** so old links keep working. You don't need to touch it. |
| `Mental_Health_ADHD_BI_POLAR.html` | A write-up (Bangla — mental health). |
| `LPB-Chotoder_Rajniti_Chotoder_orthoniti.html` | A write-up (Bangla — politics & economics). |
| `Mobile_Computing_Part-1_Study_Material.html` | A write-up (English study material). |
| `certificates/` | PDF certificates linked from the portfolio's Certifications section. |
| `image.png` | Your profile photo + favicon. |
| `logo.svg` | Logo used inside write-ups. |

**How the pieces connect:** `index.html` (the home page) lists every write-up
and links to each post file. The header links out to `portfolio.html` and your
socials. Each post file has a floating **← Write-ups / Portfolio** button to get
back.

---

## 2. Preview locally before publishing

Just double-click any `.html` file to open it in your browser. To preview the
whole site with working links, from the repo folder run:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000> in your browser. Press `Ctrl+C` to stop.

---

## 3. Add a new write-up  ← most common task

A write-up is simply its own `.html` file. You create the file, then register it
in `index.html` so it shows up on the home page.

### Step 1 — Add the post file
Save your article as an `.html` file in the **root folder** (next to
`index.html`). Use a clean name, e.g. `Linux-Privilege-Escalation.html`.

### Step 2 — Add the "back" button to the post (recommended)
Paste this **right after the `<body>` tag** of your new post. It floats a small
Write-ups / Portfolio button in the corner and hides itself when printing:

```html
<div class="site-backnav">
  <a href="index.html">← Write-ups</a>
  <a href="portfolio.html">Portfolio</a>
</div>
<style>
  .site-backnav{position:fixed;top:14px;right:14px;z-index:9999;display:flex;gap:8px;
    font-family:system-ui,Arial,sans-serif;}
  .site-backnav a{background:rgba(15,23,42,.92);color:#eaf2ff;text-decoration:none;
    font-size:13px;font-weight:600;padding:8px 14px;border-radius:999px;
    border:1px solid rgba(143,180,255,.4);box-shadow:0 6px 18px rgba(0,0,0,.3);}
  .site-backnav a:hover{background:#2563eb;color:#fff;}
  @media print{.site-backnav{display:none!important;}}
</style>
```

> Tip: if your post already has a fixed toolbar in the top-right corner, change
> `right:14px` to `left:14px` so they don't overlap.

### Step 3 — Register the post in `index.html`  ← the important one
Open `index.html`, scroll to the `<script>` near the bottom, and find the
`posts` array. **Copy an existing block and edit the values**, placing it
**above** the `/* ---- ADD NEW POSTS ABOVE THIS LINE ---- */` comment:

```js
{
    title:    "Linux Privilege Escalation — Field Notes",
    category: "Security",        // group name → becomes a filter chip automatically
    catClass: "cat-security",    // badge colour (see table in section 4)
    excerpt:  "A one or two sentence summary shown on the card.",
    meta:     "07 July 2026 · English",
    url:      "Linux-Privilege-Escalation.html"   // must match your filename exactly
}
```

- Posts appear **in the order listed** — put the newest at the **top** of the array.
- If it is **not** the first item, add a **comma** between objects.
- Reuse an existing `category` value to group posts together, or invent a new
  one — a new filter chip (with its count) appears automatically.

### Step 4 — Publish
See section 7.

**Short version:** drop the `.html` file in the folder → add one object to the
top of the `posts` array in `index.html` → push. Step 2 is optional polish.

---

## 4. Category badge colours (`catClass`)

Set `catClass` on each post to pick the badge colour:

| `catClass` | Colour | Good for |
|---|---|---|
| `cat-security` | Amber | Security / bug bounty |
| `cat-study` | Green | Study material / academic notes |
| `cat-politics` | Red | Politics & economics |
| `cat-tech` | Blue | Technology / dev |
| `cat-psychology` | Pink | Mental health / psychology |
| `cat-default` | Purple | Anything else |

The `category` text drives the **filter chips**; `catClass` only changes the
colour. Keep category names consistent so posts group cleanly.

---

## 5. Edit the header (job title & social links)

The header lives at the top of `index.html` inside `<header class="masthead">`:

- **Job title** — edit the text in `<div class="mast-role">Bug Bounty Hunter · OSINT Expert</div>`.
- **Links** — each social is an `<a>` inside `<nav class="mast-nav">`. Edit the
  `href` to change a link, or copy an `<a>…</a>` block to add another. The first
  one (`class="cta"`) is the highlighted **Portfolio** button.
- **Photo** — replace `image.png` (keep the same filename).

---

## 6. Edit the portfolio (CV) page

All portfolio content — Experience, Education, Core Expertise, Projects,
Certifications, and the hero button row (GitHub / Bugcrowd / LinkedIn / YouTube) —
lives in `portfolio.html`.

- **Add a project:** copy an existing `.project-card` inside `<section id="projects">`.
- **Add a certification:** put the PDF in `certificates/`, then copy an existing
  `.cert-card` inside `<section id="certifications">` and update the title,
  issuer, date, description and the PDF path in the `href`.
- **Add an expertise bullet:** add `<li><b>Topic</b> — short description</li>`
  inside `<ul class="expertise-list">`.

---

## 7. Publish changes (go live)

GitHub Pages serves this repo, so pushing to the `main` branch publishes it.

```bash
git add .
git commit -m "Add write-up: Linux Privilege Escalation"
git push
```

Your changes are live at <https://theuglyhaxor.github.io> within about a minute.
(To check status: `git status` shows what changed before you commit.)

---

## 8. Tips & gotchas

- **Filenames matter.** The `url` in `index.html` and every `href` must match the
  real filename **exactly**, including capitalisation.
- **Commas in the `posts` array.** Every object except the last needs a trailing
  comma. A missing/extra comma makes the list go blank — if that happens, open
  the browser Console (F12) to see the error.
- **Keep the favicon/photo filename** as `image.png` so links keep working.
- **Bangla / non-English posts** work fine — titles and excerpts can be in any
  language.
- **Test before pushing** using the local server in section 2.
