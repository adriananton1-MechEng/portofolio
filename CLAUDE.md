# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static HTML5 portfolio website for an engineering professional (mechanical/mechatronics). No build tools, no framework, no dependencies — pure HTML, CSS, and vanilla JS deployed to GitHub Pages.

## Development

**No build step.** To preview locally, serve the root directory with any HTTP server:

```bash
python -m http.server 8000
# or
npx http-server
```

**Deployment**: push to `main` → GitHub Pages auto-deploys.

## File Structure

```
index.html              # Main portfolio (hero, projects grid, skills, contact)
cv.html                 # Curriculum vitae page
project-rover.html      # Rocker-Bogie Rover (capstone project)
project-cad.html        # CAD Assembly project
project-mechanical.html # Mechanical design project
project-microscope.html # Microscope Inspection Table (MSc thesis)
```

## Architecture

### Design System

All pages share CSS custom properties defined in `:root`:

```css
--bg: #0a0c0f;        /* dark navy */
--accent: #00d4ff;    /* cyan */
--accent2: #ff6b35;   /* orange */
--text: #e8edf2;
--muted: #5a6a7a;
--mono: 'Share Tech Mono', monospace;
--display: 'Syne', sans-serif;
```

There is no shared stylesheet — each page embeds its full CSS in a `<style>` block. When adding new pages, copy and paste the full CSS from an existing page and adjust only what's needed.

### Navigation Pattern

- `index.html`: fixed nav with anchor links (`#projects`, `#skills`, `#contact`)
- Project/CV pages: nav contains a "back" link to `index.html`
- No client-side router — multi-page by plain HTML links

### Animations

Two JavaScript patterns handle all interactivity:

1. **Fade-in on scroll**: Elements with class `.fade-in` are observed via `IntersectionObserver`; the class `.visible` is added when they enter the viewport (threshold: 0.12). The observer unobserves after triggering (one-shot).

2. **Skill bar animation**: Bars carry `data-width="0.90"` (0–1 float). The observer sets `element.style.width` to `${value * 100}%` on viewport entry, triggering a CSS transition.

### Project Cards

Cards on `index.html` use class `.project-card`. The MSc thesis card uses `.project-card--purple` to override the accent color to `--purple: #a770ff`. The `.featured` modifier styles the first/highlighted card differently.

### Background Grid

The grid texture is a fixed pseudo-element (`body::before`) using a double `linear-gradient`, not CSS Grid layout.

### Responsive Breakpoints

- `900px`: two-column CV layout collapses to one column
- `768px`: navigation, hero font sizes, section padding adjust

## Content Conventions

- All content is hardcoded — no CMS or data files.
- Contact links use `mailto:` and `tel:` hrefs; no form backend.
- SVG icons in project cards are inline and decorative (opacity 0.15–0.25).
- The blinking green status dot uses a CSS `@keyframes` box-shadow pulse.
