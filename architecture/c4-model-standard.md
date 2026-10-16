# C4 Architectural Modeling Standard (GitHub Mermaid)

> Standardized system architecture modeling across Context, Container, Component, and Code levels, formatted in GitHub-native Mermaid syntax.

---

## 1. Principles of C4 Modeling

Ad-hoc box-and-arrow diagrams create ambiguity because they mix logical boundaries, deployment nodes, and transport protocols. The **C4 Model** provides a hierarchical zoomable map of software systems:

1. **Level 1: System Context**: Who uses the system and how does it integrate into the external landscape?
2. **Level 2: Container**: What high-level executables, databases, and client frontends form the overall system?
3. **Level 3: Component**: What internal modular boundaries and Clean Architecture layers exist inside each container?
4. **Level 4: Code**: Class definitions, entity invariants, and interfaces.

> [!NOTE]
> GitHub's Markdown renderer does not natively parse `C4Context` keywords. NainDev models all C4 levels using GitHub-native `flowchart TB` syntax with explicit class styling and subgraphs for 100% reliable rendering.

---

## 2. Level 1: System Context Diagram

```mermaid
flowchart TB
    classDef person fill:#0047AB,stroke:#002B66,stroke-width:2px,color:#fff;
    classDef system fill:#002B66,stroke:#38bdf8,stroke-width:2px,color:#fff;
    classDef external fill:#1e293b,stroke:#64748b,stroke-width:1px,color:#e2e8f0;

    user[Commercial User / Operations Lead]:::person
    nainOrder[NainOrder E-Commerce API]:::system
    stripe[External: Payment Gateway]:::external
    auth0[External: OIDC Identity Provider]:::external

    user -->|Submits orders & views invoices via HTTPS| nainOrder
    nainOrder -->|Processes payments| stripe
    nainOrder -->|Validates JWT tokens| auth0
```

---

## 3. Level 2: Container Diagram

```mermaid
flowchart TB
    classDef client fill:#0369a1,stroke:#0284c7,stroke-width:2px,color:#fff;
    classDef api fill:#002B66,stroke:#38bdf8,stroke-width:2px,color:#fff;
    classDef worker fill:#0f766e,stroke:#14b8a6,stroke-width:2px,color:#fff;
    classDef db fill:#334155,stroke:#94a3b8,stroke-width:1px,color:#fff;

    clientApp[Vue 3 SPA Client<br/>TypeScript / Vite]:::client
    apiGateway[ASP.NET Core Web API<br/>.NET 10 Controllers]:::api
    msgBroker[Asynchronous Queue<br/>RabbitMQ / In-Memory]:::worker
    bgWorker[Background Fulfillment Worker<br/>.NET 10 HostedService]:::worker
    sqlDb[(PostgreSQL Primary<br/>Transactional Store)]:::db
    readCache[(Redis Cache<br/>Hot Read Projections)]:::db

    clientApp -->|REST / JSON-RPC over HTTPS| apiGateway
    apiGateway -->|Transactional Writes via EF Core| sqlDb
    apiGateway -->|High-Throughput Reads via Dapper| sqlDb
    apiGateway -->|Query Projections| readCache
    apiGateway -->|Publishes OrderSubmittedEvent| msgBroker
    msgBroker -->|Consumes events| bgWorker
    bgWorker -->|Updates fulfillment status| sqlDb
```

---

## 4. Level 3: Component Diagram (Clean Architecture)

```mermaid
flowchart TB
    classDef pres fill:#1e3a8a,stroke:#3b82f6,stroke-width:1px,color:#fff;
    classDef app fill:#1e1b4b,stroke:#818cf8,stroke-width:1px,color:#fff;
    classDef domain fill:#064e3b,stroke:#34d399,stroke-width:2px,color:#fff;
    classDef infra fill:#374151,stroke:#9ca3af,stroke-width:1px,color:#fff;

    subgraph PresentationLayer [Presentation Layer]
        controller[OrdersController]:::pres
        errorHandler[ProblemDetailsMiddleware]:::pres
    end

    subgraph ApplicationLayer [Application Layer]
        command[SubmitOrderCommand]:::app
        handler[SubmitOrderCommandHandler]:::app
        validator[OrderValidator FluentValidation]:::app
        repoInterface[IOrderRepository Port]:::app
    end

    subgraph DomainLayer [Domain Layer - Core Invariants]
        orderAggregate[Order Aggregate Root]:::domain
        orderLine[OrderLineItem Value Object]:::domain
        orderStatus[OrderStatus Enum]:::domain
    end

    subgraph InfrastructureLayer [Infrastructure Layer]
        efRepo[OrderRepository EF Core Adapter]:::infra
        dapperQueries[OrderQueries Dapper Adapter]:::infra
    end

    controller -->|Dispatches| command
    command -->|Validated by| validator
    command -->|Handled by| handler
    handler -->|Loads / Mutates| orderAggregate
    orderAggregate -->|Contains| orderLine
    handler -->|Persists via| repoInterface
    repoInterface -.->|Implemented by| efRepo
    controller -->|Reads via| dapperQueries
```
