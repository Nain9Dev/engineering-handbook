# Visual Identity & Repository Showcase Standard (v1.5)

> Technical specifications, vector generation pipelines, and responsive presentation standards for repositories and online demonstrations in the NainDev ecosystem.

---

## 1. Design Philosophy: Estética Técnica Blueprint

All visual assets across the NainDev portfolio reject arbitrary decoration, generic templates, and unmeasured gradients. Instead, they reflect the identity of an autonomous software architect specialized in deterministic engineering, 3D validation, and backend architecture:

- **Mathematical Blueprint Grid**: Background grids with millimeter millimeter-inspired subdivisions (40px major cells, 8px subdivisions) rendered dynamically via vector SVG.
- **Dimensional Cotas & Callouts**: Technical dimension lines, coordinate readouts (`[0, 0, 0]`, bounding boxes, angles like `52.0°`), and system tags indicating active runtime (`.NET 10`, `Python 3.12`, `Vue 3`).
- **Surface Law & High Contrast**: Deep Blueprint Dark (`#002B66` / `#051329`) and Architectural Paper Light (`#EDF1F6` / `#F7F9FC`), conforming strictly to **WCAG 2.1 AAA** contrast standards (12.0:1 on deep blue).
- **Zero Billable Cost**: 100% vector SVG generation. Zero reliance on third-party image hosting, paid design software, or external CDNs.

---

## 2. Dimensional Specifications

| Asset Type | Canvas Dimensions | Aspect Ratio | Format | Typical File Size | Storage Path |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Hero Banner (Dark)** | 1280 × 360 px | 32:9 | Vector SVG | ~5.8 KB | `docs/assets/banner-dark.svg` |
| **Hero Banner (Light)** | 1280 × 360 px | 32:9 | Vector SVG | ~5.8 KB | `docs/assets/banner-light.svg` |
| **Social Preview (OG)** | 1280 × 640 px | 2:1 | Vector SVG | ~5.5 KB | `docs/assets/social-preview.svg` |

---

## 3. Official Color Tokens & Contrast Matrix

All banners, diagrams, and web components derive from the official NainDev Design System tokens:

| Token Name | Hex Value | Semantic Role | Luminance Contrast Ratio |
| :--- | :--- | :--- | :--- |
| `--nain-blue-deep` | `#002B66` | Primary dark surface & ink | **12.0:1 (AAA)** against light backgrounds |
| `--nain-blue` | `#0047AB` | Brand signature & primary accents | **7.4:1 (AAA)** on light surfaces |
| `--nain-green` | `#80FF00` | Precision accent, status & cotas | **10.6:1 (AAA)** on `#002B66` |
| `--nain-paper` | `#EDF1F6` | Primary light canvas | Architectural drafting paper surface |
| `--nain-surface` | `#F7F9FC` | Elevated card container | Raised surface |
| `--nain-brand-black` | `#1D1D1B` | Precision typography & solid block shadow | **14.9:1 (AAA)** |

---

## 4. GitHub README Integration Standard

Every showcase repository must embed the dual-theme banner at the very top of `README.md` using native HTML `<picture>` elements. GitHub automatically renders the appropriate vector banner based on the viewer's active theme (`prefers-color-scheme`), with zero layout shift and zero JavaScript execution:

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/banner-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="docs/assets/banner-light.svg">
  <img alt="Project Architectural Blueprint" src="docs/assets/banner-dark.svg" width="100%">
</picture>
```

### Mandatory Ecosystem Badge Sequence

Directly below the banner and project title `<h1>`, the repository must display the standardized status badges:

```markdown
# [Project Name] — [Concise Technical Subtitle]

[![ci](https://github.com/Nain9Dev/[Repo]/actions/workflows/ci.yml/badge.svg)](https://github.com/Nain9Dev/[Repo]/actions/workflows/ci.yml)
[![Demo Online](https://img.shields.io/badge/demo-online-blueviolet?style=flat&logo=githubpages&logoColor=white)](https://[subdomain].naindev.com/)
[![Ecosystem Status](https://img.shields.io/badge/status-100%25%20operational-success)](https://status.naindev.com)
[![Engineering Handbook](https://img.shields.io/badge/paved%20road-[Stack]-blue)](https://github.com/Nain9Dev/engineering-handbook)
[![Cost](https://img.shields.io/badge/cost-0.00_%E2%82%AC-22c55e)](https://github.com/Nain9Dev/engineering-handbook/blob/main/governance/zero-cost-philosophy.md)
```

---

## 5. Automated Vector Generation Pipeline

To ensure perfect mathematical consistency across all repositories without manual graphic design, assets are generated using the deterministic Python script `.github/scripts/generate_banners.py`:

```bash
# Run from repository root or central automation hub
python .github/scripts/generate_banners.py
```

The generator produces:
1. `banner-dark.svg`: Technical blueprint dark grid (`#051329`) with neon green cotas (`#80FF00`) and signature blue accents (`#0047AB`).
2. `banner-light.svg`: Precision technical drafting paper (`#EDF1F6`) with deep ink cotas (`#002B66`) and technical crosshairs.
3. `social-preview.svg`: GitHub social card preview (1280×640 px) displaying architectural layer summaries, badge tags, and the 52.0° mathematical isotype.

---

## 6. Live Demo Portals & Web Presentation

All live demonstration web applications running under `*.naindev.com` must implement:

1. **Design Tokens**: Include `https://brand.naindev.com/tokens.css` in `<head>` to maintain coherent typography, spacing, and colors.
2. **Ecosystem Cross-Navigation**: Mount `<nain-ecosystem-bar>` via `https://brand.naindev.com/ecosystem-bar.js`. This provides a zero-dependency header allowing visitors to navigate across all live projects, view operational status, and inspect architecture handbooks.
3. **Accessibility**: All interactive elements must maintain WCAG 2.1 AA compliance minimum (target AAA for primary copy).
