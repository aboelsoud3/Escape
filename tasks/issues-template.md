# Issues Template

> Standard issue format — every backlog item in BACKLOG.md references an issue file from this directory. Each issue is atomic: one task, clear acceptance criteria, single Epic parent.

## Issue Format

```markdown
# E-XXX: [Issue Title]

| Field | Value |
|-------|-------|
| **Epic** | E1–E14 (specify which) |
| **Phase** | P0–P6 |
| **Sprint** | S0–S6 |
| **Status** | To Do / In Progress |
| **Estimate** | [timebox] |
| **Size Impact** | ≈ [KB estimate] |

## Description
[One paragraph describing what this issue delivers.]

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Technical Notes
[Architecture alignment, files to modify, constraints from ADRs, etc.]

## Dependencies
[List other issues this blocks or depends on, if any.]

## Risks
[Any known risks specific to this issue.]

## Last Updated: YYYY-MM-DD
```

## Current Sprint Sprint 0: I-S0-01 Foundation Setup
| ID | Epic | Title | Status | Sprint | Size Est. | Description |
|----|------|-------|--------|--------|-----------|-------------|
| I-S0-01 | E14 | Root CMakeLists.txt | ⬜ To Do | S0 | ≈ 2 KB | Targets: pulse.exe, logging, profiler, size_tracker tool per Doc 7 sprint proposal + Doc 5 epic breakdown. |
| I-S0-02 | E14 | main.cpp Raylib Window | ⬜ To Do | S0 | ≈ 1 KB | Opens raylib window titled "PULSE / ESCAPE", renders single colored cube to validate render pipeline. No gameplay code. |
| I-S0-03 | E14 | Logging subsystem | ⬜ To Do | S0 | ≈ 1 KB | Lightweight printf-style logging macro system per Doc 8 folder structure (engine/core/logging). |
| I-S0-04 | E14 | Profiler hooks | ⬜ To Do | S0 | ≈ 1 KB | Timing blocks + frame counter overlay hooks for debugging/optimization later. Added per Doc 1 item. |
| I-S0-05 | E14 | Binary size tracker script | ⬜ To Do | S0 | ≈ 2 KB | tools/binary_size_tracker.sh runs post-build, tracks executable size against budget per Doc 21. |
| I-S0-06 | E14 | CI pipeline stub | ⬜ To Do | S0 | ≈ 3 KB | .github/workflows/ci.yml compiles check only — full test/size gates added in P6+. Per Doc 14 & Doc 8 repo structure. |
| I-S0-07 | E14 | third_party/ setup | ⬜ To Do | S0 | ≈ 1 KB | Raylib single-header + miniaudio single-header downloaded and verified per Doc 3 tech stack constraints. |
