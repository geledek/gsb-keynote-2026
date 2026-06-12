# How to Safely Edit Individual Slides in This Deck

This deck is a **single-file HTML** produced by the guizang-ppt-skill (Swiss style, Stanford Cardinal theme). Because everything lives in one file, **the biggest risk is accidentally affecting other slides or the global system**.

## Core Principle

**Treat every `<section class="slide">` as a completely isolated unit.**

Editing content, structure, or inline styles *inside one slide* will almost never touch other slides — **if and only if** you follow the rules below.

---

## 1. Safe Editing Zones (Do This)

### ✅ Always edit inside one slide block only

```html
<section class="slide" data-layout="S22" data-animate="image-reveal">
  <!-- EVERYTHING you change should be between these two tags -->
  ...
</section>
```

**How to do it in practice:**

1. In your editor, **select the entire slide** from the opening `<section` to its matching `</section>`.
2. Make all changes only within that selection.
3. Never let your cursor or search/replace leak outside that block.

### ✅ Preferred techniques (in order)

| Technique                    | When to use                                      | Risk to other slides | Example |
|-----------------------------|--------------------------------------------------|----------------------|--------|
| Inline `style="..."`        | One-off spacing, font sizes, widths, colors      | Zero                 | `style="margin-top: 4vh; font-size: min(5.2vw, 9vh)"` |
| Existing utility classes    | Most layout needs (`grid-*`, `h-xl`, `lead`, `t-meta`, `frame-img`, etc.) | Very low | `class="frame-img r-21x9"` |
| Local descriptive class     | Repeated pattern that only this slide needs      | Low (if name is unique) | `class="jcurve-stat"` (only used inside this slide) |
| `data-anim="..."`           | Trigger existing animation recipes               | Zero (recipes are global but safe) | `data-anim="title"` |

### ✅ Always keep these three attributes correct on the `<section>`

- `data-layout="Sxx"` (S01–S22 or the special cover/closing ones)
- `data-animate="..."` (must match a key in the `RECIPES` object at the bottom of the file)
- `data-image-slot="..."` (required for any image in S15/S16/S22 slides)

---

## 2. Danger Zones (Never Touch These When Editing One Slide)

These are the only parts of the file that can break **every** slide at once:

| Area | Location | What happens if you break it | Rule |
|------|----------|------------------------------|------|
| `:root` variables | Top of `<style>` block | Changes the entire color system (Stanford Cardinal, greys, paper) | Only edit when you intentionally want to re-theme the whole deck |
| Global component CSS | Inside the big `<style>` block (`.h-hero`, `.bar-row`, `.swiss-img-split`, `.canvas-card`, etc.) | Breaks layout or animation for all slides using that component | Add new components only with a clear comment. Never delete or rename existing ones |
| Animation `RECIPES` object | Bottom `<script>` section | Animations stop working or become unpredictable | Only add new recipes here. Never delete existing ones |
| `#deck`, `#nav`, canvas elements | Outside all slides | Breaks navigation, WebGL background, keyboard controls | Touch only when you know exactly what you're doing |
| The big comment block at the top of the slides area | Lines ~1231–1249 | Misleads future editors (currently still contains old IKB text) | Update only when improving documentation |

---

## 3. Recommended Workflow (Do This Every Time)

1. **Backup the slide** — Copy the entire `<section>...</section>` block into a comment or a scratch file before you start heavy editing.
2. **Work in isolation** — Use your editor's "column selection" or "select enclosing tag" feature to stay inside one slide.
3. **Use inline styles first** — 80% of tweaks (spacing, sizing, alignment) can be done with `style=""` without touching CSS.
4. **Hard refresh** — After every change: `Cmd+Shift+R` (or `Ctrl+Shift+R`) in the browser.
5. **Run the validator** when you:
   - Add or change `data-layout`
   - Add or change images (`data-image-slot`)
   - Introduce new structural patterns
   ```bash
   node /path/to/guizang-ppt-skill/scripts/validate-swiss-deck.mjs ppt/index.html
   ```
6. **Test on actual presentation hardware** (projector + clicker) before the talk.

---

## 4. Current Deck Structure (as of this writing)

| Slide | Line (approx) | `data-layout` | Purpose | Safe to edit? |
|-------|---------------|---------------|---------|---------------|
| Cover | 1250 | S01 + `.accent` | Title + center of gravity | Yes (inside the section) |
| 77% Cold Open | 1278 | S09 | Big stat + native bar chart | Yes |
| Grok Image Hero | 1319 | S22 | Boardroom photo + message | Yes — but keep `data-image-slot` |
| J-curve | 1340 | S08 | Path image + explanation | Yes |

---

## 5. Concrete Examples from This File

### Good: Editing only one slide (recommended)

```html
<!-- Inside the J-curve slide only -->
<div style="margin-top: 3vh; max-width: 48ch;">
  This paragraph only affects this slide.
</div>
```

### Good: Using an existing global class safely

```html
<h2 class="h-xl" style="font-weight:200; color: var(--accent);">
  This uses the global .h-xl definition but only on this slide.
</h2>
```

### Bad: What not to do

- Doing a project-wide Find/Replace for `font-size: min(6vw` and accidentally hitting every title.
- Deleting a CSS rule because "it looks unused on this slide" (it might be used on slide 4).
- Changing `--accent` in `:root` because you want the red "a bit brighter on slide 3".

---

## 6. When You Actually Need to Touch Global Code

Only in these situations:

- **Adding a new reusable component** → Add it to the `<style>` block with a comment like `/* === Custom component: J-curve callout (used on slide 04) === */`
- **Adding a new animation recipe** → Add it inside the `RECIPES` object at the bottom.
- **Changing Stanford branding colors** → Only edit inside `:root` (and update this guide).
- **Fixing a global bug** that genuinely affects multiple slides.

In all other cases: **stay inside the `<section>`**.

---

## 7. Quick Checklist Before Saving

- [ ] Did I only edit inside one `<section class="slide">` block?
- [ ] Did I preserve `data-layout`, `data-animate`, and `data-image-slot`?
- [ ] Did I hard-refresh the browser?
- [ ] If I added an image, is it in `images/` with the correct relative path?
- [ ] If I changed layout structure, did I run the validator?

---

**Remember**: The power (and the danger) of this format is that **one file = one source of truth**. Respect the boundaries between slides and the global system, and you can iterate extremely fast without breaking previous work.

This is the discipline that lets you safely build a 15-slide deck one slide at a time.