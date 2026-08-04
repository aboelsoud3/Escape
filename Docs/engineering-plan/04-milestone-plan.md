# PULSE / ESCAPE — Milestone Plan (§10 of KB)

---

## Phase 0: Project Foundation (2–3 days)

| #  | Name                      | Est. Time     | Acceptance Criteria / Gates |
|----|---------------------------|---------------|-----------------------------|
| M0-1 | Repo Skeleton & Branch Model | 4 hrs       | `main`, `develop`, feature branch convention; architecture folders present (§5 KB) |
| M0-2 | CMake Build System         | 4 hrs        | Compiles `pulse` target; root + module CMakeList.txt correct |
| M0-3 | Renderer Window Startup    | 4 hrs        | Opens window, renders single colored cube/frame, closes cleanly |
| M0-4 | CI Pipeline Stub           | 4 hrs        | Compile check on push; binary-size post-build tracking |
| M0-5 | Logging + Profiler Hooks   | 2 hrs        | Minimal logging macro; frame counter debug overlay |

**KB Cross-Reference:** Folder structure §5, CI rules §11.

---

## Phase 1: Minimum Fun Prototype (~1 week)

| #  | Name                          | Est. Time     | Acceptance Criteria / Gates |
|----|-------------------------------|---------------|-----------------------------|
| M1-1 | Player run state              | Days 1–2    | Forward velocity, momentum-based movement, delta-time integration |
| M1-2 | Jump mechanic                 | Day 2        | Gravity arc, once-per-grounded, visual ground-contact feedback |
| M1-3 | Slide mechanic                | Day 3        | Duck under obstacle, ~0.5–1s duration, hitbox reduction |
| M1-4 | Dash mechanic                 | Day 3        | Speed burst + brief invulnerability, energy cost, cooldown |
| M1-5 | Single road corridor          | Day 4        | Fixed straight segment sufficient for testing all states |
| M1-6 | One obstacle (static cube)   | Day 4        | Sphere-AABB collision; death triggers restart instantly |
| M1-7 | Kill Test Gate                | Day 5        | "Is the movement fun?" — subjective validation passed |

**KB Cross-Reference:** State machine pattern §7.1, physics §7.3, kill test §1.

---

## Phase 2: Core Gameplay (~3 weeks)

| #  | Name                            | Est. Time     | Acceptance Criteria / Gates |
|----|----------------------------------|---------------|-----------------------------|
| M2-1 | Smooth-follow camera system      | Days 6–9    | Lambda-based lag, banking roll, look-ahead offset proportional to speed |
| M2-2 | Camera FOV scaling + shake       | Day 9        | FOV 60°→75° by speed; near-miss exponential decay shake |
| M2-3 | AABB/sphere collision subsystem  | Days 10–12  | Player↔ground, player↔obstacles (20–30% smaller hitboxes than visuals), player↔collectibles |
| M2-4 | Near-miss detection              | Day 12       | Distance-from-obstacle tracking; threshold = radius + margin; triggers particles/audio |
| M2-5 | Scoring subsystem                | Days 13–15  | Distance score, near-miss with combo multiplier: `base × (1.5)^(combo‑level)` |
| M2-6 | Combo system                     | Day 15       | Exponential multiplier; resets on death or safe gap >2s (KB §7.4) |
| M2-7 | Procedural road segments         | Days 16–18  | Straight → curve → intersection via Catmull-Rom splines |
| M2-8 | Obstacle spawner                 | Day 18       | Barrier, vehicle, overhead, gap types spawning on generated roads |
| M2-9 | Upgrade selection cards (first 2) | Day 20      | Double jump + magnetic pickup shown every ~60s of survival |
| M2-10| Game-over + instant restart      | Day 21       | Death screen with score; <200ms restart delay, same seed replay |

**KB Cross-Reference:** Scoring math §7.4, camera §7.2, collision §7.3.

---

## Phase 3: Vertical Slice (~3 weeks)

