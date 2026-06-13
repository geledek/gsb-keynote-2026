# Slide Editing Guide

Single-file HTML deck. One `<section class="slide">` per slide. Everything else is global — don't touch it unless you know what you're doing.

---

## The one rule

**Edit only inside one `<section class="slide">…</section>` block at a time.**

Use inline `style=""` for one-off changes. Use existing classes for layout. Never touch the `<style>` block or the `<script>` block at the bottom unless adding a new recipe.

---

## Safe edits (inside a slide)

| Change | How |
|---|---|
| Text content | Edit the text directly |
| Font size / spacing | `style="font-size:min(5vw,8vh)"` |
| Color | `style="color:var(--accent)"` or `style="color:#fff"` |
| Swap an image | Change the `src="images/..."` path |
| Add a bullet | Copy an existing `<li class="bullet-item">` block |

## Never do this

- Global find/replace on CSS values
- Delete CSS rules because they "look unused"
- Change `--accent` in `:root` (affects every slide)
- Push directly to `main` — always PR through `preview`

---

## Current deck structure

| # | Slide | `data-animate` | Notes |
|---|---|---|---|
| 1 | Cover | `hero` | Cardinal red, ASCII bg |
| 2 | 95% Hook | `hook-95` | Static — no click |
| 3 | J-Curve | `jcurve-fresh` | Auto-animates on entry |
| 4 | Iceberg | `iceberg-img` | Photo bg, dark slide |
| 5 | Definition | `definition` | 5-click word transforms |
| 6 | House of Enterprise AI | `pillar-fade` | Cardinal, church image |
| 7 | Technology | `bullet-reveal` | 4-click bullets |
| 8 | Process (tracks) | `process-tracks` | Auto-animates |
| 9 | Workflow Steps | `flow-reveal` | 6-click flowchart |
| 10 | People | `pillar-fade` | Table + training bar |
| 11 | Governance | `pillar-fade` | Cardinal, hero stats |
| 12 | FAKTS (Arman) | `bullet-reveal` | People resistance framework |
| … | … | … | … |
| Last | Skills | `pillar-fade` | Cardinal, repo CTA |

---

## Click-reveal slides

These slides require clicks to advance:

- **Definition (S5):** 5 clicks — each reveals one framework word
- **Technology (S7):** 4 clicks — one bullet per click
- **Workflow Steps (S9):** 6 clicks — one flowchart box per click

All other slides auto-animate on entry.

---

## Adding a new slide

1. Copy an existing `<section>` block with a similar layout
2. Change the content inside — keep `data-animate` matching an existing recipe
3. Put any images in `images/` and reference as `src="images/filename.png"`
4. Test locally: `open gsb-keynote-2026-06-16.html`
5. Push to `preview` branch, not `main`

---

## Images

Only `images/04-iceberg-bg.png` is currently used. Add new images to `images/` with a descriptive lowercase-kebab filename.

---

## Deployment

| Branch | URL |
|---|---|
| `main` | https://gsb-eai-ppt.vercel.app |
| `preview` | Unique URL per push (shown in GitHub Actions) |
