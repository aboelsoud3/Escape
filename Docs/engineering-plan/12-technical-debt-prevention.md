# PULSE — TECHNICAL DEBT PREVENTION STRATEGY

> **Purpose:** Proactively prevent technical debt accumulation throughout project lifecycle. Technical debt is treated as a financial liability: every convenience that defers clean design incurs compound interest in future rework cost.

---

## 1. Prevention Philosophy

Technical debt for this project falls into three categories:

| Category | Debt Type | Prevention Method |
|----------|-----------|-------------------|
| **Scope Debt** | Features added without justification, bloating binary and complexity | Feature Scorecard gating (§9 in KB.md) + Kill Test enforcement |
| **Design Debt** | Quick-and-dirty architectures that block future flexibility | Code review against SOLID principles; module boundary adherence at merge gate |
| **Testing Debt** | Untested code paths accumulating unknown failure modes | Every feature ships with related test (KB.md §11: testing strategy) |

---

## 2. Prevention Rules (Non-Negotiable)

| Rule | Description | Enforcement |
|------|-------------|------------|
| **Rule 1: No Magic Numbers** | All numeric constants (> single-use inline literals) must be named `constexpr` or pulled into runtime-config table. | clang-tidy readability-identifier-naming check + review gate |
| **Rule 2: No Global Mutable State** | No file-scope statics, no raw globals modified at runtime. All mutable state owned by explicit subsystem instance. | Code review; compiler warning flags `-Wshadow -Wold-style-cast` added to catch implicit globals |
| **Rule 3: Single Responsibility Barrier** | Each module does one thing; cross-module communication via defined callback/event only — never direct data access. | Architecture review during epic planning (KB.md §5 module boundaries) |
| **Rule 4: Determinism First** | No `std::unordered_map` for game-critical paths (iteration order undefined across runs); use ordered containers or fixed-array lookups. | Seed regression test validates same output on all platforms (KB.md §14 risk R5) |
| **Rule 5: Binary Size Consciousness from Day One** | Never write code then "optimize size later" — every addition weighs KB cost against value. Post-build binary digest always available. | Binary monitoring script (file 21) fails build if executable jumps >50 KB unexplained |
| **Rule 6: No Unbounded Data Structures at Runtime** | No `std::vector` growing without upper bound; all game object pools pre-allocated with fixed max capacity (e.g., ECS entities cap at 2048). | Static assertion on container construction + review gate for any dynamic allocation in hot path |
| **Rule 7: Shaders ≤300 Lines** | No GLSL file exceeds 300 lines; larger effects require splitting into multiple passes. | File-size pre-commit hook rejects shaders over threshold |

---

## 3. Debt Tracking Mechanism

### When Technical Debt Is Inevitable — Record It

If a shortcut must be taken (e.g., contest deadline pressure), it is recorded as a tracked debt item:

1. **File:** `TODO/FIXME.md` at project root with structured fields
2. **Fields per entry:**
   - Issue ID reference → T-###
   - Description of compromise
   - Reason accepted (deadline/complexity/risk trade-off)
   - Estimated rework cost in developer-hours
   - Target resolution milestone (which phase to fix)

### Debt Review Cadence

| When | Action | Participants |
|------|--------|-------------|
| Weekly during P3–P6 | Review TODO/FIXME.md; promote any debt item approaching deadline to next sprint's priority queue | Lead Architect + Engineering Manager |
| Every milestone gate | Present: total tracked debt hours remaining vs. time until next milestone | Technical Lead reports to Creative Director (human) |
| During CI pipeline (P6+) | Binary size trend analyzed; if growth >10 KB for 3 consecutive commits without new gameplay features → flag for investigation | Automated pipeline notification |

---

## 4. Prevention by Phase

### P0–P1 (Foundation / MFP): Prevent Scope Debt
- **Gate:** Feature Scorecard applied to every proposed mechanic. No feature enters backlog scoring <7/10.
- **Mechanism:** Every new mechanic proposal requires written justification citing which design pillar(s) it advances. Creative Director sign-off required.

### P2–P3 (Core Gameplay / Vertical Slice): Prevent Design Debt
- **Gate:** Code review against SOLID + module-boundary rules before any merge to `develop`.
- **Mechanism:** PR must list which modules were changed and why; if cross-boundary changes exist, reviewer validates architectural justification.

### P4–P6 (MVP → Gold): Prevent Testing Debt
- **Gate:** No feature ships without related test or manual playtest documented in milestone review log.
- **Mechanism:** Definition of Done checklist (KB.md §12) must be completed per issue; Engineering Manager verifies before approving merge.

---

## 5. Anti-Patterns — Never Do These

| Anti-Pattern | Why It Creates Debt | Correct Approach |
|-------------|-------------------|-----------------|
| Copy-paste code between modules | Becomes inconsistent when one copy is fixed but others aren't | Extract shared into well-scoped function/struct; document interface contract |
| Inline complex math directly in game logic | Future optimization (pre-compute, SSE vectorization) requires finding scattered code | Separate computationally intensive math into modular utility library with clear API |
| Write shaders procedurally but abandon them mid-development | Dead shader code increases binary size without any benefit — direct violation of contest constraint | Merge similar shaders (KB.md §9 optimization checklist); remove entirely if no production use |
| Use `float` for collision proximity logic | Float rounding differences break determinism across platforms | Epsilon-based comparison with fixed snap distance; integer grid snap for threshold comparisons |
| Add features without removing something of equal size | Binary size increases unbounded; contest disqualification risk | Each new feature requires a "tax" — remove equivalent KB elsewhere or cut lowest-value existing feature |
