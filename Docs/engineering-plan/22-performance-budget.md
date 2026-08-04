## Doc 22 — Performance Budget

### Purpose

Define frame-time budgets for every subsystem so the game maintains stable 60 FPS target.
Every developer must understand their module's share of the 16.67ms budget.

---

### Overall Frame Budget (Target: <=16.67 ms at 60 FPS)

| Subsystem | Budget | Target % | Notes |
|-----------|--------|----------|-------|
| Physics & Input | <=2.0 ms | ~12% | Player movement, collision detection, ECS systems |
| Player Logic | <=1.5 ms | ~9% | State transitions, input processing, scoring |
| Procedural Generation | <=4.0 ms | ~24% | Road generation, building meshes, traffic spawn |
| Rendering Prep (CPU) | <=3.0 ms | ~18% | Frustum culling, matrix math, instancing setup |
| GPU Rendering | <=3.5 ms | ~21% | Draw calls, shader execution (measured via fence) |
| Audio Processing | <=0.5 ms | ~3% | Procedural synthesis, spatial audio updates |
| Input/Window | <=0.3 ms | ~2% | Polling inputs, event handling |
| Game Manager Updates | <=1.5 ms | ~9% | Adaptive Director, upgrades, flow mode machine |
| Buffer Flips & Swap | <=0.37 ms | ~2% | glSwapBuffers, hardware-bound, uncontrollable |

**Total: 16.67ms max (theoretical). Target of <=8ms during gameplay to allow peak bursts.**

---

### Content Generation Budget Per Frame

Procedural generation must be frame-rate independent from content delivery:

| Item | Per-Frame Constraint |
|------|---------------------|
| Road segments generated | <=3 new segments; previous ones kept in GPU ring buffer |
| Building instances | <=15 via instanced draw calls; merge into VBO batches |
| Obstacle spawning | <=2/frame max; batch near-miss detection within 8m radius |
| Traffic vehicle updates | Precomputed interpolation; no per-frame allocation |

---

### Per-Subtarget Budgets

Some features have different budgets depending on scene complexity:

| Subsystem | MFP (P1) | Core (P2) | Full City (P3+) |
|-----------|----------|-----------|------------------|
| Physics & Input | 0.5 ms | 2.0 ms | 2.0 ms |
| Procedural Gen | 0 ms | 1.0 ms | 4.0 ms |
| Rendering Prep | 0.3 ms | 1.5 ms | 3.0 ms |
| GPU Rendering | 0.5 ms | 1.5 ms | 3.5 ms |
| Audio | 0 ms | 0.1 ms | 0.5 ms |
| Game Manager | 0 ms | 0.3 ms | 1.5 ms |
| Total budget | <=2.3 ms | <=6.4 ms | <=16.67 ms |
