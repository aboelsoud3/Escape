# PULSE / ESCAPE — Project Roadmap

> Phased delivery plan mapping Vision → Gold for the 1.44 MB game dev contest.

---

## Phase Roadmap

| Phase | Name | Duration | Gate | Deliverable |
|-------|------|----------|------|------------|
| **P0** | Project Foundation | 2–3 days | Build Compiles | Working repo skeleton, CMake, renderer window, binary-size tracking CI |
| **P1** | Minimum Fun Prototype | ~1 week | Kill Test Gate | Run/jump/slide/dash player on straight road with one obstacle; "is it fun?" validation |
| **P2** | Core Gameplay | ~3 weeks | Functional Loop | Camera, physics/collision, scoring + combo, procedural roads, obstacles, upgrade cards, game-over loop |
| **P3** | Vertical Slice | ~3 weeks | Full Pipeline Gate | End-to-end: procedural city, ghosts, Adaptive Director, Flow Mode, HUD, audio, weather, dynamic events, menus |
| **P4** | Contest MVP | ~2–3 weeks | Feature-Complete Gate | Upgrades (double jump, mag dash, shield), extended weather, full soundtrack library, replay seed export |
| **P5** | Polish | ~2–3 weeks | Juice Gate | Particles, screen shake, UI animations, flow intensification, camera refinement, sound design overhaul |
| **P6** | Optimization | ~1–2 weeks | ≤ 1.44 MB Gate | Dead-code removal, shader/mesh merging, LTO/stripping, UPX if permitted; binary ≤ 1.44 MB |

---

## Notes on Duration

- **Total estimated duration:** ~12–16 weeks (≈3–4 months)
- Phases P0–P1 are hard-lower-bound: do not compress. Movement fun must be validated before investing in content.
- Phase durations overlap slightly; a determined solo dev with clear scope discipline can achieve ~12 weeks tight.
- Optimization (P6) is strictly last — never optimize before game is playable.
