# GSB Keynote 2026 — Enterprise AI Skills in Practice

**Live deck:** https://gsb-eai-ppt.vercel.app  
**Preview:** push to the `preview` branch → auto-deploys to a staging URL  
**Event:** Stanford GSB Chapter Singapore · 2026-06-16

---

## Quick start

```bash
git clone https://github.com/geledek/gsb-keynote-2026.git
cd gsb-keynote-2026
open gsb-keynote-2026-06-16.html   # view locally in browser
```

No build step. The deck is a single self-contained HTML file.

---

## Branch strategy

| Branch | Purpose | Auto-deploys to |
|---|---|---|
| `main` | Production — what the audience sees | https://gsb-eai-ppt.vercel.app |
| `preview` | Staging — review before going live | Unique Vercel preview URL per push |
| `feature/*` | Individual slide edits | Unique Vercel preview URL per PR |

---

## How to make a change

### For a quick slide edit

```bash
git checkout preview
# edit gsb-keynote-2026-06-16.html
git add gsb-keynote-2026-06-16.html
git commit -m "slide: describe what you changed"
git push
# → staging URL appears in GitHub Actions output
```

### For a larger change (recommended)

```bash
git checkout -b feature/your-change-name
# make edits
git add .
git commit -m "slide: describe what you changed"
git push -u origin feature/your-change-name
# → open a PR to preview on GitHub
# → merge PR to preview to test, then merge preview → main to go live
```

---

## Commit message conventions

```
slide: short description of what changed
fix:   correct a typo, broken layout, broken animation
image: add or swap a slide image
ci:    changes to deployment pipeline
```

Examples:
```
slide: add governance hero message with 35%/18% stats
fix: align training system bar on S11 people slide
image: replace iceberg SVG with photo background
```

---

## File structure

```
gsb-keynote-2026-06-16.html   ← the entire deck (single file)
images/
  *.png / *.jpg               ← slide hero images
  papers/                     ← paper cover thumbnails for carousel
assets/
  motion.min.js               ← animation library
vercel.json                   ← Vercel deployment config
.github/workflows/deploy.yml  ← CI/CD pipeline
```

---

## Navigation (in browser)

| Key | Action |
|---|---|
| `→` / `←` | Next / previous slide |
| `Space` or click | Advance click-reveal stages |
| `ESC` | Slide index overview |
| `B` | Toggle low-power mode (disables animations) |

---

## Adding an image

1. Put the image in `images/` (use lowercase-kebab filename, e.g. `12-strategy-hero.jpg`)
2. Reference it in the HTML as `src="images/12-strategy-hero.jpg"`
3. Commit and push — Vercel picks it up automatically

---

## Collaborators

- Ray Han ([@geledek](https://github.com/geledek)) — author
- Arman Tan ([@ArmanTan](https://github.com/ArmanTan)) — collaborator
