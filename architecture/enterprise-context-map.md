# NainDev Enterprise Context Map & System Topology

> Strategic Domain-Driven Design (DDD) context mapping, C4 architectural topology, and interaction blueprints across the NainDev platform ecosystem.

---

## 1. Executive Architecture Summary

The NainDev platform ecosystem is architected as a set of autonomous, highly cohesive Bounded Contexts. Rather than a monolithic codebase or an uncoordinated sprawl of microservices, each service adheres to strict strategic DDD boundaries, clear upstream/downstream integration contracts, and universal architectural invariants.

```mermaid
C4Context
    title NainDev Platform Ecosystem - System Context Diagram (C4 Level 1)

    Person(consumer, "End User / Client", "Interacts with web applications, CAD tools, and client portals.")
    Person(admin, "Platform Operator", "Monitors platform health, reconciles accounting, and manages policies.")

    Enterprise_Boundary(naindev_platform, "NainDev Platform Ecosystem") {
        System(gateway, "Portfolio Gateway & Hub", "Central entry point, brand design tokens, and universal navigation.")
        System(status_sys, "Platform Status & Observability", "Real-time blackbox monitoring, uptime metrics, and SLA accounting.")
        
        System(cad_engine, "ParametriCAD AI & Nain3D", "Core Domain: Deterministic OpenCASCADE CAD engine & 3D conformance gateway.")
        System(order_sys, "NainOrder", "Supporting Domain: High-throughput e-commerce order engine with CQRS & DDD aggregates.")
        System(notify_sys, "NainNotify", "Generic Subdomain: Asynchronous notification dispatch microservice with sub-50ms latency.")
        System(policy_sys, "NainPolicy", "Supporting Domain: High-assurance insurance lifecycle & financial ledger accounting.")
        System(drive_sys, "NainDrive", "Supporting Domain: Driving school management & decimal accounting engine.")
        System(write_sys, "NainWrite", "Generic Subdomain: Deterministic natural Spanish rewriting engine with semantic preservation.")
    }

    System_Ext(broker, "Message Broker (RabbitMQ)", "Enterprise AMQP transport for decoupled domain events.")
    System_Ext(cloudflare, "Cloudflare Edge", "DNS, SSL termination, DDoS mitigation, and global CDN delivery.")

    Rel(consumer, gateway, "Discovers and launches applications", "HTTPS")
    Rel(consumer, cad_engine, "Builds parametric solids & audits 3D models", "WebGL / HTTPS")
    Rel(consumer, order_sys, "Submits transactional purchase orders", "HTTPS / REST")
    Rel(consumer, drive_sys, "Manages classes and billing", "HTTPS / REST")
    Rel(consumer, write_sys, "Submits text for semantic rewriting", "HTTPS / Web Worker")

    Rel(admin, status_sys, "Inspects uptime and latency percentiles", "HTTPS")
    Rel(admin, policy_sys, "Audits policy balances & ledger entries", "HTTPS / REST")

    Rel(order_sys, broker, "Publishes OrderPlacedDomainEvent", "AMQP 0-9-1")
    Rel(broker, notify_sys, "Consumes events and dispatches notifications", "AMQP 0-9-1")
    Rel(order_sys, policy_sys, "Syncs financial transactions & invoices", "HTTPS / REST")
```

---

## 2. Strategic DDD Bounded Contexts

The ecosystem classifies all systems into four strategic quadrants:

```
+---------------------------------------------------------------------------------------+
|                                  CORE DOMAIN (Competitive Advantage)                  |
|  - ParametriCAD AI: Exact boundary representation (B-Rep) modeling via OpenCASCADE.    |
|  - Nain3D Gateway: Deterministic Khronos conformance & geometric invariant auditing.  |
+---------------------------------------------------------------------------------------+
|                                  SUPPORTING DOMAINS (Business Specific)               |
|  - NainOrder: CQRS order processing, stock reservations, and aggregate state.         |
|  - NainPolicy: Insurance policy lifecycle, actuarial calculations, financial ledger. |
|  - NainDrive: Fleet management, driving student scheduling, exact decimal billing.   |
+---------------------------------------------------------------------------------------+
|                                  GENERIC SUBDOMAINS (Utility / Shared)                |
|  - NainNotify: Asynchronous dispatch (email/SMS/webhooks) over MassTransit/RabbitMQ. |
|  - NainWrite: Natural Spanish text normalization and semantic invariant gating.       |
+---------------------------------------------------------------------------------------+
|                                  PLATFORM HUB & OBSERVABILITY                         |
|  - nain9dev.github.io: Single source of truth architectural gateway.                 |
|  - brandbook: Mathematical design tokens, color geometry, and accessible tokens.     |
|  - status: Serverless blackbox monitoring and rolling SLA verification.               |
+---------------------------------------------------------------------------------------+
```

