# PULSE / ESCAPE — Module Dependency Graph & Rules

> Module dependency rules, constraints, and interfacing protocols organized in a single reference table.

---

## Root Dependency Constraint Matrix

| Module | Can depend on | Cannot depend on | Constraint | Interfacing Protocol |
|--------|--------------|-------------------|------------|----------------------|
| **Engine** (Renderer, Physics, Input, Audio, ECS, Core) | *(nothing — leaf layer)* | Gameplay, Procedural, UI | No upward dependencies allowed. Engine must remain a pure foundation library. | Module-level export headers only; gameplay/code cannot include engine internal implementation details |
| **Gameplay** (Player, Camera, Scoring, Ghost, Director, FlowMode, Obstacles, Upgrades) | Engine, ECS Component Interface | Renderer internals, Audio device, UI layers | Gameplay owns game rules but never calls render/audio directly. Communication is through callback/eventbus patterns only. | Event bus / callback interface: gameplay emits events (NearMiss, Collision, UpgradeApplied); renderer/audio listen via subscription. No direct object access. Shared data via ECS component queries. |
| **Procedural** (RoadGen, BuildingGen, TrafficGen, Weather, Events, AudioGen) | Engine, Math from raylib | Gameplay (generation is blind to gameplay; Director bridges them) | Procedural generation has zero knowledge of scoring/combo/UI state. Output is pure geometry + audio data streams. | Pipeline publish: generates → pushes mesh/audio data upward via callback interfaces to Renderer/Audio Device. RoadGen publishes segment lists as raw data structs only. |
| **UI** (HUD, Menus, Upgrades UI) | Engine (Renderer for display only), ECS snapshot queries | Gameplay rules, Procedural generators | UI may only read ECS component snapshots; never store or modify gameplay-proper state directly. | Snapshot polling: UI reads scored values from Gameplay-scoring entity components each frame at fixed polling interval. Event-driven updates via callback interface when score data changes. Always one-way — never push mutable control back to gameplay. |

---

## Sub-Module Dependency Details

### Engine Layer Dependencies

| Module (Engine) | Depends On | Constraints |
|-----------------|-----------|-------------|
| **Renderer Pipeline** | Physics collision output (bounding boxes for scene culling), Input toggle events (pause/resume triggers) | Receives mesh data from Gameplay/Procedural via push buffers; never queries upstream. Draw calls ordered: procedural→gameplay→particles→UI overlay |
| **Physics Collide System** | Entity transforms from ECS; input state from Input Manager for player position verification | Emits collision events via callback to Gameplay Scoring and Director (near-miss detection); no memory leak: collision reports are scoped lifetime per-frame only |
| **Audio Device** | AudioGen synth parameters; Input UI clicks; FlowMode state change triggers | Voice pool max 8 concurrent oscillators; streams downward to miniaudio/OS; does not store audio buffers exceeding 512 KB total (KB §11 memory budget) |
| **Input Manager** | raylib polling interface | Debounce filter applied (min 50ms between duplicate inputs); translated game-action enums pushed via callback events; maps raw hardware → unified action layer |
| **ECS Registry** | *(nothing)* | Fixed at max 2048 simultaneous entities; assert-panic on exceed in debug builds; queries return const references only |

### Gameplay Layer Dependencies

| Module (Gameplay) | Depends On | Constraints |
|-------------------|-----------|-------------|
| **Player State Machine** | Input actions, Physics collision results from ECS transforms | Movement is velocity-integrated with deterministic delta-time; all motion is kinematic — never dynamic. Emit camera-state query requests per frame via ECS write-only component |
| **Camera System** | Player transform + speed from ECS (read-only) | Pure math: computes target position, banking angle, FOV value, shake impulse vector. Emits result as a CameraState struct to renderer via callback. Zero rendering calls — math only |
| **Scoring Engine** | Near-miss proximity events from Physics layer, Distance run-counter | Formulas integer-based where possible (distance × multiplier + near-miss tiers). Combo: `(1.5)^combo_level` — precompute powers in lookup table per frame to avoid float non-determinism. Emits HUD data snapshot via callback to UI polling |
| **Ghost Manager** | Player position, rotation, action-state from each frame (via ECS writes) | Circular buffer capped at fixed size; overflow discards oldest entry. Save binary log on death (<2 KB). Maximum 5 ghosts visible simultaneously per run. All stored transforms use fixed-point format for cross-platform determinism |
| **Adaptive Director** | Player behavior counters from ECS component queries (jump/slide/near-miss frequency) | Delta-tracking runs every segment boundary; weight tables updated proportionally. No ML or pathfinding — pure weighted-probability adjustments as per KB §7.6. Emits new weight table to ObstacleSpawner via interface callback |
| **FlowMode Controller** | Near-miss success counts from Scoring Engine (via ECS component) | Hidden threshold = ~30 perfect consecutive actions (configurable). On activation: broadcasts FlowMode ON event to Renderer (visual shift), AudioGen (tempo + harmony injection), and Camera (FOV +5° widening). On deactivation via safe platform or death → reversible |
| **Obstacle Spawner** | Road segment data from Procedural/RoadGen; Director weight tables from Adaptive Director | Spawns at determined road positions each segment; applies weighted probability → obstacle type selection. Emit collision-primitive AABB structs to Physics layer after spawn. Does not know game state (score, combo) directly |
| **Upgrades Manager** | Player health/survival time counters from ECS; Game class enable/disable signal | Triggers card generation every ~60s of survival time. Applies selected upgrade by modifying ECS components on player entity (e.g., add component `EnableDoubleJump = true`). Always two or four card options per selection cycle. Never saves state across runs — ephemeral to current game session only |

