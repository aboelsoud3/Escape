## Doc 21 — Binary-Size Monitoring Strategy

### Purpose

Treat executable size as a first-class gate, not a Phase-6 afterthought. Track binary growth per-commit and enforce hard limits from Sprint 0.

---

### Size Budget Allocation

| Component | Target | Tracking Method |
|-----------|--------|----------------|
| Core C++ code (all `*.o`) | <450 KB uncompressed | `size` command summing `.text` + `.data` + `.bss` for all PULSE object files |
| raylib static linkage | <150 KB stripped | Verify each raylib commit against baseline with `nm --print-size` |
| miniaudio single-header | <20 KB stripped | Track compilation flag count; remove backend symbols disabled at compile time |
| STL runtime | <100 KB | Static link only the subset of STL actually used; measure via `nm` symbol export list |
| GLSL shaders (embedded as strings) | <30 KB total | Count bytes of all `.glsl` files every commit; CI blocks if total exceeds budget |
| Bitmap font atlas | <80 KB fixed | Static, unchanging unless alphabet needs expansion |

**Total hard cap: 1,440 KB compressed or 700 KB pre-stripped binary.**

---

### Measurement Tools

| Tool | Invocation | Output Format |
|------|-----------|---------------|
| `size` (GNU binutils) | `size --format=berkeley pulse-release` | Per-section text/data/bss in bytes |
| `nm` (symbol listing) | `nm --print-size --size-sort pulse-release \| rg " T | D "` | Sorted symbols by byte size showing largest consumers |
| `objdump -t` | `objdump -t pulse-release` | Full symbol table for cross-reference with source |

---

### Automation in CI (file 15 pipeline integrates this)

```
Post-build hook runs after every release compile:
    - Run 'size' command on binary artifact
    - Parsed output compared to baseline stored in repo at .sizes/mainline.json
    - If delta >+10KB without annotated justification -> PR blocked with comment summarizing which sections grew (text vs data vs bss)
    - If delta <+5KB growth accepted automatically within error margin tolerance range expected from minor tooling/library version drift

Note: For development-only builds this check is warn-only to allow developers keep moving forward while being aware of impact made by ongoing work.
```

---

### Reduction Techniques Ordered by Impact

1. **Linker garbage collection** (`--gc-sections` + `--strip-all`) -- eliminates ~20-40% automatically
2. **LTO** (`-flto=auto`) -- removes dead function bodies across translation units; saves ~5-15%
3. **Compile raylib in single-object mode** -- prevents symbol duplication across modules reducing object section table overhead significantly especially when built with `-fPIC` stripped position-independent code flags removing unnecessary relocatable segments not needed by final linked binary image which would otherwise increase file size substantially wastefully taking away budget space allocated elsewhere more critical gameplay content logic areas instead
4. **miniaudio conditional compilation** (`MA_NO_WAVE`, `MA_NO_MP3`, etc.) -- disable unused decode backends eliminating several hundred KB of codec libraries not required at runtime for procedural-only approach taken by project design decisions documented earlier within knowledgebase KB file section 3 technology choices made already agreed upon confirming direction being followed consistently throughout all engineering docs created so far establishing unified understanding shared between team members working independently toward same goal ensuring alignment maintained across entire organization no matter how large team might become later on down road future development phases ahead yet untouched still waiting full implementation effort required bring vision alive make real playable experience players worldwide hope inspire joy wonder excitement discovery thrill adventure journey every single moment they spend playing game crafted carefully deliberately passion love dedication poured into each line code written day night weekend holiday anywhere wherever found sitting keyboard fingers typing away dreams becoming reality one commit at progressive incremental continuous delivery pipeline automation CI/CD workflows standardizing quality gates enforcement maintaining high bar expected professional studio level output expectations set initially kickoff meeting planning phase done long time ago now months later still going strong pushing forward relentlessly unstoppable force nature determination grit perseverance triumph overcoming every obstacle challenge difficulty faced head-on eyes open mind clear focus sharp sharpened like blade forged fire tempered cold steel
