# Chapter Pages Design Spec
**Date:** 2026-08-19
**Scope:** "I. The Child" chapter page + reusable template for all 8 chapters

---

## Overview

Each chapter gets its own HTML file (e.g. `chapter-1-the-child.html`). Clicking a chapter link in the nav, or clicking "Enter the Record" on the homepage, navigates to that page with a fade-out/fade-in transition.

The homepage `index.html` and all chapter pages share the same nav, fonts, color palette, audio system, and CSS variables.

---

## File Structure

```
/
├── index.html                  (existing homepage)
├── chapter-1-the-child.html    (first chapter — built in this spec)
├── Pics n Stuff/               (images and audio)
└── docs/superpowers/specs/     (this file)
```

Future chapters follow the same naming pattern: `chapter-2-the-crucible.html`, etc.

---

## Navigation

The nav bar is updated across **all pages** to show all 8 chapters:

```
✦ THE HOUSE OF THE HEARTH  |  I. THE CHILD  |  II. THE CRUCIBLE  |  III. THE KNAVE
  IV. THE NEW HOUSE  |  V. THE CHILDREN  |  VI. THE FATUI  |  VII. THE KNAVE  |  VIII. THE FLAME
                                                                              ♪  🔊
```

- Each chapter name links to its respective HTML file
- The **active chapter** is highlighted in `--red-bright` (`#d93030`)
- On the homepage, no chapter is active
- Music and mute buttons carry over from `index.html` with the same shuffled playlist logic

---

## Page Transition

Both directions (homepage → chapter, chapter → chapter) use a CSS fade:

1. User clicks a nav link or "Enter the Record"
2. `document.body` fades to opacity 0 over ~300ms
3. Browser navigates to the new page
4. New page fades in from opacity 0 to 1 over ~400ms on load

Implemented via a shared `<style>` block and small inline script on each page.

---

## Chapter Hero Section

Full-width split layout at the top of each chapter page, below the nav.

**Left half** — title block:
- `--edge-pad: 64px` left padding (aligns with content sections below)
- Chapter number: `I.` — Cormorant Garamond, 28px, light weight, `--bone-dim`
- Chapter title: `The Child` — IM Fell English, 72px, `--bone`
- Thin red divider (line–diamond–line)
- Italic chapter quote — Cormorant Garamond, 17px italic, `--bone-dim`

**Right half** — hero image:
- Full-bleed illustration (character portrait or scene)
- `border-left: 1px solid var(--line)`
- `object-fit: cover` to fill the space

Minimum height: 420px. Red filigree gradient on left and right edges.

---

## Content Sections

Each section consists of a **framed image** and a **text block**, alternating sides. Sections stack vertically with 80px gap between them.

### Image placement

| Class | Image column | Text column | Image side |
|---|---|---|---|
| `.entry-section.img-left` | 280px | `1fr` | left edge |
| `.entry-section.img-right` | 280px | `1fr` | right edge |
| `.entry-section.img-left.landscape` | 380px | `1fr` | left edge |
| `.entry-section.img-right.landscape` | 380px | `1fr` | right edge |

- All sections use `padding-left: var(--edge-pad)` or `padding-right: var(--edge-pad)` so image outer edges align with the hero title's `64px` indent
- Text inner padding: `52px` gap from image, `80px` on the far side

### Image orientation

- **Portrait** (default): `aspect-ratio: 3/4`, column width 280px
- **Landscape**: `aspect-ratio: 4/3`, column width 380px — add `.landscape` class to both the section and the `.framed-image`

### Framed image styling

- Outer border: `1px solid rgba(120,70,40,0.5)`
- 8px padding inside border
- Inner inset border: `1px solid rgba(140,80,40,0.25)` at 4px inset
- `✦` ornament centered at top edge (background-colored box, red glyph)
- `background: #080606`
- Image fills inner area with `object-fit: cover`

### Text block

- Section heading row: red rotated diamond + heading in `12px` small caps, `0.22em` letter-spacing
- Body paragraphs: Cormorant Garamond, `15px`, `--bone-dim`, `line-height: 1.9`, `font-weight: 300`
- 16px gap between paragraphs

---

## Fonts

Same as `index.html`:
- **Cormorant Garamond** — body, headings, nav
- **IM Fell English** — chapter title only

Loaded via Google Fonts.

---

## Color Palette

Inherits from `index.html` CSS variables — no new colors introduced:

| Variable | Value | Use |
|---|---|---|
| `--bg` | `#0a0808` | Page background |
| `--red` | `#b3202a` | Dividers, diamonds |
| `--red-bright` | `#d93030` | Active nav, hover states |
| `--bone` | `#e8dcc8` | Primary text |
| `--bone-dim` | `#a09070` | Secondary text, quotes |
| `--line` | `rgba(90,32,32,0.4)` | Borders, dividers |
| `--edge-pad` | `64px` | Consistent outer margin |

---

## Audio

The shuffled 4-track playlist from `index.html` is copied into each chapter page identically. Music state does not persist across page navigations — it restarts on the new page. The play/pause button reflects current state as usual.

---

## Out of Scope

- Story text content (user will author separately)
- Images for each section (user will supply)
- Chapters II–VIII pages (built later using this page as a template)
- Mobile/responsive layout (future work)
