# DECISION LOG

> Every important decision with chosen option, rationale, and rejection record. Prevents AI from undoing previous architectural choices. Updated every session. Last updated: 2026-08-04 by ADOS bootstrap session.

---

## ADR-001 | Project Memory System
| **Section** | Engineering Processes |
| **Status** | Accepted |
| **Chosen** | Repository as source of truth (ADOS four-layer pattern) |
| **Rejected Alternatives** | Ephemeral chat context only, monolithic prompt per session |
| **Reason** | Knowledge persists across sessions and model switches via version-controlled files. See ADOS docs in Docs/MM-ADOS/ |

## ADR-002 | Technology Stack
| **Section** | Tech Stack |
| **Status** | Accepted |
| **Chosen** | C++20 + raylib (single-header) + OpenGL 3.3 Core + miniaudio single-header |
| **Rejected Alternatives** | Unity/Godot/Unreal (exceeds 1.44 MB), Bullet/PhysX, FFTW, nlohmann/json |
| **Reason** | All rejected choices blow past binary budget. Single-header raylib + miniaudio = minimal footprint with zero link-time deps. Deterministic simulation via C++20 fixed-point math required for ghost replay system (KB §5.7). |

## ADR-003 | Procedural Generation
| **Section** | Design Pillars / Size Budget |
| **Status** | Accepted |
| **Chosen** | Generate everything, store almost nothing — all assets procedural from seed |
| **Rejected Alternatives** | Pre-baked geometry, texture atlases (except tiny font atlas), WAV audio samples |
| **Reason** | 1.44 MB hard constraint makes every stored asset a liability. Procedural alternative always preferred (KB §9). Deterministic seed propagation chain established in KB §8. |

## ADR-004 | Binary Size Monitoring Starting Sprint 0
| **Section** | Optimization / CI Pipeline |
| **Status** | Accepted |
| **Chosen** | Track binary size from first commit; hard gate in CI during P6+ |
| **Rejected Alternatives** | Wait until Phase 6 (optimization phase) to worry about size |
| **Reason** | Size creep is the #1 failure mode for contest games. Early monitoring prevents surprises when optimization phase arrives (KB §9, Doc 21). |

## ADR-005 | Custom Physics
| **Section** | Tech Choices |
| **Status** | Accepted |
| **Chosen** | Custom kinematic AABB/sphere collision only |
| **Rejected Alternatives** | Bullet, PhysX, Box2D |
| **Reason** | Heavy engines add ~1–2 MB for what's a simple runner with box-plane-sphere checks — trivial to implement from scratch and deterministic (KB §15). Kinematic not dynamic: delta-time velocity integration only. |

## ADR-006 | Bitmap Fonts Only
| **Section** | Rendering Vision / Size Budget |
| **Status** | Accepted |
| **Chosen** | 64×64 or 128×128 px bitmap font atlas via GLSL rendering |
| **Rejected Alternatives** | TTF/TrueType fonts, FreeType library |
| **Reason** | TrueType rendering libs add hundreds of KB; bitmap atlas on canvas = ~5–20 KB. All HUD digits and UI text covered by atlas (KB §3). |

## ADR-007 | MFP Before MVP
| **Section** | Development Lifecycle / Kill Test |
| **Status** | Accepted |
| **Chosen** | Minimum Fun Prototype must validate "is it fun?" before any content development |
| **Rejected Alternatives** | Move straight to Phase 2/3 content building |
| **Reason** | Validating core loop early prevents wasting weeks on unfun games. Movement MUST feel compelling (KB §10 Phase 1 gate). If not → throw away and iterate physics, never proceed. |

## ADR-008 | Epic/Issue Breakdown
| **Section** | Sprint/Bugzilla |
| **Status** | Accepted |
| **Chosen** | E1–E14 epics with tasks per sprint proposal (Doc 7) |
| **Rejected Alternatives** | Monolithic feature implementation, no breaking down into smaller units |
| **Reason** | Golden rule: every task must be small/measurable/reviewable/testable. Atomic issues prevent context overload and enable cleaner PR review (KB §12). |

## ADR-009 | Fixed 60 FPS Target
| **Section** | Key Decisions / Engineering Standards |
| **Status** | Accepted |
| **Chosen** | Hard lock at 60 FPS, not variable target |
| **Rejected Alternatives** | Variable frame rate (e.g., 30–60 dynamic) or 120 Hz support |
| **Reason** | Fixed 60 FPS = consistent movement/timing determinism across hardware; ghost replay system requires deterministic substeps; 8ms per physics sub-frame fits budget when instanced heavily. (KB §15.) |

## ADR-010 | Engineering Memory Classes
| **Section:** Engineering Processes / MM-ADOS |
| **Status** | Accepted |
| **Chosen:** Permanent → Long-Term → Medium → Short-Term → Ephemeral memory classes with staleness warnings (7 days max) |
| **Rejected alternatives:** All-memory-as-prompt, chat-only history dependency. The five-tier class structure prevents stale context, ensures consistent session-to-session continuity, and keeps AI memory manageable and auditable (MM-ADOS documentation.)

## ADR-011: Agent Memory Graph (AMG)
| **Section:** Engineering Processes / MM-ADOS | **Status**: Accepted |
| **Chosen:** Typed graph of engineering knowledge synced each session with coherence validation against code tree. **Rejected alternatives**: Flat markdown directory (no enforcement of cross references). AGM produces a verifiable state machine instead of free-form notes that drift out-of-sync. |
