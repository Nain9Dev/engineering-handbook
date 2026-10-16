# Deterministic 3D Asset Validation

> Engineering specification for reproducible, fail-closed validation of 3D digital assets (GLB / glTF) across e-commerce, manufacturing, and spatial computing pipelines.

---

## 1. Principles of Deterministic Validation

3D asset pipelines frequently suffer from non-deterministic evaluation caused by platform-dependent floating point rounding, uncalibrated spatial tolerances, and arbitrary threshold guessing. NainDev implements deterministic spatial validation governed by three non-negotiable principles:

1. **Quantum Area Threshold ($\varepsilon = 10^{-12}\text{ m}^2$)**: Microscopic triangle surfaces below $10^{-12}\text{ m}^2$ produce numerical instability during cross product normal calculations and are classified as degenerate geometry.
2. **Fail-Closed Aggregation**: An asset is considered **NON-CONFORMANT** unless every single evaluation check explicitly passes. A runtime error, parsing failure, or missing check result triggers immediate overall failure.
3. **Dual-Contract Output**: Every validation run emits:
   - A **Machine Contract**: Deterministic JSON adhering strictly to a versioned JSON Schema for automated CI/CD gating and ERP integration.
   - A **Human Contract**: Clean, structured GitHub Flavored Markdown report detailing exact failure coordinates, vertex indices, and remediation advice.

---

## 2. Check Taxonomy (C1 through C10)

| Check ID | Classification | Gate Type | Description | Mathematical Invariant |
| :--- | :--- | :--- | :--- | :--- |
| **C1** | Container & glTF Header | Hard Gate | Validates 12-byte header, magic bytes (`0x46546C67`), and valid JSON chunk length. | $\text{Magic} == \text{"glTF"} \land \text{Version} \in [2.0]$ |
| **C2** | Bounding Box & Dimensions | Hard Gate | Checks physical AABB against configured domain limits (X, Y, Z). | $\forall \text{axis} \in \{x,y,z\}: \text{Min} \le \Delta_{\text{axis}} \le \text{Max}$ |
| **C3** | Scale & Unit Conformance | Hard Gate | Verifies scene scale matrices do not contain unapplied transformations or non-uniform scaling. | $\| S_x - S_y \| < 10^{-6} \land \| S_y - S_z \| < 10^{-6}$ |
| **C4** | Mesh Manifoldness & Watertightness | Hard Gate | Verifies that every edge is shared by exactly 2 faces (watertight closed volume). | $\forall e \in E: \text{count}(f \mid e \in f) = 2$ |
| **C5** | Degenerate Triangles | Hard Gate | Detects collinear vertices and zero-area faces using double-precision cross products. | $\frac{1}{2} \| (v_1 - v_0) \times (v_2 - v_0) \| \ge 10^{-12}\text{ m}^2$ |
| **C6** | Face Normal Consistency | Quality Gate | Validates that face normal orientations do not flip across adjacent manifold faces. | $\vec{n}_A \cdot \vec{n}_B \ge \cos(\theta_{\text{max}})$ for smooth surfaces |
| **C7** | UV Texel Density & Overlap | Quality Gate | Measures differential texel density ($px/cm$) across all UV islands; identifies illegal overlaps. | $\text{STD}(\text{TD}_{\text{island}}) / \text{Mean}(\text{TD}_{\text{island}}) \le \tau_{\text{var}}$ |
| **C8** | Texture Resolution & Power-of-Two | Quality Gate | Asserts all embedded and external textures adhere to power-of-two (PoT) dimensions up to 4K. | $\log_2(\text{width}) \in \mathbb{N} \land \log_2(\text{height}) \in \mathbb{N}$ |
| **C9** | Material & Sampler Wraps | Quality Gate | Validates PBR roughness/metallic channels and sampler wrap modes (`CLAMP_TO_EDGE` vs `REPEAT`). | Defined in glTF 2.0 Material Specification |
| **C10** | Polycount & Draw Call Budget | Resource Gate | Gating on total vertex count, triangle count, and draw call batch count for target platform. | $\sum \text{DrawCalls} \le N_{\text{budget}}$ |

---

## 3. Numerical Stability & Floating-Point Safe Math

Cross products and determinant evaluations in 3D geometry must avoid catastrophic cancellation. Computations are performed in 64-bit IEEE-754 precision (`float64` / `double`):

```python
import numpy as np

def compute_triangle_area(v0: np.ndarray, v1: np.ndarray, v2: np.ndarray) -> float:
    """Calculates triangle surface area with high-precision cross product."""
    # Ensure float64 precision
    e1 = (v1 - v0).astype(np.float64)
    e2 = (v2 - v0).astype(np.float64)
    cross = np.cross(e1, e2)
    # Area = 0.5 * ||e1 x e2||
    area = 0.5 * np.linalg.norm(cross)
    return float(area)

def is_degenerate(v0: np.ndarray, v1: np.ndarray, v2: np.ndarray, threshold: float = 1e-12) -> bool:
    """Evaluates whether face area falls below the quantum limit."""
    return compute_triangle_area(v0, v1, v2) < threshold
```

---

## 4. Fail-Closed Aggregation Pipeline

```
     [GLB Input]
          |
          v
   [C1: Container] ---> (Invalid) ---> [HALT & FAIL REPORT]
          | (Valid)
          v
   [C2..C5: Geometry Checks] 
          |
   (Any Failure?) ===> [AGGREGATE = FAIL, ExitCode = 1]
          | (All Pass)
          v
   [C6..C10: Quality & Resource Checks]
          |
          v
   [Dual-Contract Generation]
     ├── report.json (Machine Contract)
     └── report.md   (Human Contract)
```
