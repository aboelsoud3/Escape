# PULSE / ESCAPE — Sprint Proposal

> Consolidated sprint plan mapping all phases, deliverables, acceptance criteria, and gates.

---

## Sprint Summary Table

| Sprint | Phase Duration | Goal | Deliverables | Acceptance Criteria | Status | Gate | Timebox Notes |
|--------|---------------|------|-------------|---------------------|--------|------|--------------|
| **Sprint 0** | P0: 2–3 days | Foundation | CMake builds, repo skeleton, renderer window, CI pipeline stub, logging/profiler hooks, binary-size tracking script | Compiles in <1s; renders colored cube; CI runs on every push including size gate in P6 onwards | Not started | Build & Compile Gate | Hard cap: 3 days. No scope creep — no gameplay code here |
| **Sprint 1** | P1: ~7 days | MFP — "Is moving fun?" | Run/jump/slide/dash states, straight road, one obstacle, death+restart, debug overlay | Kill Test Gate: human says "cool — how does jump feel?"; no menus/music/UI. Movement only | Not started | Kill Test Gate (subjective) | If not fun → throw away, iterate physics. Do NOT proceed to Sprint 2 |
| **Sprint 2** | P2: ~15 days | Core Gameplay Loop | Camera (smooth-follow/banking/FOV/shake), AABB/sphere collision, near-miss detection, scoring + combo multiplier `base × (1.5)^combo_level`, procedural road segments, obstacle spawner, upgrade cards x2, game-over+restart <200ms | Full run loop complete: spawn → run → collect → near-miss → combo → speed up → flow threshold → death → restart; deterministic seed replay across frames | Not started | Functional Loop Gate | 15 timeboxed days. Features must pass size gate (≤10 KB per feature from KB §11) |
| **Sprint 3** | P3: ~15 days | Vertical Slice — full pipeline | Procedural city (roads/buildings/traffic/trees/billboards), GhostRunner recorder/replayer, Adaptive Director weights, Flow Mode trigger+transform, HUD (score/combo/distance/flow/energy), procedural audio engine, weather system, dynamic events, menus (main/pause/game-over) | Every subsystem in §7 of KB functional and integrated end-to-end; same seed = same city every time on any machine | Not started | Full Pipeline Gate | 15 days. Determinism regression test every commit (§11 CI). Cross-platform testing Linux + Windows |
| **Sprint 4** | P4: ~12 days | Contest MVP — depth adds | Double jump, mag dash/magnetic pickup, energy shield, sunset/night weather, full procedural soundtrack library (>5 chords), replay seed export | Feature-complete playable entry; all upgrades functional; audio varies by seed; seed shareable between runs | Not started | Feature-Complete Gate | 12 days. Focus on content depth — polish deferred to Sprint 5 |
| **Sprint 5** | P5: ~13 days | Polish — feel wins competitions | Particles (collect/near-miss/death), screen-shake + glow + tilt curves, UI animation all ease-in-out, Flow Mode intensification, sound design overhaul, camera refinement lookup/FOV/shake tuning | Playtest with ≥5 humans; "one more run" instinct confirmed; every touchpoint has juice. All animations eased/no instant pop-ins | Not started | Juice Gate (subjective) | 13 days. No new features — pure feel/tuning only. Every system gets touch-up pass |
| **Sprint 6** | P6: ~7 days | Optimization — ≤1.44 MB | Binary size tooling, dead code removal, shader merging, mesh/VBO consolidation, release -Oz/+LTO+strip, UPX if permitted | Executable ≤1.44 MB verified; all features playable with no regression; full feature scorecard still scored ≥7/10 per KB §9 Feature Scorecard | Not started | ≤1.44 MB Gate (hard) | 7 days. Measure sizes on every commit. CI must pass size gate |

---

## Cross-Sprint Notes

- **Sprint boundaries are hard timeboxes.** If a sprint overruns, scope the smallest viable subset and cut lower-weight features first (kill test applied).
- **CI gates run through all sprints:** compile → clang-tidy → clang-format → unit tests → binary size → frame-time benchmark (§11 KB).
- **Seed regression testing** starts Sprint 0 and runs every commit through Sprint 6.
