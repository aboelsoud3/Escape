# RISK REGISTER

> Ranked risks to project success. Updated every N commits or when new risks identified. Last updated: 2026-08-04 by ADOS bootstrap session.

---

## Active Risks

| # | Risk | Severity | Probability | Owner | Mitigation | Status |
|---|------|----------|-------------|-------|------------|--------|
| R1 | MFP movement doesn't feel fun | Critical | Medium | Tech Lead + Designer | Never skip to content; Kill Test Gate required (KB §10 Phase 1) | Open |
| R2 | Executable exceeds 1.44 MB | Critical | Low-Medium | All developers | Binary size tracking from Sprint 0; CI size gate P6+; every KB justified | Active |
| R3 | MFP fails Kill Test — movement isn't fun | Very High | Medium | Tech Lead | Throw and iterate; test with real players, not solo dev intuition | Open |
| R4 | Determinism breaks across architectures/C++ versions | High | Low | Engine programmer | Epsilon-based checks only; never raw float comparison; cross-platform testing in Sprint 2+ | Open |
| R5 | Procedural generation feels repetitive | Medium-High | Medium | Designer + Tech Lead | Multiple seed permutations; Director-weighted randomness; test with 5+ seeds (KB §14) | Open |
| R6 | Feature creep exceeds contest scope | Medium | High | Producer role | Kill Test mandatory for every feature; Feature Scorecard ≥7/10 required (KB §9) | Open |
| R7 | Shader complexity exceeds frame-time budget | Medium | Low | Technical Artist | Max 300 instructions/pass per technique; profile bloom+tonemap+fog separately; merge when needed (KB §4) | Open |
| R8 | Deterministic generation breaks between compiler versions | High | Low | Engine programmer | Never rely on float equality; use integer snap or epsilon collision checks instead (KB §14) | Open |

## Resolved Risks
None yet. First implementation session not started.
