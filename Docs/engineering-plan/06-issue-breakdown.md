# PULSE / ESCAPE — Issue Breakdown (E1–E4)

---

## E1: PlayerController

**Complexity:** High — core gameplay loop, multiple states, tight physics tuning required.

### Issues under E1

| # | Issue Name | Est. Time | Acceptance Criteria |
|---|-----------|-----------|---------------------|
| 1.1 | Run state + forward momentum | 1 day | Movement is velocity-integrated (delta-time); acceleration/deceleration curves smooth; instant input response <8ms |
| 1.2 | Jump mechanic with gravity arc | 1 day | Once-per-grounded; parabolic arc via constant gravity; visual ground-contact indicator present |
| 1.3 | Slide mechanic + hitbox reduction | 1 day | Duration 0.5–1s; playable hitbox height reduced ≥40%; invulnerability frames on slide startup (0.15s) |
| 1.4 | Dash mechanic with energy cost & cooldown | 1 day | Speed burst ~2× normal for 0.3s; costs 20% energy meter; 2-second cooldown; brief invulnerability during dash duration |
| 1.5 | State machine wiring + transitions | 1–2 days | Enum states `RUNNING/JUMPING/SLIDING/DASHING/WALL_RUNNING/AIR_CONTROL`; explicit transition guards; no hidden state leakage verified by unit test |
| 1.6 | Landing recovery (stun animation) | 0.5 day | 0.15s stun proportional to fall speed; prevents infinite jump loops; smooth forward momentum resumption |

---

## E2: CameraSystem

**Complexity:** Medium — requires interpolation math but well bounded by spec.

### Issues under E2

| # | Issue Name | Est. Time | Acceptance Criteria |
|---|-----------|-----------|---------------------|
| 2.1 | Smooth-follow camera (lag interpolation) | 1 day | Target position lerped into current; lag coefficient adjustable in config; no snapping/teleporting under any condition |
| 2.2 | Banking roll (lateral velocity based) | 0.5 day | Roll angle = `lerp(-8°, +8°)` mapped to lateral speed; returns to level when player stops moving laterally |
| 2.3 | Look-ahead offset proportional to speed | 0.5 day | Frustum center X/Z shifted forward by `speed × look_ahead_coeff` (default: 3.0m buffer) |
| 2.4 | FOV scaling (60° at rest → 75° at max) | 0.5 day | Linear mapping of current normalized speed [0,1] → FOV [60, 75]; continuous update each frame without pop-in |
| 2.5 | Screen shake on near-miss/collision | 1 day | Near-miss: exponential decay impulse starting at amplitude `A` with decay rate `d`: `shake(t) = A × e^(-d×t)`; collision: larger single-impulse (2× amplitude, 3× decay) |

---

## E3: CollisionPhysics

**Complexity:** Medium — deterministic kinematic-only system; careful hitbox tuning needed.

### Issues under E3

| # | Issue Name | Est. Time | Acceptance Criteria |
|---|-----------|-----------|---------------------|
| 3.1 | AABB player↔ground collision | 0.5 day | Player stays on surfaces; falls through gaps; no tunneling at max speed per frame (sweep test if needed); grounded boolean flag always accurate |
| 3.2 | Sphere-AABB obstacle hitboxes | 1 day | Hitbox dimensions = visual model × 0.7–0.8 (20–30% smaller); "feel fair, not pixel-perfect" validated by ≥5 playtesters; no false negatives at any speed regime |
| 3.3 | Near-miss detection system | 1 day | Frame-by-frame tracking: `d = distance(player_center, obstacle_center)`; threshold = `player_radius + near_miss_margin` (configurable, default 15cm); triggers particle burst + score event + heartbeat audio on success |
| 3.4 | Collectible sphere-sphere pickup | 0.5 day | `distance(player_center, collectible_center) ≤ player_radius + pickup_radius`; magnetic pickup upgrade extends effective pickup radius ×2 (if upgrade active) |
| 3.5 | Death collision → game-over trigger | 0.5 day | Any sphere-AABB overlap between player hitbox and obstacle triggers death; instant game-over state transition; no grace frames beyond invulnerability windows explicitly defined |

---

## E4: ScoringCombo

**Complexity:** Medium — mathematical scoring expressions require precise implementation + validation.

### Issues under E4

| # | Issue Name | Est. Time | Acceptance Criteria |
|---|-----------|-----------|---------------------|
| 4.1 | Distance scoring | 0.5 day | `distance_score = meters_run × base_multiplier` (base_multiplier starts at 1.0, scales with speed tier); live HUD update every frame; integer score display (no floating-point artifacts) |
| 4.2 | Near-miss scoring tiers | 1 day | Three threshold tiers: INSANE ≤10cm → `base × 3.0`, GREAT ≤30cm → `base × 2.0`, OK ≤60cm → `base × 1.5` (where `base = distance_per_frame_score`); proximity bonus applied multiplicatively |
| 4.3 | Combo multiplier system | 1–2 days | Formula: `combo_multiplier = (1.5)^(combo_level)` where combo_increments on each successful near-miss; combo multiplies all subsequent near-miss and distance scores; **reset conditions:** (a) any death, or (b) safe gap >2s with zero near-misses, or (c) player touches ground without performing any action for >30 seconds |
| 4.4 | Combo score expression implementation | 1 day | Final score per-frame: `score_delta = distance_delta × combo_multiplier + Σ(near-miss_scores × combo_multiplier)`; all arithmetic in integer to avoid floating-point non-determinism across platforms |
| 4.5 | Flow Meter (invisible) | 1 day | Hidden meter increments on consecutive perfect near-misses (within threshold); decrements by fixed drain rate per meter traveled; triggers Flow Mode at accumulated sum = `PERFECT_THRESHOLD` (~30 actions); no UI element shown until activation |
| 4.6 | Score display + HUD feedback | 0.5 day | Score counter live-updates with smooth interpolation (±5fps smoothing to avoid jitter); combo level displayed as large floating number that pops/scales on each increment; persistent high score across runs stored locally |
