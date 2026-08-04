## Doc 17 — Git Workflow

### Goal
Define branching, commit, and review conventions to keep the rapid iteration pace of PULSE development organized without bureaucracy overhead.

### Branching Model (Trunk-Based with Short-Lived Branches)

| Branch | Purpose | Lifetime |
|--------|---------|----------|
| `main` | Stable, always-compiling line | Indefinite |
| `dev-[short-desc]` | Feature work in progress | Days 1–5 |
| `feat/[short-name]` | Named feature branch | Days 1–7 |
| `hotfix/[short-desc]** | Critical bug fix targeting main** | Hours 1–2 |

### Commit Guidelines
- Use **imperative mood**: "Fix spawn rate instead of "Fixed..."
- Prefix with component: `[renderer]`, `[collision]`, `[audio]` etc.
  - Example: `[collision] optimize AABB sweep for mobile targets`
- Keep commits **focused** and atomic: no mixing unrelated changes
- Max commit diff size: ~500 lines to enable easy review

### PR/MR Guidelines
- Title format: `17-git-workflow — component: change summary
- Every PR should close or reference the associated issue(s)
- Must pass all CI stages before merge.
- Squash merge for feature branches, keep commit history on hotfixes

### Convention Summary

| Task | Branch Prefix | Merge Strategy |
|------|---------------|---------------|
| Feature work | `feat/` | Squash to main |
| Bug fix | `fix/` | Normal merge |
| Documentation update | `docs/` | Fast-forward to docs / |
| CI/build pipeline change | `ci/` | Normal merge to ci / docs branch |

### Tagging and Versioning (SemVer-like)
- Use pre-release tags for contest milestones:
  - `v0.1-audio-solo-player`
  - `v0.3-proc-gen-complete`
  - `v1.0-released-artifact`
