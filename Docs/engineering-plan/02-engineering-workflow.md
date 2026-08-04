# PULSE / ESCAPE — Engineering Workflow

> Lifecycle diagram, MFP gate explained, and the golden rule of execution.

---

## Execution Lifecycle Diagram

```
Vision (this KB + design pillars)
        ↓
   [MFP Gate] ← Never skip this
        ↓
  Phase 0 → Project Foundation
        ↓
  Phase 1 → Minimum Fun Prototype  ("Is moving fun?")
        ↓
  Phase 2 → Core Gameplay          ("Can I play a loop?")
        ↓
  Phase 3 → Vertical Slice         ("Does everything coexist?")
        ↓
  Phase 4 → Contest MVP            ("Are all features built?")
        ↓
  Phase 5 → Polish                 ("Does it feel amazing?")
        ↓
  Phase 6 → Optimization           ("Does it fit 1.44 MB?")
        ↓
   Gold → Contest Submission
```

---

## MFP Gate Explained

The **Minimum Fun Prototype (MFP)** is the most critical checkpoint. It exists as Phase 1 scope only:

| Must Have | Explicitly Exclude |
|-----------|-------------------|
| Player run/jump/slide/dash | Score system, menus, music, SFX |
| Single straight road | Procedural city generation |
| One static obstacle (cube) | UI polish, HUD |
| Death + instant restart | Camera shake/FOV/banking |
| Debug overlay (FPS/combo/distance) | Upgrades, ghosts, weather |

**Gate Criteria:** A human (or solo dev) picks up the build and says *"Cool — how does the jump feel?"* If no → throw it away. Iterate on physics/tuning/restart. Do not proceed to Phase 2.

---

## Golden Rule of Execution

> **Never give the AI or any tools the whole project at once.**   
> Every task must be small, measurable, reviewable, testable — like a GitHub issue.

```
Issue ← one atomic task with acceptance criteria
    ↓
AI plans → human approves → AI implements → AI writes tests → AI self-reviews → Human reviews → Merge
```

See KB §12 (Agentic Coding Philosophy) for the full prompt hierarchy and review loop.
