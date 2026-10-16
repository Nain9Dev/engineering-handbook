# Universal Local Development Standard (Taskfile)

> Standardized command interface across polyglot repositories (.NET, Python, Astro, Vue 3) to ensure deterministic, friction-free local developer experience.

---

## 1. Unified Command Paradigm

Regardless of the underlying language runtime, every repository within the NainDev ecosystem must implement identical top-level task commands:

| Command | Objective | Polyglot Behavior |
| :--- | :--- | :--- |
| `task dev` | Launch the local development environment | Runs ASP.NET Kestrel, FastAPI Uvicorn, or Astro dev server. |
| `task test` | Execute the complete test suite | Runs `dotnet test`, `pytest`, or `npm test` with coverage. |
| `task build` | Compile and package production bundle | Runs `dotnet publish`, `pyinstaller`, or `npm run build`. |
| `task lint` | Check formatting and code quality | Runs `dotnet format --verify-no-changes`, `ruff check`, or `eslint`. |
| `task clean` | Remove temporary binaries and caches | Cleans `bin/`, `obj/`, `__pycache__/`, `dist/`, `.astro/`. |

---

## 2. Canonical `Taskfile.yml` Templates

### 2.1 .NET 10 Solution Template

```yaml
version: '3'

tasks:
  dev:
    desc: "Run backend API locally with hot reload"
    cmds:
      - dotnet watch --project src/NainOrder.Api run

  test:
    desc: "Execute all unit, integration, and architecture fitness tests"
    cmds:
      - dotnet test -c Release --verbosity normal

  build:
    desc: "Build release binaries"
    cmds:
      - dotnet build -c Release

  lint:
    desc: "Verify code style and formatting"
    cmds:
      - dotnet format --verify-no-changes

  clean:
    desc: "Remove compiled artifacts"
    cmds:
      - git clean -Xdf -e "!*.env"
```

### 2.2 Python (FastAPI) Template

```yaml
version: '3'

tasks:
  dev:
    desc: "Start FastAPI development server with auto-reload"
    cmds:
      - uvicorn src.api.main:app --reload --port 8000

  test:
    desc: "Execute pytest suite with coverage and layer fitness checks"
    cmds:
      - pytest -v --cov=src

  lint:
    desc: "Run Ruff formatting and linter checks"
    cmds:
      - ruff check .
      - ruff format --check .

  clean:
    desc: "Remove Python bytecode and test caches"
    cmds:
      - find . -type d -name "__pycache__" -exec rm -rf {} +
      - rm -rf .pytest_cache .coverage htmlcov
```

### 2.3 Frontend & Static Sites (Astro / Vue 3)

```yaml
version: '3'

tasks:
  dev:
    desc: "Start local development server"
    cmds:
      - npm run dev

  test:
    desc: "Run unit and contract test suites"
    cmds:
      - npm test

  build:
    desc: "Compile optimized production assets"
    cmds:
      - npm run build

  lint:
    desc: "Validate code formatting and standards"
    cmds:
      - npm run check

  clean:
    desc: "Clear dist and build caches"
    cmds:
      - rm -rf dist .astro node_modules/.cache
```
