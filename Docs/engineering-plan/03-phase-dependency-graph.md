# PULSE / ESCAPE — Phase Dependency Graph

> Dependencies map showing critical path and cross-cutting subpaths.

---

## Top-Level Critical Path

```
P0 ─→ P1 ─→ P2 ──┬──→ P3 ──┬──→ P4 ──┬──→ P5 ──┬──→ P6
                    │          │          │          │
                  └──────────┘          ↓        (optional)
                                  parallel features   contest entry
```

- **Critical Path:** P0 → P1 → P2 → P3 → P4 → P5 → P6 (all phases in sequence)
- P6 is strictly after P5; never overlap optimization with feature work.

---

## Phase Dependency Breakdown

| Parent | Dependent | Can Start When | Depends On From Parent | Notes |
|--------|-----------|----------------|------------------------|-------|
| **P0** → | P1 | Project compiles, renderer draws a cube | Working CMake target `engine + gameplay` | MFP impossible without foundation |
| **P1** → | P2 | Kill Test Gate passed (movement is fun) | PlayerController built/tested | Camera/physics require player archetype to follow/collide |
| **P2** → | P3 | All core subsystems functional, no crash | Engine + Gameplay stable integration | Vertical slice needs every system talking correctly |
| **P3** → | P4 | Full pipeline end-to-end working | RoadGen, GhostRunner, Director, Audio all integrated | MVP adds depth/content to proven foundation |
| **P4** → | P5 | All features exist, game is playable | No further gameplay changes in P5 scope | Polish requires everything present to juice |
| **P5** → | P6 | Gold candidate ready for compression | Binary complete feature set | Phase 6 only compresses/optimizes; no new features |

---

## Cross-Cutting Subpaths (parallel to critical path)

```
[Procedural Audio]  ─── embedded in P2→P3→P4→P5 (continual refinement)
[HUD/UI]            ─── starts P2, integrated P3, polished P5
[Weather/Events]    ─── begins P3, extended P4
[Shaders/VFX]       ─── builds P2, matures P3, intensifies P5
[Ghost Runner]     ─── built P3, regression-tested P3→P6
[Test Automation]   ─── seed regression runs every commit from P0 onward
                    CI: size tracking starts P0, enforced throughout all phases
```

- **Procedural Audio** and **Shaders/VFX** are cross-cutting because their work flows across multiple phases rather than belonging to a single phase.
- All phases share the requirement: deterministic seed generation + binary-size awareness per KB §9.
