# Escape
> **An adaptive procedural runner where the city learns how you play , 1.44MB size only**
>
> *Designed for the 1.44 MB Game Dev Contest.*

---

# Vision

PULSE is a high-speed, skill-based procedural runner focused on **mastery**, **flow**, and **replayability** rather than large content libraries.

The player is a futuristic courier racing through a living city that continuously adapts to their play style.

The goal is simple:

* Run.
* Adapt.
* Survive.
* Beat your previous self.

Every run should create a unique story.

---

# Core Design Pillars

## 1. Speed

The player should always feel fast.

Movement must feel smooth, responsive, and satisfying.

---

## 2. Precision

Near misses are rewarded.

Every centimeter matters.

---

## 3. Adaptation

The city evolves during gameplay.

The player should feel as if the world is responding to their decisions.

---

## 4. Mastery

Players improve through skill—not grinding.

Controls remain simple while the skill ceiling remains extremely high.

---

## 5. One More Run

The game should encourage immediate replay.

Runs are intentionally short.

Restarts are instantaneous.

---

# Core Gameplay Loop

```
Spawn

↓

Run

↓

Avoid Obstacles

↓

Collect Energy

↓

Perform Near Misses

↓

Increase Combo

↓

Choose Upgrade

↓

City Evolves

↓

Speed Increases

↓

Flow State

↓

Death

↓

Instant Restart
```

---

# Target Experience

The player should continuously experience:

* Flow
* Speed
* Risk
* Reward
* Discovery
* Improvement
* Near misses
* "One more run"

---

# Technical Philosophy

The project follows a strict rule:

> **Generate everything possible. Store almost nothing.**

Instead of storing assets, the game generates them at runtime.

Examples:

* Roads
* Buildings
* Trees
* Billboards
* Lighting
* Materials
* Weather
* Music
* Sound effects

The executable should remain extremely small while producing a visually rich experience.

---

# Technology Stack

## Language

C++20

---

## Graphics

raylib

OpenGL 3.3 Core

GLSL

---

## Audio

miniaudio

Procedural synthesizer

---

## Build

CMake

---

## Compiler

Clang

Optimized for size

* LTO
* Strip symbols
* -Os / -Oz (where supported)

---

# Rendering Goals

Stylized low-poly visuals.

Procedural materials.

Gradient-based shading.

Minimal textures.

Soft bloom.

Filmic tone mapping.

Ambient fog.

Strong silhouettes.

The game should resemble a polished indie title while remaining compatible with the contest size constraint.

---

# Architecture

```
Application
│
├── Engine
│   ├── Renderer
│   ├── Window
│   ├── Audio
│   ├── Input
│   ├── ECS
│   ├── Physics
│   └── Resource Manager
│
├── Gameplay
│   ├── Player
│   ├── Camera
│   ├── Obstacles
│   ├── Scoring
│   ├── Ghost Runner
│   ├── Flow Mode
│   └── Adaptive Director
│
├── Procedural
│   ├── Roads
│   ├── Buildings
│   ├── Traffic
│   ├── Weather
│   ├── Events
│   └── Audio Generator
│
└── UI
```

---

# Planned Features

* Endless procedural city
* Adaptive obstacle generation
* Ghost replay system
* Procedural music
* Dynamic weather
* Upgrade system
* Combo system
* Near miss detection
* Flow Mode
* Dynamic difficulty
* Replay seed support

---

# Project Milestones

## Milestone 1

Player controller

* Running
* Jump
* Slide
* Dash

---

## Milestone 2

Procedural road

Obstacle spawning

Scoring

Game over

---

## Milestone 3

Procedural city

Traffic

Weather

Lighting

---

## Milestone 4

Ghost runner

Replay system

Adaptive director

---

## Milestone 5

Procedural soundtrack

Sound effects

UI polish

Visual polish

---

## Milestone 6

Optimization

Memory reduction

Executable compression (if allowed)

Contest submission

---

# Size Budget

| Component        | Target        |
| ---------------- | ------------- |
| Executable       | < 700 KB      |
| Audio            | < 200 KB      |
| Fonts/UI         | < 80 KB       |
| Configuration    | < 30 KB       |
| Remaining Budget | Safety margin |

The project intentionally aims well below the contest limit to leave room for iteration and compliance with the final rules.

---

# Development Rules

## Every feature must satisfy at least one design pillar.

If a feature does not improve:

* Speed
* Precision
* Adaptation
* Mastery
* Replayability

it should be removed.

---

## Gameplay first.

Never sacrifice gameplay for graphics.

---

## Simplicity wins.

Prefer elegant systems over large content.

---

## Everything should be procedural when practical.

Algorithms are preferred over assets.

---

## Instant feedback.

Every player action should produce satisfying visual and audio feedback.

---

## Deterministic generation.

Given the same seed, the world should be reproduced exactly.

---

# Definition of Done

A feature is complete only when it is:

* Fun
* Readable
* Responsive
* Optimized
* Deterministic
* Documented
* Playtested

---

# Success Criteria

The project succeeds if players instinctively say:

> "I almost had it."

followed immediately by:

> "One more run."

If that reaction occurs consistently, the game has achieved its primary design goal.

