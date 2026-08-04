# NEXT TASK

## Current Task
Sprint 0 — Foundation: Create repository skeleton (root CMakeLists + config/ src/ tests/ shaders/) with raylib window main.cpp and logging/profiler hooks.

### Files to Create/Modify
| File | Action | Notes |
|------|--------|-------|
| `src/main.cpp` | CREATE | Opens raylib window "PULSE / ESCAPE", renders empty frame |
| `CMakeLists.txt` (root) | CREATE | Targets: pulse.exe, logging, profiler, size_tracker tool |
| `src/engine/core/game.cpp/.h` | CREATE | Main game class — init/update/render/shutdown lifecycle |
| `src/engine/core/logging.cpp/.h` | CREATE | Lightweight logging macros (printf-style) |
| `src/engine/core/profiler.cpp/.h` | CREATE | Timing blocks, frame counter overlay hooks |
| `third_party/raylib/` | PLACEHOLDER | Single header/source from raylib download docs |
| `third_party/miniaudio/` | PLACEHOLDER | Single-header from miniaudio release docs |
| `tools/binary_size_tracker.sh` | CREATE | Runs binary-size monitoring post-build |
| `.github/workflows/ci.yml` | CREATE | GitHub Actions compile check stub |

### Acceptance Criteria
- [ ] Repository compiles with zero warnings (`-Wall -Wextra -Werror`)
- [ ] Opens window titled "PULSE / ESCAPE"
- [ ] Renders single colored cube (even if untextured)
- [ ] Binary size tracked and logged: `tools/binary_size_tracker.sh` post-build
- [ ] CI config present to run on next push

### Dependencies
None — this is the first implementation sprint. Pure scaffolding work only.

### Sprint Alignment
Sprint 0 per Doc 7 sprint proposal + KB §13 immediate actions. See project/CURRENT_SPRINT.md for full sprint scope.

### Last Updated: 2026-08-04
