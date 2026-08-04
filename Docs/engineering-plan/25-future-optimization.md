## Doc 25 — Future Optimization Strategy

### Purpose

Outline optimization approaches to be applied ONLY after
the game is fully playable. These are Phase-6 candidates.

---

### When to Apply: Phase-6 Only

Never optimize before the Minimum Fun Prototype is proven
fun. Optimization changes behavior in hard-to-spot ways.
Phase sequence: MFP -> Vertical Slice -> MVP -> Alpha
-> Beta -> Gold -> Optimize if still over budget.

---

### Binary Size Reduction (When Over Budget)

Phase-6 techniques in priority order of expected impact:

**Tie-breaking: always prefer the approach that preserves
gameplay quality over raw size savings.**

1. **Replace STL containers with hand-written pools**
   - `std::vector<char>` allocates heap memory. Use
     inline arrays instead when size is known at compile time
   - Impact: saves 20-50 KB of implicit template instantiations
   - Risk: high. Refactors many modules. Evaluate scope first

2. **Merge all similar GLSL shaders into one**
   - Extract shared macros (color interpolation/transform)
     into a single include-style common library at top of
     each shader file using `#define` preprocessor blocks
   - Impact: saves 10-30 KB embedded string overhead
   - Risk: low. Straightforward refactoring work

3. **Remove unused raylib functions via dead code strip**
   - Compile with `-ffunction-sections -fdata-sections`
     + `-Wl,--gc-sections` to let linker remove what is not called
   - Impact: saves 30-80 KB from pull-in of full raylib API
   - Risk: low. Standard release build technique

4. **Replace large lookup tables with math**
   - Sine/cosine for camera banking lookups -> approximate
     with Taylor series poly or a smaller LUT (256 entries)
   - Impact: saves 5-15 KB of compiled data section
   - Risk: medium. Replaced implementations must match old
     values within epsilon tolerance for determinism

5. **Use float instead of double where precision allows**
   - Float has 7 significant digits, sufficient for all in-game
     coordinates in a runner camera-view distance range
   - Impact: saves 20-40 KB of compiled instructions (half-width ops)
   - Risk: low to medium. Must verify collision edge cases still work

6. **Compile without -g debug symbols**
   - Release builds never include debug info (-gz none or omit
     flag entirely. Debug builds in a separate build tree)
   - Impact: saves ~40% of .text size immediately
   - Risk: none. This is already the standard practice

7. **Strip unneeded symbols (release only)**
   - Pass `-s` or `--strip-all` to linker for production builds
   - Impact: 15-25 percent additional reduction on top of baseline
   - Risk: none but keeps debug builds separate with symbols intact

8. **Compress shader strings with zlib**
   - Inline compressed GLSL sources + tiny decompression at runtime
   - Impact: ~50% compression ratio on text shaders typical
   - Risk: medium. Decompression code must fit within budget it saves

---

### Frame-Time Optimization (When Over Budget)

Always profile first. Identify the single bottleneck frame
slice before guessing what to optimize.

1. **LOD culling pass before rendering**
   - Reduce draw calls by skipping geometry updates for
     entities outside visible camera frustum extents
   - Profile impact first. Usually saves 30-50% of GPU time

2. **Merge instanced VBOs more aggressively**
   - Fewer but larger matrices uploaded per frame vs many
     small ones with individual transform buffers per batch
   - Reduces driver overhead from glBufferSubData calls

3. **Reduce particle systems or draw modes**
   - GPU compute particles (if used) need shader support
   - Fall back to simpler trail line rendering if budget tight
