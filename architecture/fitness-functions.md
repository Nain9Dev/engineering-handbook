# Architectural Fitness Functions

> Automated tests executing within CI/CD that deterministically assert structural integrity, layer isolation, and dependency rules across .NET and Python services.

---

## 1. Why Fitness Functions?

Manual architecture reviews and verbal conventions degrade over time as codebases expand. Architectural Fitness Functions translate architectural invariants into automated test suites.

If a developer or automated tool introduces a reference from `Domain` to `Infrastructure`, the test suite fails immediately during local execution and in CI, blocking deployment.

---

## 2. .NET 10 Standards (ArchUnitNET)

In enterprise .NET solutions, architecture tests reside in a dedicated test project (`tests/*.ArchitectureTests`) using [ArchUnitNET](https://github.com/T-Systems-MMS/ArchUnitNET).

### 2.1 Canonical Invariants

1. **Domain Isolation**: The `Domain` layer must not depend on any outer layer (`Application`, `Infrastructure`, `Presentation`).
2. **Application Decoupling**: The `Application` layer must not depend on `Infrastructure` or `Presentation`.
3. **Encapsulation Invariant**: Aggregate Root entities in `Domain` must not expose public property setters (`set;`).
4. **Interface Implementation**: All Command and Query handlers must reside within `Application` and implement MediatR / CQRS interfaces.

### 2.2 Reference Implementation

```csharp
using ArchUnitNET.Domain;
using ArchUnitNET.Loader;
using ArchUnitNET.NUnit;
using static ArchUnitNET.Fluent.ArchRuleDefinition;
using Assembly = System.Reflection.Assembly;

namespace NainDev.ArchitectureTests;

public class CleanArchitectureFitnessTests
{
    private static readonly Architecture Architecture =
        new ArchLoader().LoadAssemblies(
            Assembly.Load("NainOrder.Domain"),
            Assembly.Load("NainOrder.Application"),
            Assembly.Load("NainOrder.Infrastructure"),
            Assembly.Load("NainOrder.Api")
        ).Build();

    private readonly IObjectProvider<IType> DomainLayer =
        Types().That().ResideInAssembly("NainOrder.Domain").As("Domain Layer");

    private readonly IObjectProvider<IType> ApplicationLayer =
        Types().That().ResideInAssembly("NainOrder.Application").As("Application Layer");

    private readonly IObjectProvider<IType> InfrastructureLayer =
        Types().That().ResideInAssembly("NainOrder.Infrastructure").As("Infrastructure Layer");

    private readonly IObjectProvider<IType> PresentationLayer =
        Types().That().ResideInAssembly("NainOrder.Api").As("Presentation Layer");

    [Fact]
    public void Domain_MustNot_DependOn_OtherLayers()
    {
        IArchRule rule = Types().That().Are(DomainLayer)
            .Should().NotDependOnAny(ApplicationLayer)
            .AndShould().NotDependOnAny(InfrastructureLayer)
            .AndShould().NotDependOnAny(PresentationLayer);

        rule.Check(Architecture);
    }

    [Fact]
    public void Application_MustNot_DependOn_InfrastructureOrPresentation()
    {
        IArchRule rule = Types().That().Are(ApplicationLayer)
            .Should().NotDependOnAny(InfrastructureLayer)
            .AndShould().NotDependOnAny(PresentationLayer);

        rule.Check(Architecture);
    }

    [Fact]
    public void Entities_MustHave_EncapsulatedSetters()
    {
        IArchRule rule = Classes().That().ResideInNamespace("NainOrder.Domain.Entities")
            .Should().HaveImmutableProperties();

        rule.Check(Architecture);
    }
}
```

---

## 3. Python Standards (AST Layer Linters)

In Python and FastAPI microservices, structural layering is verified via an automated Abstract Syntax Tree (AST) test running under `pytest` with zero external dependencies.

### 3.1 Layer Invariants

```
src/
├── domain/          # PURE: Never imports application, infrastructure, or api
├── application/     # ORCHESTRATION: Imports domain; NEVER infrastructure or api
├── infrastructure/  # ADAPTERS: Imports domain and application; NEVER api
└── api/             # PRESENTATION: Imports application and domain DTOs
```

### 3.2 Reference Implementation (`tests/test_architecture_fitness.py`)

```python
import ast
import os
import pytest

ROOT_SRC = os.path.abspath(os.path.join(os.path.dirname(__file__), "..", "src"))

FORBIDDEN_IMPORTS = {
    "domain": ["infrastructure", "api", "web"],
    "application": ["infrastructure", "api", "web"],
    "infrastructure": ["api", "web"],
}

def get_imported_modules(filepath: str) -> list[str]:
    """Parses Python file AST to extract all imported top-level modules."""
    with open(filepath, "r", encoding="utf-8") as f:
        tree = ast.parse(f.read(), filename=filepath)

    imports = []
    for node in ast.walk(tree):
        if isinstance(node, ast.Import):
            for alias in node.names:
                imports.append(alias.name)
        elif isinstance(node, ast.ImportFrom) and node.module:
            imports.append(node.module)
    return imports

@pytest.mark.parametrize("layer,forbidden", FORBIDDEN_IMPORTS.items())
def test_clean_architecture_layer_isolation(layer: str, forbidden: list[str]):
    """Asserts that inner layers never import from outer layers."""
    layer_dir = os.path.join(ROOT_SRC, layer)
    if not os.path.exists(layer_dir):
        pytest.skip(f"Layer directory {layer} not present")

    violations = []
    for root, _, files in os.walk(layer_dir):
        for file in files:
            if not file.endswith(".py"):
                continue
            path = os.path.join(root, file)
            imports = get_imported_modules(path)
            for imp in imports:
                for bad in forbidden:
                    if imp.startswith(bad) or f".{bad}" in imp:
                        violations.append(f"{os.path.relpath(path, ROOT_SRC)} imports '{imp}'")

    assert not violations, f"Layer '{layer}' violates Clean Architecture:\n" + "\n".join(violations)
```
