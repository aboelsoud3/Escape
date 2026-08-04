## Doc 20 — Code Review Checklist

### Purpose

Every PR must pass this checklist before merging to `develop` or `main`. Review is binary: pass = merge-ready, fail = return for changes. No exceptions based on seniority or PR size.

---

### 1. Architecture Compliance

- [ ] New code lives in the correct module directory (`engine/`, `gameplay/`, `procedural/`, `ui/`). Cross-module boundary rules respected — no Gameplay depending on Renderer, no Procedural depending on Gameplay.
- [ ] New types follow the ECS pattern where applicable (component is a plain data struct; system is a function operating on components).
- [ ] No new global state introduced. All mutable state owned by an explicit object or pool.
- [ ] Dependency graph updated if this PR introduces or modifies inter-module dependencies.

### 2. Functional Correctness

- [ ] New mechanic has unit tests covering at least the happy path and one failure path.
- [ ] Procedural code produces deterministic output for identical seeds (seed regression test passes).
- [ ] State machine transitions are exhaustive — every enum value has its transition documented or tested; no unreachable states.
- [ ] Edge cases considered: 0 delta time, negative speed input, overflow of combo counter, player at world boundary.

### 3. Performance Impact

- [ ] Frame-time budget respected: ≤16.67 ms total per frame (≤8 ms for P3+ with full city).
- [ ] Draw call analysis: instanced rendering used for repeated geometry; draw calls per frame ≤80 target.
- [ ] Memory allocations: no heap allocation inside hot paths (`Update()`, `Render()`, or per-frame code). Pre-allocated pools or stack variables only.
- [ ] Shader instruction count ≤300 lines/lines per fragment pass. Merged with similar passes where possible.
- [ ] No unbounded loops — every loop has a documented maximum iteration count based on known data bounds.

### 4. Binary Size Impact

- [ ] Post-build size check: binary growth ≤10 KB for significant features, ≤5 KB for minor changes. Exceeding these requires justification in the PR description.
- [ ] No unnecessary third-party dependencies added. If new library included, it was evaluated against `KB.md §9` criteria (footprint vs. complexity saved).
- [ ] Unused code removed: dead functions, unreachable branches, commented-out blocks, temporary debug shaders not cleaned up.

### 5. Code Quality

- [ ] Compiles with zero warnings under `-Wall -Wextra -Werror`.
- [ ] Naming conventions followed (PascalCase classes, struct members `snake_case_`, UPPER_CASE constexpr; see Doc 19).
- [ ] Include order follows convention: project header → stdlib → third-party → internal.
- [ ] All new public APIs have doxygen-style comments explaining purpose, parameters, and return value.
- [ ] Line length ≤100 chars (soft) / ≤120 chars (hard exception). No tab characters in the diff.

### 6. Game Design Alignment

- [ ] New feature supports at least one of the five design pillars: Speed, Precision, Momentum, Flow, Mastery, or Replayability.
- [ ] Passes the Kill Test (`KB.md §1`): "If we removed this tomorrow, would players miss it?"
- [ ] Increases skill ceiling rather than replacing skill with grinding or content loading.
- [ ] Does not break the instant-replay loop (no persistent state between runs except ghost data).

### 7. PR Quality

- [ ] PR title follows convention: `module: brief description` e.g., `player: implement slide invulnerability frames`.
- [ ] PR description includes: what was changed, why it was changed, test evidence (screenshots, perf numbers, seed hashes), and any known limitations.
- [ ] All CI checks pass before requesting review (compile + unit tests + seed regression + size gate).
