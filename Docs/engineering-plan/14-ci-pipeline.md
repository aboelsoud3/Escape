## Doc 14 — CI Pipeline

### Goal
Automate build, test, and check verification on every push/PR for PULSE's 1.44 MB game contest entry.

### Pipeline Stages

| Stage | Tool | Description |
|-------|------|-------------|
| Lint | clang-tidy / cpplint | Static analysis enforcement |
| Compile:Linux | GCC + Raylib (CMake) | Standard debug+release builds |
| Compile:Windows | MSVC / MinGW | Cross-platform compile check |
| Compile:WebAssembly | emcc + Raylib-Web | Web target verification |
| Test:Unit | Catch2 / GTest | Unit test runner |
| Size Check | custom tooling | Binary size <= 1.44 MB for contest submission |
| Artifact:Pack | compressors | Lzma / Zip output packaging |

### Trigger Conditions
- **Push to main**: All stages run
- **Pull Request**: Stages above (Compile, Test, Size Check) run
- **Contest Submission Build**: Full pipeline with compression targets

### Budget Constraints
Each stage must minimize token/execution overhead. Total CI runtime target: under 10 minutes.

### Key Decisions
- Minimalist pipeline over complex orchestration; prefer CMake-native build system features
- Cross-platform compile checks ensure no platform-lock-in before contest deadline
- Binary size check enforced as a hard gate (CI fails if artifact exceeds budget)

