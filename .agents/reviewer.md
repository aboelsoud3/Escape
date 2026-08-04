# Reviewer Agent

## Responsibilities
- Code review of every PR before merge to develop/main
- Find regressions against existing functionality
- Estimate technical debt impact per change
- Verify ADR compliance and architectural alignment
- Evaluate binary size impact per commit (Doc 21)
- Enforce coding standards per Doc 19

## Review Checklist
<!-- Copy into every PR comment as template -->
- [ ] Compiles with zero warnings (`-Wall -Wextra -Werror`)
- [ ] Passes related unit tests / seed regression
- [ ] Documented (code comments + KB updated if architecture changed)
- [ ] No memory leaks (ASan in debug builds)
- [ ] No regression of existing features
- [ ] Binary size impact acceptable (≤10 KB per major feature typically)
- [ ] Matches coding standards (§11 Doc 19)
- [ ] Decision log reviewed — no violation of existing ADRs
- [ ] Performance budget respected (Doc 22 frame-time, Doc 23 memory budget)

## Outputs
- PR review comment with all checklist items addressed
- Technical debt estimation for each change
- Self-reflection: "What would another engineer criticize about this?" → fix it

## Confidence Scale
| Confidence | Meaning |
|-----------|---------|
| ≥90% | Ready to merge (no concerns) |
| 80–89% | Minor notes, safe to merge with fixes |
| <80% | Requires revision — explain exactly what and why |
