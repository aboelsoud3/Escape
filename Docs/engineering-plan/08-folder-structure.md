# PULSE / ESCAPE — Folder Structure

> Repository tree matching architecture described in KB §5 and throughout all engineering docs.

---

```
pulse/
│
├── Docs/                          # All design + engineering documentation
│   ├── KB.md                      # Knowledge base (single source of truth)
│   ├── 0.agentic-coding-tips.md   # AI workflow / prompt hierarchy
│   ├── 1.init-prompt.md           # Base initialization prompt
│   ├── context/                   # Source material for KB
│   │   ├── 1.guidelines.md        # 1.44 MB tech guidelines
│   │   ├── 2.points-to-follow.md  # Core requirements checklist
│   │   ├── 3.initial-estimation.md# Timeline estimations
│   │   ├── 4.tips-small-but-good.md# Demoscene / visual tips
│   │   ├── 5.engagement-factors.md# "One More Run" psychology
│   │   ├── 6.game-play_tips.md    # Compounding rules, risk/reward
│   │   ├── 7.game-concept.md      # Game concept + twist + core loop
│   │   ├── 8.initial-plan.md      # Phase roadmap + architecture
│   │   └── 9.MFP.md               # MFP roadmap + sprint backlog
│   └── engineering-plan/          # Generated planning docs (this directory)
│       ├── 01-project-roadmap.md
│       ├── 02-engineering-workflow.md
│       ├── 03-phase-dependency-graph.md
│       ├── 04-milestone-plan.md
│       ├── 05-epic-breakdown.md
│       ├── 06-issue-breakdown.md
│       ├── 07-sprint-proposal.md
│       ├── 08-folder-structure.md
│       ├── 09-architecture-proposal.md
│       └── 10-module-dependencygraph.md
│
├── src/                           # All source code
│   ├── engine/                    # Engine layer — core systems
│   │   ├── renderer/              # Rendering pipeline, shader loaders, VBO/VAO management
│   │   │   ├── pipeline.cpp/.h    # Pipeline orchestrator (HDR → bloom → tonemap → fog → vignette)
│   │   │   ├── shader_loader.cpp/.h  # Shader compilation + linking + hot-reload
│   │   │   ├── mesh_instancer.cpp/.h # Instanced geometry builder
│   │   │   └── post_process.cpp/.h     # Bloom, tonemapping, fog passes
│   │   ├── physics/               # Collision system — AABB/sphere only
│   │   │   ├── collider.cpp/.h    # AABB collision primitives
│   │   │   ├── near_miss.cpp/.h   # Proximity detection + scoring events
│   │   │   └── kinematic.cpp/.h   # Physics integration (no rigid-body)
│   │   ├── audio/                 # Audio subsystem — miniaudio device + synth dispatcher
│   │   │   ├── device.cpp/.h      # miniaudio initialization, output routing
│   │   │   └── synth_dispatcher.cpp/.h  # Voice pooling (max 8 voices)
│   │   ├── input/                 # Input abstraction layer
│   │   │   └── input_manager.cpp/.h # Keyboard/mouse/gamepad debounce + mapping
│   │   ├── ecs/                   # Lightweight ECS registry
│   │   │   ├── registry.cpp/.h    # Entity/component/system registration
│   │   │   └── pool.cpp/.h        # Object pool pre-allocation (max 2048 entities)
│   │   ├── resource/              # Shader loader, memory allocator, profiler hooks
│   │   │   └── resource_mgr.cpp/.h  # Centralized asset/resource tracking
│   │   └── core/                  # Application lifecycle + subsystem registry
│   │       ├── game.cpp/.h        # Main game class (entry point, update loop)
│   │       └── logging.cpp/.h     # Lightweight logging (printf-style macros)
│   │
│   ├── gameplay/                  # Game rules + player systems
│   │   ├── player/                # PlayerController — run/jump/slide/dash state machine
│   │   │   └── controller.cpp/.h  # State machine + physics integration
│   │   ├── camera/                # Smooth-follow, banking, FOV scaling, shake
│   │   │   └── camera_system.cpp/.h # Camera logic (not rendering — pure math)
│   │   ├── scoring/               # Distance/near-miss/combo/flow meter
│   │   │   └── scoring_engine.cpp/.h  # Score formulas + combo management
│   │   ├── ghost/                 # Ghost recorder/replayer (position+rotation per frame)
│   │   │   └── ghost_manager.cpp/.h # Circular buffer recording + replay rendering hooks
│   │   ├── director/              # Adaptive Director — weighted probability tables
│   │   │   └── director.cpp/.h    # Tracking deltas → adjusting spawn weights
│   │   ├── flowmode/              # Flow Mode trigger + state machine
│   │   │   └── flow_controller.cpp/.h # Threshold tracking + transform signaling
│   │   ├── obstacles/             # Obstacle types + spawner logic
│   │   │   └── spawner.cpp/.h     # Weighted spawning on road segments
│   │   └── upgrades/              # Roguelite upgrade cards (double-jump, shield, etc.)
│   │       └── upgrade_mgr.cpp/.h # Card selection + effect application
│   │
│   ├── procedural/                # World generation (seed-driven, deterministic)
│   │   ├── roadgen/               # Catmull-Rom spline → road segments
│   │   │   └── road_generator.cpp/.h # Spline generation + mesh extrusion
│   │   ├── buildinggen/           # Seed → footprint → extrude → windows → lighting
│   │   │   └── building_gen.cpp/.h  # Noise-driven building placement
│   │   ├── trafficgen/            # Procedural vehicle path-following on roads
│   │   │   └── traffic_gen.cpp/.h   # Vehicle spawning + movement logic
│   │   ├── weather/               # Rain (GPU particles), fog/storm density control
│   │   │   └── weather_system.cpp/.h # Weather state machine + visual drivers
│   │   ├── events/                # Dynamic events: bridge collapse, drone swarm, etc.
│   │   │   └── event_manager.cpp/.h # Event triggers + effect orchestration
│   │   └── audiogen/              # Procedural audio: FM music + oscillator SFX
│   │       └── audio_gen.cpp/.h   # BPM/chord/melody seeds → synth parameters
│   │
│   └── ui/                        # All user interface systems
│       ├── hud/                   # Score, combo, distance, flow meter, energy display
│       │   └── hud_renderer.cpp/.h
│       ├── upgrades/              # Upgrade card selection UI (every ~60s)
│       │   └── upgrade_ui.cpp/.h
│       └── menus/                 # Title screen, pause overlay, game-over summary
│           └── menu_system.cpp/.h # Menu hierarchy + transitions
│
├── shaders/                       # All GLSL shader sources (embedded at build time)
│   ├── vertex/                    # Vertex shaders
│   │   ├── main.vert              # Default: procedural mesh transform + displacement
│   │   └── instanced.vert         # Instanced rendering (roads, buildings, trees)
│   ├── fragment/                  # Fragment/pixel shaders
│   │   ├── gradient.frag          # Gradient-based materials (no textures)
│   │   ├── rim_light.frag         # Fresnel-based rim lighting
│   │   └── sky_gradient.frag      # Procedural sky gradient
│   ├── post/                      # Post-processing passes
│   │   ├── bloom_pass.frag        # 2-pass Gaussian blur + HDR accumulation
│   │   ├── tonemap.frag           # Filmic tone mapping (Reinhard)
│   │   └── fog_vignette.frag      # Exponential height fog + vignette
│   └── particles/                 # Particle system shaders
│       └── particle.vert/frag     # GPU particle rendering pass
│
├── tests/                         # Unit + regression tests
│   ├── unit/                      # Deterministic generator tests
│   │   ├── road_gen_test.cpp      # Seed-repeatability: same seed = same road
│   │   ├── building_gen_test.cpp  # Building footprint from noise determinism
│   │   └── scoring_test.cpp       # Combo formula validation: (1.5)^n verified
│   ├── integration/               # Full-system regression tests
│   │   └── seed_regression.cpp    # World state comparison 100 frames apart
│   └── perf/                      # Frame-time benchmark scenes
│       └── benchmark_scene.cpp    # Auto-generated city for frame-time baseline
│
├── third_party/                   # External dependencies (source/headers only)
│   ├── raylib/                    # raylib source + headers (windowing, input, OpenGL)
│   ├── miniaudio/                 # miniaudio single-header (audio layer)
│   └── stb/                       # stb_image / stb_perlin if needed (tiny footprint)
│
├── config/                        # Project configuration
│   ├── game_config.json           # Tunable parameters: speeds, thresholds, sensitivity
│   │                             # + audio BPM defaults + physics gravity/constants
│   └── contest_rules.txt          # Contest rule copy for reference
│
├── tools/                         # Build/dev utility scripts
│   ├── binary_size_tracker.sh     # Runs `size`, logs KB per build to CI artifacts
│   └── shader_embedder.py         # Embeds GLSL files as constexpr strings in C++
│
├── .github/workflows/             # GitHub Actions CI pipeline
│   └── ci.yml                     # Compile → clang-tidy → clang-format → tests → size gate
│
├── CMakeLists.txt                 # Root CMake: targets for src/ modules, tests, tools
├── README.md                      # Public-facing high-level pitch + contest overview
└── .gitignore                     # Excludes build/, IDE files, binary artifacts
```

---

## Key Structural Notes

| Node | Purpose |
|------|---------|
| `src/engine/` | Layer 0 — core systems nobody outside should directly call; this is the base |
| `src/gameplay/` | Layer 1 — game rules that depend on engine but own player/score/camera logic |
| `src/procedural/` | Layer 2 — world generation that depends on engine only; blind to gameplay content |
| `src/ui/` | Layer 3 — rendering + input dependent; never directly accesses gameplay data (writes snapshot queries via event callbacks) |
| `shaders/` | GLSL source; compiled & embedded as constexpr strings during build by `shader_embedder.py` |
| `config/` | Runtime-tunable parameters loaded at startup from JSON (gameplay balance without code changes) |
| `third_party/` | Vendored headers only — no prebuilt binaries; includes raylib source, miniaudio header |
| `tools/` | Build-time helper scripts for embedding shaders and tracking binary size over time |
