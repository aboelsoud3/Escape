# IMPLEMENT WORKFLOW — Expanded Execution Pipeline

> Execute AFTER Resume + Approval. Based on validation.md Stage 3 expanded pipeline. Every issue MUST follow this exact sequence.

## Full Pipeline (validation.md Stage 3)

```text
Resume → Understand → Plan → Approve → Implement → Compile → Test → Review → Refactor → Document → Update Memory → Commit → Session Handoff
```

---

## Gate 1 — Context Verification

Before proceeding, verify agent has read ALL of:
- [ ] `PROJECT_STATUS.md`
- [ ] `WORKING_MEMORY.md`
- [ ] `CURRENT_SPRINT.md`
- [ ] `NEXT_TASK.md`
- [ ] `DECISION_LOG.md`
- [ ] `ARCHITECTURE_STATUS.md`

**IF NOT READ → STOP.** Do not proceed. Caller must pass these files as context.

---

## Gate 2 — Planning (Human Approval Required)

AI produces:
```
Understanding: [What this task means in system context]
Implementation Plan: [Step-by-step approach; no code yet]
Risks: [Technical risks, potential regressions]
Files: [Exact files to create/modify with size estimate]
Tests: [How this will be tested]
```

- [ ] Human approved plan before implementation begins

---

## Gate 3 — Implementation → Compile (Warnings = Failure)

During implementation:
- Code style follows Doc 19 exactly (KnR braces, naming conventions)
- No "TODO" placeholders; all functions fully implemented
- Documentation inline per KB §12 standards
- No commented-out code in final version

Compilation checks:
- [ ] Compiles with zero warnings (`-Wall -Wextra -Werror`)
- **Warnings = failure. Halt and fix before proceeding.**

---

## Gate 4 — Testing (No Skipped Tests)

- [ ] Manual test against NEXT_TASK.md acceptance criteria
- [ ] Seed determinism verified if applicable
- [ ] No skipped tests allowed
- If tests fail → return to Refactor

---

## Gate 5 — Reviewer Sign-Off

Reviewer agent must sign off on:
```
Self-Critique:
1. Possible bugs? → [specific]
2. ADR violation risk? → [confirm none or explain exception with proposed ADR amendment]
3. Performance concern? → [explain or confirm negligible impact]
4. Binary size impact? → [total KB added/removed]
5. Would another engineer understand this in 6 months? → Yes/No + reasoning
6. Does it match the approved plan exactly? → Yes/No
```

- [ ] Reviewer agent sign-off recorded

---

## Gate 6 — Memory Update (Mandatory Before Done)

Before marking task complete, update ALL of:
- [ ] `WORKING_MEMORY.md` (rolling RAM — add delta only)
- [ ] `PROJECT_STATUS.md` (if phase/sprint/threshold changed)
- [ ] `CURRENT_SPRINT.md` (task progress)
- [ ] `DECISION_LOG.md` (only if new ADR needed)
- [ ] `BACKLOG.md` (issue status update — see Issue Lifecycle below)

**"Done" is only valid when this gate passes.**

---

## Session Handoff Output

When ending session, append to `SESSION_HANDOFF.md`:

```
Handoff Entry:
Completed: [criteria met vs NOT met]
Files Changed: [path + delta summary per file]
Known Issues: [none or list with severity]
Risks Introduced: [new risks from this session]
Confidence: XX%
Next Priority: [immediate next step or blockers to address]
```
