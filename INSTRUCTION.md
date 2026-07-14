# Site Maintenance Guide — theuglyhaxor.github.io

Everything you need to update your site and add write-ups. No build tools, no
frameworks — just plain HTML/CSS/JS files you edit and push to GitHub.

---

## 1. What's in this repo

| File / folder | What it is |
|---|---|
| `index.html` | **Home page = your Portfolio.** Hero, About, Experience, Skills, Projects, Certifications, a Write-ups call-to-action, and Contact. This is what visitors see first at `theuglyhaxor.github.io`. |
| `writeups.html` | **Your Write-ups page.** A filterable grid of all posts. **Posts are stored in a small JavaScript array inside this file** (see section 3). Reached from the portfolio's "Write-ups" nav link and buttons. |
| `cv.html` | **Printable one-page CV** (A4). Open it and click "Print / Save as PDF". Uses `avatar.png`. |
| `portfolio.html` | Legacy address — now **redirects to `index.html`** so old links keep working. Don't need to touch it. |
| `blog.html` | Legacy address — now **redirects to `writeups.html`**. Don't need to touch it. |
| `Mobile_Computing_Part-1_Study_Material.html` | A write-up (English — study material). |
| `Mental_Health_ADHD_BI_POLAR.html` | A write-up (Bangla — mental health). |
| `LPB-Chotoder_Rajniti_Chotoder_orthoniti.html` | A write-up (Bangla — politics & economics). |
| `certificates/` | PDF certificates linked from the portfolio's Certifications section. |
| `avatar.png` | Your circular profile avatar (used on `index.html` and `cv.html`). |
| `image.png` | Older profile photo / favicon (still referenced by a couple of pages). |
| `cover.png`, `logo.svg` | Social cover image and logo used inside write-ups. |

**How the pieces connect:** `index.html` (portfolio) links to `writeups.html`
(the list of every post) via the nav and the "See all write-ups" button. Each
write-up file has a floating **← Write-ups / Portfolio** button to get back.
`cv.html` is the printable CV.

---

## 2. Preview locally before publishing

Double-click any `.html` file to open it in your browser. To preview the whole
site with working links, from the repo folder run:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000> in your browser. Press `Ctrl+C` to stop.

---

## 3. Add a new write-up  ← most common task

A write-up is simply its own `.html` file. You create the file, then register it
in **`writeups.html`** so it shows up on the Write-ups page.

### Step 1 — Add the post file
Save your article as an `.html` file in the **root folder** (next to
`index.html`). Use a clean name, e.g. `IDOR-in-Acme-API.html`.

### Step 2 — Add the "back" button to the post (recommended)
Paste this **right after the `<body>` tag** of your new post. It floats a small
Write-ups / Portfolio button in the corner and hides itself when printing:

```html
<div class="site-backnav">
  <a href="writeups.html">← Write-ups</a>
  <a href="index.html">Portfolio</a>
</div>
<style>
  .site-backnav{position:fixed;top:14px;right:14px;z-index:9999;display:flex;gap:8px;
    font-family:system-ui,Arial,sans-serif;}
  .site-backnav a{background:rgba(15,23,42,.92);color:#eaf2ff;text-decoration:none;
    font-size:13px;font-weight:600;padding:8px 14px;border-radius:999px;
    border:1px solid rgba(143,180,255,.4);box-shadow:0 6px 18px rgba(0,0,0,.3);}
  .site-backnav a:hover{background:#22d3ee;color:#04121a;}
  @media print{.site-backnav{display:none!important;}}
</style>
```

> Tip: if your post already has a fixed toolbar in the top-right corner, change
> `right:14px` to `left:14px` so they don't overlap.

### Step 3 — Register the post in `writeups.html`  ← the important one
Open `writeups.html`, scroll to the `<script>` near the bottom, and find the
`posts` array. **Copy an existing block and edit the values**, placing it
**above** the `/* ---- ADD NEW POSTS ABOVE THIS LINE ---- */` comment:

```js
{
    title:    "IDOR in Acme API — Full Account Takeover",
    category: "Security",        // group name → becomes a filter chip automatically
    catClass: "cat-security",    // badge colour (see table in section 4)
    excerpt:  "A one or two sentence summary shown on the card.",
    meta:     "English · Bug Bounty",
    url:      "IDOR-in-Acme-API.html"   // must match your filename exactly
}
```

- Posts appear **in the order listed** — put the newest at the **top** of the array.
- If it is **not** the first item, add a **comma** between objects.
- Reuse an existing `category` value to group posts together, or invent a new
  one — a new filter chip (with its count) appears automatically.

### Step 4 — Publish
See section 7.

**Short version:** drop the `.html` file in the folder → add one object to the
top of the `posts` array in `writeups.html` → push. Step 2 is optional polish.

---

## 4. Category badge colours (`catClass`)

Set `catClass` on each post to pick the badge colour:

| `catClass` | Colour | Good for |
|---|---|---|
| `cat-security` | Amber | Security / bug bounty |
| `cat-study` | Green | Study material / academic notes |
| `cat-politics` | Red | Politics & economics |
| `cat-psychology` | Pink | Mental health / psychology |
| `cat-tech` | Blue | Technology / dev |
| `cat-default` | Purple | Anything else |

The `category` text drives the **filter chips**; `catClass` only changes the
colour. Keep category names consistent so posts group cleanly.

---

## 5. Edit the portfolio (home page)

All portfolio content lives in `index.html`:

- **Roles / tagline** — the animated `roles` list is in the `<script>` at the
  bottom; the visible role line is in the hero (`<div class="role">`).
- **About / Skills** — edit the chips inside the `#about` and `#skills` sections.
- **Add a project** — copy an existing `.card` inside `<section id="projects">`.
- **Add a certification** — put the PDF in `certificates/`, then edit the card in
  `<section id="certifications">` (title, issuer, date, and the PDF path in `href`).
- **Photo** — replace `avatar.png` (keep the same filename).

---

## 6. Edit the CV

`cv.html` is a self-contained printable CV. Edit the sidebar (contact, skills,
tools) and the main column (summary, experience, education) directly in the HTML,
then open it and use **Print / Save as PDF** (A4) to export.

---

## 7. Publish changes (go live)

GitHub Pages serves this repo, so pushing to the `main` branch publishes it.

```bash
git add .
git commit -m "Add write-up: IDOR in Acme API"
git push
```

Your changes are live at <https://theuglyhaxor.github.io> within about a minute.
(To check status: `git status` shows what changed before you commit.)

---

## 8. Tips & gotchas

- **Filenames matter.** The `url` in `writeups.html` and every `href` must match
  the real filename **exactly**, including capitalisation.
- **Commas in the `posts` array.** Every object except the last needs a trailing
  comma. A missing/extra comma makes the list go blank — if that happens, open
  the browser Console (F12) to see the error.
- **Keep `avatar.png`** as the filename for the profile photo so links keep working.
- **Bangla / non-English posts** work fine — titles and excerpts can be in any language.
- **Test before pushing** using the local server in section 2.
