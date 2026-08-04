# Session End Protocol

> Algorithm every session executes before ending. Never skip — this is how project memory stays fresh across sessions.

## Phases

### Phase 1 — Implementation Verification

Before documenting anything:

1. Verify code compiles (zero warnings: `-Wall -Wextra -Werror`)
2. Run test suite — if no tests exist, create skeleton tests for new functionality
3. Manual verification against NEXT_TASK.md acceptance criteria checklist

### Phase 2 — Self Review (Architect Lens)

Before returning code, answer honestly:

```text
## Self-Critique
1. What bugs could this introduce? [be specific, not generic]
2. Does this violate any existing ADR in DECISION_LOG.md? Explain why or confirm no violation.
3. Performance impact: frame-time cost of changes
4. Binary size impact: approximate KB added/removed
5. Future maintenance: would another engineer understand this in 6 months?
6. Architecture alignment: does it match the approved plan?

[FIX any issues found — do NOT return broken code to say "I'll fix later"]
```

If confidence < 80% on any question → rework that area before proceeding.

### Phase 3 — Update Documentation

Update ALL of these that are relevant (create if they don't exist yet):

| Document | When to Update |
|----------|---------------|
| `project/WORKING_MEMORY.md` | Always — rolling summary must reflect latest state |
| `project/PROJECT_STATUS.md` | When phase/sprint/task completion changes |
| `project/CURRENT_SPRINT.md` | When any sprint task moves from ⬜ to ✔ or new tasks discovered |
| `project/DECISION_LOG.md` | When a new architectural decision is made (format: ADR-XXXX) |
| `project/LESSONS_LEARNED.md` | When a mistake was caught early enough to prevent recurrence |
| `project/RISK_REGISTER.md` | When known risks evolve or new risks emerge |
| `project/BACKLOG.md` | When any issue status changes from ⬜ to ✔, or new issues identified |

### Phase 4 — Generate Session Handoff

Write this summary at the end of every session:

```markdown
## Session Handoff — [YYYY-MM-DD]

### Completed
[List what was accomplished vs. NEXT_TASK.md acceptance criteria]

### Files Changed
[Path + brief description for each modified/created file]

### Known Issues
[Any bugs, limitations, or incomplete areas with this work]

### Risks This Session Introduced
[New coupling, size growth potential, test coverage gaps]

### Recommendations for Next Agent
[What should the next agent do immediately?]
[What decisions need human review?]
[Are there architectural refinements to consider?]

### Confidence for Handoff
XX% — [reasoning: tests pass, ADRs honored, architecture aligned, etc.]
```

### Phase 5 — Commit Summary & Git Workflow

1. Create descriptive commit per Doc 17 conventions:
   - `feat(module): brief description`
   - `docs(project): update working memory after Sprint 0 bootstrap`
   - `fix(system): resolve [specific issue]`
   - `perf(module): reduce frame-time / binary size by [X KB]`

2. If feature branch: create `feature/<module>-<task>` per Doc 18

3. Verify PR follows review checklist (Doc 20) before asking for merge approval

## Memory Update Gate
- Never end a session without updating WORKING_MEMORY.md — stale memory is worse than no memory
- If last update would exceed 7 days without new work, flag to user as warning
