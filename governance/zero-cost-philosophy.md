# Zero-Cost Infrastructure Philosophy (0,00 EUR)

> Operating a high-availability, multi-application architectural portfolio and testing ecosystem without incurring recurring cloud hosting expenses.

---

## 1. The 0,00 EUR Constraint

Professional software architecture must demonstrate fiscal discipline as well as technical excellence. Demonstrations, portfolio systems, and open-source benchmarks must achieve industrial-grade reliability, security, and response times with **zero billable cloud resources** ($0.00\text{ EUR/month}$).

This constraint eliminates waste, forces high-efficiency algorithmic designs, and ensures demonstrations never break due to depleted subscription credits or surprise cloud billing spikes.

---

## 2. Infrastructure Topology Matrix

```
       [Public Web User / Recruiter / Enterprise Client]
                                |
                                v
               +----------------------------------+
               |  Cloudflare Edge (Free Tier)     |
               |  - Anycast Global CDN & Caching  |
               |  - Automatic Full SSL / TLS      |
               |  - DDoS & Web Application Shield |
               +-----------------+----------------+
                                 |
         +-----------------------+-----------------------+
         |                                               |
         v                                               v
+-------------------------------+               +-------------------------------+
| GitHub Pages (Static / WASM)  |               | Serverless Free Compute/DB    |
| - naindev.com (Main Astro UI) |               | - Neon PostgreSQL (0.5 GiB)   |
| - brand.naindev.com (Tokens)  |               | - Supabase Free Tier          |
| - status.naindev.com (Status) |               | - Cloudflare Workers / D1     |
| - Demos (3D WebGL / Canvas)   |               +-------------------------------+
+-------------------------------+                               |
                                                                v
                                                +-------------------------------+
                                                | Local Development VPS         |
                                                | - Ollama (Local LLM Engine)   |
                                                | - Docker Compose Workbenches  |
                                                +-------------------------------+
```

---

## 3. Four Pillars of Zero-Cost Engineering

### 3.1 Edge Compilation & Static Hosting
- **Mechanism**: Pre-render application shells, documentation, and dynamic routes at build time using static site generators (Astro) or single-page application bundlers (Vite).
- **Benefit**: Infinite scalability via Cloudflare edge caching without origin server load. Origin is hosted for free on GitHub Pages.

### 3.2 Client-Side Heavy Computation (WASM & WebGL)
- **Mechanism**: Rather than spinning up costly cloud GPU clusters for 3D mesh rendering, UV calculations, and geometry validation demonstrations, calculations are compiled to WebAssembly (C++ / Rust / Pyodide) or rendered natively via Three.js / Canvas in the user's browser.
- **Benefit**: Zero server CPU/GPU cost; latency is reduced to local browser execution.

### 3.3 Ephemeral Serverless & Sleep-to-Zero Persistence
- **Mechanism**: For enterprise demos requiring live transactional databases, utilize serverless PostgreSQL instances with automated cold-storage scale-to-zero (e.g., Neon or Supabase free tiers).
- **Benefit**: Zero continuous compute fees during idle hours; instant wake-up when live demonstrations are triggered.

### 3.4 Scheduled Automation via GitHub Actions Free Tiers
- **Mechanism**: Ecosystem-wide synthetic monitoring (`status.naindev.com`), deterministic 3D regression benchmarks, and nightly vulnerability scanning execute on GitHub-hosted Ubuntu runners (unlimited minutes on public repositories).
- **Benefit**: Eliminates dedicated synthetic monitoring SaaS subscriptions (e.g., Datadog, Pingdom, New Relic) while maintaining an open, auditable history of uptime and latency.

---

## 4. Cost Governance Rules

1. **No Facturable Resource Without Authorization**: Activating any resource that can incur a credit card charge requires explicit prior authorization `[M]`.
2. **Fail-Closed Budgeting**: If a third-party API or provider alters its free-tier quotas, the system must degrade gracefully or fallback to local execution, never incurring automatic overage fees.
3. **FOSS First**: Prefer permissive open-source solutions (MIT, Apache 2.0, BSD) over proprietary cloud-native vendor locks.
