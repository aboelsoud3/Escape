# AEOS Audit Script

> Run this AFTER bootstrap to validate all infrastructure before any game code. Per Doc 3 Stage 12.

---

## Audit Checklist

### Directory Structure
| Check | Path | Status |
|-------|------|--------|
| project/ exists | `project/` | ✅ |
| tasks/ exists | `tasks/` | ✅ |
| .agents/ exists | `.agents/` | ✅ |
| .aeos/ exists | `.aeos/` | ✅ |

### Memory Documents (per Doc 1)
| File | Exists | Bootstrap Marked | Audit Result |
|------|--------|-----------------|--------------|
| PROJECT_STATUS.md | ✅ | ✅ | ✅ correct |
| WORKING_MEMORY.md | ✅ | ✅ | ✅ correct |
| CURRENT_SPRINT.md | ✅ | ✅ | ✅ correct |
| NEXT_TASK.md | ✅ | ✅ | ✅ correct |
| DECISION_LOG.md | ✅ | ✅ | ✅ correct |
| LESSONS_LEARNED.md | ✅ | ⚠️ "LESSONS Learned.md" | ❌ bootstrap naming mismatch |
| RISK_REGISTER.md | ✅ | ✅ | ✅ correct |
| BACKLOG.md | ✅ | ✅ | ✅ correct |
| DAILY_LOG.md | ❌ | — | ⬜ NEEDS CREATION |
| CHANGELOG_AI.md | ❌ | — | ⬜ NEEDS CREATION |
| ROADMAP.md | ❌ | — | ⬜ NEEDS CREATION |
| ARCHITECTURE_STATUS.md | ❌ | — | ⬜ NEEDS CREATION |
| BUILD_STATUS.md | ❌ | — | ⬜ NEEDS CREATION |
| SIZE_BUDGET.md | ❌ | — | ⬜ NEEDS CREATION |
| PERFORMANCE.md | ❌ | — | ⬜ NEEDS CREATION |
| OPEN_QUESTIONS.md | ❌ | — | ⬜ NEEDS CREATION |
| PROJECT_HEALTH.md | ❌ | — | ⬜ NEEDS CREATION (per Doc 3 Stage 2) |
| SESSION_HANDOFF.md | ❌ | — | ⬜ NEEDS CREATION (per Doc 3 Stage 2) |

### Agent Roles
| File | Exists | Correct Name |
|------|--------|-------------|
| architect.md | ✅ | ✅ |
| gameplay.md | ✅ | ✅ |
| renderer.md | ✅ | ✅ |
| reviewer.md | ✅ | ✅ |
| qa.md | ✅ | ✅ |
| producer.md | ✅ | ✅ |
| session_start.md | ✅ | ✅ |
| session_end.md | ✅ | ✅ |

### Workflow Scripts (.aeos/)
| File | Exists |
|------|--------|
| bootstrap.md | ✅ |
| resume.md | ✅ |
| implement.md | ✅ |
| review.md | ✅ |
| retrospective.md | ✅ |
| release.md | ✅ |
| audit.md | ❌ (this file) |

---

## Validation Questions
- [x] Is there a single source of truth for project status? → PROJECT_STATUS.md ✅
- [x] Can a new engineer resume this project after six months? → KB + all memory docs + .agents/ roles ✅ (pending creation of missing docs below)
- [x] Does every task have acceptance criteria? → NEXT_TASK.md + issues-template.md ✅
- [x] Is there a defined session start and session end protocol? → .agents/session_start.md & session_end.md ✅

## Audit Result

```text
Bootstrap Validation: FAILED
  - Memory system: PARTIAL (10 of 17 doc 1 files present)
  - Agent roles: ALL 8 present ✅
  - Issue templates: created ✅
  - Workflow scripts: INCOMPLETE (audit.md missing)
  - Naming alignment: bootstrap.md has "LESSONS Learned.md" mismatch ❌
  - Missing from project/: DAILY_LOG, CHANGELOG_AI, ROADMAP, ARCHITECTURE_STATUS, BUILD_STATUS, SIZE_BUDGET, PERFORMANCE, OPEN_QUESTIONS, PROJECT_HEALTH, SESSION_HANDOFF (10 files)
  - Not Started: Sprint 0 tasks not assigned to agent session
Action Required: Create missing docs + fix bootstrap naming → re-run audit
```

---

## Post-Audit Remediation

When this audit runs during the initial bootstrap phase, it correctly fails because the infrastructure is still being built. To proceed:

1. Create all missing `project/` memory documents
2. Fix `.aeos/bootstrap.md` line 13 naming ("LESSONS Learned.md" → "LESSONS_LEARNED.md")
3. Re-run this audit; result should change to PASSED
4. Mark bootstrap checklist items as ✅ only after validation passes

Last Audit Run: 2026-08-04
Audit Status: FAILED — remediation required before Sprint 0 work begins
