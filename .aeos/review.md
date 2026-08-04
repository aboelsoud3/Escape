# Review Workflow

> Execute before any merge to develop or main. Architecture audit, performance audit, binary-size audit, maintainability audit.

## Pre-Conditions
- [ ] Implement workflow completed → code compiles + self-reviews done
- [ ] Session handoff documented → WORKING_MEMORY.md updated

## Audit Types

### A. Architecture Compliance Review
| Check | Criteria | Pass/Fail |
|-------|----------|-----------|
| Module boundaries | No Engine ↔ Gameplay direct coupling | _/_ |
| Dependency graph | All depends-permitted per Doc 10 | _/_ |
| Folder structure | Files match KB §5 and Doc 8 exactly | _/_ |
| ADR alignment | No existing DECISION_LOG violations | _/_ |
| New architecture changes | Documented as new ADR? Yes / N/A | _/_ |

### B. Performance Audit
| Check | Criteria | Pass/Fail |
|-------|----------|-----------|
| Frame-time budget | Added logic doesn't exceed per-frame budget (~8ms @ 120Hz) | _/_ |
| Draw calls added | ≤ incremental draw calls acceptable for new feature | _/- |
| Memory allocation | No per-frame heap allocations (use object pool instead) | _/_ |
| Shader instructions | Each pass ≤300 instructions (Doc 24) | _/- |

### C. Binary Size Audit
| Check | Criteria | Pass/Fail |
|-------|----------|-----------|
| Current executable size | Tracked post-build (tools/binary_size_tracker.sh) | _/- |
| Impact of this PR | Estimated KB added/removed | _/- |
| Per-feature budget | ≤10 KB per major feature typically acceptable | _/_ |

### D. Maintainability Audit
| Check | Criteria | Pass/Fail |
|-------|----------|-----------|
| Code readability | Can new developer understand without asking creator? | _/- |
| test coverage | Unit tests exist and run for deterministic systems | _/_ |
| doc update | Implementation matches architectural docs | _/- |
| coding standards compliance (Doc 19) | Naming, formatting, line lengths per spec | _/- |

## Review Verdict Template
```text
## Review Result: APPROVED / APPROVED_WITH_NOTES / REJECTED

Architecture: [PASS/FAIL] — notes
Performance: [PASS/FAIL] — notes
Binary Size: [PASS/FAIL] — notes (+X KB or -X KB)
Maintainability [PASS/FAIL] — notes

Confidence Score: XX% for this merge
Issues Found: [# of issues found during review]
Suggested Fixes: [list if any, or "none"]
```

## Confidence Scale
| Score | Verdict |
|-------|---------|
| ≥90% | MERGE APPROVED; no concerns |
| 80–89% | APPROVED WITH NOTES (trivial fixes suggested); safe to merge after human review |
| < 80% | REJECTED — list specific issues + suggested fixes for agent to address |
