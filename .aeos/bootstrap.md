# Bootstrap Workflow

> Execute BEFORE Sprint 0. Only runs once: initialize all ADOS infrastructure files and directories in this repository.

## Checklist

- [ ] `project/` directory exists with all memory documents (created)
  - [x] PROJECT_STATUS.md
  - [x] WORKING_MEMORY.md
  - [x] CURRENT_SPRINT.md
  - [x] NEXT_TASK.md
  - [x] DECISION_LOG.md
   - [x] LESSONS_LEARNED.md
   - [x] RISK_REGISTER.md
   - [x] BACKLOG.md
- [ ] `project/` directory has Phase 0 tracking documents (bootstrap phase 1)
   - [x] ARCHITECTURE_STATUS.md
   - [x] BUILD_STATUS.md
   - [x] SIZE_BUDGET.md
   - [x] PERFORMANCE.md
   - [x] OPEN_QUESTIONS.md
   - [x] PROJECT_HEALTH.md
   - [x] SESSION_HANDOFF.md
   - [x] CHANGELOG_AI.md
   - [x] DAILY_LOG.md
   - [x] ROADMAP.md
- [ ] `tasks/` directory exists with epic/issue templates (created)
  - [x] issues-template.md
  - [x] epics-template.md
- [ ] ` .agents/` directory exists (created)
  - [x] architect.md
  - [x] gameplay.md
  - [x] renderer.md
  - [x] reviewer.md
  - [x] qa.md
  - [x] producer.md
  - [x] session_start.md
  - [x] session_end.md
- [ ] `.aeos/` directory exists (this file + other workflow scripts)
  - [x] bootstrap.md (this file itself, being read during bootstrap validation!)
  - [x] resume.md
  - [x] implement.md
  - [x] review.md
  - [x] retrospective.md
  - [x] release.md
  - [x] audit.md

## Bootstrap Validation Questions
- [ ] Is there a single source of truth for project status? → PROJECT_STATUS.md ✅
- [ ] Can a new engineer resume this project after six months? → KB + all memory docs + .agents/ roles ✅
- [ ] Does every task have acceptance criteria? → NEXT_TASK.md + issues-template.md ✅
- [ ] Is there a defined session start and session end protocol? → .agents/session_start.md & session_end.md ✅

## Bootstrap Self-Audit Output
```text
Bootstrap Validation: PASSED
  - Memory system: operational
  - Agent roles: defined
  - Issue templates: created for Sprint 0
  - Workflow scripts: complete (7 workflows)
  - Architecture alignment with KB: confirmed
  - All ADR decisions logged
Ready to proceed: Sprint 0 active state ✅
```
