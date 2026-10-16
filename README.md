# NainDev Engineering Handbook

> The definitive engineering standards, architectural blueprints, quality gates, and governance policies for the NainDev ecosystem.

[![Ecosystem Status](https://img.shields.io/badge/status-active-success)](https://status.naindev.com)
[![Brand Identity](https://img.shields.io/badge/brand-v1.5-blue)](https://brand.naindev.com)
[![Zero-Cost Budget](https://img.shields.io/badge/budget-0.00_EUR-brightgreen)](governance/zero-cost-philosophy.md)

---

## 1. Engineering Tenets

1. **Determinism Over Heuristics**: Software systems must yield reproducible, mathematically verifiable outcomes. Ambiguous heuristics, uncalibrated thresholds, and non-deterministic validations are strictly prohibited.
2. **Spec-Driven Engineering**: Code is the final artifact of a disciplined pipeline. Every project begins with numbered documentation phases (`docs/`), explicit acceptance criteria, and Architecture Decision Records (ADRs).
3. **Test-First Verification (TDD)**: A feature or fix without an automated test does not exist. Tests bracket every structural refactor and define the boundary of acceptable behavior.
4. **Clean Layering & High Cohesion**: Domain logic remains completely decoupled from databases, frameworks, and transport protocols. Dependencies flow inward.
5. **Zero-Cost Production Footprint**: Portfolio applications and technical demonstrations operate at exactly **0,00 EUR** recurrent infrastructure cost by leveraging high-performance edge compute, static compilation, free-tier serverless datastores, and GitHub public automation.

---

## 2. Paved Road & Technology Radar

```
+---------------------------------------------------------------------------------+
|                                 ADOPT (Default)                                 |
+---------------------------------------------------------------------------------+
| Backend:      ASP.NET Core (.NET 10) Clean Architecture, Python 3.12+ (FastAPI) |
| 3D / Data:    Trimesh, NumPy, SciPy, Shapely, PyDantic v2                       |
| Persistence:  PostgreSQL (Supabase/Neon), EF Core 10 (Writes), Dapper (Reads)    |
| Frontend:     Vanilla JS (ESM), Astro (Static/SSR), Vue 3 (Strict TypeScript)   |
| Testing:      xUnit v3, Vitest, Playwright, PyTest + Hypothesis, Testcontainers |
| Edge/Hosting: GitHub Pages, Cloudflare Edge DNS/SSL, GitHub Actions CI/CD       |
+---------------------------------------------------------------------------------+
```

### Radar Quadrants

- **Backend Enterprise**: ASP.NET Core controllers with Clean Architecture for enterprise workflows, transactional boundaries, and high-throughput APIs.
- **Data, Geometry & AI**: Python (FastAPI + Pydantic v2) for spatial computing, deterministic 3D mesh evaluation (GLB/glTF), computer vision, and local AI orchestration.
- **Frontend & Presentation**: Astro for content-driven web portals; Vue 3 with strict TypeScript for rich interactive SPAs; Vanilla Modern JavaScript (ES Modules, Web Components) for universal zero-dependency widgets (e.g., `<nain-ecosystem-bar>`).
- **Database & Persistence**: PostgreSQL as the single source of truth; EF Core for aggregate writes; Dapper for high-throughput zero-overhead projections.

---

## 3. Handbook Directory

| Section | Topic | Summary |
| :--- | :--- | :--- |
| **Architecture** | [Clean Architecture](architecture/clean-architecture.md) | Strict 4-layer separation (Domain, Application, Infrastructure, Presentation) for .NET and Python. |
| **Architecture** | [Dual Persistence Pattern](architecture/dual-persistence.md) | CQRS-inspired split: transactional writes via EF Core, sub-millisecond reads via Dapper. |
| **Architecture** | [Deterministic 3D Validation](architecture/deterministic-validation.md) | Khronos GLB/glTF standards, quantum threshold ($10^{-12}\text{ m}^2$), checks C1-C10, fail-closed gating. |
| **Architecture** | [Architectural Fitness Functions](architecture/fitness-functions.md) | Automated tests in CI asserting layer boundaries (ArchUnitNET for .NET, AST linters for Python). |
| **Architecture** | [C4 Architectural Modeling](architecture/c4-model-standard.md) | Context, Container, and Component modeling using GitHub-native Mermaid flowchart syntax. |
| **Architecture** | [Legacy .NET Modernization](architecture/legacy-dotnet-modernization.md) | Strangler Fig proxying via YARP, WCF-to-gRPC, and shadow running for zero-downtime upgrades. |
| **Architecture** | [3D Conformance Rubric](architecture/3d-conformance-rubric.md) | Industrial audit criteria and pass/fail thresholds for digital assets across geometry and PBR. |
| **Governance** | [Spec-Driven Lifecycle](governance/spec-driven-lifecycle.md) | The canonical numbered `docs/` folder convention (`00` through `08`), TDD gates, and task tagging. |
| **Governance** | [ADR Template](governance/adr-template.md) | Standardized Architectural Decision Record format for recording design trade-offs. |
| **Governance** | [Technical Due Diligence](governance/technical-due-diligence.md) | Staff architect evaluation framework for codebases, modularity, FinOps, and resilience. |
| **Governance** | [Two-Audience README Standard](governance/readme-specification.md) | Dual-audience repository documentation pattern for executive buyers and senior architects. |
| **Governance** | [Dependabot Governance](governance/dependabot-governance.md) | Standard grouped weekly dependencies across npm, pip, nuget, and actions to eliminate PR noise. |
| **Governance** | [Universal Local Development](governance/local-development.md) | Unified developer experience via standard Taskfile commands (task dev, task test, task build). |
| **Governance** | [Zero-Cost Philosophy](governance/zero-cost-philosophy.md) | Principles and architectures for running a multi-application professional portfolio at 0,00 EUR. |
| **Governance** | [Visual Identity Standard](governance/visual-identity-standard.md) | Technical blueprint aesthetic, dual-theme vector banners, and showcase integration. |

---

## 4. Work Tracking Standard

All engineering tasks within repository roadmaps (`docs/05-tasks.md`) use strict ownership tagging:

- `[A]` **Autonomous**: Task executable end-to-end by automated CI or paired AI assistant without manual intervention.
- `[M]` **Mutual / Action Required**: Requires human authorization, external account access, or key provisioning.
- `[H]` **Human Only**: Reserved exclusively for the principal architect (e.g., business strategy, client agreements, final production approvals).

---

## 5. Security & Isolation Standard

- **Client Isolation**: Private and client codebase repositories remain strictly isolated. No business rules, private credentials, or internal schemas are ever transferred to open-source or portfolio repositories.
- **Sanitization Gate**: All public materials, design books, and architecture documentation undergo automated scanning for confidential names, internal paths, and client identifiers before publishing.
