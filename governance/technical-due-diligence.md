# Staff Architect Technical Due Diligence Checklist

> Comprehensive evaluation framework used during architectural audits, startup technical due diligence, and enterprise vendor assessments.

---

## 1. Architectural Integrity & Modularity

- [ ] **Dependency Inversion**: Are business entities strictly decoupled from database, message bus, and transport mechanisms?
- [ ] **Boundary Encapsulation**: Do Domain Aggregates protect their state invariants, or are they anemic data bags with public setters?
- [ ] **Bounded Contexts**: Are domain models cleanly separated, avoiding multi-tenant cross-boundary god objects?
- [ ] **Architectural Fitness Functions**: Does CI automatically fail if a layering violation is introduced?

---

## 2. Code Quality & Verification Gates

- [ ] **TDD / Test Coverage**: Does every non-trivial domain behavior have an accompanying automated unit test?
- [ ] **Contract Verification**: Are external HTTP/gRPC interfaces bracketed with strict schema validation (Pydantic / FluentValidation)?
- [ ] **Deterministic Behavior**: Are floating-point calculations, geometry measurements, and financial figures free of platform-dependent rounding drift?
- [ ] **Property-Based Testing**: Are boundary cases verified with generative testing (Hypothesis, FsCheck) where applicable?

---

## 3. Operational Resilience & Reliability

- [ ] **Graceful Degradation**: If an AI model or external API fails, does the system circuit-break or degrade smoothly?
- [ ] **Zero-Downtime Deployments**: Can modern services be deployed and upgraded without taking the platform offline (Strangler Fig, Blue-Green)?
- [ ] **Health & Observability**: Are real-time availability and latency probes publicly or internally visible (`status.naindev.com`)?
- [ ] **Data Recovery (RPO/RTO)**: Are point-in-time recovery and snapshot restoration verified on a scheduled cadence?

---

## 4. Supply Chain & FinOps Governance

- [ ] **Zero-Cost Baseline**: Do staging, portfolio, and demonstration environments operate at $0.00\text{ EUR}$ recurrent hosting fees?
- [ ] **Dependency Hygiene**: Are automated dependency updates grouped weekly via Dependabot to prevent PR fatigue?
- [ ] **Secret Exposure Auditing**: Do pre-commit hooks and CI gates enforce automated scanning for private keys and credentials?
- [ ] **License Conformance**: Are all third-party dependencies verified against permissive open-source licenses (MIT, Apache 2.0, BSD)?
