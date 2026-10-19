# Ecosystem Licensing Policy

> Strategic dual-tier licensing framework protecting personal brand, identity, and commercial software while maintaining zero friction for enterprise audits and open-source developer tooling.

---

## 1. Dual-Tier Licensing Rationale

As an autonomous full-stack software architect delivering enterprise backends, deterministic validation, and modern web applications, repositories in the NainDev ecosystem serve two distinct purposes requiring different legal boundaries:

1. **Demonstrators & Ecosystem Tooling**: Created to prove technical competence, teach architectures, and provide consumable tools. These require **permissive licensing (MIT)** so corporate CTOs and legal departments can audit and test code without friction.
2. **Personal Brand, Identity & Core Products**: Represent individual reputation, commercial copy, design systems, and business IP. These require **protective licensing (Source-Available / All Rights Reserved or AGPL-3.0)** to prevent impersonation, plagiarism, and unauthorized commercial exploitation.

---

## 2. Canonical Licensing Matrix

| Category | Repositories | License | Rights Granted | Explicit Restrictions |
| :--- | :--- | :---: | :--- | :--- |
| **Identity & Brand** | `nain9dev.github.io` (`naindev.com`), `brandbook`, `Nain9Dev` | **All Rights Reserved** *(Source-Available)* | Local inspection, architectural auditing, educational review. | No public re-hosting, deployment, copying of text/case studies, or trademark usage. |
| **Showcase & Blueprints** | `NainLegacyModernizer`, `template-dotnet-clean-arch`, `template-fastapi-vue-clean-arch` | **MIT** | Free usage, modification, and integration into internal projects. | Attribution notice must be preserved. No liability. |
| **Tools & Agent Protocols** | `nain-mcp-validator`, standalone CLI utilities | **MIT** | Unrestricted integration into AI agents, IDEs, and build pipelines. | Attribution notice must be preserved. No liability. |
| **Full Products & Engines** | `NainConfigurator`, core parametric kernels | **AGPL-3.0-or-later** | Freedom to run, study, and inspect source code. | Network copyleft: modifications deployed over network/SaaS must be released under AGPL. |

---

## 3. Data Privacy Standard

A software license governs intellectual property rights, **not data privacy**. In accordance with the [Engineering Handbook Tenets](../README.md):
- Private client codebases remain strictly quarantined in isolated workspaces.
- Real API secrets, certificates, and production database connection strings are never committed to any repository.
- Sensitive working notes, business roadmaps, and client meeting logs reside exclusively in the git-ignored `docs/private/` directory.
