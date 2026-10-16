# Clean Architecture Blueprint

> Standardized 4-layer architecture specification for enterprise .NET 10 and Python services within the NainDev ecosystem.

---

## 1. Architectural Philosophy

Clean Architecture separates business concerns from infrastructural mechanisms. The core principle is the **Dependency Rule**:

$$\text{Domain} \longleftarrow \text{Application} \longleftarrow \text{Infrastructure / Presentation}$$

Source code dependencies must point only inward toward higher-level policies. Entities at the center know nothing of databases, web frameworks, network protocols, or serialization formats.

```
       +-------------------------------------------------------+
       | Presentation (Controllers, Endpoints, ViewModels)    |
       |  +-------------------------------------------------+  |
       |  | Infrastructure (EF Core, Dapper, Mail, Adapters)|  |
       |  |  +-------------------------------------------+  |  |
       |  |  | Application (Commands, Queries, UseCases) |  |  |
       |  |  |  +-------------------------------------+  |  |  |
       |  |  |  | Domain (Entities, Value Objects)   |  |  |  |
       |  |  |  +-------------------------------------+  |  |  |
       |  |  +-------------------------------------------+  |  |
       |  +-------------------------------------------------+  |
       +-------------------------------------------------------+
```

---

## 2. Layer Specifications

### 2.1 Domain Layer (`Core.Domain`)
- **Responsibility**: Pure enterprise business rules, state invariants, and domain types.
- **Components**:
  - `Entities`: Objects defined by identity, maintaining invariants through encapsulated methods.
  - `Value Objects`: Immutable objects defined solely by their structural attributes.
  - `Domain Events`: Notifications of state transitions that have occurred within the domain boundary.
  - `Domain Exceptions`: Strongly typed business failure modes.
- **Constraints**:
  - Zero external NuGet or PyPI packages (except core language extensions like `MediatR.Contracts` or `Pydantic`).
  - No database references or ORM attributes (`[Table]`, `[Key]`).

### 2.2 Application Layer (`Core.Application`)
- **Responsibility**: Orchestration of domain entities to fulfill explicit use cases.
- **Components**:
  - `Commands & Handlers`: Intent to mutate state (e.g., `CreateAssetCommand`, `SubmitReviewHandler`).
  - `Queries & Handlers`: Intent to retrieve read models without modifying state.
  - `DTOs & Mappers`: Plain data structures for cross-boundary transport.
  - `Interfaces`: Ports for persistence, external services, and asynchronous message brokers (`IAssetRepository`, `IClockService`).
  - `Pipeline Behaviors`: Cross-cutting concerns such as logging, validation, and transaction management.
- **Constraints**:
  - Does not reference database connections, HTTP contexts, or file systems directly.
  - All input parameters are validated against strict contracts before execution (e.g., FluentValidation or Pydantic V2).

### 2.3 Infrastructure Layer (`Infrastructure`)
- **Responsibility**: Implementation of interfaces defined by Application and Domain layers.
- **Components**:
  - `Persistence`: EF Core DbContext, entity configurations, migrations, Dapper connection factories.
  - `Adapters`: Cloud storage clients (S3/GCS), email dispatchers, external HTTP clients.
  - `Time & System`: Implementation of real-time clocks and process launchers.
- **Constraints**:
  - Encapsulates all third-party SDK dependencies.
  - Catches provider-specific exceptions (e.g., `SqlException`, `BotoCoreError`) and maps them to domain or application abstractions.

### 2.4 Presentation Layer (`Presentation` / `Api`)
- **Responsibility**: Translation between external transport mechanisms (HTTP, gRPC, CLI) and application use cases.
- **Components**:
  - `Endpoints / Controllers`: Thin route handlers mapping requests to Application use cases.
  - `Middleware`: Global exception handling, security headers, correlation IDs, rate limiting.
  - `Contracts`: API request/response schemas exposed to consumers.
- **Constraints**:
  - No direct database access.
  - Zero business logic in controllers.

---

## 3. Implementation Parity: .NET vs Python

| Layer Concept | .NET 10 | Python (FastAPI) |
| :--- | :--- | :--- |
| **Domain Entity** | `public class Asset : EntityBase<AssetId>` | `@dataclass` or custom class with private state |
| **Value Object** | `public record Dimensions(double X, double Y, double Z);` | `@dataclass(frozen=True)` with invariant validation |
| **Use Case Handler** | `IRequestHandler<TCommand, Result<TResponse>>` | Dedicated callable class or async function `UseCase(repo)` |
| **Ports (Interfaces)** | `public interface IAssetRepository` | `typing.Protocol` or `abc.ABC` |
| **Persistence Adapter** | `AssetRepository : IAssetRepository` | `SqlAlchemyAssetRepository(IAssetRepository)` |
| **Validation** | `AbstractValidator<TCommand>` (FluentValidation) | `pydantic.BaseModel` with field validators |
| **API Boundary** | ASP.NET Core Controller with `[ApiController]` | FastAPI APIRouter with dependency injection (`Depends`) |
