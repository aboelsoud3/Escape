# PULSE / ESCAPE — WORKING MEMORY

> Rolling RAM: intentionally short. One stop to resume work quickly. Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

## What We're Doing Now

**Sprint 0 — Foundation:** Repository skeleton, CMake, raylib window, CI stub, logging/profiler hooks, binary-size tracking. No gameplay code allowed in Sprint 0.

**Designated phase:** Phase 0 (P0) → Next: Phase 1 (MFP). Every session reads this first and updates it last.

## What Was Completed Recently

- **KB.md compiled** — Single source of truth with all design/tech/architecture decisions
- **25 engineering plan docs created** — Roadmap through Sprint 6, including epic/task breakdowns, CI pipeline, build targets, size budgets, review checklists
- **ADOS memory system bootstrapped** — Four-layer Agent + Project Memory framework
- **Architecture defined** — Engine → Gameplay → Procedural → UI layers per KB §5
- **Coding standards set** — Doc 19 conventions, naming rules, K&R braces, line lengths

## What Is in Progress

Sprint backlog defined but not yet started. Next actionable item: create repo skeleton with CMakeLists.txt + raylib window main.cpp (Sprint 0 per Doc 7 sprint proposal).

## What Is Blocked

Nothing blocked — all planning artifacts exist and align. The only blocker is that Sprint 0 tasks have not been assigned to an agent session yet.

## Active Assumptions

- **raylib via single-header mode** — minimal binary footprint; windowing, input, OpenGL access all from one header
- **miniaudio single-header** — synthesizer-based audio, zero stored samples
- **C++20 deterministic simulation** — fixed timestep physics substep, same seed = same world every time
- **OpenGL 3.1 Core + GLSL 330** — no modern OpenGL extensions needed beyond core profile

## Recent Decisions (chronological)

| Date | Decision | Impact |
|------|----------|--------|
| 2026-08-04 | ADOS Bootstrap completed | All planning docs and memory system now live in-repo; game code still not started |
| 2026-08-04 (earlier) | CMake preferred over Make/SPIRAL | Cross-platform standard tooling for raylib/miniaudio vendoring |
| 2026-08-04 (earlier) | Bitmap font atlas > TTF | Saves ~hundreds of KB in binary; all HUD/digits rendered by GLSL |

## What Should the Next Agent Do First

1. Start Sprint 0 session: read WORKING_MEMORY.md → PROJECT_STATUS.md → Doc 7 sprint proposal
2. Create `pulse/` repo skeleton (root CMakeLists + config/ src/ tests/ shaders/)
3. Build a simple main.cpp that opens a raylib window with title "PULSE / ESCAPE"
4. Verify: compiles <1s, binary size tracked, CI runs on first push

## Current Context State Machine Status

```
State: BOOTSTRAP COMPLETE
Next State: SPRINT_0_ACTIVE
Previous States: planning, documentation, memory-system-bootstrap
Confidence: 95% — all foundational artifacts exist and are consistent with KB §13 guidance
Known issues: None
Risks to watch: Sprint creep (adding gameplay code in Sprint 0 = violation of Doc 7 constraints)
```
