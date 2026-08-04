# PULSE / ESCAPE — Architecture Proposal

> Root system components list, layer separation diagram, data flow boxes, and subsystem role tables.

---

## Architectural Layer Diagram

```
+---------------------------------------------------------------+
|                    APPLICATION LAYER                          |
|  +-------+   +--------+   +-----------+   +--------------+   |
|  | Game  |──▶│ HUD    │──▶│ Menus     │──▶│ UpgradeUI    |   |
|  | Class |   | Renderer|   | System   |   | Card System  |   |
|  +-------+   +--------+   +-----------+   +--------------+   |
+----------------------------------------------------------------+
                              ▼ Data flow (event snapshots)
+---------------------------------------------------------------+
|                      GAMEPLAY LAYER                           |
|  +----------+  +---------+  +------------+  +-------------+   |
|  | Player   |  | Camera  |  | Scoring    |  | Ghost       |   |
|  | State    |  | System  |  | Engine     |  | Manager     |   |
|  | Machine  |→▶│ (math)  |→▶│ + Combo    |→▶│ Recorder/   |   |
|  +----------+  +---------+  +------------+  │ Replayer    |   |
|              +---------+  +-------------+   +-------------+   |
|              |Adaptive │  | FlowMode   |←─┐                |   |
|              |Director |→▶│ Controller |──▶│ triggers      |   |
|              +---------+  +------------+  │ visual/audio   |   |
|              +-------------+  +---------+  └────────--------|   |
|              | Obstacle  |←▶│ Upgrades │                    |   |
|              | Spawner   |──▶│ Manager │                    |   |
|              +-------------+  +---------+                    |   |
+---------------------------------------------------------------+
                              ▼ generation (seed-driven, blind)
+---------------------------------------------------------------+
|                     PROCEDURAL LAYER                          |
|  +------------+  +-----------+  +----------+  +-----------+   |
|  | RoadGen    |  |BldgGen    |  | Traffic  |  | Weather/  |   |
|  | Catmull-Rom│→▶| Noise→    |→▶| Gen      |→▶| Events    |   |
|  | Splines    |  | extrude   |  | path-fol.|  | Manager   |   |
|  +------------+  +-----------+  +----------+  +-----------+   |
|              +-----------+     +-------------+               |
|              |AudioGen   |←──▶| FM Synth +  |               |
|              | Seeds→    |     | Oscillators |               |
|              +-----------+     +-------------+               |
+---------------------------------------------------------------+
                              ▼ GL + mesh data
+---------------------------------------------------------------+
|                      ENGINE LAYER                             |
|  +----------+  +---------+  +--------+  +--------+  +------+|
|  | Renderer |  |Physics  │  |Input   │  |Audio  │  | ECS  ||
|  │ Pipeline │←▶│ Collide │←▶│Manager │  │Device  │←▶|Rect. │||
|  │ (OpenGL) │──▶│ System │  │debounce│  +--------+  +------+|
|  +----------+  +---------+           +---------+            |
|                                                          +--+--+        |
|                                                          |Logging||
|                                                          +-------+        |
+---------------------------------------------------------------+
                              ▼ raylib + miniaudio
+---------------------------------------------------------------+
|                   PLATFORM / RUNTIME                          |
|  +-----------+   +-------------+   +---------------------+    |
|  │ raylib    │   │ OpenGL 3.3  │   │  miniaudio          |    |
|  │ windowing │──▶│ Core Profile│←→▶│ audio I/O           |    |
|  │ + input   │   │ (GLSL prog.)│   │ oscillator playback |    |
|  +-----------+   +-------------+   +---------------------+    |
+---------------------------------------------------------------+
```

---

## Subsystem Role Tables

### Engine Layer (bottom — foundation)

