# Two-Audience README Standard

> Engineering specification for repository documentation designed simultaneously for executive decision-makers and senior technical architects.

---

## 1. The Two-Audience Philosophy

Public portfolio repositories must serve two distinct personas with competing objectives:

1. **The Executive / Commercial Evaluator** (CTO, VP of Engineering, Recruiter, Enterprise Client):
   - Objectives: Wants to know *what business problem was solved*, *the ROI achieved*, *the total operating cost*, and *where to test the live demonstration*.
   - Time window: **30 seconds**.
2. **The Senior Architect / Staff Engineer** (Tech Lead, Principal Engineer, Security Reviewer):
   - Objectives: Wants to see *the C4 architecture model*, *Clean Architecture layer boundaries*, *how to run it locally via Taskfile / Docker*, and *ADR trade-off decisions*.
   - Time window: **5 to 10 minutes**.

---

## 2. Standard Document Anatomy

```
+-----------------------------------------------------------------+
| SECTION 1: EXECUTIVE & COMMERCIAL SUMMARY (Top 30%)             |
| - Project Title & Verified Badges (Live, Status, Cost 0.00 €)   |
| - The Problem & Business Impact                                 |
| - Verified Outcomes (Latency, Throughput, Cost Reduction)       |
| - 1-Click Live Demonstration Link                               |
+-----------------------------------------------------------------+
| ===================== THE TECHNICAL FOLD ====================== |
+-----------------------------------------------------------------+
| SECTION 2: ARCHITECTURAL & DEEP-DIVE SPECIFICATION (Lower 70%)  |
| - C4 Architecture Diagram (Context & Container via Mermaid)     |
| - Clean Architecture Layer Mapping                              |
| - Local Developer Experience (Taskfile & Docker Compose)        |
| - Architectural Decision Records (ADRs)                         |
| - OpenAPI / API Contract Specs                                  |
+-----------------------------------------------------------------+
```

---

## 3. Structural Checklist

Every flagship showcase repository must contain:

- [ ] **Badge Row**: Live link, Ecosystem Status, Operating Budget (0.00 EUR), CI status.
- [ ] **Executive Pitch**: Three bullet points detailing the problem, solution, and measurable outcome.
- [ ] **Interactive Link**: Clickable link to the production web application or demo.
- [ ] **C4 Diagram**: Native Mermaid flowchart showing components and transactional boundaries.
- [ ] **Local Run Protocol**: Exact command line instructions (`task dev`, `docker compose up`).
- [ ] **Key ADRs**: Links to `docs/decisions/` highlighting critical trade-offs.
