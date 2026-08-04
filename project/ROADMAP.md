# ROADMAP

> Project roadmap from bootstrap through all phases. Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

## Phase Overview

| Phase | Name | Status | Target Work |
|-------|------|--------|-------------|
| P0 | Foundation | ⬜ Not Started | CMake, raylib window, CI stub, logging/profiler hooks, binary-size tracking |
| P1–P2 | MFP — Minimum Fun Prototype | ⬜ Not Started | Run/jump/slide/dash; camera system; collision physics; scoring/combo; road generator |
| P3 | Ghost Runner + Adaptive Director | ⬜ Not Started | Ghost recorder/replay obstacle spawner; weighted probability tracking |
| P4–P5 | Content + Juice | ⬜ Not Started | Physics/visual polish layer 2; particle effects; screen shake; glow curves |

## Major Milestones

| Milestone | Gate Criteria | Target Spring |
|-----------|--------------|---------------|
| Sprint 0 complete | Repository compiles <1s, renders cube, binary tracked | Foundation |
| MFP Kill Test | "Is movement fun?" validated by human | Phase 1–2 |
| Ghost + Director ready | Replay matches seed; difficulty adapts | Phase 3 |
| Full visual juice | All effects active under 60 FPS budget | Phase 5 |
| Final submit | ≤1.44 MB binary, all gates passed | Phase 6+ |

## Sprint Sequence

```
P0 (Bootstrap/Sprint 0) → P1–P2 (MFP/Sprint 1–2) → P3/Sprints 3–4 → P5/Sprint 5 → … → Submission
```
