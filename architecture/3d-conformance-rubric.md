# 3D Asset Conformance Audit Rubric

> Deterministic evaluation rubric and gating thresholds for industrial 3D assets (GLB / glTF) across geometry, UV mapping, materials, and runtime budgets.

---

## 1. Conformance Gating Architecture

```
                       [Candidate 3D Asset]
                                |
               +----------------+----------------+
               |                                 |
         (Hard Gates)                     (Quality Gates)
               |                                 |
         Must Pass 100%                   Weighted Threshold (>=85%)
   - Container Integrity (C1)        - Face Normal Smoothing (C6)
   - Bounding Box Alignment (C2)     - UV Texel Density Variance (C7)
   - Unit Scale [1.0] (C3)           - Power-of-Two Textures (C8)
   - Mesh Manifoldness (C4)          - PBR Material Standards (C9)
   - Zero Degenerate Faces (C5)      - Draw Call / Poly Budget (C10)
               |                                 |
               +----------------+----------------+
                                |
                                v
               [Conformance Decision Engine]
                   PASS (100% Hard + >=85% Quality)
                   FAIL (Any Hard Gate or <85% Quality)
```

---

## 2. Detailed Evaluation Rubric

| Category | Check | Evaluation Method | Pass Criterion | Hard Gate? |
| :--- | :--- | :--- | :--- | :---: |
| **Format** | C1: GLB Container | Khronos Validator / Header Parser | Magic `0x46546C67`, glTF 2.0 valid JSON chunk | **YES** |
| **Bounds** | C2: Bounding Box | Oriented Bounding Box (OBB) & AABB | Fits within domain envelope $\Delta x, \Delta y, \Delta z$ | **YES** |
| **Scale** | C3: Unit System | Scene Root Transformation Matrix | Uniform scale $[1, 1, 1]$ in meters ($1\text{ unit} = 1\text{ m}$) | **YES** |
| **Topology** | C4: Manifoldness | Half-Edge Data Structure Analysis | Watertight geometry; each edge shared by exactly 2 faces | **YES** |
| **Geometry** | C5: Degenerate Faces | Triangle Cross-Product Area | Surface area $\ge 10^{-12}\text{ m}^2$ (Quantum Threshold) | **YES** |
| **Normals** | C6: Normal Continuity | Face-to-Face Dot Product | Smooth transitions, zero flipped winding orders | NO |
| **Texturing** | C7: Texel Density | Differential UV Space Ratio ($px/cm$) | $\frac{\sigma(\text{TD})}{\mu(\text{TD})} \le 0.20$ across UV islands | NO |
| **Textures** | C8: Resolution | Image Dimension Inspector | Width & height are power-of-two ($\le 4096$) | NO |
| **PBR** | C9: Material Wrap | Sampler Inspection | Valid metallic-roughness channels; correct wrap modes | NO |
| **Budget** | C10: Draw Calls | Scene Graph Traversal | Mesh primitives $\le 16$, triangles $\le 150,000$ | NO |

---

## 3. Dual-Contract Output Deliverable

Every conformance run yields two deterministic artifacts:

1. **`conformance-report.json`**: Machine-readable payload for automated CI gating, automated ERP ingestion, or e-commerce catalog publishing.
2. **`conformance-report.md`**: Human-readable audit report containing exact vertex coordinate breakdowns, failed island IDs, and actionable 3D artist remediation steps.
