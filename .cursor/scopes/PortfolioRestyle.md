# Portfolio Restyle — Design Spec

**Status:** Implemented  
**Owner:** Doris Thadar  
**Scope:** Visual restyle of static HTML/CSS portfolio (good-enough-for-now)  
**Created:** 2026-07-07

---

## 1. Purpose & Problem

The portfolio works well structurally and in content, but reads as a generic “AI-generated dev portfolio”: Cormorant Garamond + Inter + JetBrains Mono, centered hero, uppercase mono labels, cream/burgundy used in predictable ways.

**Goal:** Restyle so the site feels unmistakably *engineering* and unmistakably *Doris* — someone who cares about aesthetics and wants to stand apart — without being girly, overly technical, or template-like.

**Primary audience:** Recruiters and hiring managers (clear projects, resume path).  
**Secondary audience:** Anyone asking “who is Doris?” (About, personality, story).

---

## 2. Success Criteria

- [ ] Site no longer feels like a stock Claude/Cursor portfolio template
- [ ] Burgundy + cream identity is preserved but applied with more restraint and intention
- [ ] Typography reads as classic engineering document (Arial body, Times titles) — no Google Fonts dependency
- [ ] Circuit spine evolved into a subtle waveform motif on every page
- [ ] Light and dark mode both work; toggle persists across pages
- [ ] Multi-page structure unchanged (Home, About, Projects, Resume, Contact + project case studies)
- [ ] Project case-study depth and content untouched
- [ ] Motion is subtle; respects `prefers-reduced-motion`
- [ ] Mobile remains usable (no regression on nav, readability, tap targets)

---

## 3. Design Principles

| Principle | Meaning |
|-----------|---------|
| **Engineering, not technical** | Signal through motif (waveform, report layout, precise type) — not terminal cosplay or stock PCB art |
| **Aesthetic, not template** | Intentional asymmetry and document-like structure over centered hero + card grid clichés |
| **Doris, not girly** | Warm and personal through layout and copy; no pastels, florals, soft gradients, or “personal brand” fluff |
| **Restraint over decoration** | Burgundy as accent, not wallpaper; one signature motif (waveform spine), not many competing effects |
| **Good enough for now** | CSS-variable restyle + shared JS snippet; no framework migration |

---

## 4. Color System

### 4.1 Keep (light mode base)

| Token | Value | Role |
|-------|-------|------|
| `--red` | `#7A1818` | Primary accent |
| `--red-light` | `#A82020` | Hover states |
| `--red-muted` | `rgba(122, 24, 24, 0.12)` | Subtle fills |
| `--cream` | `#F5F0EB` | Page background |
| `--cream-dark` | `#EDE7DF` | Borders, grid gaps |
| `--ink` | `#1A1A1A` | Primary text |
| `--ink-soft` | `#4A4040` | Body secondary |
| `--ink-muted` | `#8A7E7E` | Metadata, captions |
| `--white` | `#FDFAF7` | Card/surface background |

### 4.2 Dark mode (new)

Warm charcoal with burgundy tint — **not** pure `#000`, **not** purple/blue glow.

| Token | Proposed value | Role |
|-------|----------------|------|
| `--cream` (dark bg) | `#1C1816` | Page background |
| `--cream-dark` | `#2A2420` | Borders |
| `--white` (dark surface) | `#252019` | Cards |
| `--ink` | `#EDE7DF` | Primary text |
| `--ink-soft` | `#B8AEA8` | Body secondary |
| `--ink-muted` | `#7A706A` | Metadata |
| `--red` | `#C44A4A` | Slightly lifted burgundy for contrast on dark |

Implementation: `[data-theme="dark"]` on `<html>`, all components use CSS variables.

### 4.3 Explicitly avoid

- Purple/teal/cyan gradients
- Neon or high-saturation accents
- Blue hover fills on cards (current `--blue-muted` hover — replace with burgundy-tinted or neutral)
- “AI slop” palette shifts

---

## 5. Typography

### 5.1 Font stack (system fonts only)

Remove all Google Fonts `<link>` tags from every HTML file.

