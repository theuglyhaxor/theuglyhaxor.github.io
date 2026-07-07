# Site Maintenance Guide — theuglyhaxor.github.io

Everything you need to update your portfolio and blog. No build tools, no
frameworks — just plain HTML/CSS/JS files you edit and push to GitHub.

---

## 1. What's in this repo

| File / folder | What it is |
|---|---|
| `index.html` | The main **portfolio** page (nav, hero, Experience, Education, Core Expertise, Projects, Certifications, Blog preview). |
| `blog.html` | The **blog archive** — lists every post with category filters. Posts are stored in a small JavaScript array inside this file. |
| `Mobile_Computing_Part-1_Study_Material.html` | A blog post (English study material). |
| `LPB-Chotoder_Rajniti_Chotoder_orthoniti.html` | A blog post (Bangla — politics & economics). |
| `certificates/` | PDF certificates linked from the Certifications section. |
| `image.png` | Your profile photo + favicon. |
| `logo.svg` | Logo used inside blog posts. |

**How the pieces connect:** the navbar on `index.html` links to `blog.html`.
`blog.html` lists all posts and links to each post file. Each post file has a
floating **← Blog / Portfolio** button to get back.

---

## 2. Preview locally before publishing

Just double-click any `.html` file to open it in your browser. To preview the
whole site with working links, from the repo folder run:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000> in your browser. Press `Ctrl+C` to stop.

---

## 3. Add a new blog post  ← most common task

A blog post is simply its own `.html` file. You create the file, then register
it in `blog.html` so it shows up in the archive.

### Step 1 — Add the post file
Save your article as an `.html` file in the **root folder** (next to
`index.html`). Use a clean name, e.g. `Linux-Privilege-Escalation.html`.

### Step 2 — Add the "back" button to the post (recommended)
Paste this **right after the `<body>` tag** of your new post. It floats a small
Blog / Portfolio button in the corner and hides itself when printing:

```html
<div class="site-backnav">
  <a href="blog.html">← Blog</a>
  <a href="index.html">Portfolio</a>
</div>
<style>
  .site-backnav{position:fixed;top:14px;right:14px;z-index:9999;display:flex;gap:8px;
    font-family:system-ui,Arial,sans-serif;}
  .site-backnav a{background:rgba(16,44,82,.92);color:#eaf2ff;text-decoration:none;
    font-size:13px;font-weight:600;padding:8px 14px;border-radius:999px;
    border:1px solid rgba(143,211,223,.4);box-shadow:0 6px 18px rgba(0,0,0,.28);}
  .site-backnav a:hover{background:#12b3c7;color:#04263a;}
  @media print{.site-backnav{display:none!important;}}
</style>
```

### Step 3 — Register the post in `blog.html`  ← the important one
Open `blog.html`, scroll to the `<script>` near the bottom, and find the
`posts` array. **Copy an existing block and edit the values**, placing it
**above** the `/* ---- ADD NEW POSTS ABOVE THIS LINE ---- */` comment:

```js
{
    title:   "Linux Privilege Escalation — Field Notes",
    category: "Security",        // group name → becomes a filter chip automatically
    catClass: "cat-security",    // badge colour (see table in section 4)
    excerpt: "A one or two sentence summary shown on the card.",
    meta:    "07 July 2026 · English",
    url:     "Linux-Privilege-Escalation.html"   // must match your filename exactly
}
```

- If it is **not** the first item in the list, add a **comma** between objects.
- Reuse an existing `category` value to group posts together, or invent a new
  one — a new filter chip (with its count) appears automatically.

### Step 4 — (Optional) Feature it on the homepage
The **"From the Blog"** box on `index.html` is a hand-picked, **English-only**
list. To feature the new post there, copy an existing `<a class="blog-card">`
block in that section and edit the category, title, summary and `href`. Skip
this if you only want the post in the archive.

### Step 5 — Publish
See section 8.

**Short version:** drop the `.html` file in the folder → add one object to the
`posts` array in `blog.html` → push. Steps 2 and 4 are optional polish.

---

## 4. Blog category badge colours (`catClass`)

Set `catClass` on each post to pick the badge colour:

| `catClass` | Colour | Good for |
|---|---|---|
| `cat-security` | Amber | Security / bug bounty |
| `cat-study` | Green | Study material / academic notes |
| `cat-politics` | Red | Politics & economics |
| `cat-tech` | Blue | Technology / dev |
| `cat-default` | Purple | Anything else |

Categories (the `category` text) drive the **filter chips**; the `catClass`
only changes the colour. Keep category names consistent so posts group cleanly.

---

## 5. Add a new project

Projects live in `index.html` inside the `<section id="projects">` block. Copy
an existing `.project-card` and edit it:

```html
<div class="project-card">
    <div class="project-head">
        <h3>Project Name</h3>
        <span class="tag featured">Featured</span>   <!-- optional; remove if not featured -->
    </div>
    <p>Short description of what the project does.</p>
    <div class="tag-row">
        <span class="tag">Python</span>
        <span class="tag">Automation</span>
    </div>
    <div class="project-links">
        <a href="https://github.com/theuglyhaxor/REPO-NAME" target="_blank">↗ View on GitHub</a>
    </div>
</div>
```

---

## 6. Add a new certification

1. Put the PDF in the `certificates/` folder.
2. In `index.html`, inside `<section id="certifications">`, copy the existing
   `.cert-card` and edit the title, issuer, date, description, and the PDF path
   in the `href`:

```html
<a class="cert-link" href="certificates/YOUR-FILE.pdf" target="_blank">
    View Certificate ↗
</a>
```

---

## 7. Edit the portfolio content

All in `index.html`. The page sections, in order, are:

1. **Navbar** — links at the very top.
2. **Hero** — name, subtitle, intro paragraph, and the button row
   (GitHub / Bugcrowd / LinkedIn / YouTube).
3. `#experience` — job/internship timeline items.
4. `#education` — degree info.
5. `#skills` — **Core Expertise** bullet list (`<ul class="expertise-list">`).
   Add a bullet: `<li><b>Topic</b> — short description</li>`.
6. `#projects` — see section 5.
7. `#certifications` — see section 6.
8. `#blog` — the homepage "From the Blog" preview (English-only, hand-picked).

To change your photo, replace `image.png` (keep the same filename).
To change social links, edit the `<a class="btn">` tags in the hero.

---

## 8. Publish changes (go live)

GitHub Pages serves this repo, so pushing to the `main` branch publishes it.

```bash
git add .
git commit -m "Add blog post: Linux Privilege Escalation"
git push
```

Your changes are live at <https://theuglyhaxor.github.io> within about a minute.
(To check status: `git status` shows what changed before you commit.)

---

## 9. Tips & gotchas

- **Filenames matter.** The `url` in `blog.html` and every `href` must match the
  real filename **exactly**, including capitalisation.
- **Commas in the `posts` array.** Every object except the last needs a trailing
  comma. A missing/extra comma makes the blog list go blank — if that happens,
  open the browser Console (F12) to see the error.
- **Keep the favicon/photo filename** as `image.png` so links keep working.
- **Bangla / non-English posts** still work fine in the archive; the homepage
  preview is intentionally English-only.
- **Test before pushing** using the local server in section 2.
