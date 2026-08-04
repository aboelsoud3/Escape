## Doc 18 — Branch Strategy

### Goal
Define when and how to use different branch types, merge policies, and protection rules for PULSE development.

### Branch Types and Use Cases

| Type | Pattern | Created By | Merged Into | Lifespan |
|-------|---------|------------|-------------|----------|
| `main` | bare | — | — | Permanent |
| Feature | `feat/short-name` | Developer | main | 1–7 days |
| Fix | `fix/short-desc` | Developer | main | 1–3 days |
| Hotfix | `hotfix/short-desc` | Any | main | Hours only |
| Release candidate | `release/vX.Y` | Lead dev | main + tags | 1–5 days |

### Protection Rules (main)
- Require all CI pipeline stages to pass before merge
- Require at least 1 manual approval (can be self-review for solo project)
- No force-pushes; always rebase or use squash merge only

### Branch Lifecycle Pattern

```
start → feat/[name]  ←→  main
                              ↑
release: [tag] release/latest release tags
```

### When to Use Each Type

| Scenario | Priority Type | Example Name | Merge After CI |
|---------|---------------|-------------|----------------|
| New feature | `feature/proc-texture-mixing` → Squash merge to main | | |
| Bug in renderer | `fix/debug-when-not-calling | |
| Hotfix for runtime crash or logic error | `hotfix/crash-on-empty-level` | Fast forward direct to main branch. **
| Merge release candidate (release v0.X)  release/vX.Y → main + tag |

### Branching Rules Summary
- Keep feature branches short-lived: aim to merge same day; max 3 days for any single feature |
- Push frequently to remote origin repository to avoid losing work |
- Always rebase from main before starting a new branch or PR |
- Protect the `main` branch: require all CI checks pass and at least 1 manual approval before merging in |