| Component | Parent Block | Role | Data Flow |
|-----------|-------------|------|-----------|
| **Renderer Pipeline** | Renderer | Orchestrates render passes: vertex → fragment → HDR → bloom → tone-map → fog → vignette → UI overlay | Receives mesh data from Gameplay/Procedural; pushes GL draw calls downward |
| **Physics Collide System** | Physics | AABB + sphere collision detection; kinematic-only integration (no rigid-body) | Reads entity transforms each frame; emits collision events upward to Gameplay |
| **Input Manager** | Input | Keyboard/mouse/gamepad abstraction with debounce layer | Polls raylib input; translates to game-actions; pushes discrete events up |
| **Audio Device** | Audio | miniaudio device init, output routing, context management | Receives synth voice requests from AudioGen; streams samples downward to OS audio |
| **ECS Registry** | ECS | Lightweight entity/component/system registration + 2048 entity pool | Central registry; Gameplay + Procedural query components via ECS interface |

### Gameplay Layer (middle — rules)

| Component | Parent Block | Role | Communication Pattern |
|-----------|-------------|------|-----------------------|
| **Player State Machine** | Player | Enum states (RUN/JUMP/SLIDE/DASH), velocity-integrated movement, invulnerability frames | Emits physics-collisions to Physics layer; receives events from Scoring/Upgrade via callbacks |
| **Camera System** | Camera | Math-only: smooth-follow interpolation, banking roll, FOV scaling, shake impulses (no rendering) | Reads player transform + speed every frame; pushes camera state to Renderer |
| **Scoring Engine** | Scoring | Distance scoring, near-miss tiers, combo formula `base × (1.5)^combo_level`, flow meter | Receives near-miss events from Physics; emits HUD-update snapshots to UI layer |
| **Ghost Manager** | Ghost | Per-frame recorder `(position, rotation, action_state)` in circular buffer; replay rendering hooks | Records during gameplay; compares distances against current player position via event queries |
| **Adaptive Director** | Director | Weighted probability tracking (jump/slide/near-miss frequency) → adjusts obstacle spawn weights | Pushes weight deltas to Procedural/ObstacleSpawner; reads player behavior via ECS component queries |
| **Flow Mode Controller** | FlowMode | Hidden threshold (~30 perfect actions); triggers visual + audio transformation signals | Receives near-miss success counts from Scoring; broadcasts FlowMode state change to Renderer + AudioGen |
| **Obstacle Spawner** | Obstacles | Types (barrier, vehicle, overhead, gap) spawned via director-weighted tables on road segments | Listens to Director weight updates; requests from Procedural/RoadGen for spawn positions |

### Procedural Layer (top — world content)

| Component | Parent Block | Role | Communication Pattern |
|-----------|-------------|------|-----------------------|
| **RoadGen** | Procedural | Catmull-Rom spline generation → straight/curve/intersection/bridge/tunnel segments → mesh extrusion | Publishes segment geometry data to Renderer via VBO pipeline; blind to gameplay (Director bridges) |
| **BuildingGen** | Procedural | Seed → noise footprint → random dimensions → extrude → window placement grid overlay | Deterministic from main seed; pushes building meshes upward to Renderer instancing system |
| **TrafficGen** | Procedural | Vehicle path-following on generated road splines with deterministic timing | Independent of gameplay; vehicles are obstacles from Physics layer's perspective |
| **Weather/System + Events** | Procedural | Rain particles, fog density, storm thresholds; bridge collapse, drone swarm events | Weather affects visibility/physics parameters; Director triggers event selection based on performance threshold |
| **AudioGen** | Procedural | BPM/chord/melody seeds → FM synthesis parameters; oscillator SFX (footsteps, wind, near-miss) | Listens to speed + combo state from Gameplay via callback hooks; feeds voice requests to Audio Device |

### Application Layer (top — entry points)

| Component | Parent Block | Role |
|-----------|-------------|------|
| **Game Class** | Application | Main loop orchestrator: init → update → render → shutdown; subsystem wiring at startup |
| **HUD Renderer** | UI | Score, combo, distance, flow meter, energy bar display with live-smoothing updates |
| **Menu System** | UI | Title/seed input → pause overlay → game-over summary + ghost comparison + instant restart |
| **Upgrade Card System** | UI | 2–4 card selection every ~60s; double-jump/mag-dash/shield/wall-run option generation and application |
