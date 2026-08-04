# PERFORMANCE

> Performance tracking for low-end mobile devices. Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

## Target Devices (Doc §3)

| Device class | Minimum memory | Performance target |
|--------------|----------------|--------------------|
| Low-end Android | 2 GB RAM | ≥ 500mW power draw, < 7 MB binary per playground spec |
| Low-end WebGL | Browser budgeting | Lightweight shaders only |
| Windows/Linux | Per user system | Frame pacing consistent with core requirements |

## Current Performance State (Pre-computation)

No performance measurements exist yet — profiling infrastructure not built. The following are planned targets:

| Metric | Target | Measured | Profiler Status |
|--------|--------|----------|-----------------|
| Input latency (touch) | ≤ 16ms at 60fps | Not measured | Profiler scaffolded, not run |
| Frame budget (render) | 8.3 ms @ 60fps / 5.56 ms @ 90fps | Not measured | Profiler scaffolded, not run |
| Audio latency | < 20 ms buffer | Not measured | miniaudio selected; engine not built |
| CPU frame budget | ≤ 4 ms @ 1 FPS for procedural gen + physics | Not measured | Profiler scaffolded, not run |

## Known Performance Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| Dynamic allocation in game loop → GC on Android | Critical | Stack-allocated math types per Doc 9 §4.4; zero heap during gameplay |
| Fragment shaders (not vertex) bottleneck on mobile GPUs | High | RenderPipeline defined in ADR-003/004 for shared rendering codepath |
| Memory allocation stuttering | High | Pre-allocate ghost data via 512-entry circular buffer + fixed-size pools |
