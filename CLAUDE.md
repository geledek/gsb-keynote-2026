# CLAUDE.md

Guidance for Claude Code (and any agent) editing this repository.

This is a **single-file HTML keynote deck** (`index.html`) for a Stanford GSB
talk. One `<section class="slide">` per slide. The `<style>` block and the
`<script>` block at the bottom are global infrastructure — treat them as
load-bearing.

---

## The one rule

**Edit only inside one `<section class="slide">…</section>` block at a time.**

- Use inline `style=""` for one-off changes.
- Use existing classes for layout.
- Never touch the `<style>` or `<script>` block unless you are deliberately
  adding a new animation recipe — and say so explicitly when you do.

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

## Rules for adding, removing, or reordering slides

### 1. Keep page numbers in sync (verify, every time)

Page numbers live as **hard-coded text** in each slide's footer:

```html
<div class="t-meta">03 OF 18</div>
```

Nothing renumbers them automatically — they go stale the instant a slide is
added, removed, or reordered. **After any structural change, treat renumbering
as part of the change, not a follow-up.**

1. Count the deck: `grep -cE '<section class="slide' index.html`
2. Search for `OF [0-9]+` and update **every** footer to `NN OF TOTAL`
   (zero-padded `NN` = slide index + 1, `TOTAL` = the count from step 1).
3. Confirm the **right-corner page number on the final slide equals `TOTAL`**,
   and that no two slides share the same `NN` — this is the fastest way to
   catch a missed footer.
4. Reload and click through every slide to verify.

> Status note: footers currently say `OF 18` but the deck has 20 slides — they
> are stale and must be corrected on the next edit. If you don't want to
> maintain numbers by hand, delete them entirely — better no number than a
> wrong one in front of a GSB audience.

### 2. Alternate slide colors — avoid monochrome runs

Slides come in three tones, set by the section class:

- `class="slide"` — paper / white (default)
- `class="slide accent"` — cardinal red
- `class="slide dark"` — dark photo background

**Do not place three or more same-toned slides back to back.** Alternate
white and red (with the occasional dark slide as a tonal break) so the deck
has rhythm instead of a monochromic wall. When you insert a slide, check its
neighbors and pick the class that breaks up any run.

### 3. Mimic the patterns of the first 10 slides

Slides 1–10 are the reference design language for the deck. Before building
anything new, **read the nearest slide in 1–10 with a similar purpose and copy
its structure** — `data-layout` naming, `data-animate` recipe, footer markup,
spacing, type scale, and color cadence. New slides should look like they were
authored alongside the first 10, not bolted on afterward.

### 4. Animate parallel elements on click

When you add **parallel / sibling elements** (a row of cards, a set of bars,
a multi-step list, comparison columns), do not reveal them all at once. Make
them **reveal one-by-one on click**, matching the existing staged slides:

- Add `data-stage="0"` to the `<section>`.
- Follow an existing recipe (e.g. `tech-arch`, `layers-stack`, `flow-reveal`)
  rather than inventing new script — copy the slide it already lives on.
- Reveal order should carry the argument (e.g. bottom-to-top, least → most
  important), as the current staged slides do.

Single, standalone content stays static — staging is for parallel elements.

---

## Adding a new slide (checklist)

1. Copy an existing `<section>` with a similar layout — ideally from slides 1–10.
2. Change the content; keep `data-animate` matching an existing recipe.
3. Pick the section color class to alternate tone vs. neighbors (rule 2).
4. If it has parallel elements, stage their reveal on click (rule 4).
5. Put images in `images/` as `descriptive-lowercase-kebab.png` and reference
   `src="images/filename.png"`.
6. **Renumber every footer** `NN OF TOTAL` and verify the last slide (rule 1).
7. Test locally (`open index.html`) and click through.
8. Push to `preview`, never `main`.

---

## Animation recipes reference

`data-animate` on a `<section>` selects a behavior from the recipe dictionary
in the bottom `<script>`. Staged slides also carry `data-stage="0"`. Current
recipes in use include: `hero`, `hook-95`, `jcurve-fresh`, `iceberg-img`,
`pillar-fade`, `layers-stack`, `tech-arch`, `flow-reveal`, `pipeline`.
Reuse an existing name; only add a new recipe if no existing one fits.

---

## Deployment

| Branch | URL |
|---|---|
| `main` | https://gsb-eai-ppt.vercel.app |
| `preview` | Unique URL per push (shown in GitHub Actions) |

- Never push directly to `main` — always PR from `preview`.
- One approval required before merging.
- Test on the staging URL before requesting review.

## Navigation (in browser)

| Key | Action |
|---|---|
| `→` / `←` | Next / previous slide |
| `Space` or click | Advance animations |
| `ESC` | Slide index |
