# reactorcoregames.github.io — Maintenance Context

## Why this exists

Pinterest rejected Reactorcore's API app registration because a GitHub repo link didn't qualify as a real privacy policy page. The solution was a proper GitHub Pages site at `https://reactorcoregames.github.io` with a privacy policy at `https://reactorcoregames.github.io/privacy`. This also replaces/supplements the Linktree at `https://linktr.ee/reactorcore` as Reactorcore's primary online home base.

Repo name: `reactorcoregames.github.io` (GitHub Pages auto-serves it at the URL above).

Once live, resubmit the Pinterest app registration with:
- Company website: `https://reactorcoregames.github.io`
- Privacy policy: `https://reactorcoregames.github.io/privacy`

---

## Owner context

- Solo developer — games, mods, tools, 2D/3D art, LEGO mecha builds, AI-assisted creative work
- Not a company — no employees, no org, no servers, no user data
- Privacy-conscious: no tracking, no analytics, no cookies anywhere on this site
- Contact: reactorcoregames@gmail.com

---

## File structure

```
index.html             ← Main page (home/linktree)
favicon.ico            ← Must stay at root
robots.txt
sitemap.xml
launch-site.bat        ← Local dev: runs Python HTTP server on port 8080
privacy/
    index.html         ← Privacy policy (Pinterest requirement)
catalogue/
    index.html         ← Dynamic catalogue (fetches posts.csv)
assets/
    style.css          ← Entry point — imports all partials via @import
    style-base.css     ← CSS variables, reset, island/button base classes
    style-index.css    ← Home page layout only
    style-privacy.css  ← Privacy page layout only
    style-catalogue.css ← Catalogue card layout only
    style-responsive.css ← All media queries (3 breakpoints)
    posts.csv          ← 227 entries: title, url, hashtags
    banner.jpg         ← Hero image (top of home page)
    collage.jpg        ← 5×5 project grid (about section)
    mood1.jpg          ← Atmospheric full-width image
    mood2.jpg          ← Lower-priority atmospheric image
    design-banner.jpg  ← AI ambient image collage (lower priority)
    qr-small.png       ← 64px QR → Linktree
    qr-large.png       ← 512px QR → used on privacy page
    icons/             ← 18 platform/social icons (all .jpg)
```

---

## CSS architecture

**All CSS variables live in `assets/style.css`** (the `:root` block). Edit colors there — not in the partials.

### Island color system

Three island types, each with a base color, highlight (top/left border), and lowlight (bottom/right border) to create a beveled panel look:

| Class | Background | Use |
|---|---|---|
| `island-red` | `#4a1a1a` | Content/stuff section |
| `island-green` | `#1a3d22` | About, socials, other highlights |
| `island-blue` | `#162040` | Work/professional, contact, footer |

### Button color rule (cyclic)

Buttons contrast with their parent island:

| Island | Button class | Text color |
|---|---|---|
| Red island | `.btn-green` | `#7dffaa` |
| Green island | `.btn-blue` | `#7ab8ff` |
| Blue island | `.btn-red` | `#ff8888` |

Never put a green button on a green island. The contrast cycle is: red→green, green→blue, blue→red.

### Neon accent

`--neon: #ff6a00` (orange) — used for button hover glows. **Not** used for the animated background — that uses a blue/cyan flow (`#007bff`). Don't assume the neon variable drives the background animation.

### Breakpoints (in `style-responsive.css`)

| Width | Change |
|---|---|
| ≤1100px | 2-column layout |
| ≤860px | Mobile — single column, stacked islands |
| ≤560px | Small phones — further padding/font reduction |

---

## Animated background

`.animated-bg` in `style-base.css` — a fixed full-viewport div with a vertically scrolling dark/blue gradient. Pure CSS animation (`bgFlow` keyframe), no JS. Z-index: -1.

---

## Catalogue page

**How it works:**
1. Page loads → JS fetches `/assets/posts.csv`
2. CSV is parsed client-side (custom parser that handles quoted fields)
3. Entries are grouped by domain (`new URL(url).hostname`)
4. Each domain gets a color from an 8-entry ROYGBIV+Brown palette (domain string → hash → palette index)
5. Each domain renders as a collapsible `<details>` element
6. Each entry renders as a card: title (as link) + hashtags row

**How to update the catalogue:** Overwrite `assets/posts.csv`. The page re-renders automatically on next load. CSV columns: `title`, `url`, `hashtags`.

**8-color domain palette:**

| Border | Background |
|---|---|
| `#ff4444` | `#7a1f1f` |
| `#ff8c00` | `#7a3d00` |
| `#e6d000` | `#5a5200` |
| `#44cc44` | `#1a4a1a` |
| `#4488ff` | `#1a2a5a` |
| `#7755ee` | `#2a1a5a` |
| `#cc44cc` | `#4a1a4a` |
| `#aa7744` | `#3d2a10` |

---

## Disabled/hidden links

Two links are commented out in `index.html` inside the "Other highlights" island (row-5). To re-enable, uncomment the block between `<!-- Disabled links` and `-->`:

- Unity Asset Store — `https://assetstore.unity.com/publishers/3412`
- LEGO MOCs (Rebrickable) — `https://rebrickable.com/users/Reactorcore/`

---

## Technical constraints (do not violate)

- No JS frameworks, no npm, no build steps — pure HTML + CSS + vanilla JS
- JS is only used in `catalogue/index.html` for CSV fetch/render
- No inline `<style>` blocks in HTML — all CSS goes in the `assets/` partials
- No analytics, no tracking scripts, no cookies — anywhere
- All new colors as CSS variables in `assets/style.css`; don't hardcode colors in partials
- Icon images: fixed square size via CSS, `object-fit: cover` — don't add width/height attributes

---

## Deployment

Push to the `main` branch of the `reactorcoregames.github.io` GitHub repo. GitHub Pages auto-deploys. No build step, no CI needed.

To preview locally: run `launch-site.bat` (starts Python HTTP server on port 8080) or manually run `python -m http.server 8080` from the project root.

---

## What not to do

- Don't add a `<style>` tag inside any HTML file
- Don't add Google Analytics, Cloudflare Web Analytics, or any tracking pixel
- Don't add a `package.json` or any build tooling
- Don't add a navbar or hamburger menu — the site has no persistent nav; each subpage has its own back-to-home button. This was a deliberate removal for cleaner layout and better space use.
- Don't add server-side code — this is static files only (GitHub Pages)
