# PULSE / ESCAPE — Epic Breakdown (E1–E14)

> Path in `src/` hierarchy, phase scope, one-line capability summary.

| # | Epic Name | Module Tree (src/) | Phase | Capabilities |
|---|-----------|--------------------|-------|-------------|
| **E1** | PlayerController | `gameplay/player/` + `gameplay/state/` | P0–P2 | run/jump/slide/dash/wall-run/air-control state machine; enum states (RUN→JUMP→SLIDE→DASH); inv. frames; momentum-based delta-time movement; landing recovery stun |
| **E2** | CameraSystem | `gameplay/camera/` + `engine/renderer/` | P2 | smooth-follow lerp interpolation; banking roll ±8° on lateral vel; FOV 60°→75° by speed; look-ahead proportional to speed; shake: `A·e^(-d·t)` |
| **E3** | CollisionPhysics | `engine/physics/` | P1–P2 | AABB/sphere kinematic-only; hitboxes 20–30% smaller than visuals (fair deaths); near-miss: `dist < radius + margin`; collect sphere-sphere; death → game-over trigger |
| **E4** | ScoringCombo | `gameplay/scoring/` | P2 | distance = meters × base_mult; near-miss tiers INSANE(10cm)/GREAT(30cm)/OK(60cm); combo = `base × (1.5)^n`; reset on death or safe gap >2s; flow meter invisible threshold ~30 actions |
| **E5** | ProceduralRoadSystem | `procedural/roadgen/` + `procedural/meshgen/` | P2–P3 | Catmull-Rom splines (straight/curve/intersection/bridge/tunnel); seed-deterministic footprint→extrude→windows; instanced VBO/VAO/IBO rendering |
| **E6** | ObstacleSpawner | `gameplay/obstacles/` | P2 | barrier/vehicle/overhead/gap/combination types; weighted probability from Director via callback; Sphere-AABB per type; ≥1.2× hitbox→visual for fair deaths |
| **E7** | GhostRunner | `gameplay/ghost/` | P3 | per-frame `(pos/rot/action)` in fixed circular buffer; binary log <2KB/save; 5-best ghost overlay colored by run quality; fixed-point transforms for cross-platform determinism |
| **E8** | AdaptiveDirector | `gameplay/director/` | P3 | tracks jump/slide/near-miss freq + lane pref + perf score via ECS queries; delta-tracking adjusts spawn weights each segment; new events at thresholds, no ML |
| **E9** | HUDUpgradeUI | `ui/hud/` + `ui/upgrades/` + `ui/menus/` | P2–P3–P5 | live HUD (score/combo/dist/flow/energy) with smoothing; upgrade cards 2–4 every ~60s (dble-jump/mag-dash/shield/wall-run); title/pause/game-over menus with ghost comparison |
| **E10** | ProceduralAudioEngine | `engine/audio/` + `procedural/audiogen/` | P3–P4 | FM synth: BPM/chord/bassline/melody seeds; oscillator SFX (footsteps/wind/near-miss/UI); max 8 voices; flow mode adds harmony layers + tempo boost; replaces all stored audio |
| **E11** | WeatherDynamicEvents | `procedural/weather/` + `procedural/events/` | P3–P4 | rain (GPU particles), fog/storm shader density, storm gameplay effects via ECS; events: bridge collapse/drone swarm/road split/power outage triggered by Director weights |
| **E12** | VisualJuiceSystem | `engine/render/particles/` + `engine/render/effects/` | P5 | particle bursts (collect/near-miss/death/speed); shake+glow+tilt curves per event; UI ease-in-out ~200ms; all exponential decay |
| **E13** | RenderingPipelineShaders | `shaders/` → `engine/renderer/` | P2–P6 | procedural mesh→VBO/VAO/IBO instanced→vertex→fragment→HDR→bloom→tonemap→fog→vignette→UI; 9 techniques (sky grad/gradients/instancing/bloom/tonemap/fog/rim/shadows/SDF); ≤300 instr/pass |
| **E14** | ProjectFoundationBuildSystem | root CMake + `third_party/` + CI config | P0 | root + module CMakeLists; raylib/miniaudio vendored in third_party; `main`/`develop`/`feature/<mod>-<task>`; CI: compile→clang-tidy→clang-format→tests→binary-size gate |
