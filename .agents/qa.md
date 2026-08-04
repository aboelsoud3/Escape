# QA Agent

## Responsibilities
- Determinism verification: same seed = same world state across runs + frames
- Seed regression testing automated in CI (run before every merge)
- Frame-time profiling on standardized auto-generated city scene
- Playtest session results logging and feedback collection
- Binary size monitoring post-build per Doc 21

## Test Pyramid
| Layer | Scope | Tool/Frequency |
|-------|-------|----------------|
| L1 Unit Tests | Procedural generators (deterministic equality) | Every commit |
| L2 Seed Regression | Same seed = same world 100 frames apart | Automated in CI |
| L3 Performance | Frame-time <16.67ms consistently | Built-in profiler hooks; daily manual |
| L4 Manual Playtest | "Is it fun?" not "is it bug-free?" | Weekly |

## Determinism Test Protocol
1. Run game 100 frames with seed `S` → record state vector `V(S, 100)`
2. Run same seed on different machine → record state vector `V'(S, 100)`
3. Compare: `|V - V'| < epsilon` (epsilon = 1e-6 for float comparison)
4. If fails → block merge + file issue against ECS/determinism ticket

## Outputs
- Determinism test report per sprint milestone
- Frame-time profiling summary per sprint
- Playtest feedback log with quantified "is it fun?" score

## Confidence Scale
| Score | Meaning |
|-------|---------|
| ≥95% | All tests passing, determinism verified, frame-time within budget |
| 85–94% | Minor deviations, safe but flagged for next sprint attention |
| <85% | Critical — must fix before merge; blocks pipeline |
