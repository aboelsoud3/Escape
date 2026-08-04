## Doc 24 — Rendering Budget

### Purpose

Define GPU rendering budgets to maintain stable 60 FPS target.
Every shader pass and draw call has an allocated budget share.

---

### Draw Call Budget Per Frame

| Render Batch | Max Calls | Technique |
|-------------|-----------|----------|
| Ground/road mesh | <=5 | Instanced for repeated segments |
| Buildings (lod0)  | <=150 | GPU instancing, merged VBOs |
| Buildings (lod1)  | <=300 | Simplified geometry; no windows |
| Traffic vehicles   | <=30 | Box meshes, single shader pass |
| Trees/environment  | <=50 | Simple cone/frustum shapes |
| Collectibles       | <=20 | Single mesh, animated via shader |
| UI overlay         | <=10 | Quad-based screen-space rendering |
| Particle effects   | <=4 | GPU-compute or point sprites |

Total soft cap: 80 draw calls per frame target.

---

### Shader Instruction Budgets

Each GLSL shader pass has instruction count limits:

| Shader Pass | Max Instructions | Notes |
|-----------|-----------------|-------|
| Main vertex | <=150 | Model, view, projection transforms |
| Main fragment | <=250 | Gradient materials only |
| Bloom blur (x2) | <=80/pass | 2-pass Gaussian blur |
| Tone mapping | <=60 | Reinhard/filmic LUT-free |
| Fog/vignette | <=100 | Screen-space only |
| Player particle trail | <=120 | Simple sprite shader |

Shader merging strategy: share common macros for
similar material types (buildings/windows/traffic).

---

### Memory Bandwidth Budget

Avoid textures entirely. Use procedural gradients in
shaders instead. This eliminates texture memory bandwidth
pressure and texture atlas upload overhead per frame.

---

### Level-of-Distance Strategy

- lod0 (within 20m): Full detail, window geometry, materials
- lod1 (within 50m): Simplified buildings, no windows
- lod2 (beyond 50m): Colored boxes only, wireframe optional
- Cull beyond camera frustum completely before upload
- Use depth-first rendering: draw back-to-front for fog
