# AI Timeline — AGENT.md

## Project overview
Single-page web application that displays an interactive timeline of Artificial Intelligence milestones from the 1980s to 2025. Built as a self-contained HTML file with no external dependencies.

## Repository structure
```
Prueba_Dev_IA/
├── ai-timeline.html   # Main application — all HTML, CSS and JS in one file
└── AGENT.md           # This file
```

## Running locally
Serve the project with Python's built-in HTTP server:
```bash
python3 -m http.server 8080
```
Then open: http://localhost:8080/ai-timeline.html

## Architecture
`ai-timeline.html` is a single self-contained file split into three logical sections:

- **CSS** (`<style>`) — dark-theme design system using CSS custom properties, responsive layout (flexbox), category colour coding, and card hover effects.
- **HTML** (`<body>`) — semantic markup for header, legend, decade filter bar, and timeline events. Each `.event` element carries `data-decade` for filtering.
- **JS** (`<script>`) — two small features:
  - `IntersectionObserver` — fades cards in as they scroll into view.
  - Decade filter — shows/hides `.event` and `.decade-label` elements by `data-decade` attribute.

## Adding a new event
1. Open `ai-timeline.html`.
2. Locate the correct decade block (marked with `<!-- ══ 20XXs ══ -->`).
3. Copy an existing `.event` div and update:
   - `data-decade` attribute — must match the decade block (e.g. `"2020"`).
   - CSS category class on the `.event` element (`cat-research` | `cat-hardware` | `cat-milestone` | `cat-model` | `cat-product`).
   - `.card-year`, `.card-title`, `.card-desc` inner text.
   - `<span class="tag ...">` label and its matching category class.

## Category reference
| Class | Colour | Use for |
|---|---|---|
| `cat-research` | Indigo | Papers, algorithms, theoretical advances |
| `cat-hardware` | Cyan | Hardware, datasets, infrastructure |
| `cat-milestone` | Pink | Historic competitive or societal milestones |
| `cat-model` | Green | Model architectures and releases |
| `cat-product` | Amber | Commercial products and public launches |

## Styling conventions
- All colours defined as CSS custom properties on `:root`.
- No external fonts, frameworks, or CDN dependencies — the file works fully offline.
- Responsive breakpoint at `600px`: timeline collapses to a left-aligned single column.

## Known limitations
- No build step or bundler — edit `ai-timeline.html` directly.
- Timeline data is hardcoded in HTML; no JSON/API backend.
