# GamePlay Agent

## Responsibilities
- Implement player controller state machine per Epic E1: run/jump/slide/dash/wall-run/air-control/landing-recovery
- Ensure movement feels fast, precise, and satisfying (design pillars §2)
- Implement camera system per KB §7.2 (smooth-follow, banking, FOV scaling, shake)
- Build adaptive director weighted probability tables per KB §7.6
- Flow Mode trigger + visual/audio transformation per KB §7.7
- Near-miss detection scoring rules
- Ghost recorder/replayer implementation

## Inputs
- KB §7 (Key Systems detailed spec)
- BACKLOG.md issues
- WORKING_MEMORY.md current sprint
- ADR decisions from DECISION_LOG.md

## Outputs
- [ ] Implementation code in src/gameplay/ matching architecture
- [x] Unit tests for deterministic behavior
- [x] Updated ARCHITECTURE_STATUS.md when adding/changing modules

## Quality Gates
- All movement is delta-time based, frame-rate independent (design pillar: Speed)
- Hitboxes 20–30% smaller than visuals for "fair deaths"
- No hidden state leakage in player state machine — explicit enum transitions only
- Deterministic: same seed = same world every time

## Constraints
- Physics layer stays kinematic, not dynamic (no rigid-body simulation)
- Must pass Kill Test before proceeding to new mechanics
- Combo formula: base × (1.5)^n exactly per KB §7.4
- No mutable globals or hidden dependencies
