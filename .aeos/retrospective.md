# Retrospective Workflow

> Execute at end of every sprint (or every N commits). Producer mode: assess team health, architectural evolution, technical debt trends, and plan next sprint.

## Check Items

### Completed This Sprint
```text
[Epic] [Issue ID]: [what was delivered vs what was planned]
[Epic] [Issue ID]: [what was delivered vs what was planned]
[If none yet — list "Sprint not started" status with blockers or reasons for inaction]
```

### Blocked / Not Completed
```text
[I-XXX]: [blocked by why] → next action to unblock
[or "none - sprint scope completed all tasks"]
```

### Risks Identified This Sprint
| Risk | Severity | Probability | Action Taken | Status |
|------|----------|-------------|--------------|--------|
[New risks that emerged during implementation go here — especially any related to: binary size creep, architectural drift, determinism failures]

### Lessons Learned
```text
Lesson 1: [what did we learn this sprint] → apply going forward? Yes/No
Lesson 2: [repeat as needed]
```

### Architecture Problems
| Problem | Module | Impact | Recommendation |
|----------|--------|--------|-----------------|
| [e.g. coupling between Gameplay and Renderer] | [specific module] | [high/medium/low | [what to do about it] |

### Technical Debt Summary
```text
Total: [LOW/MEDIUM/HIGH] debt level
Worst offenders:
1. [Module/File]: [debt description & why it exists + proposed fix date]
2. [repeat as needed]
```

### Next Sprint Plan
| Priority | Epic/Issue | Sprint Phase | Timebox | Notes |
|----------|-----------|--------------|---------|-------|
| 1 [must do first | [epic / issue # | P0–P6 or "Sprint X" | Hours/days] | Any context needed for next agent to pick up fast [e.g. known blockers, architectural notes, size impact estimates]

## Retrospective Summary
```text
## Sprint Retrospective — Sprint N (YYYY-MM-DD → YYYY-MM-DD)
Completed: [X/Y tasks completed across all epics]
Blocked: Yes/No → [what's blocking & who can unblock them]
Risks: # of risks identified this sprint → total active risk count now = X
Lessons: # of lessons learned this sprint (accumulated in LESSONS_LEARNED.md)
Architecture Health: GOOD / FAIR / POOR → [reasoning for your assessment — look at coupling, duplication, test coverage, binary size regression, and design alignment]
Technical Debt Level LOW / MEDIUM / HIGH ] → [which modules have the most and when should they be addressed]
Next Sprint Focus: [one-sentence summary of what comes next + why it matters]
Confidence for Next Sprint OK? Yes/No — [explain briefly with reasoning, metrics or blockers driving confidence level above 90%]
```

### When to Run This
- End of every sprint
- Every N commits during long implementation sessions  
- After any architecture-violating incident that requires immediate escalation
- Before moving from one Sprint phase (e.g., before MFP → Vertical Slice)
- If retrospective score is POOR → pause and address immediately
