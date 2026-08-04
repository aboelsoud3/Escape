# Resume Workflow — Enhanced per validation.md Stage 9

> Execute at the start of every session. Synchronize project memory, validate documentation, identify current task.
> Generates a complete Repository → Architecture → Sprint → Task summary before any work begins.

## Steps

1. **Read Constitution** — Open `Docs/KB.md` §1–§5 for vision and design pillars
2. **Read Project Status** — Open `project/PROJECT_STATUS.md` to know where we are
3. **Read Working Memory** — Open `project/WORKING_MEMORY.md` for rolling RAM
4. **Validate Context Age** — Check WORKING_MEMORY last updated < 7 days, warn if stale
5. **Check Project Health** — If PROJECT_HEALTH.md exists, review metrics
6. **Identify Blocked Work** — If any blockers in PROJECT_STATUS → report before proceeding
7. **Sprint Alignment** — Confirm CURRENT_SPRINT scope matches KB §10 phase boundaries
8. **Current Task Identification** — Read `project/NEXT_TASK.md` for single atomic task
9. **Produce Full Resume Output** (Stage 9 mandate)

---

## Stage 9 Output Template — MUST be generated every resume

```text
=== RESUME REPORT ===

## Repository Summary
- Bootstrap: [PASSED/FAILED per BOOTSTRAP_REPORT.md]
- All critical docs present: YES/NO
- Memory files fresh (< 7 days): YES/NO
- Any contradictions found: YES/NO (list if yes)

## Architecture Summary
- Last ADR date: [date or "N/A"]
- Architecture gaps vs plan: [list any]
- Key risks from ARCHITECTURE_STATUS.md: [top 2]

## Sprint Summary
- Current phase: [Phase name]
- Current sprint: [Sprint ID]
- Completion: [XX%]
- Sprint in-progress count: [# tasks in IN_PROGRESS + IMPLEMENTED + UNDER_REVIEW]
- Sprint blocked count: [# issues blocked or FAILED gates]
- Next milestone: [milestone name + target date if known]

## Current Issue
- Epic: [Epic name/ID]
- Task: [Task ID/title]
- Status: [state from lifecycle]
- Acceptance criteria: [from NEXT_TASK.md or equivalent]
- Gate compliance: [Gate 1-6 pass status per implementation pipeline]

## Recent Decisions (last 3 sprints)
1. [ADR or decision + date + summary]
2. ...

## Known Risks
| ID | Risk | Severity | Mitigation Status |
|----|------|----------|-------------------|
| R-001 | [short desc] | High/Med/Low | Active/Resolved/Pending |

## Recommended Next Action
[Specific, atomic action tied to a single backlog issue or document gap. One sentence.]

## Estimated Time for This Session
[Estimated duration based on task scope. If > 2 days → break into sub-issues.]
```

---

## Resume Summary (Legacy Template — kept for compat)

```text
Phase: [X] | Sprint: [N] | Completion: [XX%]
Last State: [what happened last session or in BOOTSTRAP mode if first session]
Next Action: [NEXT_TASK short title]
Blocked: Yes/No — [if blocked, by whom and why]

WORKING_MEMORY.md status: Fresh (last updated YYYY-MM-DD) / STALE WARNING
PROJECT_STATUS.md alignment with KB §10: Verified / NEEDS UPDATE
```

---

## Stop Conditions

- WORKING_MEMORY > 7 days stale → warn user before proceeding
- PROJECT_HEALTH shows FAIL on Build or Tests → block resume
- Phase not aligned with KB (§13 says "Phase 0 Foundation") → STOP and ask for context
- Resume report Generation < 80% complete fields → recommend full architecture review before resuming.

## Confidence Check

If < 80% after reading all documents above → recommend full architecture review before resuming.
