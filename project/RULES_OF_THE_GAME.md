# RULES OF THE GAME (Project Constitution)

> These are non-negotiable rules that govern all work on this project. 
> Based on validation.md stages 7, 10, 11 — enforceable by any agent or human reviewer.

---

## Issue Lifecycle (validation.md Stage 7)

Every issue in BACKLOG.md MUST follow this state machine:

```
BACKLOG → READY → IN_PROGRESS → IMPLEMENTED → UNDER_REVIEW → APPROVED → MERGED → DONE
```

Status transitions enforced per task row in BACKLOG.md using exact uppercase labels only:
- `BACKLOG` = discovered but not yet planned for a sprint
- `READY` = approved, dependencies satisfied, waiting for sprint slot
- `IN_PROGRESS` = agent has started work per NEXT_TASK.md
- `IMPLEMENTED` = coded + self-verified (Gate 3 compliance met)
- `UNDER_REVIEW` = submitted to reviewer agent per Gate 5
- `APPROVED` = reviewer sign-off obtained, memory updated per Gate 6
- `MERGED` = changes committed and branch merged to main
- `DONE` = verified against original acceptance criteria; milestone tracked

**Rule: No skipping states. NEVER jump from IN_PROGRESS → APPROVED.**

---

## Milestone Freeze Checklist (validation.md Stage 10)

Before closing any milestone, ALL of these must pass:

- [ ] Architecture Review — ADRs reflect actual implementation
- [ ] Performance Review — frame budget targets met on target device class
- [ ] Binary Review — size still within budget; no untracked asset bloat
- [ ] Documentation Review — all docs aligned with current code state
- [ ] Memory Review — rolling memory files consistent, no orphaned deltas
- [ ] Technical Debt Review — TODOs and FIXMEs consolidated into new backlog issues

**If any review fails → milestone is FROZEN until all pass.**

---

## Weekly Knowledge Consolidation (validation.md Stage 11)

Algorithm applied to LESSONS_LEARNED.md:

```text
1. Gather lessons marked within last 7 days
2. Merge duplicates by ID or content similarity
3. Promote "foundational" lessons → highlight in PROJECT_STATUS.md or KB.md
4. Archive obsolete lessons → move to LESSONS_LEARNED.md/archive/ sub-section
5. Ensure WORKING_MEMORY.md stays under target size (< 20 KB) — promote details to KB.md
```

- [ ] This consolidation pattern has not yet been executed (first run planned for sprint close)

---

## Agent Authority Limits

| Agent | Allowed To Do | Must Escalate When |
|-------|---------------|-------------------|
| Gameplay | Player controller, movement, states | Architecture layer changes |
| Renderer | Shader code, rendering pipeline | Physics/collision interface changes |
| Architect | ADR proposals, architecture docs | Sprint boundary violations |
| Reviewer | Reject non-compliant work | Design decisions beyond existing ADRs |
| Producer | Task prioritization, sprint scope | Phase transitions (require human sign-off) |
| QA | Testing, gate verification | Code authorship (write only tests/docs) |

---

## Phasic Guardrails (validation.md Stage 8)

**Phase 0 (Foundation)** — NO gameplay code. Only: CMake, raylib window, logging, profiler hooks, CI stub, binary size tooling.
**Phase 1 (MFP)** — movement states only. Kill test: "Is movement fun?" MUST pass content gates.
**Phase 2+** — features added per backlog scope.

Agent must verify CURRENT_PHASE boundary compliance as Gate 1 before every implementation session.

---

## Final Rule

> *"Don't let the coding agent decide whether to follow the process—the process should decide what the coding agent is allowed to do next."*
> — validation.md Stage 12 principle

This file IS the enforcement mechanism for that principle.

Any rule violation → STOP session and require Producer + human review before resuming.

