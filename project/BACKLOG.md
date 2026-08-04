# BACKLOG

> Project backlog organized by Epic → Issue → Task hierarchy with strict lifecycle per RULES_OF_THE_GAME.md Stage 7.
> 
> States follow: BACKLOG → READY → IN_PROGRESS → IMPLEMENTED → UNDER_REVIEW → APPROVED → MERGED → DONE
> 
> Updated every sprint planning session. Last updated: 2026-08-04 by ADOS bootstrap validation.

---

## How To Use This File (validation.md Stage 7 enforcement)

1. **All statuses must be exact uppercase codes** from the state machine above. No abbreviations.
2. **Issue columns**: `Status` column is mandatory — no blank statuses.
3. **Transitions require gate checks**: Moving to IMPLEMENTED requires Gate 3 (compile + verify); to UNDER_REVIEW requires Gate 5 (reviewer sign-off).
4. **Sprint column maps to phase**: S0=Phase 0 (Foundation), S1+=Phases per roadmap.

---

## EPIE14: Project Foundation (P0) — Phase 0

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I14-01 Root CMakeLists.txt | BACKLOG | S0 | Targets: pulse.exe + logging + profiler + size_tracker |
| I14-02 main.cpp with raylib window | BACKLOG | S0 | Title "PULSE / ESCAPE", renders empty frame |
| I14-03 Logging subsystem | BACKLOG | S0 | Light-weight printf-style macros (KB §3) |
| I14-04 Profiler hooks | BACKLOG | S0 | Timing blocks + frame counter overlay |
| I14-05 Binary size tracking script | BACKLOG | S0 | tools/binary_size_tracker.sh post-build |
| I14-06 CI pipeline stub | BACKLOG | S0 | Compile check only on every push |

---

## Epic E1: Player Controller (P1–P2) — Phase 1 (MFP)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I01 Run movement state | READY | S1 | Forward velocity, momentum-based delta-time |
| I02 Jump with gravity arc | READY | S1 | Once-per-grounded only (MFP scope) |
| I03 Slide mechanic | READY | S1 | Duck under obstacles 0.5-1.0s; inv frames |
| I04 Dash mechanic | READY | S1 | Speed burst + brief invulnerability; costs energy |
| I05 State machine wiring | BACKLOG | S2 | Enum states (RUN→JUMP→SLIDE→DASH) |
| I06 Landing recovery stun | BACKLOG | S2 | 0.15s proportional to fall distance |

---

## Epic E2: Camera System (P2)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I07 Smooth-follow camera | BACKLOG | S2 | Lag-based lerp interpolation; banking ±8° |
| I08 FOV scaling | BACKLOG | S2 | 60° at rest → 75° at max speed |
| I09 Camera shake | BACKLOG | S2 | Near-miss exponential decay; heavy collision impulse |

---

## Epic E3: Collision Physics (P1–P2)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I10 AABB ground detection | READY | S1 | Ground plane collision for first playtest |
| I11 Sphere-AABB obstacle collision | BACKLOG | S2 | 20-30% smaller hitboxes (fair deaths) |
| I12 Near-miss detection | BACKLOG | S2 | Distance tracking; threshold = radius + margin |

---

## Epic E4: Scoring + Combo (P2)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I13 Distance scoring | BACKLOG | S2 | meters × base multiplier |
| I14 Near-miss scoring tiers | BACKLOG | S2 | INSANE(10cm) / GREAT(30cm) / OK(60cm) |
| I15 Combo multipliers | BACKLOG | S2 | base × (1.5)^combo_level |

---

## Epic E5: Procedural Road System (P2–S3)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I16 Catmull-Rom road generation | BACKLOG | S2 | Straight → curve → intersection bridge/tunnel |
| I17 Instanced mesh rendering | BACKLOG | S3 | VBO/VAO/IBO instancing for performance |

---

## Epic E6: Obstacle Spawner (P2)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I18 Barrier obstacle type | BACKLOG | S2 | Ground-level barrier geometry |
| I19 Overhead obstacle type | BACKLOG | S2 | Requires player to slide under |
| I20 Combination obstacles | BACKLOG | S2 | Multi-part spatial puzzles |

---

## Epic E7: Ghost Runner (P3)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I21 Ghost recorder (circular buffer) | BACKLOG | S3 | Per-frame position+rotation |
| I22 Ghost replay + rendering | BACKLOG | S3 | Semi-transparent silhouettes colored by run quality |

---

## Epic E8: Adaptive Director (P3)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I23 Weighted probability tracking | READY | S3 | Tracks jump/slide/near-miss frequency |
| I24 Spawn weight adjustment | BACKLOG | S3 | Director → obstacle spawner via callback |

---

## Epic E9: HUD + UI (P2–S3)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I25 Distance score display | BACKLOG | S2 | Live score counter in debug or HUD |
| I26 Combo level indicator | BACKLOG | S2 | Multiplier visible overlay |
| I27 Flow meter visualization | BACKLOG | S3 | Hidden → visual during playtest validation |

---

## Epic E10: Procedural Audio (P3–S4)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I28 FM synth engine base | BACKLOG | S3 | BPM/chord/melody from seed |
| I29 Oscillator-based SFX | BACKLOG | S3 | Footsteps, wind, near-miss events |

---

## Epic E11: Weather + Events (P3–S4)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I30 Rain GPU particle system | BACKLOG | S3 | Visual-only weather effect first |
| I31 Dynamic event triggers | BACKLOG | S4 | Bridge collapse, drone swarm, road split |

---

## Epic E12: Visual Juice (P5)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I32 Particle burst effects | BACKLOG | P5 | Collect / near-miss / death visual juice |
| I33 Screen shake + glow curves | BACKLOG | P5 | All exponential decay, all ease-in-out (200ms) |

---

## Epic E13: Rendering Pipeline (P2–S6)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I34 Shader pipeline assembly | BACKLOG | S? | Vertex → fragment → HDR bloom → tonemap |
| I35 Post-processing passes | BACKLOG | P5 | Bloom, fog, vignette — merge to fit budget |

---

## Epic E14: Optimization (P6)

| Issue | Status | Sprint | Notes |
|-------|--------|--------|-------|
| I40 Dead code removal pass | BACKLOG | P6 | Unlinked shaders, unused structs removed |
| I41 Shader/mesh merging | BACKLOG | P6 | Merge similar GLSL; merged VBOs where possible |
| I42 Release build -Oz + LTO + strip | BACKLOG | P6 | Contest submission targeting ≤1.44 MB |
