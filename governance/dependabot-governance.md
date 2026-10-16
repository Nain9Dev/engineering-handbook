# Dependabot Governance & Grouping Standard

> Policies and configuration standards for automated dependency updates across polyrepo architectures with zero alert fatigue.

---

## 1. The Alert Fatigue Problem

In ecosystems spanning 15+ repositories across multiple language runtimes (.NET, Python, Node), unconfigured dependency bots generate hundreds of isolated pull requests. This causes:

1. **Review Exhaustion**: Architects ignore notifications due to overwhelming volume.
2. **CI Resource Waste**: Each patch version bump triggers separate continuous integration runs.
3. **Merge Conflicts**: Staggered PRs across interdependent sub-packages conflict with one another.

---

## 2. Grouped Updates Policy

All repositories within the NainDev ecosystem adopt **Grouped Dependabot Updates** governed by the canonical `.github/dependabot.yml` specification:

1. **Weekly Frequency**: Updates run on **Mondays at 06:00 UTC**, leaving weekdays free for feature delivery.
2. **Semantic Version Partitioning**:
   - `minor` and `patch` releases are automatically bundled into a single weekly pull request per ecosystem.
   - `major` breaking version updates require explicit manual evaluation or separate isolated review.
3. **PR Limits**: A maximum of 3 concurrent pull requests per ecosystem are permitted.

---

## 3. Canonical Configuration Pattern

```yaml
version: 2
updates:
  # 1. GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    groups:
      actions:
        patterns: ["*"]

  # 2. Frontend / Node.js
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    groups:
      dependencies:
        dependency-type: "production"
        update-types: ["minor", "patch"]
      dev-dependencies:
        dependency-type: "development"
        update-types: ["minor", "patch"]

  # 3. Python Microservices
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    groups:
      python-dependencies:
        patterns: ["*"]
        update-types: ["minor", "patch"]

  # 4. .NET 10 Solutions
  - package-ecosystem: "nuget"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    groups:
      dotnet-dependencies:
        patterns: ["*"]
        update-types: ["minor", "patch"]
```