---

## 3. Integration Patterns & Context Relationships

Between bounded contexts, strict relationship patterns govern data ownership and contract stability:

| Upstream Context | Downstream Context | Relationship Pattern | Protocol / Transport | Contract Guarantee |
| :--- | :--- | :--- | :--- | :--- |
| **NainOrder** | **NainNotify** | **Customer / Supplier** | AMQP 0-9-1 (RabbitMQ via MassTransit) | At-least-once delivery, decoupled asynchronous fire-and-forget, sub-50ms dispatch. |
| **NainOrder** | **NainPolicy** | **Published Language** | REST / HTTPS (RFC 7807 ProblemDetails) | Strongly typed transactional invoice payloads, integer cents currency representation. |
| **Nain3D** | **ParametriCAD AI** | **Shared Kernel** (Geometry Math) | In-process Python bindings (`trimesh`, `numpy`, OpenCASCADE) | Universal quantum threshold ($10^{-12}\text{ m}^2$), checks C1 through C10. |
| **NainConfigurator** | **Nain3D** | **Conformist** | HTTPS / GLB Artifact Exchange | Assets strictly conform to glTF 2.0 validation rubric before configuration rendering. |
| **External Clients** | **All Backends** | **Open Host Service (OHS)** | REST / JSON with OpenAPI 3.1 schema | Versioned routes, CORS allowlists, strict schema validation. |

---

## 4. Container Topology & Protocol Blueprint (C4 Level 2)

```mermaid
C4Container
    title NainDev Platform Container Architecture (C4 Level 2)

    Container(spa_hub, "Platform Hub & Portal", "Astro / TypeScript", "Portfolio discovery and architectural gateway.")
    Container(spa_cad, "ParametriCAD Client", "Vue 3 / Three.js", "Interactive parametric CAD canvas with real-time WebGL rendering.")
    Container(spa_order, "NainOrder Client", "Vanilla JS / ESM", "Accessible order placement and inventory control console.")

    Container(api_cad, "ParametriCAD API", "Python 3.12 / FastAPI", "Solid modeling, topological analysis, and STEP/STL export.")
    Container(api_order, "NainOrder API", ".NET 10 / ASP.NET Core", "Clean Architecture CQRS command and query processor.")
    Container(api_notify, "NainNotify Worker", ".NET 10 / MassTransit", "Asynchronous event subscriber and dispatch pipeline.")
    Container(api_policy, "NainPolicy API", ".NET 10 / ASP.NET Core", "Double-entry bookkeeping and policy state machine.")

    ContainerDb(db_order, "Order Database", "PostgreSQL", "Stores orders, line items, and concurrency tokens.")
    ContainerDb(db_policy, "Policy Database", "SQL Server / PostgreSQL", "Stores immutable policy ledger and financial entries.")
    ContainerDb(cache_broker, "Message Broker", "RabbitMQ", "Durable queues and topic exchanges for domain events.")

    Rel(spa_hub, api_cad, "Invokes CAD endpoints", "HTTPS / REST")
    Rel(spa_cad, api_cad, "Sends parametric parameters", "HTTPS / REST")
    Rel(spa_order, api_order, "Submits order commands", "HTTPS / REST")

    Rel(api_order, db_order, "Dual Persistence: EF Core writes, Dapper reads", "TCP / TLS")
    Rel(api_order, cache_broker, "Publishes OrderPlacedEvent", "AMQP 0-9-1")
    Rel(cache_broker, api_notify, "Consumes events", "AMQP 0-9-1")
    Rel(api_policy, db_policy, "Audited financial ledger", "TCP / TLS")
```

---

## 5. Universal Architectural Invariants

Every system inside the NainDev ecosystem enforces five non-negotiable invariants:

1. **Inward Dependency Rule**: Domain aggregates never import from Application, Infrastructure, or Presentation layers. Enforced via automated architecture fitness functions (`NetArchTest` in .NET, `AST` inspection in Python) in continuous integration.
2. **Deterministic Representation**: All financial amounts are stored as 64-bit integer cents (avoiding binary IEEE 754 floating-point inaccuracies). All 3D geometric validations execute against formal tolerance thresholds ($10^{-12}\text{ m}^2$).
3. **Optimistic Concurrency Control**: All state-modifying entities expose version tokens (`xmin` in PostgreSQL, `rowversion` in SQL Server, or integer version fields) to guarantee that race conditions result in clean `409 Conflict` responses rather than silent data overwrites.
4. **Structured Error Protocol (RFC 7807)**: API errors conform strictly to ProblemDetails JSON payloads, eliminating non-standard ad-hoc error formats and safeguarding internal stack traces from leaking to clients.
5. **Observability & Traceability**: All HTTP requests and AMQP messages carry W3C Trace Context headers (`traceparent`) for end-to-end distributed transaction tracing.
