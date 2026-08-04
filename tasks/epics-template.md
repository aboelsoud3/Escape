# Epics Template

> Each Epic represents a major feature or subsystem (E1–E14 per Doc 5 epic breakdown). Epics roll up multiple issues. Sprint scope is always a subset of one or more Epics.

## Epic Format

```markdown
# E: [Epic Name]

| Phase | Sprint | Status |
|-------|--------|--------|
| P0–P6 | S0–S6 | Not Started / In Progress / Complete |

## Description
[One sentence capability summary.]

## Scope Boundaries
[What is IN scope vs. OUT of scope for this epic. Always clear boundaries, no creep.]

## Epic Tasks (auto-generated from linked issues above in issues template).
| Issue ID | Task Description | Sprint | Status | Size Est. |
|----------|------------------|--------|--------|-----------|

Last Updated: YYYY-MM-DD
```

## Epic Summary Reference
[All E1–E14 Epics listed per Doc 5 epic breakdown table with status tracked.]

| # | Epic Name | Phase | Sprint Scope | Size Est. | Status | Notes |
|---|-----------|-------|--------------|-----------|--------|-------|
| E1 | PlayerController | P1–P2 | S1/S2 | ~20 KB | To Do | Run/jump/slide/dash/wall-run/air-control |
| E2 | CameraSystem | P2 | S2 | ~5 KB | To Do | Smooth-follow, banking, FOV scaling |
| E3 | CollisionPhysics | P1–P2 | S1/S2 | ~8 KB | To Do | AABB/sphere kinematic-only collision |
| E4 | ScoringCombo | P2 | S2 | ~5 KB | To Do | Distance + near-miss/combo multiplier system |
| E5 | ProceduralRoadSystem | P2–P3 | S2/S3 | ~15 KB | To Do | Catmull-Rom splines, instanced mesh rendering |
| E6 | ObstacleSpawner | P2 | S2 | ~8 KB | To Do | Barrier/vehicle/overhead/gap generation + spawn weights |
| E7 | GhostRunner | P3 | S3 | ~10 KB | To Do | Per-frame recorder/replayer circular buffer system |
| E8 | AdaptiveDirector | P3 | S3/S4 | ~5 KB | To Do | Weighted probability tracking for spawn adjustments |
| E9 | HUDUpgradeUI | P2–P3 | S2/S3 | ~10 KB | To Do | Live HUD + upgrade card selection menus |
| E10 | ProceduralAudioEngine | P3-P4 | S3/S4 | ~12 KB | To Do | FM synth engine, oscillator-based SFX per Doc 7 spec |
| E11 | WeatherDynamicEvents | P3–P4 | S3/S4 | ~8 KB | To Do | Rain fog storm events via Director weights |
| E12 | VisualJuiceSystem | P5 | S5 | ~10 KB | To Do | Particles, screen shake, glow curves all per Epic 12 spec |
| E13 | RenderingPipelineShaders | P2–P6 | S2/S5/S6 | ~20 KB | To Do | Full GLSL pipeline: vertex→fragment→HDR→bloom tonemap fog vignette |
| E14 | ProjectFoundationBuildSystem | P0 | S0 | ~5 KB | To Do | Repository skeleton + CMake + CI + logging + profiler |
