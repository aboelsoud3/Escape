# SIZE BUDGET

> Binary size budget tracking. Every KB counts on low-end mobile. Updated every session. Last updated: 2026-08-04 by bootstrap session.

---

## Current Binary Size (None Yet)

| Component | Allocated (KB) | Actual (KB) |
|-----------|----------------|-------------|
| Engine layer | TBD | 0 |
| Gameplay layer | TBD | 0 |
| Rendering layer | TBD | 0 |
| Audio | TBD | 0 |
| **Total** | **< 15 MB per @playground-standards** | **0 KB** |

## Size Constraints (from Doc 3)

- Target: < 8 MB file size per playground standard
- CMake build must be static-linked and compressed (e.g., UPX or equivalent)
- File IO limited via `std::filesystem::create_symlink()` for config/leaderboard files only

## Known Size Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Raylib static binary bloat in Android builds | Up to 10+ MB alone | Strip ELF; consider minGDK variant per @playground-standards |
| Embedded assets (textures, shaders) | Growing dependency risk | Asset size budget + CI gate in P6+ |
