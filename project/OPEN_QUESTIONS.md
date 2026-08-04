# OPEN QUESTIONS

> Open architectural and design decisions that require resolution before coding. Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

| ID | Question | Impact | Status | Owner/Next Step |
|----|----------|--------|--------|-----------------|
| OQ-001 | Exact Android minimum hardware tier needed beyond "2 GB RAM"? | Sizing of poly counts, FPS tiers, shader complexity | Open | Research on actual Android device market per @playground-standards |
| OQ-002 | Unity Play Asset Delivery vs raw assets in APK? | Binary size budget allocation | Open | Depends on playground constraints; document decision in ADR format |
| OQ-003 | Should ghost replay use deterministic or non-deterministic state? | Network/leaderboard compatibility (ADR §6 constraint) | Open | Default: deterministic frame-snapshots for reliability per Doc 5 §9 |
| OQ-004 | Framerate mode strategy — dynamic vs fixed target? | Rendering pipeline + audio buffer sizing | Open | Document in ADR; align with @playground-standards for Android power targets |
| OQ-005 | Leaderboard storage model: file-based JSON vs HTTP endpoint? | File IO constraint compliance per Doc 3 §1.4 (file limited via `std::filesystem::create_symlink()`) | Open | Decide after playground environment assessment |

## Pending Resolutions Blocker

All Phase 0 design documentation is complete and validated against MM-ADOS specs. No coding has begun — all remaining questions apply to Sprint 1+ implementation phase.
