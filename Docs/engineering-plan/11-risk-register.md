# PULSE — RISK REGISTER

> **Status:** Active · Phase 0 (Pre-production) · Last Updated: 2026-08-04

## Legend

| Severity | Color | Meaning |
|----------|-------|---------|
| Critical | 🔴 | Kill-shot — project cannot complete if realized |
| High | 🟠 | Must be addressed before P3 (Vertical Slice) |
| Medium | 🟡 | Address during P3–P4; may cause scope reduction |
| Low | 🟢 | Monitor; manageable with planned contingencies |

---

## Active Risks

### R1 — Executable Exceeds 1.44 MB

- **Severity:** 🔴 Critical · **Probability:** Medium
- **Description:** Combined binary + embedded shaders + procedural code exceeds contest limit.
- **Impact:** Disqualification or forced scope reduction late in P6.
- **Mitigation:** Binary size tracked every commit (file 21). Each module KB budgeted from Sprint 0, not just at P6. Lowest Feature Scorecard items removed first.
- **Trigger:** Post-build ≥1,350,000 bytes (91% cap) → emergency pruning.
- **Contingency:** Strip debug info; apply UPX if allowed; merge all shader files aggressively; remove compiler RTTI/exceptions overhead.

### R2 — MFP Movement Does Not Feel Fun

- **Severity:** 🔴 Critical · **Probability:** Medium during P1 iteration
- **Description:** Core movement (run/jump/slide/dash) fails to engage testers in Minimum Fun Prototype.
- **Impact:** Project foundation reworked; all later-phase investment wasted if not caught early.
- **Mitigation:** Never skip P1 gate. Parameter-driven physics exposed to runtime config for rapid iteration. Rotate minimum 3 unique players per testing session.
- **Trigger:** 2+ testers report "not fun" by end of Sprint 1 Week 4 → revert to P1 iteration only (no scope creep).
- **Contingency:** Redesign gravity curve; increase forward acceleration feel via camera FOV scaling + motion blur instead of raw physics tweaks.

### R3 — Procedural Generation Feels Repetitive / Predictable

- **Severity:** 🟠 High · **Probability:** Medium–High (surfaces P3+)
- **Description:** After 3–5 runs, players perceive identical obstacle patterns and city layouts.
- **Impact:** Replayability drops; engagement plummets.
- **Mitigation:** Multiple seed permutations per run; Adaptive Director adjusts spawn probability weights based on player behavior deltas (not pure randomness). Test ≥5 seeds during every P3 session.
- **Trigger:** Players identify pattern after ≤2 runs → widen Adaptive Director weight ranges immediately.
- **Contingency:** Increase noise function randomness; add seasonal/weather modifiers; introduce deterministic event hooks at milestone distances (every 500m).

### R4 — Shader Complexity Exceeds Frame-Time Budget

- **Severity:** 🟠 High · **Probability:** Medium
- **Description:** Bloom, tone mapping, fog passes exceed ≤16.67ms frame budget during peak gameplay.
- **Impact:** Performance degradation during most important moments (when "juice" is needed most).
- **Mitigation:** Each shader pass ≤300 GLSL instructions. Profile passes separately before combining. Use pre-computed lookup tables for complex math instead of runtime function calls.
- **Trigger:** Any single post-process >4ms average across ≥500 frames → simplify/cull that pass immediately.
- **Contingency:** Reduce bloom to 8-tap; merge tone mapping + fog into one shader; half-resolution FBO for volumetric fog.

### R5 — Deterministic Generation Breaks Across Architectures

- **Severity:** 🟠 High · **Probability:** Medium early, Low after fix
- **Description:** Same seed produces different world states between machines or Clang versions. Ghost replay diverges because float-point results differ across platforms.
- **Impact:** Ghost Runner broken; competition integrity destroyed at contest.
- **Mitigation:** Fixed-timestep physics substep (not delta-time floating); integer-snap collision distances (no raw float comparisons for equality); test cross-platform early and often during P2.
- **Trigger:** Two machines on same seed diverge after ≥100 frames → lock all random calls to deterministic RNG only; add seed-regression test in CI (file 14).
- **Contingency:** Replace floating-point collision thresholds with epsilon-based comparison (+fixed integer snap distance).

### R6 — Procedural Audio Sounds Repetitive / Poor Quality

- **Severity:** 🟡 Medium · **Probability:** High during P3
- **Description:** FM synth engine produces audio that sounds repetitive or low-quality, undermining immersion.
- **Impact:** Audio becomes liability rather than asset; players notice "loops too much."
- **Mitigation:** ≤8 concurrent oscillator voices; wide chord progression seed pool (≥15 chords); note timing seeded separately from pitch so rhythm pattern changes per run. Design test: 5 runs must each sound musically distinct.
- **Trigger:** Testers identify melody loop within first minute of any run → fix in P4 audio expansion pass.
- **Contingency:** Reduce voice count to 6; add random micro-variations (±5 cents pitch drift, subtle timing jitter ≤2ms) that remain imperceptible but break mechanical pattern perception.

### R7 — Feature Creep Exceeds Contest Scope

- **Severity:** 🟠 High · **Probability:** Medium during P3–P4
- **Description:** Accumulation of small features pushes scope beyond what is feasible within contest timeline and 1.44 MB budget simultaneously.
- **Impact:** Game incomplete, unpolished, or exceeds size limit at submission.
- **Mitigation:** Every new feature scored using Feature Scorecard (KB.md §9): Replayability ≥3 / Originality ≥3 / Cost ≤3 required to join game. Below 7/10 weighted score → defer as non-contest content. Apply Kill Test for every new addition: "If we removed this tomorrow, would players miss it?"
- **Trigger:** >5 mid-P3 scope-additions proposed simultaneously → freeze feature queue until current backlog cleared.
- **Contingency:** Revert to P4 MVP baseline; strip all features scoring <7/10 retroactively.

### R8 — Cross-Platform Compatibility Issues

- **Severity:** 🟡 Medium · **Probability:** Low–Medium
- **Description:** Game works on developer's Linux machine but fails or performs poorly on Windows (or vice versa) during contest submission testing.
- **Impact:** Contest entry unusable on one required target platform; potential disqualification if both platforms expected by rules.
- **Mitigation:** Cross-platform testing starting Sprint 2; identical build configuration scripts for Clang/MSVC with CMake; test raylib behavior parity (mouse sensitivity, key scancodes) early.
- **Trigger:** Any gameplay-incompatible behavior on non-primary platform during P3 testing → block progress until fixed.
- **Contingency:** Lock target to one primary OS for first contest entry; document second-platform work as post-contest follow-up.

---

## Inactive / Resolved Risks

### R9 — raylib Binary Size Too Large (RESOLVED)

- **Status:** ✗ Inactive
- **Resolution:** raylib static link adds ~200 KB worst case; still within 700 KB executable budget after removing most default fonts/shapes from export. Validated acceptable.

### R10 — No Storage of Any Assets Needed (RESOLVED)

- **Status:** ✗ Inactive
- **Resolution:** Design confirms procedural generation for all content. Zero assets = zero asset management risk. Addressed in technical philosophy (§ 3 of KB.md).
