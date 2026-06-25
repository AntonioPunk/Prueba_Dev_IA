# AI Timeline

An interactive, single-page timeline of Artificial Intelligence milestones from the 1980s to the present day. No frameworks, no build step, no dependencies — just one HTML file.

![AI Timeline preview](https://img.shields.io/badge/status-active-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![Zero dependencies](https://img.shields.io/badge/dependencies-none-lightgrey)

---

## Features

- **40+ years of AI history** — curated milestones from expert systems (1980s) to autonomous AI agents (2025)
- **Decade filter** — instantly narrow the view to any era
- **Category tagging** — events classified as Research, Hardware/Data, Historic Milestone, Model/Architecture, or Product
- **Scroll animations** — cards fade in via `IntersectionObserver` as you scroll
- **Fully responsive** — collapses to a single-column layout on mobile
- **Zero dependencies** — no npm, no CDN, no build pipeline; works offline

---

## Getting started

Clone the repo and serve it with any static file server.

```bash
git clone https://github.com/<your-username>/ai-timeline.git
cd ai-timeline
python3 -m http.server 8080
```

Open [http://localhost:8080/ai-timeline.html](http://localhost:8080/ai-timeline.html).

> Any static server works: `npx serve`, `live-server`, nginx, Apache, GitHub Pages, etc.

---

## Project structure

```
.
├── ai-timeline.html   # Entire application — HTML + CSS + JS in one file
├── AGENT.md           # Guidelines for AI agents and automated contributors
└── README.md          # This file
```

The deliberate choice to keep everything in a single file is a constraint, not an oversight. It means the project has no toolchain overhead and can be shared as a single attachment or dropped into any web server with zero configuration.

---

## Adding or editing events

Events are hardcoded in the HTML. Each one follows this structure:

```html
<div class="event cat-model" data-decade="2020">
  <div class="dot"></div><div class="spacer"></div>
  <div class="card">
    <div class="card-year">2020</div>
    <div class="card-title">GPT-3</div>
    <div class="card-desc">Brief description of the milestone.</div>
    <span class="tag cat-model">Model</span>
  </div>
</div>
```

**Rules to follow when adding an event:**

1. Place it inside the correct `<!-- ══ 20XXs ══ -->` block.
2. Set `data-decade` to the opening year of the decade (e.g. `"2010"` for 2012).
3. Apply one category class to both `.event` and `.tag`:

| Class | Colour | When to use |
|---|---|---|
| `cat-research` | Indigo | Papers, algorithms, theoretical breakthroughs |
| `cat-hardware` | Cyan | Hardware, datasets, infrastructure |
| `cat-milestone` | Pink | Historic competitive or societal events |
| `cat-model` | Green | Model architectures and releases |
| `cat-product` | Amber | Commercial launches and public products |

---

## Design decisions

| Decision | Rationale |
|---|---|
| Single HTML file | Zero toolchain; trivially portable and deployable |
| CSS custom properties | Consistent theming with no preprocessor needed |
| `IntersectionObserver` | Native browser API; no scroll-event listeners to throttle |
| Hardcoded data | Keeps complexity minimal; a JSON/API backend would be overkill for this scope |
| No TypeScript / bundler | Constraint of the project; complexity budget is intentionally low |

---

## Contributing

1. Fork the repository.
2. Add or update events following the guidelines in [AGENT.md](AGENT.md).
3. Test locally (`python3 -m http.server 8080`) and verify the layout at multiple viewport widths.
4. Open a pull request with a clear description of the change.

Please do not introduce external dependencies or restructure the single-file architecture without prior discussion.

---

## License

MIT © Antonio