### Procedural Layer Dependencies

| Module (Procedural) | Depends On | Constraints |
|---------------------|-----------|-------------|
| **RoadGen** | Main seed (passed from player input or random generator); segment type selection via deterministic sub-seed from main | Catmull-Rom spline interpolation for all road geometry; generates straight/curve/intersection/bridge/tunnel shapes. Outputs mesh data only (positions, normals, UVs) — game semantics (obstacle positions) are opaque to RoadGen. Always deterministic: same seed = identical segment layout forever |
| **BuildingGen** | Per-building sub-seeds derived from main seed; building footprint noise parameters | Noise → footprint shape → random dimensions → extrude height → window grid overlay via procedural placement. Window emissive (lit/unlit) determined by seeded probability values independent of gameplay time-of-day. All windows procedurally textured — no image files used at all |
| **TrafficGen** | Road spline data from RoadGen; deterministic vehicle spawn schedule per sub-seed | Vehicles follow road spline path with speed proportional to current player speed (not absolute). Path-follow uses spline interpolation — vehicles remain locked to road surfaces. No collision between vehicles themselves (no A* or pathfinding needed) |
| **Weather System** | Director difficulty score + time-of-day cycle trigger from Game class; seed-derived weather type selection | Rain: GPU particles (max particle count depends on frame-time budget of 16.67 ms target). Fog: exponential height fog density parameter pushed to shader uniforms. Storms apply gameplay-effect via ECS component write-signal (surface friction modifier). All visual effects via shader parameters only — no stored texture atlases |
| **Events Manager** | Director trigger signals; road geometry data from RoadGen for placement context | Bridge collapse, drone swarm, road split, power failure — each event modifies road geometry temporarily or permanently. Events are scheduled by Director weights and activated at distance thresholds from player position. Event effects via ECS component writes to relevant systems (visual shift for power failure: darken all emissives) |
| **AudioGen** | Player speed + combo level + FlowMode state from Game class/callback hooks; main seed-derived music parameters | FM synthesis generates BPM/chord scale/bassline/melody deterministically. Max concurrent voices = 8 — enforce globally via audio pool allocator. Footstep SFX generated per foot-ground contact event (distance-based volume attenuation). Near-miss whooshes: oscillator pitch sweep triggered by near-miss events from Physics directly to AudioGen |

### UI Layer Dependencies

| Module (UI) | Depends On | Constraints |
|-------------|-----------|-------------|
| **HUD Renderer** | ECS scoring component snapshots read at fixed polling interval (60 Hz max for display updates) | Smooth interpolation between score values to avoid jitter on HUD numbers. Zero gameplay mutation — HUD is strictly observational display system only. All elements ease-in-out over ~200ms transitions. Combo level displayed as large floating number with pop/scale animation per increment event |
| **Menu System** | Game class (game state toggle: pause/resume/quit/restart trigger signals) | Main menu → title + seed input + start; Pause → resume/restart/quit buttons; Game-over → score summary + top-5 ghost comparison + instant-restart link. Transition animations between menus always eased-in-out (no screen-pop). Seed stored in config, not code — editable without recompile |
| **Upgrade Card System** | Upgrade Manager via ECS component queries + selection result callbacks from player input | 2–4 upgrade cards displayed simultaneously; options shuffled deterministically per seed each cycle. Player selects one via click/keyboard/gamepad → callback to Upgrade Manager to apply modification. Visual feedback on hover/selection with animation > no instant pop-in on card reveal |

---

## Compile-Time Enforcement

| Mechanism | Implementation |
|-----------|---------------|
| **CMake target link libraries** | `engine` is a library target; `gameplay`, `procedural`, `ui` each declare `target_link_libraries(gameplay engine)` enforcing compile-time dependency ordering. A circular dependency causes CMake configure error (early violation detection). Gameplay, Procedural, UI targets cannot link to each other — linker will fail if upstream include headers from sibling modules are referenced |
| **Include-guard pattern** | Engine headers use `#pragma once`; all gameplay-internal struct visibility uses forward-declaration pimpl pattern. Upstream modules compile only stable API signatures — internal implementation details hidden behind opaque pointers |
| **Zero-runtime reflection dependencies** | No RTTI, no `<typeinfo>` in gameplay/procedural code — explicit cast-to-Knowing-interface at registration time via ECS factory functions. Precludes accidental cross-module dependencies that sneak in through `dynamic_cast` or type-based exploration |

---

## Interfacing Protocol Reference

| Pattern | Used Between | Description |
|---------|-------------|-------------|
| **Callback interface (functional callback / std::function)** | Gameplay ↔ Renderer/Audio for state updates; Director ↔ Spawner for weights | Decoupled one-way notification — sender does not need to know receiver implementation details. Lambda capture used only within registering scope (no dangling pointer risk) |
| **Event bus / publish-subscribe** | Physics → Scoring (near-miss events), Scoring → HUD (score update requests) | Broadcaster pattern: event struct emitted by source; all subscribers receive copy per-frame via std::vector of callbacks or ECS system queries. Scope-bound lifetime guarantees no stale event references |
| **ECS component query** | Gameplay modules read player state; UI reads scoring components; Director reads behavior counters | Component data is flat memory layout (SoA: Structure-of-Arrays for cache-friendly iteration). Query returns const_iterator — write-only through explicitly-named write APIs. Data ownership stays with producing module |
| **Snapshot polling** | HUD Renderer, Menu System observe ECS snapshots at fixed intervals | Non-functional-push; UI side decides how frequently to poll (never faster than 60 Hz). Polling interval configurable per UI module needs. No events fired — pure data snapshot read pattern |
