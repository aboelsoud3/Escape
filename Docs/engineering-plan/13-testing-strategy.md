# PULSE — TESTING STRATEGY

> **Purpose:** Ensure every feature ships verified and correct. Testing is layered: unit tests for deterministic code, seed regression for procedural generation, performance benchmarks for frame-time budgets, and manual playtesting for "fun" validation.

---

## 1. Testing Pyramid

```
          ╭──────────────╮
         │    Playtest   │  ← subjective "is it fun?" feedback (weekly)
        ╱    sessions     ╲
      ──┼──────────────────┼──
     ╱   Seed Regression   │  ← same seed = same world, deterministic (automated CI)
    ╱       tests          ╲
  ──┼──────────────────────┼──
 ╱        Unit Tests       │  ← procedural generators, scoring math, physics formulas (automated CI)
╱                            ╲
──────────────────────────────
     Code Under Test
```

---

## 2. Test Layers

### Layer 1: Unit Tests (Automated — every commit)

| What to Test | Where | Framework | Frequency |
|-------------|-------|-----------|-----------|
| Procedural road segment generation (Catmull-Rom math) | `procedural/roadgen/` | GoogleTest or custom minimal harness | Every commit |
| Building footprint noise function output | `procedural/buildinggen/` | Same | Every commit |
| Combinatorial scoring formula correctness | `gameplay/scoring/` | Same | Every commit + on every scoring rule change |
| Near-miss threshold distance math | `gameplay/scoring/nearmiss_test.cpp` | Same | Every near-miss behavior update |
| Player physics integration (gravity arc, velocity Euler step) | `gameplay/player/` | Same | Every player mechanic change |
| State machine transitions | `gameplay/player/state_machine_test.cpp` | Same | Any state addition/modification |

**Framework choice:** GoogleTest is included via FetchContent from CMake. For P0–P1, use a minimal homegrown harness (fewer dependencies = smaller compile footprint) until confirmed binary size budget accommodates test framework in dev builds only (never in release).

### Layer 2: Seed Regression Tests (Automated — every commit to `develop`)

| Test | Description | Failure Condition |
|------|-------------|------------------|
| Full world seed repeatability | Run P3-level city generation from seed `42` for 10,000 frames → compare against baseline. Same position of every building, road segment, vehicle. | Any divergence in entity count or transform data. |
| Ghost replay parity | Record ghost data for a reference run with seed `7`; replay same seed on another machine — positions match within floating-point epsilon (≤1 cm) at every recorded frame. | Position delta >1cm on any frame during playback compared to original recording. |
| Procedural audio consistency | Generate the first 30 seconds of audio from a seed → PCM comparison against reference. | Exact byte-match failure (deterministic oscillator output must match). |

**Implementation approach:** Save `std::vector<FrameSnapshot>` (position + rotation per relevant entity) for each frame; serialize to binary file post-test-run. Diff two hex dumps or compute SHA-256 hash comparison of snapshot buffers. If hashes differ → test fails and CI blocks merge.

### Layer 3: Performance Benchmark (Automated — every push to P3+)

| What to Measure | Budget | Measurement Method |
|-----------------|--------|-------------------|
| Total frame time (CPU + GPU) | ≤16.67 ms average across ≥500 frames in-game profiler hooks; record via `std::chrono::high_resolution_clock` | Average over 500 continuous frames of P3-level city content with dense population |
| Draw call count | ≤80 calls per frame via instanced rendering (road, buildings, traffic as batches) | CMake-integrated performance tester app running in release mode logs call count per frame via raylib GetDrawCalls() or OpenGL glDrawArraysInstanced invocation tracing |
| Frame-time stddev (jitter) | σ < 4 ms at all times (high variance = perceived stutter) | Standard deviation of measured frame times computed in-profile; >4 sigma triggers warning to developer during session |
| Shader instruction count per pass | ≤300 instructions per GLSL fragment/vertex shader | Pre-processing build step counts tokens in compiled `.glsl` files via post-compile script parsing glslc output |

### Layer 4: Manual Playtest Sessions (Weekly / gated)

| Session Type | Participants | Focus | Success Criteria |
|-------------|-------------|-------|-----------------|
| MFP Playtest | ≥3 independent testers per session (minimum two complete strangers; one known gamer + one non-gamer per rotation) | Core movement fun factor: "Does moving feel rewarding?" | ≥80% of players continue playing beyond 5 minutes without prompting |
| Gameplay Playtest | Same pool, post-P2 completion | Scoring clarity; near-miss feedback quality; combo retention comprehension | Players identify that near-clear misses give bonus points (without UI tutorial text explaining it explicitly) within their first 10-minute session |
| Flow Mode Discovery Playtest | ≥5 players across multiple sessions testing ≥3 different seeds | Whether Flow Mode triggers organically without any tutorial/hint shown to player about its existence or mechanics | At least one player in ≥60% of test sessions reaches Flow Mode by trial and error alone (no hints given) |
| Contest Readiness Playtest | Judges-substitute panel ≥3 people familiar with prototype/game jam scene | Holistic assessment: fun, polish, replayability at P4/MVP state | Panel reports "I would submit this to a 1.44 MB contest" unanimously or by ≥2 of 3 majority consensus |

---

## 3. Test Automation in CI (file 15 details pipeline; file 14 defines testing strategy)

```
Push to any branch / PR:
    ↓
[Compile Debug] + [Compile Release]
    ↓
[Unit Tests execute] ← fails immediately on test failure
    ↓
[Seed Regression against baseline for procedural modules only] ← blocks merge if mismatch (only run on develop pushes where applicable)
    ↓
[Performance Benchmark Scene Runs (500 frames)] ← warn-only (don't block merge; report in PR comment)
    ↓
[Binary Size Check] ← fails if executable exceeds 700 KB budget without annotated justification commit diff explaining reason for size spike
```

---

## 4. Definition of Test Done Per Feature

Before a feature issue is considered complete:
- Unit tests written for the new/changed math and logic (Layer 1)
- Seed determinism confirmed across ≥2 machines in CI (Layer 2, where applicable to procedural/gameplay systems)
- Manual playtest confirms mechanic works as designed at expected performance budget (Layer 4)
- Performance metrics within acceptable ranges if system interacts with rendering/physics (Layer 3 — automated check passes or has documented exception approved by Technical Lead)

---

## 5. Test Environment Setup

| Component | Development | CI Runner |
|-----------|------------|-----------|
| OS | Linux (primary dev machine); Windows cross-compiled on GitHub Actions runner | Ubuntu 22.04 LTS x64 runner with raylib, GL, miniaudio included via CMake FetchContent or apt |
| Compiler | Clang ≥ 14 with `-Wall -Wextra -Werror -std=c++20` | Same flags; verify identical behavior across `ubuntu-latest` default toolchain |
| GPU | NVIDIA RTX 30-series or equivalent for local profiling | GitHub Actions runner has headless libGL via `libgl1-mesa-dev`; test runs without display in CI use virtual framebuffer (`xvfb-run`) |

---

## 6. Regression Traceability

All tests are mapped to issues:
- Every issue (file 06) includes a "Related Tests" field enumerating which unit/seeding/perf tests cover it.
- Test code lives alongside game code in same subdirectory (`*_test.cpp` files next to implementation) for developer discoverability.
- Feature completeness requires test coverage evidence (not line-coverage percentage; just proof the feature's behavior is exercised).
