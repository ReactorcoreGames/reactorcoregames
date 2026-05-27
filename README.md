# reactorcoregames.github.io

Personal homepage for **Reactorcore** — game designer, developer, modder, 2D/3D artist, and AI prompt engineer. Serves as a home base replacing Linktree, and hosts a privacy policy required for Pinterest API access.

**Live site:** https://reactorcoregames.github.io

---

## Pages

| Page | URL | Description |
|---|---|---|
| Home | `/` | Link hub — bio, social links, support/hire links, contact |
| Catalogue | `/catalogue/` | All 227 projects across every platform, searchable and grouped by domain |
| Privacy Policy | `/privacy/` | Required for Pinterest API app registration |

---

## Tech stack

- Pure HTML + CSS + vanilla JS
- No frameworks, no build tools, no npm
- Hosted on GitHub Pages (push to `main` → auto-deploys)

---

## Local preview

Run the included launcher:
```
launch-site.bat
```
Or manually:
```
python -m http.server 8080
```
Then open `http://localhost:8080` in a browser. A local server is required because the catalogue page fetches `assets/posts.csv` via `fetch()`, which browsers block on `file://` URLs.

---

## Updating the catalogue

The catalogue page is driven entirely by `assets/posts.csv`. To add, remove, or edit entries, overwrite that file. The page re-renders on next load — no build step needed.

CSV format:
```
title,url,hashtags
My Project,https://example.com/project,#tag1 #tag2 #tag3
```

---

## Updating links or text

All links and copy live directly in the HTML files — no CMS, no data layer. Edit `index.html` for the home page, `privacy/index.html` for the privacy policy.

**Disabled links** (commented out in `index.html`): Unity Asset Store and Rebrickable are present but commented out. Uncomment the block in the "Other highlights" island to re-enable them.

---

## Changing colors

All color values are CSS variables defined at the top of `assets/style.css`. Edit there — don't touch the partial files for color changes.

---

## File structure

```
index.html
favicon.ico
robots.txt
sitemap.xml
privacy/
    index.html
catalogue/
    index.html
assets/
    style.css          ← CSS entry point (@imports partials)
    style-base.css     ← Variables, reset, island + button base
    style-index.css    ← Home page layout
    style-privacy.css  ← Privacy page layout
    style-catalogue.css ← Catalogue layout
    style-responsive.css ← Media queries
    posts.csv          ← Catalogue data (227 entries)
    banner.jpg
    collage.jpg
    mood1.jpg / mood2.jpg
    qr-small.png / qr-large.png
    icons/             ← Platform icons
```
