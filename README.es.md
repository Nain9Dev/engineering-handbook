<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/banner-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="docs/assets/banner-light.svg">
  <img alt="Plano arquitectónico de NainDev Engineering Handbook" src="docs/assets/banner-dark.svg" width="100%">
</picture>

# Manual de Ingeniería NainDev (Engineering Handbook)

> Los estándares definitivos de ingeniería, planos arquitectónicos, puertas de calidad y políticas de gobernanza para el ecosistema NainDev.

[![Language: English](https://img.shields.io/badge/Language-English-blue.svg)](README.md)
[![Versión en Español](https://img.shields.io/badge/Versión-Español-red.svg)](#)
[![Estado del ecosistema](https://img.shields.io/badge/status-active-success)](https://status.naindev.com)
[![Identidad de marca](https://img.shields.io/badge/brand-v1.5-blue)](https://brand.naindev.com)
[![Presupuesto cero](https://img.shields.io/badge/budget-0.00_EUR-brightgreen)](governance/zero-cost-philosophy.md)

---

## 1. Principios de ingeniería

1. **Determinismo sobre heurística**: Los sistemas de software deben producir resultados reproducibles y matemáticamente verificables. Se prohíben estrictamente las heurísticas ambiguas, los umbrales no calibrados y las validaciones no deterministas.
2. **Ingeniería dirigida por especificaciones (Spec-Driven)**: El código es el artefacto final de un proceso disciplinado. Cada proyecto comienza con fases numeradas de documentación (`docs/`), criterios de aceptación explícitos en sintaxis EARS y Registros de Decisiones de Arquitectura (ADRs).
3. **Verificación test-first (TDD)**: Una funcionalidad o corrección sin una prueba automatizada no existe. Las pruebas blindan cada refactorización estructural y definen el límite de comportamiento aceptable.
4. **Capas limpias y alta cohesión**: La lógica de dominio permanece completamente desacoplada de bases de datos, frameworks y protocolos de transporte. Las dependencias fluyen siempre hacia el interior.
5. **Huella de producción a coste cero**: Las aplicaciones del portfolio y las demostraciones técnicas operan con exactamente **0,00 EUR** de coste recurrente de infraestructura mediante computación estática en el borde, almacenamiento serverless gratuito y automatización pública de GitHub.

---

## 2. Camino pavimentado y radar tecnológico

```
+---------------------------------------------------------------------------------+
|                               ADOPTAR (Por defecto)                             |
+---------------------------------------------------------------------------------+
| Backend:      ASP.NET Core (.NET 10) Clean Architecture, Python 3.12+ (FastAPI) |
| 3D / Datos:   Trimesh, NumPy, SciPy, Shapely, Pydantic v2                       |
| Persistencia: PostgreSQL (Supabase/Neon), EF Core 10 (Escrituras), Dapper (Lecturas)|
| Frontend:     Vanilla JS (ESM), Astro (Estático/SSR), Vue 3 (TypeScript estricto)|
| Pruebas:      xUnit v3, Vitest, Playwright, PyTest + Hypothesis, Testcontainers |
| Edge/Hosting: GitHub Pages, Cloudflare Edge DNS/SSL, GitHub Actions CI/CD       |
+---------------------------------------------------------------------------------+
```

### Cuadrantes del radar

- **Backend Enterprise**: Controladores ASP.NET Core con Clean Architecture para flujos empresariales, límites transaccionales y APIs de alto rendimiento.
- **Datos, Geometría e IA**: Python (FastAPI + Pydantic v2) para computación espacial, evaluación determinista de mallas 3D (GLB/glTF), visión por computador y orquestación local de IA.
- **Frontend y Presentación**: Astro para portales orientados a contenido; Vue 3 con TypeScript estricto para SPAs interactivas; JavaScript moderno nativo (ES Modules, Web Components) para componentes universales sin dependencias (ej. `<nain-ecosystem-bar>`).
- **Base de datos y Persistencia**: PostgreSQL como fuente única de verdad; EF Core para escrituras en agregados; Dapper para proyecciones de lectura de latencia inferior al milisegundo.

---

## 3. Directorio del manual

| Sección | Tema | Resumen |
| :--- | :--- | :--- |
| **Arquitectura** | [Mapa de contextos empresariales](architecture/enterprise-context-map.md) | Contextos delimitados de DDD estratégico, planos de interacción y topología C4 Nivel 1/2. |
| **Arquitectura** | [Clean Architecture](architecture/clean-architecture.md) | Separación estricta de 4 capas (Domain, Application, Infrastructure, Presentation) para .NET y Python. |
| **Arquitectura** | [Patrón de doble persistencia](architecture/dual-persistence.md) | Separación inspirada en CQRS: escrituras transaccionales con EF Core, lecturas ultrarrápidas con Dapper. |
| **Arquitectura** | [Validación determinista 3D](architecture/deterministic-validation.md) | Estándares Khronos GLB/glTF, umbral cuántico ($10^{-12}\text{ m}^2$), verificaciones C1-C10, puertas fail-closed. |
| **Arquitectura** | [Funciones de idoneidad arquitectónica](architecture/fitness-functions.md) | Pruebas automatizadas en CI que imponen límites de capas (NetArchTest para .NET, linters AST para Python). |
| **Arquitectura** | [Modelado arquitectónico C4](architecture/c4-model-standard.md) | Modelado de Contexto, Contenedor y Componente mediante sintaxis nativa de diagramas Mermaid en GitHub. |
| **Arquitectura** | [Modernización de .NET heredado](architecture/legacy-dotnet-modernization.md) | Proxying Strangler Fig mediante YARP, WCF a gRPC y ejecución en sombra con cero inactividad. |
| **Arquitectura** | [Rúbrica de conformidad 3D](architecture/3d-conformance-rubric.md) | Criterios de auditoría industrial y umbrales de superación/fallo para modelos 3D y materiales PBR. |
| **Arquitectura** | [FinOps y planificación de capacidad](architecture/finops-capacity-planning.md) | Modelado de costes en dos niveles: portfolio a 0,00 EUR frente a escala empresarial de 10k pet/min. |
| **Gobernanza** | [Ciclo de vida Spec-Driven](governance/spec-driven-lifecycle.md) | Convención canónica numerada de carpetas `docs/` (`00` a `08`), puertas TDD y etiquetado de tareas. |
| **Gobernanza** | [Plantilla de ADR](governance/adr-template.md) | Formato estandarizado de Registro de Decisiones de Arquitectura para documentar compensaciones de diseño. |
| **Gobernanza** | [Due Diligence técnica](governance/technical-due-diligence.md) | Marco de evaluación de arquitecto Staff para bases de código, modularidad, FinOps y resiliencia. |
| **Gobernanza** | [Estándar de README para dos audiencias](governance/readme-specification.md) | Patrón de documentación de repositorios para decisores ejecutivos y arquitectos senior. |
| **Gobernanza** | [Gobernanza de Dependabot](governance/dependabot-governance.md) | Actualizaciones agrupadas semanales en npm, pip, nuget y actions para eliminar ruido en PRs. |
| **Gobernanza** | [Desarrollo local universal](governance/local-development.md) | Experiencia de desarrollo unificada mediante comandos estándar de Taskfile (task dev, task test, task build). |
| **Gobernanza** | [Filosofía de cero coste](governance/zero-cost-philosophy.md) | Principios y arquitecturas para ejecutar un portfolio multi-aplicación profesional a 0,00 EUR. |
| **Gobernanza** | [Estándar de identidad visual](governance/visual-identity-standard.md) | Estética blueprint técnica, banners vectoriales con doble tema e integración en repositorios. |
| **Gobernanza** | [Estándar de localización de UI](governance/ui-localization-standard.md) | Estándar de producto bilingüe (español por defecto, selector a inglés, código y docs 100% en inglés). |
| **Gobernanza** | [Política de licencias](governance/licensing-policy.md) | Estrategia de licenciamiento en dos niveles: Derechos Reservados para marca y portfolio vs. MIT/AGPL para código. |
| **Gobernanza** | [Tres Pilares de la Perfección](governance/three-pillars-perfection-standard.md) | Doctrina de calidad: Claridad visual UI, UX sin fricción (regla de los 3 pasos) e ingeniería tolerante a fallos. |

---

## 4. Estándar de seguimiento de tareas

Todas las tareas de ingeniería en las hojas de ruta de repositorios (`docs/05-tasks.md`) utilizan un etiquetado estricto de propiedad:

- `[A]` **Autónomo**: Tarea ejecutable de principio a fin por CI automatizado o asistente de IA sin intervención manual.
- `[M]` **Mutuo / Acción requerida**: Requiere autorización humana, acceso a cuentas externas o provisión de credenciales.
- `[H]` **Solo humano**: Reservado exclusivamente para el arquitecto principal (ej. estrategia de negocio, acuerdos o aprobaciones finales de producción).

---

## 5. Estándar de seguridad y aislamiento

- **Aislamiento de clientes**: El trabajo para empresas vive estrictamente aislado. Queda prohibido trasladar reglas de negocio, credenciales o esquemas internos a repositorios abiertos o personales.
- **Puerta de sanitización**: Todos los materiales públicos, guías de diseño y documentación arquitectónica se someten a escaneo automatizado para evitar nombres confidenciales o identificadores corporativos antes de su publicación.

---

**Autor**: Aitor Nain Mendoza Vallejo  
**Estándares**: [NainDev Engineering Handbook](https://github.com/Nain9Dev/engineering-handbook)
