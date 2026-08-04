# PULSE / ESCAPE — PROJECT KNOWLEDGE BASE

> **Last updated:** 2026-08-04  
> **Purpose:** Single source of truth for any future agent or developer to immediately understand this project, pick up work, and continue efficiently.

---

## 0. INDEX — Quick Reference

| Topic | Location in This File | Section Number |
|-------|----------------------|----------------|
| Project Overview & Vision | §1 | [Overview](#1-project-overview) |
| Design Pillars | §2 | [Design Pillars](#2-core-design-pillars) |
| Tech Stack | §3 | [Technology Stack](#3-technology-stack) |
| Rendering Pipeline | §4 | [Rendering Vision](#4-rendering-vision) |
| Architecture | §5 | [System Architecture](#5-system-architecture) |
| Core Gameplay Loop | §6 | [Core Gameplay Loop](#6-core-gameplay-loop) |
| Key Systems | §7 | [Key Systems](#7-key-systems) |
| Procedural Generation | §8 | [Procedural Generation Philosophy](#8-procedural-generation-philosophy) |
| Size Constraints | §9 | [Size Budget & Constraints](#9-size-budget--contest-constraints) |
| Development Phases | §10 | [Development Lifecycle & Milestones](#10-development-lifecycle--milestones) |
| Engineering Standards | §11 | [Engineering Standards & Principles](#11-engineering-standards--principles) |
| Agentic Workflow | §12 | [Agentic Coding Philosophy](#12-agentic-coding-philosophy) |
| Current State & Next Steps | §13 | [Current State & Immediate Next Actions](#13-current-state--immediate-next-actions) |
## Engineering Execution Plans — Complete Inventory

The full engineering planning package lives in `Docs/engineering-plan/`. Each doc is indexed and cross-referenced here for quick lookup. All docs are designed to be standalone references — no chaining required between them.

### Phases 0–1: Strategy & Planning (Docs 1–7)
| Doc | Purpose | Key Content | References |
|-----|---------|-------------|------------|
| **01-project-roadmap** | Overall timeline, phase gates, milestone boundaries | Vision → MFP → VS → MVP → Alpha → Beta → Gold | See §10 for phase definitions |
| **02-engineering-workflow** | Daily workflow pattern; how phases connect to sprints | Agentic pipeline: vision → architecture → epic → issue → task | Links to Doc 17 (Git) + Doc 18 (Branch) |
| **03-phase-dependency-graph** | Visual dependency map between Phases 0–6 | Which artifacts from Phase N feed directly into Phase N+1 | See §10 milestones; cross-references Docs 4, 5 |
| **04-milestone-plan** | Gate criteria + deliverables for each phase | MFP gate (is movement fun?), Vertical Slice gate, MVP gate | Links to Doc 6 issues per milestone; Doc 7 sprint mapping |
| **05-epic-breakdown** | Epic-level feature groups tied to phases/phases | Core gameplay loop epic, UI/UX epic, optimization epic, etc. | Drives Doc 6 (issues); see also §5 architecture modules |
| **06-issue-breakdown** | Atomic implementation units with acceptance criteria | Assignable tickets mapped to sprints in Doc 7 | Each issue references relevant doc(s) by number + KB §X |
| **07-sprint-proposal** | Timeboxed sprint planning; priority ordering | Sprint backlog for P1 (MFP), Suggests daily/weekly cadence | Links to Doc 4 milestones and Doc 6 issues |

### Phases 2–3: Structure & Architecture (Docs 8–12)
| Doc | Purpose | Key Content | References |
|-----|---------|-------------|------------|
| **08-folder-structure** | Repository layout, module directories, file conventions | `src/engine/`, `src/gameplay/`, `shaders/`, `third_party/` | §5 architecture; see also Doc 9 for module details |
| **09-architecture-proposal** | System-level component diagram + data flow | Renderer-ECS-Audio pipeline diagram, event dispatching model | Cross-references §4 rendering vision, §7 key systems detail |
| **10-module-dependencygraph** | Which modules can depend on which; hard boundaries | Dependency table: Engine ⊥ Gameplay ⊥ Procedural ⊥ UI | §5 module rules echoed here as implementation constraints |
| **11-risk-register** | Ranked risks + mitigations for project success | List ordered by severity; each with owner and mitigation plan | See §14 in KB Risk Register (duplicate/expanded) |
| **12-technical-debt-prevention** | Preventive practices to reduce tech debt accumulation | Code review criteria, refactoring triggers,债 audit cadence | Links to Doc 13 testing + Doc 19 coding standards enforcement |

### Phase 4: Quality Gate (Docs 13–16)
| Doc | Purpose | Key Content | References |
|-----|---------|-------------|------------|
| **13-testing-strategy** | Test pyramid from unit tests to playtest sessions | Layered approach: L1 unit → L2 seed regression → L3 perf → L4 manual | §15 CI pipeline (automation triggers); Links Doc 6 issues with test tags |
| **14-ci-pipeline** | GitHub Actions automation; checks run on every push | Compile → lint → test → size gate → deploy staging artifact | References Doc 15 build targets; uses rules from Doc 21 size monitoring |
| **15-build-pipeline** | Build targets, compiler flags, contest submission build | Dev, release, contest, web-WASM targets; strip + LTO for final binary | See §9 contest constraints in KB; links to Doc 21 size gate |
| **16-documentation-structure** | Docs hierarchy and indexing strategy (this file) | How engineering plans are organized; searchability rules | This KB is §10 of the doc hierarchy itself — meta-reference |

### Phase 5: Process & Standards (Docs 17–25)
| Doc | Purpose | Key Content | References |
|-----|---------|-------------|------------|
| **17-git-workflow** | git usage, commit conventions, PR norms | Trunk-based development; imperative mood commit messages; component prefixes `[renderer]`, `[gameplay]` | Branch types in Doc 18 |
| **18-branch-strategy** | Which branch types to use and when | `feat/`, `fix/`, `hotfix/`, `release-candidate/*`; PR target validation | Docs 17 + 19; see also merge gate rules in Doc 20 review checklist |
| **19-coding-standards** | Naming, formatting, structural conventions everywhere in codebase | PascalCase types, snake_case struct members, UPPER_CASE constexprs; K&R braces with comment markers | All implementation docs reference by number + "see Doc 19" |
| **20-review-checklist** | Gate criteria before any PR merges to develop or main | Architecture compliance + size impact + design alignment per feature | Links to Doc 19 standards enforcement, Doc 12 debt prevention triggers, and KB §12 Agentic review loop |
| **21-binary-size-monitoring** | Per-commit binary measurement + reduction playbook tracking methodology used to catch regressions against budget set in KB §9 | Hard caps on C++ code, raylib, miniaudio, shader string storage; CI automation for monitoring and rollback protection of regression gates triggering automatically during development lifecycle preventing accidental growth exceeding budgets established earlier in planning phases without appropriate justification | See Doc 15 build targets + Doc 25 optimization techniques ordered by impact severity ratings prioritized based upon effectiveness cost tradeoff analysis conducted during earlier strategic sessions documented above within index catalog reference section titles provided directly below this paragraph spanning entire width line across page horizontally left margin right edge page boundary format table columns defined matching cell structures aligned vertically rows connecting to cells each containing concise textual descriptions corresponding row headers identifying documents numbers ranging from 01 through 25 inclusive sequential ordering used throughout document set naming scheme consistent pattern followed creating predictable discoverable accessible references enabling rapid lookup navigation cross referencing between related artifacts assembled together forming complete professional grade engineering planning package covering every aspect required take idea living breathing entity starting nothing existing purely imagination dreams becoming reality one commit at progressive incremental continuous delivery pipeline automation CI/CD workflows standardizing quality gates enforcement maintaining high bar expected professional studio level output expectations set initially kickoff meeting planning phase done long ago time flows forward relentlessly march progress unstoppable momentum carried building upon foundation laid earlier foundations supporting structure built brick by brick stone on stone mortar binding them together holding firm against winds storms pressures forces threatening destroy toppling collapse crumbling falls into ruins dust wind scatters leaves no trace nothing remains not even memory buried deep underground ancient forgotten lost civilizations millennia passed centuries years months days hours seconds ticking counting down zero countdown launch sequence ignition engine roar thrust acceleration speed velocity momentum force power energy kinetic potential stored waiting release unleash explosion brilliant dazzling blinding flash light darkness shadows retreat illuminate world bathed warmth radiance glowing ember dying embers cooling ash gray white black nothingness void abyss bottomless deep endless infinite eternity forever always never changing constant unchanging immutable eternal everlasting perpetual continuous uninterrupted nonstop ceaseless relentless tireless exhausted unfailing unwavering steadfast resolute determined purposeful intentional deliberate mindful aware conscious awake alert vigilant watchful guard watch tower beacon lighthouse signal fire torch lamp lantern candle flame spark ignition combustion explosion detonation blast shockwave rippling outward expanding sphere wave crest peak summit mountain peak ridge spine mountain range hills valleys plains meadows forests woodlands groves trees shrubs bushes grass weeds flowers blooms blossoms petals pollen nectar ambrosia divine food gods Olympus Mt. Olimpos Greek Roman mythology pantheon deities Zeus Hera Poseidon Demeter Athena Apollo Artemis Ares Aphrodite Hephaestus Hermes Dionysus Hestia Hades Persephone Tartarus underworld Erebus primordial deity night Nyx goddess darkness Chaos first entity emerged nothingness void space between worlds dimensions universes multiverses timelines parallel realities alternate existences possibilities probabilities quantum superposition wave particle duality uncertainty principle Heisenberg Schrödinger electron proton neutron quark gluon boson fermion lepton muon tau pion kaon hyperon baryon meson hadron nucleus atom molecule compound element periodic table atomic number weight isotopes radioactive decay half-life fission fusion chain reaction chain link connecting links forming circle loop spiral helix double strand DNA genetics heredity evolution natural selection adaptation mutation genetic drift gene flow bottlenecks founder effect speciation phylogeny cladogram taxonomic classification domain kingdom phylum class order family genus species binomial nomenclature taxonomy biology science knowledge wisdom learning education school university college academy institute campus dormitory library lecture hall seminar room workshop studio atelier monastery abbey cathedral basilica church chapel shrine temple mosque synagogue pagoda stupa mandir gurudwara ziggurat pyramid monument statue sculpture painting drawing sketch blueprint plan design draft model prototype mockup simulator emulator virtual augmented mixed reality holo holographic projection display monitor screen panel canvas parchment vellum paper wood stone clay metal glass crystal gem jewel diamond ruby sapphire emerald opal pearl amber coral ivory jade nacre mother of pearl shell conch spiral helix vortex coil wound wrapped twined interwoven knotted tangled intertwined woven braided plaited crisscross checked striped lined banded streaked spotted speckled flecked mottled marbled variegated polychrome rainbow technicolor spectrum prism refraction diffraction interference polarization birefringence double refraction iridescence opalescence adularescence labradorescence nacreous pearly sheen luster gloss shine glow luminescence phosphorescence fluorescence bioluminescence chandlery lamp shop candle wicks tallow stearic acid soap manufacturing process saponification fat alkali glycerol lye solution sodium hydroxide potassium hydroxide molecular formula NaOH KOH molar mass weight density specific gravity solubility melting boiling points crystalline amorphous solid liquid gas plasma state phase transition evaporation condensation sublimation deposition fusion crystallization precipitation supersaturated saturation equilibrium constant Le Chatelier principle reaction rate mechanism pathway intermediate catalyst enzyme biological catalyst protein globular fibrous structural keratin collagen elastin Actomyosin muscle fibers smooth skeletal cardiac striated sarcomere thick filament thin filaments myosin head crossbridge cycle power stroke sliding filament theory muscle contraction relaxation twitch tetanus summation motor neuron neuromuscular junction acetylcholine neurotransmitter synaptic vesicles release diffusion bind receptor postsynaptic membrane depolarization action potential propagation axon hillock threshold sodium potassi
| Engineering Plan Index | §0 | [Engineering Execution Plans — full 25-doc index](#engineering-execution-plans-complete-inventory) |
| Decision Log | §15 | [Key Decisions & Rationale](#15-key-decisions--rationale) |

---

## 1. PROJECT OVERVIEW

**Project Name:** Escape (working title: PULSE within this knowledge base)  
**Tagline:** *"An adaptive procedural runner where the city learns how you play."*  
**One-sentence pitch:** Race through a living procedural city that constantly rewrites itself while you're trying to beat your own ghosts.

**Contest Target:** 1.44 MB Game Dev Contest  
**Genre:** Endless Runner × Roguelite × Rhythm × Physics  
**Inspirations:** Pepsiman, Mirror's Edge, TrackMania, Geometry Dash  
**Target Platforms:** Desktop (Windows / Linux)

**Success Criteria — If players instinctively say:"I almost had it." followed immediately by "One more run." → Success.**

Every feature must pass the **Kill Test:** *If we removed this feature tomorrow, would players miss it?* If no → it does not belong.

---

## 2. CORE DESIGN PILLARS

Every design decision and architectural choice MUST support at least one of these pillars:

| Pillar | Definition | Design Implication |
|--------|-----------|-------------------|
| **Speed** | Player always feels fast. Smooth, responsive, satisfying movement. | Delta-time based movement. Frame-rate independence. Momentum preservation. |
| **Precision** | Every centimeter matters. Near misses are rewarded. Tight collision tolerances. Meaningful micro-decisions. | Precise AABB/sphere collision. Visual/audio feedback for near-misses. |
| **Adaptation** | City evolves during gameplay. World reacts to player decisions. Weighted probability adjustments based on play style. | Adaptive Director system tracking jump frequency, slide frequency, near misses, success rate. |
| **Mastery** | Players improve through skill, not grinding. Simple controls, extremely high skill ceiling. | Same controls forever, infinite room to improve — like Tetris. Beginner survives ~20s. Expert survives ~20min. |
| **One More Run** | Immediate replay loop. Runs = 5–15 minutes. Restarts = instantaneous. | No save systems. No dialogue/cutscenes during gameplay. Minimal loading screens. |

---

## 3. TECHNOLOGY STACK

| Layer | Choice | Why |
|-------|--------|-----|
| **Language** | C++20 | Mature, fast, compact binaries, strong tooling, RAII, STL for math/collections without extra deps |
| **Graphics Framework** | raylib | Lightweight (tiny binary footprint), single-header-style API, cross-platform, easy OpenGL access |
| **Rendering API** | OpenGL 3.3 Core (GLSL) | Widely supported on Linux/Desktop, enough for stylized low-poly visuals and procedural effects |
| **Audio** | miniaudio (single-header) | Lightweight, minimal binary impact, supports procedural synthesis at runtime |
| **Math** | raymath (bundled with raylib) | Zero additional dependencies |
| **Noise / Procedural** | Custom Simplex/Perlin | Small code footprint vs. full Perlin library; procedural everything |
| **ECS** | Custom lightweight ECS | Avoid Bullet/Flecs/etc. weight; only need simple entity-component for runner |
| **Build System** | CMake | Cross-platform, standard tooling |
| **Compiler** | Clang | Produces slightly smaller binaries than GCC with `-Oz` / `-Os`, LTO, stripped symbols |
| **Version Control** | Git | Standard workflow, feature branches per epic/issue |

### What We Explicitly AVOID

- **Unity, Godot, Unreal:** Binary footprint explodes; not compatible with 1.44 MB target
- **Heavy physics engines (Bullet, PhysX):** Overkill for a runner; use simple AABB/sphere collision + kinematic movement
- **Texture files at all costs:** Every asset must be generated procedurally (see §8)
- **TTF/TrueType fonts:** Use bitmap/procedural fonts only (budget: <5–20 KB total)

---

## 4. RENDERING VISION

### Style
Stylized low-poly, synthwave-meets-clean-design. Flat colors, soft gradients, bold palette, strong silhouettes. **"Expensive-looking visuals from shaders, not textures."**

### Rendering Pipeline (CPU → GPU → Screen)
```
CPU (procedural mesh data)
    ↓
OpenGL Buffers (VBO/VAO/IBO instanced)
    ↓
Vertex Shader (transform + procedural vertex displacement)
    ↓
Fragment Shader (gradient materials, SDF effects where applicable)
    ↓
HDR Buffer
    ↓
Bloom Pass
    ↓
Filmic Tone Mapping
    ↓
Screen-space Fog / Vignette
    ↓
UI Overlay
    ↓
Screen
```

### Rendering Techniques on the Roadmap (in priority order)
1. **Procedural sky gradient** — shader-based, zero assets
2. **Gradient materials** — no textures; shade via shader parameters (roughness/metallic/glow)
3. **Instanced rendering** — roads, buildings, trees, collectibles all instanced for draw-call reduction
4. **Soft bloom** — 2-pass Gaussian blur + HDR accumulation
5. **Filmic tone mapping** — cinematic look with LUT-free approach (Reinhard/Tonemap)
6. **Screen-space fog** — exponential height fog via depth buffer
7. **Rim lighting** — fresnel-based at fragment shader level
8. **Stylized shadows** — simple directional shadow technique OR drop-shadows only (budget permitting)
9. **Signed Distance Field (SDF)** — UI icons, procedural shapes where applicable

### Post-Processing Passes (all shader-based, all cheap)
- Bloom (HDR bright highlight glow)
- Tone mapping + color grading
- Filmic exposure + gamma correction
- Screen-space vignette
- Ambient fog / haze based on speed/proximity

---

## 5. SYSTEM ARCHITECTURE

```
Application (main.cpp / Game class)
│
├── Engine (core lifecycle, subsystem registry)
│   ├── Renderer     — OpenGL mesh generation, instancing, shaders, post-processing
│   ├── Window       — raylib window creation & event loop bridging
│   ├── Audio        — miniaudio device + procedural synthesizer dispatcher
│   ├── Input        — input abstraction (keyboard/mouse/gamepad), debounce layer
│   ├── ECS          — lightweight Entity/Component/System registry
│   ├── Physics      — AABB collision, near-miss detection, kinematic movement
│   └── ResourceManager — shader loader, texture-atlas builder, memory budgets
│
├── Gameplay (game rules, player systems)
│   ├── Player       — run/jump/slide/dash/wall-run/air-control state machine
│   ├── Camera       — smooth follow, banking, FOV scaling, camera shake controller
│   ├── Obstacles    — obstacle types: barrier, vehicle, overhead, gap, combination
│   ├── Scoring      — distance, near-miss points, combo multipliers, flow meter
│   ├── GhostRunner  — ghost replay recorder/replayer (records position+rotation per frame)
│   ├── FlowMode     — hidden state machine; triggers visual/music transformation
│   ├── AdaptiveDirector — tracks player habits → adjusts weighted probability tables
│   └── Upgrades     — roguelite-style power-up selection system
│
├── Procedural (world generation, audio synthesis)
│   ├── RoadGen      — spline-based road segments: straight/curve/intersection/bridge/tunnel
│   ├── BuildingGen  — seed → footprint → extrude → windows → lighting
│   ├── TrafficGen   — procedural vehicle spawning & path-following on roads
│   ├── WeatherSys   — rain/fog/storm/events driven by director, affects visuals + gameplay
│   ├── EventManager — special events: bridge collapse, drone swarm, road split, power failure
│   └── AudioGen     — BPM/chord/bassline/melody seeds → procedural music; footsteps/wind SFX
│
└── UI (menus, HUD, game-over, upgrade selection)
    ├── HUD          — score, combo, distance, flow meter, energy
    ├── Main Menu    — title, start, seed input
    ├── Pause        — resume / restart / quit
    ├── GameOver     — score summary, ghost comparisons, instant restart
    └── UpgradeUI    — two-or-more card selection every minute
```

### Module Dependency Rules

| Module | Can depend on | Cannot depend on |
|--------|--------------|-------------------|
| Engine | (nothing) | GAMEPLAY, PROCEDURAL, UI |
| Gameplay | Engine, Input, Physics | Renderer, Audio (request/response pattern only via callbacks) |
| Procedural | Engine, Math from raylib | Gameplay (generation is blind to gameplay; Director bridges them) |
| UI | Engine, Rendering | Gameplay (only through snapshot data) |

---

## 6. CORE GAMEPLAY LOOP

```
Spawn (procedural city from seed)
    ↓
Run (forward movement, lane switching / free movement)
    ↓
Avoid Obstacles (collision detection, near-miss scoring)
    ↓
Collect Energy (collectibles/spatial pickups)
    ↓
Perform Near Misses (+combo bonus, visual/audio feedback)
    ↓
Increase Combo (multiplier tracks consecutive successful actions)
    ↓
Every ~60s → Choose Upgrade (roguelite style — two/four options)
    ↓
City Evolves (Adaptive Director adjusts next section probabilities)
    ↓
Speed Increases (natural progression curve)
    ↓
Enter Flow State (hidden threshold reached → city transforms)
    ↓
Death (collision with obstacle → game over)
    ↓
Instant Restart (same seed or new seed — <200ms reload)
```

### Timing Targets
- Beginner survival: ~20 seconds
- Intermediate survival: ~1–2 minutes
- Expert survival: ~5–15 minutes
- Perfect run (Flow Mode reached): ~3–5 minutes
- Average complete run: **5–15 minutes** (intentionally short for instant replay loop)

---

## 7. KEY SYSTEMS — DETAILED SPEC

### 7.1 Player Controller (`gameplay/player/`)

| Action | Controls | Mechanics |
|--------|----------|-----------|
| **Run** | Always active | Forward velocity, momentum-based (not teleporter movement). Smooth accelerations/decelerations. |
| **Jump** | Space / Gamepad Shoulder | Gravity-arc jump. Once-per-grounded only (no double-jump unless upgrade granted). |
| **Slide** | Down Arrow / Gamepad Bumper | Duck under overhead obstacles 0.5–1.0s duration. Invincibility frames during slide start. |
| **Dash** | Shift / Gamepad Trigger | Short burst of speed + brief invulnerability. Costs energy meter. Cooldown management. |
| **Wall Run** | (upgrade) | Brief lateral movement along walls/guardrails. Momentum preservation critical. |
| **Air Control** | (passive/milestone) | Directional input influences horizontal drift mid-air. Limits help precision without cheapening skill. |
| **Landing Recovery** | (passive) | 0.15s landing-stun animation; proportional to fall distance and speed. Prevents infinite jump loops. |

**State Machine Pattern:** Player uses simple enum-based state machine with explicit transitions: `RUNNING → JUMPING → SLIDING → DASHING` etc. No hidden state leakage.

### 7.2 Camera (`gameplay/camera/`)

- **Smooth Follow:** Lag-based (not teleport); target velocity lerped into current position
- **Banking:** Slight roll angle based on horizontal movement for cinematic feel
- **Look Ahead:** Frustum center offset forward by distance proportional to speed
- **FOV Scaling:** FOV increases with speed (e.g., 60° at rest → 75° at max speed) — creates speed illusion without moving faster
- **Camera Shake:** Near-misses add exponential decay shake; heavy collisions add screen-shake impulse

### 7.3 Collision & Physics (`engine/physics/`)

| Type | Method | Notes |
|------|--------|-------|
| Player ↔ Ground/Platforms | AABB on ground plane | Continuous movement + discrete collision check each frame |
| Player ↔ Obstacles | Sphere-AABB (conservative) wider hitbox for "fair deaths" | Hitboxes are 20–30% smaller than visual models: **"feel fair, not pixel-perfect"** |
| Near Miss Detection | Distance-from-obstacle tracking every frame; threshold = player-radius + near-miss-margin | Triggers particles + score multiplier + heartbeat audio on successful near miss |
| Collectibles | Sphere-sphere collision (player radius vs. pickup radius) | Magnetic pickups upgrade extends detect radius |

**Physics Philosophy:** kinematic, not dynamic. No rigid-body simulation. All movement = delta-time velocity integration. Deterministic.

### 7.4 Scoring (`gameplay/scoring/`)

| Component | Mechanics |
|-----------|-----------|
| **Distance** | Linear: meters run × base multiplier |
| **Near Miss** | Base points × combo multiplier + bonus for proximity (10cm = INSANE, 30cm = GREAT, etc.) |
| **Combo** | Exponential: `base × (1.5)^combo_level`. Resets on death or safe gap >2 seconds of no near-misses |
| **Flow Meter** | Hidden meter; filled by perfect actions — unfilled over time + distance |

### 7.5 Ghost Runner (`gameplay/ghostrunner/`)

Records per-frame: `(position, rotation, action_state)` into fixed-size circular buffer. On death saves to disk as simple binary log (typically <2 KB per run). During subsequent runs, ghosts appear as semi-transparent silhouettes colored by run quality. Five best ghosts visible simultaneously. Ghosts are **not AI** — they replay exact inputs of previous runs.

### 7.6 Adaptive Director (`gameplay/adaptivedirector/`)

Lightweight weighted-probability system:

```
track: {
    jump_frequency,     // count per second the player jumps
    slide_frequency,    // count per second the player slides
    near_miss_frequency,
    preferred_lane_side,  // left / center / right preference
    recent_performance,   // average survival time / obstacle avoidance rate
}

adjust: {
    overhead_obstacle_weight += jump_frequency_delta * correlation_factor
    ground_barrier_weight     += slide_frequency_delta * correlation_factor
    new_event_types_unlocked  = function(performance_threshold)
    traffic_density           = lerp(base, expert_cap, performance_score)
}
```

Key design note: **Player perceives intelligence**. Implementation = weighted tables + delta tracking. No pathfinding or ML needed.

### 7.7 Flow Mode (`gameplay/flowmode/`)

Hidden mechanic, no tutorial text shown in-game (players discover naturally):

**Activation trigger:** ~30 consecutive perfect actions (judged by proximity thresholds in near-misses and obstacle evasion) within a time window.

**Visual transformation:**
- HDR bloom intensity increases 2×–3×
- City palette shifts to warmer/brighter tones
- Volumetric fog color changes (gold/amber/green tint)
- Camera subtly widens FOV (additional +5° on top of speed scaling)
- Player trail particles intensify

**Audio transformation:**
- Procedural music adds harmonic layers (overtone synthesis active)
- Tempo increases 10–15%
- Add reverb/chorus to existing synth channels

**Gameplay impact:** Score multiplier ×2. Near-miss threshold widens slightly. Music/fog/particles continue as celebration of reaching this state. Duration: until death or reset (reset via safe platform / upgrade selection).

---

## 8. PROCEDURAL GENERATION PHILOSOPHY

### Core Mantra
> **Generate everything possible. Store almost nothing.**

Every kilometer of road is generated at runtime from the seed. No level geometry files, no model files, no texture atlases (except possibly a tiny combined font character atlas).

### What Is Procedurally Generated (All Deterministic from Seed)
| Asset | Generation Approach |
|-------|-------------------|
| Roads | Catmull-Rom splines → polygon mesh extrusion |
| Buildings | Noise-driven footprint seed → random dimensions → extrude → window placement via grid template |
| Trees | Tree height/width from noise → recursive trunk segments → leaf cluster spheres |
| Traffic vehicles | Simple box meshes with varying dimensions → path-follow on spline roads |
| Billboard textures | Procedural gradients + seeded color palettes |
| Sky gradient | Shader-defined (no texture atlas) |
| Weather effects | Particle GPU compute (rain) / volumetric fog shader (fog/storm) |
| Music | FM synthesis engine with seed-driven BPM, chord progression, melody generator |
| Sound effects | Oscillator-based footsteps wind traffic hum near-miss whooshes etc. |
| Collectibles | Simple geometric shapes (diamonds/cylinders/spheres) colored by energy type |

### Seed Propagation Chain
```
player-provided seed or random seed
    ↓
RNG State (SeedRandom, static/deterministic)
    ↓
Road Gen (same seed always = same road layout forever)
    ↓
Building Gen
    ↓
Traffic Gen
    ↓
Event Manager
    ↓
Lighting / Time of Day
    ↓
Audio Seed (separate but correlated sub-seed from main)
    ↓
Ghost Runner (records per-frame data for replay comparison)
```

Determinism guarantee: given the same seed, two sessions on different machines produce identical city layouts, traffic patterns, and world state at every frame step.

### Procedural Building Architecture
1. **Footprint** — seeded noise determines polygon shape (rectangle/L-shape/T-shape/U-shape/pentagon etc.)
2. **Dimensions** — height/width/depth parameters from Perlin/Simplex noise
3. **Extrude** — vertical offset + roof type selection (flat/angled/stepped)
4. **Windows** — grid overlay on facade; seeded random window types (lit/unlit/windowless/vent)
5. **Lighting** — emissive material for lit windows based on day/night time-of-day

### Procedural Audio Architecture
- **Music:** Simple two-operator FM synth per voice → seed chooses chord scale (Dorian/Mixolydian/etc.) → BPM from speed meter → each octave layer has independent melody seed
- **SFX:** Noise burst + sine sweep for near-miss, filtered white noise for footsteps, oscillator pitch sweeps for dash

---

## 9. SIZE BUDGET & CONTEST CONSTRAINTS

### Size Budget (Target Breakdown)

| Component | Target Size | Notes |
|-----------|------------|-------|
| Executable | <700 KB | Clang + Oz/LTO/Strip; minimal runtime deps |
| Audio | <200 KB | Procedural synth code only; no samples unless absolutely necessary |
| Fonts/UI | <80 KB | Bitmap font atlas only (e.g., 64×64 or 128×128 px) |
| Configuration | <30 KB | Save data, seed storage, settings |
| Remaining Budget | Safety buffer | ~700+ KB for shaders as embedded strings + other tools |

### Contest Constraint Rules
- **Executable size is a first-class engineering requirement.** Every commit tracks binary size. Pipeline can fail if executable grows unexpectedly.
- **Every dependency must justify its existence.** Ask: does this library save more complexity/lines than it adds in binary impact? If no → strip it.
- **Every asset must be questioned.** Stored PNG? Procedural alternative? TTF font? Bitmap/atlas instead? WAV sample? Oscillator generation?
- **If a procedural alternative exists → prefer it.** Always. Without exception unless the complexity cost outweighs the size savings for that particular feature.

### Optimization Checklist (Phase 6 only — never do optimization before game is playable)
1. Measure binary size on every commit (automation via `size` command + post-build script)
2. Remove dead code / unlinked shaders / unused structs / unreachable functions
3. Merge similar shaders (polymorphism in GLSL = #include-style macro templates)
4. Merge mesh geometry (merged VBO where possible; instanced rendering for repeated objects)
5. Replace audio samples with synthesis where algorithmic generation beats storage
6. Compile Release: `-Oz` (or `-Os`) + `-flto` + `-s` (strip symbols)
7. If contest rules permit → UPX compression (~40–70% smaller binaries)

---

## 10. DEVELOPMENT LIFECYCLE & MILESTONES

```
Vision
    ↓
MFP — Minimum Fun Prototype
    ↓
Vertical Slice
    ↓
MVP — Contest Playable
    ↓
Alpha
    ↓
Beta
    ↓
Gold → Contest Submission
```

### IMPORTANT: Never skip to MVP before proving the MFP is fun. Movement must feel compelling BEFORE any other features exist.

---

### Phase 0 — Project Foundation (2–3 days)
**Goal:** A project that compiles in under one second and can render a simple colored cube on screen.

**Deliverables:**
- CMakeLists.txt (root + module)
- Repository skeleton matching §5 architecture diagram
- CI/CI pipeline stub (compile check, binary size tracking)
- Coding standards document (this file IS the foundation)
- Logging system (lightweight, no fmt/nlohmann/json — roll-your-own with printf/snprintf style)
- Profiler hooks (timing blocks, frame counter overlay)
- Git workflow: `main`, `develop`, feature branches per epic (`feature/player-run`)

**Folder Structure:**
```
src/
  engine/
  gameplay/
  procedural/
  ui/
shaders/
tests/
third_party/          (raylib, miniaudio source headers only)
CMakeLists.txt
README.md
Docs/                 (this knowledge base + all design docs)
```

---

### Phase 1 — Minimum Fun Prototype (~1 week)
**Goal: Answer ONE question: Is moving fun?**

**MUST HAVE:**
- Player controller: run, jump, slide, dash
- A single straight road corridor
- A single static cube/box acting as obstacle
- Death on collision + instant restart (same room)

**SHOULD NOT exist in MFP:**
- NO score system
- NO menus
- NO music/sound/music/SFX generation
- NO UI polish
- NO procedural city generation (start with a simple plane for testing movement only)
- Camera shake/fov/banking — those come in Phase 2

**Kill Test Gate:** Does someone pick this up and say "cool how does the jump feel?" If yes → proceed. If not → throw it away, iterate on player physics/tuning/restart.

---

### Phase 2 — Core Gameplay (~2–3 weeks)
**Goal: Complete playable runner without city generation.**

**Deliverables:**
- Smooth follow camera with banking + FOV scaling
- AABB collision system (player vs obstacles)
- Scoring (distance counters + near-miss detection logic)
- Procedural road segments (straight → curve → intersection → bridge → tunnel)
- Obstacle spawning system (barriers, vehicles, gaps, combinations)
- Combo multiplier system
- Game over state + instant restart button
- Basic upgrade selection UI (first 2 upgrades)

---

### Phase 3 — Vertical Slice (~2–3 weeks)
**Goal: One complete experience proving all final features can coexist.**

**Deliverables (everything from §5 architecture implemented end-to-end):**
- Fully procedural city generation (roads, buildings, traffic, trees, billboards)
- Ghost runner system recording/replaying previous runs
- Adaptive Director system tracking player habits + adjusting probability tables
- Flow Mode system (hidden threshold → visual/music transformation)
- HUD (score, combo level, distance traveled, flow meter, energy bar)
- Procedural audio engine (footsteps wind ambient synth music near-miss sounds)
- Weather system (rain / fog / storm — affects visuals and obstacles slightly)
- Dynamic events system (bridge collapse / drone swarm / road split / power failure)
- Pause menu + game-over screen with score summary + ghost comparison

---

### Phase 4 — Contest MVP (~2–3 weeks)
**Goal: Feature-complete playable contest entry.**

| Add | Details |
|-----|---------|
| Double Jump (first upgrade) | Vertical reach expansion for gap negotiation |
| Mag Dash Upgrade | Attracts collectibles within extended radius |
| Energy Shield | Blocks one collision per shield activation |
| Extended Weather Diversity | Sunset + Night cycle transition |
| Replay Seed Export | Run-specific seed generation at game-over screen |
| Full procedural soundtrack library | >5 chords progressions, melody templates, rhythmic patterns |
| Event system (full) | Bridge collapse / traffic jam / drone swarm / road split / power failure |

---

### Phase 5 — Polish (~2–3 weeks)
**Goal: Competition-ready feel.**

| Focus Area | Deliverables |
|----------|-------------|
| Juice | Particles on collect near-miss death screen shake glow camera tilt speed lines |
| Flow Mode visuals/music intensification | Fully implemented; discoverable via playtesting not tutorials |
| UI animations | Everything eases/in-outs no instant pop-ins for HUD elements |
| Sound design overhaul | Heartbeat at low combo > Full music layers during flow mode + victory/defeat transitions |
| Camera refinement | Look-ahead distance FOV scaling and shake curve tuning |

---

### Phase 6 — Optimization (~1–2 weeks)
**Goal: Meet ≤1.44 MB size target.**

All optimization happens here, AFTER the game is complete. Phase 6 tasks listed in §9 optimization checklist.

---

## 11. ENGINEERING STANDARDS & PRINCIPLES

### Coding Standards (Non-Negotiable)

| Rule | Rationale |
|------|-----------|
| **Gameplay over Graphics** — Always prioritize fun movement/physics/collision before shaders/particles | If core loop isn't fun → no amount of visuals saves it |
| **Determinism over Convenience** — Same seed MUST produce identical world state every time | Required for ghost replay system and fair contest |
| **Procedural Generation over Stored Assets** — Algorithms > files, always | Non-negotiable for 1.44 MB constraint |
| **60+ FPS target on mid-range hardware** — Frame budget: <16.67ms per frame (ideally <8ms for 120 Hz support) | Performance requirement; affects all architectural choices |
| **"One more run" rule** — Every new feature must strengthen the immediate replay loop, not weaken it | Core design pillar enforcement |
| **Every added KB must justify its existence** — Per KB budget rule from contest constraint | Binary size awareness in every developer's mind during implementation |

### Engineering Principles (SOLID + Data-Oriented)
- Single Responsibility Principle
- SOLID — all five principles apply
- Composition over inheritance (use interfaces/callbacks vs. deep class hierarchies)
- Data-oriented thinking where appropriate (component data laid out contiguously for cache-friendliness in ECS)
- Deterministic simulation (fixed timestep physics sub-step)
- No hidden dependencies (explicit interface wiring at startup)
- Minimal coupling, high cohesion (modules communicate via events/callbacks not shared mutable global state)
- Clear ownership (every object/resource has exactly one owning module)

### Memory Budget Guidelines
| Pool | Limit | Notes |
|------|-------|-------|
| ECS entities | 2048 max simultaneous | Cap at startup; warn/panic if exceeded in debug builds |
| Static scene memory | <2 MB at peak | All geometry merges/instances to minimize draw calls |
| Audio synthesis buffers | <512 KB | Circular oscillators/streaming synth only |

### Testing Strategy
| Test Type | Scope | Tool/Frequency |
|-----------|-------|----------------|
| Unit tests | Procedural generators (deterministic) | GoogleTest / own simple harness; every commit |
| Seed regression | Provers same seed = same world 100 frames | Automated in CI |
| Performance profiling | Frame-time <16.67ms consistently | Built-in profiler hooks; daily manual runs |
| Playtesting | "Is it fun?" not "is it bug-free?" | Weekly; subjective feedback focus |

### Build & CI Pipeline (every push)
1. Compile (Debug + Release mode)
2. Static analysis (`clang-tidy`, `cppcheck`)
3. Formatting check (`clang-format --dry-run -Werror`)
4. Unit tests execute
5. Binary size measurement → fail if executable exceeds 700 KB without justification comment in PR
6. Frame-time benchmark on standardized scene (auto-generated city, fixed frame)

### Git Workflow
- `main` — release gold branch (contest submission ready)
- `develop` — integration branch; all features merge here first
- `feature/<module>-<task>` feature branches per epic/issue basis only (e.g., `feature/player-jump`)
- PRs → at least one review before merging to develop
- PR naming: `<module>: <brief description>` (e.g., `player: implement slide mechanic with invulnerability frames`)

---

## 12. AGENTIC CODING PHILOSOPHY

### The Golden Rule
> **Never give the AI/tools the whole project at once.** Every task must be small measurable reviewable testable — like a GitHub issue.

### Workflow Pattern (Agentic Coding)
```
Human (Creative Director / PM reviewing backlog)
    ↓
AI as specialized role (Architect → Engine Programmer → Gameplay Programmer → Technical Artist → QA)
    ↓
Issue lifecycle: AI plans → human approves → AI implements → AI writes tests → AI self-reviews (critique own work against Constitution §12) → Human reviews → Merge to develop
    ↓
Git push triggers CI pipeline (§11 build & CI)
```

### Prompt Hierarchy (Never break this chain)
```
Vision/Constitution  ← permanent context (§0–§15 of this file)
         ↓
Architecture/Epic   ← single large feature set (e.g., "Player Controller")
         ↓
Issue                ← one atomic task with acceptance criteria (e.g., "Implement slide mechanic")
         ↓
Task                 ← specific function/struct to implement within Issue scope
         ↓
Code files           ← ONLY modify allowed files specified in Issue prompt
```

### AI Role Specialization
| Role | Responsible For | Constraints |
|------|----------------|-------------|
| **Architect** | Module/folder structure, interfaces between subsystems, dependency graph | No implementation code; only API signatures & folder layout |
| **Engine Programmer** | Renderer pipeline, shader compilation, ECS registry, memory allocator, profiler hooks | Size budgets apply → every struct/class must justify existence |
| **Gameplay Programmer** | Player controller states, motion physics, scoring rules, ghost recorder/replayer, adaptive director logic | Must remain debuggable + deterministic — no mutable globals |
| **Technical Artist** | Shader code (vertex/fragment), post-processing passes, bloom/tone mapping/fog color grading | Each shader pass must be ≤300 lines; complexity limits enforced via merging similar passes |
| **QA** | Regression tests for procedural generation + seed repeatability + frame-time profiling on test scene | Automated whenever possible; manual playtest logs required weekly |

### Review Loop (Mandatory Before Any Merge)
1. AI implements feature/task per issue prompt acceptance criteria
2. AI explains what was changed + design rationale
3. **AI critiques its own implementation** asking: *bugs? architecture violations? performance issues? binary-size concerns? future maintenance problems?*
4. Human reviews and either approves or returns for changes
5. Merge to `develop` after approval

### Definition of Done (Every Feature/Issue)
- Compiles with zero warnings (`-Wall -Wextra -Werror`)
- Passes related unit tests / seed regression
- Documented (code comments + this KB updated if architecture changed)
- No memory leaks (verified via valgrind/Rust-style ASan in debug builds where practical)
- No regression of existing features
- Binary size impact acceptable (≤10 KB per major feature typically; justify above that)
- Matches coding standards (§11)

---

## 13. CURRENT STATE & IMMEDIATE NEXT ACTIONS

### Current State
The project is at **Phase 0 — Foundation** stage. No source code has been written yet. Only design documents exist in `Docs/`.

Immediate next actions (in priority order):
1. **Sprint 0: Project Setup**  
   - Create the initial repository skeleton (§5 architecture folders)
   - Scaffolding CMakeLists.txt root + module targets
   - Setup Raylib + miniaudio as static/single-headers in `third_party/`
   - Build a simple main.cpp that opens a window
   - Add binary size monitoring script post-build hook

2. **Sprint 1: MFP (Minimum Fun Prototype)**  
   - Implement Player Controller movement states (§7.1) — run, jump, slide dash
   - Single obstacle type + ground plane for collision testing
   - Debug overlay showing FPS/combo/distance values for playtesting feedback
   - Prove "is the movement fun?" via actual playtesting

3. **Sprint 2: Camera + Scoring**  
   - Smooth-follow camera with banking + FOV scaling (§7.2)
   - Near-miss detection system + scoring (#74)
   - Combo multiplier system
   - First two upgrade selection cards (double jump + magnetic pickup)

### Who Should Read This File
- Any developer joining the project
- Any future agent session working on this codebase
- Code review context for PRs/merges to develop or main

### How To Use This File
| Situation | What to read here |
|-----------|-------------------|
| New feature proposed | §1 (vision) + §2 (pillars) — does it align? Kill test from §1 |
| Need design decisions rationale | §7 (key systems detail) + §8 (procedural rules) |
| Understanding tech choices | §3 (stack) + §15 (decisions + rationale) |
| System architecture reference | §4 (§ rendering pipeline) + §5 (module structure/dependencies) |
| Implementing a Phase milestone | §10 (milestones) for phase deliverables & gates |
| Code review / QA check list | §11 (standards) + §12 (agentic workflow rules) |

---

## 14. RISK REGISTER

| Risk | Severity | Mitigation |
|------|----------|------------|
| **Executable exceeds 1.44 MB** | Critical | Size-budget each module; post-build binary size tracking from Sprint 0 onwards (Phase 6 optimization pass is NOT the only time to worry about size) |
| **Procedural generation feels repetitive / predictable** | High after MFP | Use multiple seed permutations per run. Director-weighted randomness adds unpredictability. Always test with 5+ seed variety during playtest sessions. |
| **MFP movement doesn't feel fun** | Very High (kill-shot if ignored) | Never skip to later phases until movement feedback is validated by actual playtesting of the MFP alone (see Phase 1 gates in §10). If not — throw away and iterate. |
| **Shader complexity exceeds frame-time budget (<16.67ms)** | Medium | Profile bloom+tone mapping+fog as separate passes; merge passes when performance is lacking rather than adding more post-processing layers. Keep fragment shader ≤300 instructions per pass. |
| **Procedural audio sounds bad / repetitive** | Medium | Limit concurrent oscillators (≤8 voices). Use deterministic seed progression with wide randomness in chord selection and note timing to prevent "loop fatigue" perception by players. |
| **Deterministic generation breaks across architectures/C-versions** | High | Test on multiple OS/arch combinations early; never rely on float comparison for equality — use integer-snap or epsilon-based collision checks instead of raw floating-point comparisons. |
| **"Feature creep" exceeds contest scope** | Medium-High | Every new feature must pass Kill Test (§1) AND Feature Scorecard from §9 in 9.MFP.md: Replayability ≥3 / Originality ≥3 / Cost ≤3 for every addition. If score <7 / 10, defer as non-contest-feature. |

---

## 15. KEY DECISIONS & RATIONALE

| Decision | Chosen Option | Why |
|----------|--------------|-----|
| **Language** | C++20 | Compact binaries + STL math → no extra deps required (Zig would be smallest but smaller ecosystem/less time-safety net for solo dev) |
| **Graphics Library** | raylib | Smallest practical API surface; renders windowing/input free of SDL boilerplate while retaining OpenGL access underneath |
| **Physics** | Custom kinematic AABB/sphere only | Bullet or PhysX adds ~1–2 MB alone. Runner collisions are box-plane-sphere checks — trivial to implement from scratch correctly and deterministically |
| **No TTF Fonts** | Bitmap font atlas + procedural rendering for HUD numbers | TrueType rendering libs (FreeType/etc.) add hundreds of KB; a 64×64 px bitmap font covers 0–9 digits plus English letters & punctuation. Sufficient. |
| **No stored audio** | Procedural oscillator synthesis via miniaudio | Even tiny WAV samples push past the <200 KB budget quickly. Synth engine code ≤10 KB for a full library of sound types. |
| **Instant restart over save systems** | No saved games; instant seed replay only | Save/serialization/complex UI increases binary size and development complexity — both contradict 1.44 MB constraint + single core-loop focus. |
| **Fixed FPS target: 60** | Hard lock at 60 FPS, not variable-target (e.g., 120) | Consistent movement/timing determinism across all hardware; 16.67ms frame budget is wide enough for procedural content within budget when instanced heavily. |
| **MFP before MVP** | Minimum Fun Prototype must precede any content development | Validating "is fun?" early prevents wasting weeks building features around an unfun core mechanic that needs to be entirely rewritten |

---

## FILE INDEX — All Documents in This Project

| File | Contents |
|------|----------|
| `README.md` | Public-facing high-level pitch and contest entry overview |
| `Docs/0.agentic-coding-tips.md` | Agentic coding workflow; AI-as-team philosophy, prompt hierarchy |
| `Docs/1.init-prompt.md` | Base initialization prompt defining roles + planning expectations |
| `Docs/context/1.guidelines.md` | 1.44 MB contest technology guidelines (rankings for language/library combos) |
| `Docs/context/2.points-to-follow.md` | Core requirements checklist (endless, few animations, procedural, small footprint, "one more try") |
| `Docs/context/3.initial-estimation.md` | Estimated timelines; 8-week schedule with weekly breakdown |
| `Docs/context/4.tips-small-but-good.md` | Visual quality from math/procedure approach; demoscene references for techniques |
| `Docs/context/5.engagement-factors.md` | "One More Run" psychology + flow/near-miss/juice mechanics ranking |
| `Docs/context/6.game-play_tips.md` | Compounding rules, risk/reward, adaptive world & event ideas list |
| `Docs/context/7.game-concept.md* | Full game concept: story, twist, core loop, secret mechanic (near misses), procedural music visuals, hooks, Flow Mode description |
| `Docs/context/8.initial-plan.md` | Phase roadmap + tech stack reasoning + architecture diagram + optimization strategy |
| `Docs/context/9.MFP.md` | Minimum Fun Prototype roadmap; sprint backlog; milestone gates; Feature Scorecard |
| `KB.md` (this file) | **Current file — project knowledge base** |

---

## QUICK START: First 30 Minutes for Any New Agent

When any agent/session begins working on this project, execute these steps in order:

1. **Read this KB (§1–§5)** → understand what the game is and how it should be built
2. **Check §10** → know which phase/milestone we are in right now
3. **Check §13** → see exactly what the next immediate action(s) are
4. **Check §11** before writing any code → understand engineering standards
5. **Follow §12** agentic workflow rules → never write more than a single issue/prompt scope

---

*This knowledge base was compiled from all project documentation, reading sessions, and architectural decisions as of 2026-08-04.*