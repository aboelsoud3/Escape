# Session Start Protocol

> Algorithm every session executes before any work begins. Deterministic pattern — never skip.

## Phases

### Phase 1 — Synchronize Knowledge

Read the repository as if you're a new engineer joining the team:

1. Read `.agents/session_start.md` (this file)
2. Read `project/PROJECT_STATUS.md` → what phase/sprint are we in?
3. Read `project/WORKING_MEMORY.md` → rolling RAM — current context
4. Read `project/NEXT_TASK.md` → exactly one task to implement
5. Read `project/CURRENT_SPRINT.md` → sprint goals and scope boundaries
6. Read `project/DECISION_LOG.md` → what decisions have been made? Never undo them.

### Phase 2 — Validate Context

Before proceeding, verify:

- [ ] WORKING_MEMORY.md was last updated <7 days ago (warn if stale)
- [ ] PROJECT_STATUS phase aligns with KB §10 current state
- [ ] CURRENT_SPRINT scope doesn't violate any ADR in DECISION_LOG.md
- [ ] No conflicting information between documents — STOP and ask for clarification if conflicts found

### Phase 3 — Load Working Memory

Produce a condensed understanding:

```text
Project Understanding: [what we're building]
Current Objective: [what this sprint/task targets]
Potential Risks: [known blockers or concerns relevant to task]
Implementation Plan: [high-level approach, not detailed code]
Confidence: XX% — [reasoning for confidence level]
```

If Confidence < 80% → STOP. Recommend investigation before implementation.

### Phase 4 — Analyze Sprint + Task

- Review sprint boundaries per Doc 7 (never add tasks outside sprint scope)
- Read NEXT_TASK.md acceptance criteria verbatim
- Identify which source files will be modified per KB §5 architecture map
- Determine impact on: binary size, other modules, existing features, CI pipeline
- Check if task requires Architect review before implementation

### Phase 5 — Plan & Wait for Approval

Present implementation plan to human. Include:

```text
## Implementation Plan

### Files to Create/Modify
[List each file + what changes]

### Architecture Alignment
[Which modules are affected? Per Doc 10 dependency graph?]

### Size Impact Estimation
[Approximate KB impact per file]

### Testing Strategy
[Unit tests / seed regression / frame-time benchmark needed?]

### Dependencies on Other Systems
[Any cross-module impacts to flag?]

WAIT FOR APPROVAL before writing any code.
```

## Context Gate
- No writing code until Phase 5 approval granted