| Role | Stack | Notes |
|------|-------|-------|
| **Body / UI** | `Arial, Helvetica, sans-serif` | `--sans` |
| **Titles / hero / h1–h3** | `"Times New Roman", Times, Georgia, serif` | `--serif` |
| **Metadata / labels** | `Arial, Helvetica, sans-serif` | Small size, optional `letter-spacing: 0.06em`, **no uppercase spam**, **no monospace** |

CSS variable rename (semantic):

```css
--sans:  Arial, Helvetica, sans-serif;
--serif: "Times New Roman", Times, Georgia, serif;
/* Remove --mono; replace usages with --sans + label styling */
```

### 5.2 Typographic tone changes

| Element | Before | After |
|---------|--------|-------|
| Nav links | Uppercase, 0.12em tracking | Sentence case, normal tracking |
| Section eyebrows | Mono, uppercase, wide tracking | Arial, small caps or regular, burgundy, tighter |
| Buttons | Mono uppercase | Arial, sentence case, slightly larger |
| Hero name | Cormorant, centered, thin | Times, left-aligned, regular/bold weight |
| Page titles | Large serif, centered feel | Times, left-aligned, report-header feel |
| Footer | Mono, wide tracking | Arial, normal size |

### 5.3 Remove inline font overrides

Clean up inline `style="font-family: 'Times New Roman'..."` on `index.html` highlight cards once global serif is applied to headings.

---

## 6. Layout & Structure

### 6.1 Unchanged

- Page routes: `index.html`, `about.html`, `projects.html`, `resume.html`, `contact.html`, `projects/*.html`
- Nav items and footer structure
- All project content, images, copy
- Case-study page depth and sections

### 6.2 Restyle changes

**Home hero**
- Shift from centered to **left-aligned** within content column
- Remove or simplify vertical decor text
- Bio block reads like an intro paragraph, not landing-page pitch
- CTA row stays; social icons stay

**Inner pages**
- Page titles left-aligned with optional thin waveform rule beneath (not full-width divider)
- Section padding consistent: content sits right of waveform spine gutter

**Projects**
- Keep grid of project cards but restyle surfaces (borders, hover) to match new tokens
- Replace blue hover with `--red-muted` or subtle border highlight
- Case-study pages: report-style headers, Arial body, Times section titles

**About / Resume / Contact**
- Inherit global styles; minimal page-specific overrides
- Volunteer border-left accent stays (fits engineering report vibe)

---

## 7. Signature Element — Waveform Spine

Evolve `.circuit-spine` into a **signal trace** motif.

### 7.1 Implementation approach

Replace the CSS-only vertical line + pulsing dot with:

1. **Fixed left column** (~48px gutter on desktop, ~24px mobile)
2. **Inline SVG** sine/scope path, burgundy, `opacity: 0.2–0.35`
3. **Optional:** second faint harmonic path offset by a few px (depth, not clutter)
4. **Subtle animation:** slow vertical drift or opacity pulse on the trace (3–4s ease-in-out)
5. **Scan dot:** small burgundy circle that travels along the path (replaces current `::before` pulse) — very subtle

### 7.2 HTML change (all pages)

```html
<!-- Before -->
<div class="circuit-spine"></div>

<!-- After -->
<aside class="waveform-spine" aria-hidden="true">
  <svg viewBox="0 0 40 800" preserveAspectRatio="none">...</svg>
</aside>
```

Keep class rename OR retain `.circuit-spine` with new styles — prefer `.waveform-spine` for clarity.

### 7.3 Section dividers (optional, phase 1.5)

Horizontal micro-waveform between major home sections instead of plain `.divider` — only if time allows; not blocking.

---

## 8. Components

### 8.1 Navigation

- Add **theme toggle** button (sun/moon icon or simple text “Dark” / “Light”) in nav, right side before hamburger
- Persist via `localStorage` key: `theme`
- On load: read storage → else `prefers-color-scheme`
- Nav background uses `var(--cream)`; dark mode border uses `var(--cream-dark)`

### 8.2 Buttons

- `.btn-primary`: burgundy fill, Arial, sentence case, no letter-spacing hack
- `.btn-ghost`: burgundy border, transparent fill
- Hover: `--red-light`, `translateY(-1px)` — keep subtle

### 8.3 Cards (highlights, featured, projects)

- White/dark surface with 1px `var(--cream-dark)` border
- Hover: `var(--red-muted)` background OR border-color shift to `var(--red)` — **not** blue
- Remove gradient thumb fallback where possible; keep project images

