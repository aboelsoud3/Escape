# ARCHITECTURE STATUS

> Real architecture reality check (not just intended design). Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

## Component Status

| Component | Status | Notes |
|-----------|--------|-------|
| Engine Core | ⬜ Not Started | CMake targets + raylib integration planned |
| Input System | ⬜ Not Started | Keyboard/gamepad polling defined in specs, not yet coded |
| Player Controller | ⬜ Not Started | Run/jump/slide/dash states designed, implementation pending Sprint 1 |
| Camera System | ⬜ Not Started | Smooth-follow + FOV scaling specified, not built |
| Physics (AABB/Sphere) | ⬜ Not Started | Custom kinematic collision defined in ADR-005 |
| Procedural Road Gen | ⬜ Not Started | Catmull-Rom specification ready |
| Obstacle Spawner | ⬜ Not Started | Barrier/overhead/combo types designed |
| Ghost Runner | ⬜ Not Started | Circular buffer recorder + replay spec ready |
| Adaptive Director | ⬜ Not Started | Weighted probability tracking designed |
| Audio (FM Synth) | ⬜ Not Started | miniaudio single-header selection done, engine not built |
| Scoring System | ⬜ Not Started | Distance/combo/near-miss tiers defined |
| Rendering Pipeline | ⬜ Not Started | Shader pipeline specified but not implemented |

## Architecture Layers (KB §5)

```
Rendering Layer (GLSL shaders + framebuffer → post-processing)
    ↑
Gameplay Layer (player controller, obstacles, scoring, director)
    ↑
Engine Layer (input, physics, audio, procedural generators)
    ↑
Foundation (CMake, raylib, logging, profiler, CI) — Sprint 0 only
```

## Known Architectural Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| Coupling between rendering and gameplay layers | Medium | Strict interface boundaries per Doc 10 dependency graph |
| Binary size creep across layers | Critical | SIZE_BUDGET.md tracking from Sprint 0; CI gate in P6+ |
