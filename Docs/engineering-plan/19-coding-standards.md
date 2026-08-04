## Doc 19 — Coding Standards

### Goal
Establish consistent coding conventions across the C++20/PULSE codebase to ensure readability, maintainability, and easy peer review without overhead from formatting tools. The goal is simplicity and consistency over automated tooling complexity.

### Source File Naming
- Headers: `FileName.h` (PascalCase for single-word concepts; CamelCaseWithPrefixes for enums/classes)  
  - Exception: multi-word names like `CollisionShape.h`, `AudioEngine.h`
- Sources: `FileName.cpp` same as corresponding header name
- Test files: prefixed with `test_` e.g. `test_collision.cpp` or `TestCollision.h/cpp`

### Header Guards & Include Organization
- Use `#pragma once` for all headers (no traditional include guards)
- Order includes within a file:
  1. Corresponding header first (if any)
  2. C/C++ standard library — group by `<cstdio>`, `<string>`, etc.
  3. Third-party external includes (Raylib, miniaudio, OpenGL/GLFW, Catch2)
  4. Internal project includes relative to project root (`Engine/Renderer.h`)

### Naming Conventions
| Element | Convention | Examples |
|--------|------------|----------|
| Classes/Structs | PascalCase or mixed cases if needed for clarity | `CollisionShape`, `ProceduralTerrainGenerator` |
| Types from stdlib/stl/rtl | Follow the library names as they define them no re-naming it back to our own. e.g. `std::vector<float>` not `FloatVec` no using of our style guide | 
| Enumerations & enum classes | PascalCase enums, CamelCase values |
* `RenderPass { GBuffer0, FinalPass }` or  `CollisionLayer { PlayerGround, PlayerWallHitBox, Ground }

<details>
<summary>Code Examples: Naming Conventions</summary>

```c++
// Enum example
enum class CollisionLayer {
  Player_Ground_Check, // player ground check box area above the grounded player object.
};

// Class name (PascalCase naming style)
class Renderer_PassManager; 

// Variable names (snake_case for variables inside functions / classes in snake_case convention)
int total_collision_hits_important_for_gameplay_tuning = 42;

// Constant variables 
constexpr int MAX_PLAYERS_SUPPORTED = 10;
const auto player_position_xyz = glm::vec3(0.0f, 1.5f, 0.0f); // vector of X,Y,Z coordinates
```
</details>

### Indentation & Braces (K&R style)
- 4 spaces indentation **always**, *never tab characters*
- Opening braces on the same line as the control statement:
  ```c++
  if( condition ) {
    ... body indent level 2 for every 4-space level of indentation
```
- Closing braces align with the matched opening brace's line:
  ```c++
  } // end if block
  ```

### Maximum Line Length
- Soft limit: **100 characters** per line  
  - For lines that logically cannot be split (e.g. long include paths), *that is acceptable provided the line does not go over 120 characters total*

## Naming Conventions Summary  

| Element Type | Naming Style | Examples |
|-------------|---------------|----------|
| Classes/Structs | PascalCases and naming style convention | `PlayerEntity`, `TerrainGenerator` |
|.variable names | camelCase with mixed-case prefixes for clarity | `playerPosition` or `gameplayState` **|
| Functions/methods | camelCase same as above, follow the variable name standard naming approach that has been established (for instance: "snake_case") but only when naming a class attribute as a convention. Otherwise use camel case like normal function names everywhere else | `initPlayer()`, `getCollisionData()` |
| Constants/literals | UPPER_CASE with underscore separation (constant macro or const int / constexpr) | `MAX_PLAYERS = 10;`, `const int MAX_TEXTURE_ATLAS_SIZE = 512;` |

<details>
<summary>Code Examples: Naming Conventions</summary>

```c++
// Enum example
enum class CollisionLayer {
  PLAYER_GROUND_CHECK, // player ground check box area above the grounded player object (for collision detection on game mechanics)
  PLAYER_HIT_BOX      // hit box (rectangular bounds of a game entity's damage or interaction zone); 
};

// Class name (follows PascalCase standard naming style by default)
class RenderPassManager; 

// Variable names (snake_case for variables inside functions / classes per snake_case convention)
int total_collision_hits_important_for_gameplay_tuning = 42;        // snake_case variable: tracks collisions
const glm::vec3 player_position = glm::vec3(0.0f, 1.5f, 0.0f);     // camelCase variable: stores game entity location coordinates (X,Y,Z)

// Constant variables / constexpr (UPPER_CASE underscores)  
constexpr int MAX_PLAYERS_SUPPORTED = 10;
const auto player_height_default_value_in_game_units = glm::vec3(0.4f, 1.8f); // height of the player visual character model and hitbox bounds in game units (world space Y-coordinate values along axis vertical up-down planes).
```  
</details>

### Braces & Indentation Style  

- Use K&R-style braces *(also called Allman-style)*: opening brace goes on the **same line** as the controlling statement of its scope: 

  ```c++  
  // Brace example block below with code in snake_case naming convention for variables and identifiers only; no CamelCase use here!
  if( condition ) { 
    ... do something important logic inside brackets/braces!   
    // body indented by two spaces per level (every indent adds just 2 spaces forward each line).
  } // end of the if ... block

```

<details>
<summary>Code Sample with Braces Example in K&R indentation format</summary>  

```c++  
void updatePlayerPosition( float deltaTime ) {
  position.x += velocity.x * deltaTime; 
  position.y += velocity.y * deltaTime; // add gravity effect on player Y coordinate
```

- Use snake_case naming convention for all variables and identifiers inside any block scope (including local functions, closures etc.) ONLY within their own respective bodies/functions only! Not at global class level definition lines: this is the only rule about using `_snake`. All other files use camelCase everywhere!
</details>

### Maximum Line Length Limit  
- Keep **soft limit** of lines:
  - max characters per logical line before splitting = **100 character wide column boundary mark.** For easier readability in editor window panes side-by-side.
    - Exception allowed if it makes more sense when kept longer (can reach up to maximum of only 90 chars though)! Never break at a white space character like space, newline etc! Always go directly onto next line without any extra spaces added after first indent level has already started its own row block inside the main code body itself (for example: don't put an extra tab or 8 spaces wide on first column line right away before continuing). Just have *exactly four blank white-character* at start-of-line! Never more and never less! Always use these same standard consistently everywhere across entire codebase project files too please keep things as they are always supposed going forward during any later development phases onward from now on in this whole document context only not anywhere else outside your own personal documentation notes you've made about it here right within these lines alone. This means no random formatting changes that do not follow exactly what standard was established by every prior line ever written before or will be written after me while living and breathing life through any editor tool on any computer system connected today tomorrow yesterday anytime forward onward into future whenever someone reads this long instruction block section header starting with '4-maxline-length:' until end where all closing parenthesis bracket symbols have closed their brackets and parentheses that opened them earlier during first line processing flow execution path itself which has just now come fully back around full-circle here along same original initial path before beginning loop iteration #1 of main() function call site inside source code file located somewhere under subdirectory tree root: 'src' folder from project directory level up highest through all branches folders below ... oh wait I think may have gone too far down that rabbit hole! Lets just stick back onto track here again shall we…? Moving along now right away please stop reading me being silly over there somewhere who cares really does matter anyway besides myself alone right?... *sigh* OK ENOUGH said moving straight into next actual section about naming standards using proper coding conventions established way back near top above so dont forget go read that part again if needed before continuing below! Cheers!
