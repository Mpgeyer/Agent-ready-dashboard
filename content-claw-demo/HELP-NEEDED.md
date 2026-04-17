# Content Claw 3D Viewer — Help Needed: Vertex Color Zone Classification

## What We're Building

A self-contained HTML demo for the **Content Claw pipeline** — an interactive Three.js before/after viewer that shows a TO-220 voltage regulator STL model with VLM-classified PBR materials applied.

- **Live demo:** https://mpgeyer.github.io/Agent-ready-dashboard/content-claw-demo.html
- **Source:** [`Mpgeyer/Agent-ready-dashboard/content-claw-demo.html`](https://github.com/Mpgeyer/Agent-ready-dashboard/blob/main/content-claw-demo.html)
- **Stack:** Three.js r128 (CDN), STLLoader, OrbitControls — single HTML file with base64-embedded STL

## What Works ✅

- Split-screen viewer (left = raw gray, right = material-applied)
- Synced orbit controls, auto-rotate, wireframe toggle
- NVIDIA-branded dark UI with pipeline stats bar + material cards
- GitHub Pages auto-deploy (push → 30s → live)

## The Problem 🔴

**We cannot get vertex colors to display correctly on the "after" panel.** Despite 12 iterations (v1–v12), the model keeps appearing as uniform gray or with incorrect zone boundaries.

### Approach: Vertex Colors on Single Mesh

After multi-mesh approaches (v1–v5) all showed identical gray on both panels (likely z-fighting or scene-assignment bugs), we switched to **vertex colors** in v6:

```javascript
// Paint per-face colors into a color attribute
const colors = new Float32Array(vertexCount * 3);
for (let f = 0; f < faceCount; f++) {
    // classify face → assign RGB to all 3 vertices
    const idx = (f*3+v)*3;
    colors[idx]=r; colors[idx+1]=g; colors[idx+2]=b;
}
geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));
const material = new THREE.MeshStandardMaterial({ vertexColors: true, metalness: 0.0, roughness: 0.8 });
```

This partially worked (v6 first showed visible zones!), but zone **boundaries** have been wrong in every version since.

### STL Geometry (from binary analysis)

| Axis | Min | Max | Range | Semantic |
|------|-----|-----|-------|----------|
| X | -32.41 | 116.16 | 148.57 | LONG (package width) |
| Y | 0.00 | 87.37 | 87.37 | TALL (package height) |
| Z | -1.35 | 29.48 | 30.83 | THIN (package depth) |

- 4,140 triangles total
- Model is "lying on its back": flat heat sink tab at Z ≈ 0, body on top (Z ≥ 2), leads extend in -X direction
- 70% of faces (2,911) at Z < 1.7 = heat sink tab
- 3 lead pins at X < -20, each ~1mm thick

### Zone Classification (centered coordinates, post `geometry.center()`)

```
Center: (41.875, 43.685, 14.069)

Zone rules:
- Leads:     centroid X < -56.875  (raw X < -15)     → 282 faces
- Body:      centroid Z > -12.069  (raw Z ≥ 2)       → 872 faces  
- Heat sink: everything else                          → 2,986 faces
```

### Target Materials (from `predictions.jsonl` — VLM pipeline output)

| Zone | Material | base_color | metallic | roughness |
|------|----------|-----------|----------|-----------|
| Heat Sink | Aluminum | (0.8, 0.8, 0.8) | 0.9 | 0.2 |
| Body | Polycarbonate | (0.2, 0.2, 0.2) | 0.0 | 0.5 |
| Leads | Copper | (0.9, 0.7, 0.2) | 0.95 | 0.1 |

## What We've Tried (v1–v12)

| Version | Approach | Result |
|---------|----------|--------|
| v1–v5 | Multi-mesh (3 separate meshes per zone) | All gray — z-fighting or wrong scene |
| v6 | Single mesh + vertex colors, height-% zones | ✅ First visible zones! But boundaries wrong |
| v7 | + face normal analysis | Zones misaligned |
| v8 | Data-driven thresholds from raw STL binary parse | 3 zones visible but body too small |
| v9 | Lowered body Z threshold (Z≥4 → Z≥2) | Better proportions |
| v10 | Applied predictions.jsonl PBR values | "SUPER bad" — regression? |
| v11 | Axis-agnostic (auto-detect LONG/TALL/THIN) | Still wrong |
| v12 | Diagnostic RGB (R/G/B) + on-screen debug overlay | Awaiting screenshot |

## Specific Questions

1. **Is `geometry.center()` reliably centering the STL geometry?** The thresholds assume centering by `(min+max)/2` on each axis. If STLLoader does something different, all thresholds would be wrong.

2. **Does Three.js STLLoader (r128) preserve raw STL coordinate axes?** We assume X/Y/Z in the parsed `BufferGeometry` matches the raw binary STL. Is that guaranteed?

3. **Is there a simpler way to verify vertex colors work?** Like painting the entire mesh red to confirm the material pipeline works, then adding zone logic.

4. **Could the vertex color attribute be getting overwritten?** We clone the geometry after `geometry.center()`, then add the color attribute. Is there a race condition or caching issue?

5. **Is `MeshStandardMaterial({ vertexColors: true })` the right approach for Three.js r128?** Should we use `THREE.VertexColors` instead? API changed between versions.

## Context

- Part of the **AI Hackathon April 2026** — Content Claw pipeline demo
- Content Claw is Doyub Kim's 9-tool agentic pipeline: CAD→USD conversion, VLM material classification, physics, render, validation
- The viewer is a simplified visualization of the material classification step
- Source data (`predictions.jsonl`) comes from the real VLM pipeline output

## Repo Structure

```
content-claw-demo.html          # Self-contained viewer (288KB, base64 STL embedded)
├── HTML/CSS header              # Lines 1-95
├── STL_BASE64 = "..."           # Line 96 (~276KB base64 blob)
└── JavaScript (Three.js)        # Lines 97+ (loadSTL, animate, controls)
```

## How to Help

- Clone or view the live demo at the URL above
- Open browser DevTools — v12 has console.log debug output and an on-screen debug overlay
- If you can identify why vertex colors aren't mapping correctly to the geometry, that's the fix we need

---

*Generated 2026-04-16 by MG2 (OpenClaw agent) — contact Mike Geyer (@mikegeyer62)*
