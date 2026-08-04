# CURRENT_SPRINT

## Sprint Name: Sprint 0 — Foundation
## Phase Gate: Build Compiles
## Timebox: 2–3 days (hard cap)

### Sprint Goal
Create a working repository skeleton that compiles in under one second, renders a simple colored cube on screen, and has initial CI pipeline stub + binary-size tracking.

### Tasks
| Task | Epic | Status | Notes |
|------|------|--------|-------|
| Root CMakeLists.txt | E14 | ⬜ Not Started | Targets: pulse_main; logging; profiler; size_tracker tool |
| src/engine/core/game.cpp/.h | E14 | ⬜ Not Started | Main game class (entry point, update loop) |
| main.cpp | E14 | ⬜ Not Started | Opens raylib window, title "PULSE / ESCAPE" |
| Config raylib static + miniaudio single-header | E14 | ⬜ Not Started | third_party/ directory with dependencies |
| Logging subsystem (lightweight printf-style macros) | E14 | ⬜ Not Started | engine/core/logging.cpp/.h |
| Profiler hooks (timing blocks, frame counter) | E14 | ⬜ Not Started | Simple profiling macros |
| Binary size tracking script | E14 | ⬜ Not Started | tools/binary_size_tracker.sh |
| CI pipeline stub (.github/workflows/ci.yml) | E14 | ⬜ Not Started | Compile check only (tests/size in P6+) |
| .gitignore | E14 | ✅ Done | Already exists in repo |
| Colored cube render test | E14 | ⬜ Not Started | Renders single cube to validate pipeline |

### Acceptance Criteria
- [ ] `cmake` configures without errors
- [ ] `cmake --build .` compiles successfully
- [ ] Binary size tracked post-build (logged in KB)
- [ ] CI runs on every push
- [ ] Compiles in under 1 second

### Definition of Done
Repository skeleton ready for gameplay sprint. No gameplay code exists yet. Move to Sprint 1 only when human validates: "project compiles → renders cube → seed replay works → is it fun?"

### Last Updated: 2026-08-04
