# Spec-Driven Engineering Lifecycle

> The canonical software delivery standard: requirements precede architecture, tests precede implementation, and every phase is tracked in numbered `docs/`.

---

## 1. The Core Law of Spec-Driven Engineering

Code changes never begin inside source files. Every non-trivial feature, architectural shift, or structural refactor begins inside the repository's `docs/` specification pipeline:

$$\text{Charter} \longrightarrow \text{Requirements} \longrightarrow \text{Architecture} \longrightarrow \text{Data Model} \longrightarrow \text{ADR} \longrightarrow \text{Tasks} \longrightarrow \text{Tests} \longrightarrow \text{Code}$$

A task is not considered completed until its corresponding automated test suite executes cleanly and is documented in the traceability matrix.

---

## 2. The Numbered `docs/` Structure

Every production repository in the NainDev portfolio maintains an explicit `docs/` directory structured by phase:

```
docs/
├── 00-charter.md           # Business context, objectives, non-goals, and constraints
├── 01-requirements.md      # Functional & Non-Functional specifications (MoSCoW)
├── 02-open-questions.md    # Ambiguities, blocked assumptions, and interview notes
├── 03-architecture.md      # Layering, sequence diagrams, component topologies
├── 04-data-model.md        # Relational DDL, document schemas, and entity graphs
├── decisions/              # Architectural Decision Records (ADRs)
│   ├── ADR-0001-*.md
│   └── ADR-0002-*.md
├── 05-tasks.md             # Sequenced task checklist with ownership tags
├── 06-blockers.md          # Active impediments, upstream tickets, and dependencies
├── 07-traceability.md      # Matrix mapping requirements to tests and code commits
└── 08-runbook.md           # Operational handbook, deployment steps, and sanity checks
```

---

## 3. Task Ownership Taxonomy

Every task listed within `docs/05-tasks.md` must be prefaced by one of three deterministic ownership markers:

| Marker | Name | Definition & Protocol |
| :---: | :--- | :--- |
| `[A]` | **Autonomous** | Can be executed from start to finish by automated tooling, CI runners, or paired AI coding assistants without asking for intermediate manual input. |
| `[M]` | **Mutual / Action Required** | Requires action from the human operator before execution can proceed (e.g., DNS provider configuration, API key retrieval, manual payment approval). Tooling must halt and notify. |
| `[H]` | **Human Only** | Strictly reserved for the principal architect (e.g., business decisions, client agreements, final production push authorization). Never executed autonomously. |

---

## 4. Test-Driven Verification (TDD) Gate

1. **Write the Failing Test First**: Define the interface and assert expected behavior before creating the implementation.
2. **Minimal Viable Implementation**: Write the smallest unit of code necessary to transition the test from red to green.
3. **Refactor Under Green**: Clean up structure, improve readability, and remove redundancy while keeping the test suite green.
4. **Bracketted Refactors**: When refactoring existing code, run the baseline test suite immediately prior to the edit and immediately post-edit to verify behavioral preservation.

---

## 5. Commit & History Standards

- **Conventional Commits**: All commit messages must follow the standard type conventions:
  - `feat:` New user-facing capability or API feature.
  - `fix:` Bug resolution or mathematical calibration.
  - `docs:` Documentation, ADR, or `docs/` updates.
  - `refactor:` Code restructuring without behavior alteration.
  - `test:` Addition or correction of test fixtures.
  - `ci:` Workflow and pipeline improvements.
- **Language**: English only across commits, branches, PR titles, and code symbols.
- **No Artifact Timestamps or AI Markers**: Commit history must remain professional, concise, and clean of automated boilerplate.
