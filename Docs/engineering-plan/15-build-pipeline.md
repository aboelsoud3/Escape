## Doc 15 — Build Pipeline

### Goal
Define how PULSE compiles from source to a compressed executable artifact ready for the 1.44 MB contest submission.

### Build Targets

| Target | Compiler | Output | Purpose |
|--------|----------|--------|---------|
| `pulse-dev` | GCC/Clang `-g -O0 -fsanitize=address,undefined` | Unoptimized debug binary | Development & debugging |
| `pulse-release` | GCC/Clang `-O3 -flto -s` | Optimized release binary (no strip) | Testing size budget |
| `pulse-contest` | Same as release + `strip --strip-all` + xz compression | Final contest artifact | <= 1.44 MB submission |
| `pulse-web` | Emscripten `emcc -Os -sWASM=1 -sALLOW_MEMORY_GROWTH=0` | WASM module | Web platform port |

### Compiler Flags (Release)
- `-O3`: Max optimization for performance
- `-std=c++20`: Language standard
- `-flto=auto`: Link-time optimization to reduce binary size
- `-fwhole-archive`: Embed static dependencies without symbol stripping
- `-ffunction-sections -fdata-sections`: Enable linker garbage collection (`--gc-sections`)
- `#ifdef PULSE_RELEASE_BINARY_SIZE < 1.44MB` validation at build time

### Linked Dependencies (Static)
1. **Raylib** — games framework (targeting minimal embedded dependency set via CMake subdirectory or system package)
2. **miniaudio** — audio engine (`--no-default-backend` to strip unused backends)
3. **OpenGL/GLFW** — platform/windowing context (system libs)

### Build System
- Top-level `CMakeLists.txt` defining all targets and flags
- Subdirectory `Engine/` for core game logic
- Subdirectory `Runtime/` for platform-specific entry points
- Optional Emscripten toolchain file (`cmake/emscripten.cmake`) for WASM build

### Binary Size Budget Enforcement
Step 1: After linking but before stripping, run binary size analysis script. If >1.2 MB raw binary, enter iterative reduction mode.
Step 2: Strip all symbols (`strip --strip-all`).
Step 3: Compress with xz (`xz -9 --mem=64`).
Step 4: Verify output <= 1.44 MB.

### Build Commands
```bash
# Development build
cmake -B build/Debug -DCMAKE_BUILD_TYPE=Debug
cmake --build build/Debug

# Release build for size checks
cmake -B build/Release -DCMAKE_BUILD_TYPE=Release
cmake --build build/Release
./tools/binary_size_checker build/Release/pulse-release

# Compressed contest artifact
strip build/Release/pulse-release
xz --threads=0 -9 build/Release/pulse-release
```

### Key Decisions
- `-flto` is mandatory for all release builds to minimize size via cross-module optimization
- Use CMake subdirectory or fetchcontent for dependency management rather than external package managers (more portable, reproducible)
- Binary size checker runs post-build as an automated step before any submission build
