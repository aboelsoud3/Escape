# Release Workflow

> Execute before any milestone submission, contest release, or feature-complete merge. Verify all quality gates are passed and prepare release artifacts.

## Pre-Release Checklist

### Content Completeness
| Criterion | Status | Notes |
|-----------|--------|-------|
| All MVP Epic/Issues implemented per Doc 7 Sprint Goal table | [ ] Not Started / In Progress |
| Every Epic has matching acceptance criteria in BACKLOG.md | [ ] |
| All Sprints to date have retrospective documented | [ ] |

### Build & CI Verification
| Criterion | Status | Notes |
|-----------|--------|-------|
| Compiles with zero warnings (-Wall -Wextra -Werror) | [ ] |
| CI pipeline passes on last commit | [ ] |
| All unit tests passing | [ ] |
| Seed regression: same seed = same world 100 frames apart | [ ] |
| Frame-time benchmark within budget (<16.67ms average) | [ ] |

### Size Verification
| Criterion | Target | Actual | Status |
|-----------|--------|--------|--------|
| Executable size | ≤ approx _ KB (current) | ___ KB | _/_ |
| Audio subsystem | < 200 KB total budget | **___ KB | [ ] |
| Fonts/UI assets | < 80 KB total budget | **___ kb | [ ] |
| Total binary footprint | < 1.44 MB max limit | **___ KB | [ ] |

### Feature Scorecard (per KB §9)
Apply to every feature planned for release:
Score each as 1–5, minimum score = 7 required per feature, where Replayability, Originality, Cost (lower cost is better). Features scoring < 7/10 → defer to post-contest backlog.

### Quality Gates (from Producer Agent criteria)
| Gate | Passed? | Notes |
------|--------|-------|
| Kill Test Gate ("is movement fun") | [ ] | **Must pass for Phase** transitions _/_ |
| Functional Loop Gate (full run loop end-to-end) | [ ] | Required before Vertical Slice release |
| Full Pipeline Gate (§7 every subsystem functional integrated) | [ ] ] | Before MVP release |
| Feature-Complete Gate (all Sprint N content verified) | [ ] | Before Sprints N+1 launch |
| Juice Gate (one more run instinct) | [ ] | Playtest with ≥5 users needed |
| ≤1.44 MB Hard Gate | [ ] | Final binary submission required |

### Release Documentation Updates
- [ ] WORKING_MEMORY.md fully updated to reflect release state
- [ ] PROJECT_STATUS.md shows completion percentage and phase gate status
- [ ] ALL ADRs before decision made documented by DECISION_LOG.md (last entry)
- [ ] LESSONS_LEARNED.md entry added for sprint
- [ ] SPRINT_RETROSPECTIVE.md written per retrospective workflow
- [ ] RISK_REGISTER.md updated with resolved risks

## Release Commit Message Template
```text
release: Sprint N complete — [one sentence summary]

Delivered: [Epic list completed]
Size impact: +X KB total (executable → Y KB)
Tests: % passing | Frame-time: Z ms avg | Binary size: ___ KB
Quality Gates passed: [list which gates cleared]
Risk register: [new/updated risks per retrospective workflow]
Known issues before merge: [none or list with severity and workaround if any are present]
```

### Confidence Scale for Release Readiness
| Score | Verdict |
|-------|---------|
| ≥ 95% | RELEASE READY, all gates passed
80–4 % | Conditional release — minor issues pending but no blocking items |
| < 80% | NOT READY — address blocked issues before proceeding to next sprint phase ] |