| #  | Name                                | Est. Time     | Acceptance Criteria / Gates |
|----|--------------------------------------|---------------|-----------------------------|
| M3-1 | Procedural city (roads+buildings+traffic+trees+billboards) | Days 22–27 | Every element deterministic from seed; §8 pipeline complete |
| M3-2 | Ghost runner recorder/replayer       | Days 25–28  | Per-frame `(position, rotation, action_state)` in circular buffer; <2 KB per run save |
| M3-3 | Adaptive Director                    | Days 27–30  | Weighted-probability tables track jump/slide/near-miss frequency → adjust weights (KB §7.6) |
| M3-4 | Flow Mode system                     | Days 29–31  | ~30 consecutive perfect actions trigger; visual + music transformation active |
| M3-5 | HUD                                | Days 30–32  | Score, combo level, distance, flow meter, energy bar (live, smooth-updating) |
| M3-6 | Procedural audio engine              | Days 31–34  | FM synth music + oscillator SFX (footsteps, wind, near-miss whooshes) |
| M3-7 | Weather system                       | Days 33–35  | Rain particles via GPU compute; fog/storm density driven by director; affects gameplay |
| M3-8 | Dynamic events system                | Days 34–36  | Bridge collapse, drone swarm, road split, power failure events triggered selectively |
| M3-9 | Menus (main/pause/game-over)         | Days 35–37  | Seed input on title; pause → resume/restart/quit; game-over score summary + ghost comparison |

**KB Cross-Reference:** All systems §7, procedural §8.

---

## Phase 4: Contest MVP (~2–3 weeks)

| #  | Name                                    | Est. Time     | Acceptance Criteria / Gates |
|----|------------------------------------------|---------------|-----------------------------|
| M4-1 | Double jump upgrade                      | Days 38–40  | Vertical reach expansion; gap negotiation validated |
| M4-2 | Mag Dash + magnetic pickup upgrade       | Days 40–42  | Attracts collectibles within extended radius |
| M4-3 | Energy Shield (one-hit block)            | Days 42–44  | Visual activation indicator; cooldown between uses |
| M4-4 | Extended weather (sunset/night cycle)    | Days 43–46  | Sky gradient shift + fog density transition |
| M4-5 | Full procedural soundtrack library       | Days 44–49  | >5 chord progressions, melody templates, rhythmic patterns from seed |
| M4-6 | Replay seed export (game-over screen)    | Day 47       | Seed string displayed; pasteable for share/replay |

**KB Cross-Reference:** §10 Phase 4 deliverables.

---

## Phase 5: Polish (~2–3 weeks)

| #  | Name                                     | Est. Time     | Acceptance Criteria / Gates |
|----|-------------------------------------------|---------------|-----------------------------|
| M5-1 | Juice: particles on collect/near-miss/death | Days 48–50 | All triggers have particle bursts; performance budget met |
| M5-2 | Screen shake + glow + camera tilt tuning | Days 50–53  | Curves tuned per event type; exponential decay matches feel targets |
| M5-3 | UI animations (all ease/in-out)           | Days 51–54  | No instant pop-ins; consistent easing duration (~200ms) |
| M5-4 | Flow Mode visual/music intensification    | Day 54       | Verified in playtest: visible/audible transformation occurs naturally without tutorial |
| M5-5 | Sound design overhaul                      | Days 53–56  | Heartbeat near low combo; music layers during flow; victory/defeat transitions |
| M5-6 | Camera refinement (look-ahead/FOV/shake)  | Day 56       | Playtest validation: speed illusion strong without excessive actual speed |

**KB Cross-Reference:** §10 Phase 5 deliverables.

---

## Phase 6: Optimization (~1–2 weeks)

| #  | Name                                     | Est. Time     | Acceptance Criteria / Gates |
|----|-------------------------------------------|---------------|-----------------------------|
| M6-1 | Binary size measurement tool              | Day 2        | Post-build script tracks KB; CI fails if executable >700 KB without justification |
| M6-2 | Dead-code + unused shader removal         | Days 3–5    | Static analysis + manual review clears reachability report |
| M6-3 | Shader pass merging                       | Days 5–8    | Duplicate passes combined; max ≤300 instruction/line limit per pass maintained |
| M6-4 | Mesh/VBO instance consolidation           | Days 7–9    | Draw calls reduced ≥40% from P5 baseline |
| M6-5 | Release compilation (-Oz/-Os + LTO + strip) | Day 10   | Binary size ≤ 1.44 MB total |
| M6-6 | UPX compression (if contest allows)       | Day 11       | Final binary verified against contest rules; compressed output tested playably |

**Acceptance Gate:** Executable ≤ 1.44 MB **and** full feature set playable without regression.

**KB Cross-Reference:** Size budget §9, optimization checklist §9.
