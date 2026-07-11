# Dossier — Resume & Portfolio Builder

A single self-contained website that turns a structured intake survey into a typeset, downloadable **resume (PDF)** and a matching, downloadable **portfolio site (HTML)** — with optional live import from GitHub.

No backend, no database, no account. Everything runs client-side in the browser tab.

---

## Features

- **10-step guided intake** — Identity, Connect accounts, Summary, Skills, Experience, Education, Projects, Latest Work, Extras, Design
- **Profile photo upload** — shown on both the resume and portfolio
- **GitHub live connect** — enter a username to pull your public profile and repositories directly from the GitHub API; one-click use your avatar as your photo, your bio as your summary, and import selected repos as Projects
- **Tag-style inputs** for skills, certifications, and languages
- **Repeatable sections** — add/remove any number of jobs, degrees, projects, or "Latest Work" updates
- **15 resume designs** — 5 layouts (Classic, Sidebar, Banner, Boxed, Minimal) × 3 type treatments (Mono, Serif, Modern)
- **15 portfolio designs** — the same 5×3 system, chosen independently of the resume
- **Portfolio background picker** — 8 preset colors plus a custom color picker, with automatic text/card contrast
- **"Latest Work" feed** — post recent launches/updates that appear as a feed on the portfolio and a "Recent Work" section on the resume
- **Downloads**
  - Resume → real PDF (via `html2canvas` + `jsPDF`, multi-page if content overflows one sheet)
  - Portfolio → standalone `portfolio.html` file, fully self-contained, ready to host anywhere (Netlify, GitHub Pages, your own server)
- **Edit anytime** — "Edit answers" returns to the survey without losing anything already entered

---

## Getting started

Just open `index.html` in any modern browser. That's it — there's no build step, no server, and no installation.

To host it (optional):
1. Upload `index.html` to any static host (Netlify, Vercel, GitHub Pages, S3, etc.)
2. Done — it works as a static file with no server-side code required.

---

## How it works

| Step | What happens |
|---|---|
| 1–9 | User fills in the survey; all answers are held in memory (JS variables/DOM), nothing is uploaded anywhere |
| GitHub connect | Client-side `fetch()` calls to the public, unauthenticated GitHub REST API (`api.github.com`) — works because GitHub's API allows CORS and doesn't require a backend |
| Generate | On the final step, all fields are collected into a single state object and rendered into HTML for both documents |
| Resume PDF | The rendered `#resumePaper` element is captured with `html2canvas` and written to a PDF with `jsPDF`, paginating automatically if it's taller than one page |
| Portfolio download | The same render function is reused to build a fully standalone HTML document (CSS inlined, fonts linked from Google Fonts) and offered as a `Blob` download |

---

## Tech stack

- Plain HTML/CSS/JavaScript — no framework, no build tools
- [Google Fonts](https://fonts.google.com/): Fraunces, IBM Plex Sans, IBM Plex Mono, Playfair Display, Space Grotesk
- [html2canvas](https://github.com/niklasvh/html2canvas) — DOM-to-canvas capture for PDF export
- [jsPDF](https://github.com/parallax/jsPDF) — PDF generation
- GitHub REST API (`api.github.com`) — public, unauthenticated, used for the GitHub connect feature

All third-party libraries are loaded from cdnjs.cloudflare.com; Google Fonts are loaded from fonts.googleapis.com. An internet connection is required for fonts, the PDF/canvas libraries, and the GitHub connect feature — everything else works fully offline once loaded.

---

## Known limitations

- **LinkedIn has no public API for this kind of import**, and building a real "Connect" would require a backend plus an approved LinkedIn OAuth app, so it isn't included. The Identity step still has a plain LinkedIn URL field for linking out to your profile.
- **GitHub API rate limits** — unauthenticated requests are capped at 60/hour per IP by GitHub. Fine for normal personal use, but repeated testing in a short window can hit the limit.
- **No persistence** — refreshing or closing the tab clears the form (by design: no browser storage is used, nothing is saved anywhere). Generate and download your documents before closing the tab.
- **PDF fidelity** — the resume PDF is a rasterized image of the rendered page (not real selectable PDF text), which keeps layout fidelity perfect across all 15 styles but means the text isn't searchable/copyable in the PDF.

---

## Customization

Everything lives in the one `index.html` file:

- **Colors / fonts** — CSS custom properties at the top of the `<style>` block (`--brass`, `--slate`, `--font-display`, etc.)
- **Resume/portfolio layouts** — `LAYOUTS` and `TREATMENTS` arrays in the `<script>` block generate the 15×15 style combinations; add a new layout or treatment id and matching CSS rules (`.r-layout-*`, `.r-type-*`, `.p-layout-*`, `.p-type-*`) to expand the set
- **Background presets** — `PORTFOLIO_BG_PRESETS` array
- **Survey questions** — each step is a `<div class="step" data-step="N">` block in the HTML; add fields and read them in `collectState()`

---

## File structure

```
index.html   — the entire application (structure, styles, and logic)
```

Single file by design, so it can be opened, shared, or hosted with zero setup.