### 8.4 Footer

- Arial, normal case, muted color
- Optional: small waveform glyph or “—” rule centered above copyright

---

## 9. Motion & Interaction

| Effect | Allowed | Notes |
|--------|---------|-------|
| Hover color/border transitions | Yes | 0.2s |
| Button slight lift | Yes | 1px |
| Scroll fade-in on sections | Yes | Keep existing IntersectionObserver; tune to 16px translate |
| Waveform pulse / scan dot | Yes | Slow, low opacity |
| Parallax | No | |
| Cursor effects | No | |
| Heavy scroll animations | No | |

Respect `@media (prefers-reduced-motion: reduce)` — disable waveform animation and scroll reveals.

---

## 10. Technical Implementation Plan

### Phase 1 — Foundation (priority)

1. Update `css/style.css`:
   - New CSS variables (light + dark)
   - Typography tokens and global element styles
   - Waveform spine styles
   - Nav, buttons, footer, section base
   - Theme toggle styles
   - Remove `--mono`, `--blue`, `--blue-muted` usages

2. Add `js/theme.js` (or inline in shared snippet):
   - Theme init + toggle + localStorage

3. Update **all 15 HTML files**:
   - Remove Google Fonts links
   - Replace `.circuit-spine` with waveform SVG markup
   - Add theme toggle to nav (same markup on every page)
   - Include theme script

### Phase 2 — Page-specific polish

4. `index.html`: left-align hero, move inline `<style>` rules into `style.css` under `#hero` etc.
5. `projects.html`, `about.html`, `resume.html`, `contact.html`: align page-specific styles to new tokens
6. `projects/*.html` (9 files): nav/spine/theme only unless case-study styles reference old fonts

### Phase 3 — QA

7. Visual pass: light + dark on all page types
8. Mobile pass: nav, spine gutter, hero readability
9. Remove dead CSS and inline font hacks

---

## 11. Files Touched (estimate)

| File | Changes |
|------|---------|
| `css/style.css` | Major update |
| `js/theme.js` | New (optional; may inline) |
| `index.html` | Fonts, spine, nav toggle, hero layout |
| `about.html`, `projects.html`, `resume.html`, `contact.html` | Fonts, spine, nav toggle |
| `projects/*.html` (×9) | Fonts, spine, nav toggle |
| Inline `<style>` blocks | Gradually consolidate into `style.css` where practical |

**Not in scope:** Content edits, new pages, image changes, build tooling, hosting config.

---

## 12. Out of Scope

- Rewriting copy or project write-ups
- Adding a JS framework or build step
- New project pages or restructuring information architecture
- Custom font files (@font-face)
- Bento grids, glassmorphism, gradient meshes
- Blog or CMS
- SEO overhaul beyond existing meta tags
- Full consolidation of all page-specific CSS (nice-to-have, not blocking)

---

## 13. Open Questions (resolved)

| Question | Decision |
|----------|----------|
| Overall vibe | Engineering but not too technical; aesthetic but not template-y |
| Color | Keep burgundy + cream, used differently; not bright, not AI palette |
| Typography | Arial body, Times (or serif) titles; drop Inter/Cormorant/JetBrains |
| Layout | Keep multi-page; restyle only |
| Circuit spine | Keep and evolve → waveform |
| Motion | Subtle |
| Audience | Recruiters + who is Doris |
| Must keep | Burgundy identity, page structure, case-study depth |
| Inspiration | None — follow this spec |
| Scope | Good enough for now; light + dark mode |
| Personality | Screams engineering, not typical; Doris; not girly |

---

## 14. Acceptance Checklist (for GO! implementation)

Before marking done:

- [ ] No Google Fonts requests in network tab
- [ ] Theme toggle works and persists on refresh across pages
- [ ] Waveform spine visible on desktop; gracefully scaled/hidden on very small screens if needed
- [ ] Home hero is left-aligned
- [ ] No blue card hover remains
- [ ] All 15 HTML pages share consistent nav + spine + theme
- [ ] `prefers-reduced-motion` disables animations
- [ ] Doris approves “this feels like me” on a quick visual skim

---

## 15. Approval

**Does this capture your intent? Any changes needed?**

When ready to implement, reply: **GO!**
