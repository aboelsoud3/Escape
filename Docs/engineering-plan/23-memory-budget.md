## Doc 23 — Memory Budget

### Purpose

Define memory allocation budgets for every subsystem.
Prevent unbounded growth through pool-based
allocations and hard caps enforced at startup.

---

### Memory Category Budgets

| Category | Limit | Method |
|----------|-------|--------|
| ECS entities | 2048 max | Pool allocator; warn/panic if exceeded |
| Static scene geometry | <2 MB peak | Instancing + VBO merges |
| Audio synthesis buffers | <512 KB | Circular oscillators only |
| Procedural road buffer | <500 KB | Ring buffer of N segments |
| Building data | <800 KB | GPU-only beyond lod_distance |
| Texture/mesh atlas | <200 KB | One small font atlas max |
| Ghost replay data | <5 MB total | Circular buffer, oldest-first |
| String storage (UI) | <30 KB | Immutable read-only segment |

Every subsystem uses a fixed pool or arena allocator.
No heap new in hot path code. Pools initialized at
engine startup with exact byte sizes above.

---

### Leak Detection Strategy

Debug builds call `Pool::verify_consistency()` every 30s:

- Total bytes allocated vs available per pool
- Any orphaned allocations tracked in circular
  buffer doubly-linked free-list head pointers
- Snapshot on any assertion failure for analysis

---

### Memory Profiling in Development

Use valgrind massif (Linux) or Visual Studio debugger
memory profiler (Windows) for periodic snapshots:

- Profile every two weeks during early dev
- Snapshot after loading a full city scene + 60s sim
- Commit largest snapshot as baseline; compare deltas
  against before merge to develop branch
