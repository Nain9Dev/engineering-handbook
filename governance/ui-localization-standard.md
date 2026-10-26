# Product UI Localization & Bilingual Delivery Standard

> Engineering specification for bilingual client-facing interfaces across the NainDev portfolio, establishing Spanish (`es-ES`) as the primary product locale, deterministic English translation, and complete separation between repository engineering artifacts and user-facing presentation.

---

## 1. The Core Duality Principle

A strict architectural boundary separates repository engineering from product presentation:

1. **Repository Engineering Layer (100% English)**:
   - All source code, identifiers, types, architecture documentation (`docs/`), specifications, commit messages, comments, pull requests, automated tests, API route definitions, and data models must be written strictly in professional, concise English without AI markers or conversational filler.
2. **Product Presentation Layer (Default `es-ES` + Paired `en-US`)**:
   - The default, canonical user interface language for end-user products and demonstrations is **professional European Spanish (`es-ES`)**.
   - Every interactive web client or demonstration application must provide a prominent, paired language switcher offering instant switching to **professional, redundancy-free English (`en-US`)**.
   - **Contractual Domain Exception**: When a client specification or formal commercial requirement explicitly designates an alternative target locale (e.g., Italian, French, German), that locale replaces the default.

---

## 2. Architecture & Delivery Models

Depending on application architecture, localization is delivered via one of two deterministic patterns:

### Pattern A: Static Pre-Rendered Routing (SSG / Multipage)
- Canonical example: `nain9dev.github.io`.
- Spanish pages reside at the root path (`/`).
- English pages are pre-rendered at build time under the `/en/` namespace.
- Dual-alternative `hreflang` metadata and self-referential canonicals are injected into `<head>`.
- Language switchers use standard anchors:
  ```html
  <nav class="language-switcher" aria-label="Idioma" translate="no">
    <a href="/" lang="es-ES" hreflang="es" aria-current="page">Español</a>
    <a href="/en/" lang="en-US" hreflang="en">English</a>
  </nav>
  ```

### Pattern B: Single-Page Application / Client-Side State (SPA / PWA)
- Canonical examples: `parametricad-ai`, `SistemaOposicionesTAI`, `ProspectAI`.
- UI strings are extracted into versioned dictionary files (`locales/es.json` and `locales/en.json`).
- Minimal client runtime: zero heavy external framework dependencies (avoid oversized i18n runtimes where a lightweight typed dictionary resolver suffices).
- Active locale is persisted in `localStorage` (`nain_locale`), defaulting to `es`.
- Changes trigger a broadcast event across the DOM:
  ```typescript
  window.dispatchEvent(new CustomEvent('nain:locale-change', { detail: { locale: 'es' | 'en' } }));
  ```

---

## 3. Cross-Ecosystem Standard: `<nain-ecosystem-bar>`

The shared navigation bar (`brandbook/ecosystem-bar.js`) mounted across all `*.naindev.com` demonstration applications provides ecosystem-wide language synchronization:

1. Displays an integrated `[ES | EN]` toggle button in the primary control row.
2. Reads stored preference from `localStorage.getItem('nain_locale')`.
3. Synchronizes internal ecosystem bar labels (navigation items, architectural drawer tabs, telemetry headings) between Spanish and English.
4. Broadcasts `nain:locale-change` so host applications adapt dynamically without page reloads.

---

## 4. Deterministic Translation Pipeline (Zero Runtime Cost)

In alignment with the Zero-Cost Philosophy (FinOps):

- **No Paid APIs at Runtime**: Public applications must never invoke paid translation services (e.g., Google Cloud Translate, OpenAI API) in the user's browser.
- **Offline / Local Inference via NainWrite**: Translations from `es-ES` to `en-US` are generated during local development or build pipelines using local Ollama models orchestrated by NainWrite with strict JSON schema validation and fail-closed invariant preservation (numbers, units, technical cotas, and currencies must remain exact).
- **Human Editorial Gate**: Proposed English translations must be deterministically checked and approved into version control before deployment.

---

## 5. Structural Compliance Checklist

For any application presenting a user interface in the NainDev ecosystem:

- [ ] **Native Spanish Source**: Primary UI copy authored in clear, professional European Spanish (`es-ES`).
- [ ] **Bilingual Switcher**: Paired button or pill (`[ES | EN]`) visible in the header or via `<nain-ecosystem-bar>`.
- [ ] **Engineering English**: 0% Spanish in variable names, functions, comments, commits, and `docs/`.
- [ ] **Zero Cost Runtime**: Translations stored as static assets; zero client-side translation network requests.
- [ ] **Accessibility (WCAG 2.1 AA)**: Interactive switchers must include `aria-label`, correct `lang` attributes, and keyboard focus outlines.
