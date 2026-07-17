---
title: "Building the Mover Cover System"
excerpt: "How I built a network-predicted cover system on Unreal Engine's experimental Mover plugin - wall classification, corners, gaps, a quaternion bug, and NetSerialize compression."
layout: single
permalink: /blog/mover-cover-system/
author_profile: false
read_time: true
related: true
categories:
  - devlog
tags:
  - mover-cover-system
  - phantomwire
  - unreal-engine
  - gameplay-programming
---

Cover mechanics are one of those systems that look deceptively simple from the outside. Press a button, character hugs a wall, duck behind it. But building one that works robustly across rotated geometry, network prediction, concave corners, and Mover's experimental movement architecture turned out to be one of the deeper technical problems I've worked on.

This is a writeup on how I built the **Mover Cover System** - a C++ plugin that adds a full cover gameplay system to any Mover-based character with a single actor component.

---

## Why Mover?

Epic's Mover plugin is a ground-up rewrite of the character movement system, designed to be modular, network-prediction-aware, and simulation-driven. It's still marked experimental, but it's clearly the direction UE5 is heading, and building on it early means everything the plugin does is designed for the future of Unreal movement - not bolted onto legacy `UCharacterMovementComponent` assumptions.

The tradeoff is that Mover has almost no third-party ecosystem yet. If you want a cover system, you build it yourself.

---

## Architecture

The plugin has three layers:

**`UMoverCoverComponent`** - an actor component you drop onto your character. It owns all the cover detection traces, wall tracking, edge/corner detection, and gap scanning. It also manages the input widget prompts and the `FCoverModeInputs` struct that feeds the simulation.

**`UCoverWalkingMode`** - a Mover movement mode registered on the `CharacterMoverComponent`. It derives from `UWalkingMode` so floor detection, gravity, and collision all work normally. The only override is `GenerateMove_Implementation`, which replaces the player's directional input with cover-locked lateral movement, wall depth correction, edge peeking, and corner transition driving.

**`FCoverModeInputs`** - a custom Mover input struct that carries all cover state through the simulation. Wall normals, locations, corner data, gap data, flags for every state. This struct is `NetSerialize`d so both client and server read the same data and make the same movement decisions at the same simulation tick.

Mode transitions (`UCoverEntryTransition`, `UCoverExitTransition`) are wired onto the Walking and Cover modes respectively. Rather than calling `QueueNextMode` directly from game-thread code, the transitions read `bShouldBeInCoverMode` from `FCoverModeInputs` each simulation tick and fire when the flag changes. This means mode switching happens inside Mover's prediction system - not outside it - which is the difference between multiplayer that works and multiplayer that desynchs.

---

## Wall Detection and Classification

Entering cover fires a sphere sweep from the character's forward vector. On hit, it runs a secondary horizontal trace at head height toward the wall to classify the surface:

- **Full cover** - wall blocks the head-height trace. Character stands.
- **Half cover** - wall doesn't reach head height. Character crouches.

This classification reruns every tick during `UpdateCoverWall` so it updates dynamically as the character shimmies along surfaces of varying height - useful for walls that transition from full to half mid-length.

![Half cover crouch pose behind a 1m wall, with GenerateMove debug output showing wall and corner state](/assets/images/blogs/mcs/HalfCoverDebug.jpg)

A wall depth tracking sweep fires each tick to keep the character snapped to the surface. Wall normals and tangents are recomputed each tick so shimmying across rotated or diagonal geometry stays stable.

![Character shimmying along a wall, debug lines showing wall normal and lateral movement target](/assets/images/blogs/mcs/ShimmyDebug.jpg)

![Standing full cover pose against a 2.5m wall, with wall normal and target debug lines](/assets/images/blogs/mcs/FullCoverDebug.jpg)

---

## The Quaternion Bug

Early on, the character would spin erratically when against walls rotated on the yaw - a 40-degree wall, for example. The debug output showed a normal like `(-0.64, -0.77, 0)` and the angular velocity flipping sign every other frame.

The cause: `ToRotationVector()` on a quaternion is unstable near the 180-degree singularity. When the desired facing is just over 180 degrees from the current facing, the rotation vector flips direction frame to frame, producing the spin.

The fix is one line:

```cpp
if (ToFacing.W < 0.0f)
{
    ToFacing = FQuat(-ToFacing.X, -ToFacing.Y, -ToFacing.Z, -ToFacing.W);
}
```

Negating all four components gives the mathematically identical rotation via the shorter arc. `q` and `-q` represent the same orientation - this just ensures we always take the path under 180 degrees.

---

## Corners and Gaps

**Outside corners** are detected with an L-shaped probe: a lookahead trace in the shimmy direction, then from the end of that trace, a second trace in the reverse direction looking for a connecting wall. If that second trace hits, and if a floor check at the corner point finds ground to land on, `bIsAtCorner` is set and the corner prompt widget appears.

![Character peeking around an outside corner with the corner transition prompt visible](/assets/images/blogs/mcs/CornerPeekPrompt.jpg)

When the player presses the corner input, `bWantsToGoAroundCorner` is set in the input struct. The movement mode reads this flag and drives the character toward the corner point at `CornerTransitionSpeed` - bypassing the normal acceleration limiter so it's instant rather than easing up over a second.

**Gap movement** was a later addition. At wall edges with no wall ahead, stepped perpendicular traces scan outward in `GapStepSize` increments looking for a coverable wall across open space. The first valid hit (with floor beneath it) becomes the gap target. Short gaps use a spin (character sidesteps), long gaps trigger a sprint (character runs face-first, rotating to face the travel direction mid-transit). Both are handled entirely within `UCoverWalkingMode` - no mode changes, no transitions.

---

## Networking

Mover uses rollback-style network prediction. The client produces inputs, runs simulation locally, and sends inputs to the server. The server runs the same simulation with the same inputs. If the server and client disagree, the client gets corrected.

For cover to work in multiplayer, the server needs to make the same movement mode decisions as the client. That means the cover flags - especially `bShouldBeInCoverMode` - must travel through the replicated input collection, not just exist on the game thread.

The transitions handle this. Every simulation tick, `UCoverEntryTransition::Evaluate_Implementation` reads `bShouldBeInCoverMode` from `FCoverModeInputs`. If it's true and the current mode is Walking, it returns the Cover mode name. Client and server both run this evaluation independently with the same input data, so they transition at the same tick.

Server-side validation in the transition independently traces to verify the claimed `WallHitLocation` has an actual wall there, and checks that the character isn't suspiciously far from it. Requests that fail validation are rejected, leaving the client in Walking mode until a legitimate wall is found.

One networking issue worth noting: `FCoverModeInputs` has a lot of fields. 12 FVectors, 11 booleans, several floats. At raw serialization that's around 1500 bits per struct instance. Mover bundles multiple input frames per RPC, and the total was exceeding the 16384-bit `MaxNumBits` limit.

The fix was to compress `NetSerialize` - booleans packed into a 32-bit flags field (11 bits), normals quantized to 16 bits per component, world locations truncated to integers. Compressed size is around 750 bits per instance, well within the limit.

---

## Editor Visualization

The editor module adds a **Cover Debug** toggle to the viewport toolbar's View Modes dropdown. When enabled, a subsystem scans static mesh surfaces near the editor camera every 0.5 seconds and applies a translucent overlay material:

- Green - full cover
- Yellow - half cover  
- Red - too low

It works with Nanite since it applies via material override on the mesh component rather than drawing geometry. Original materials are restored when the toggle is off.

---

## What's in the Plugin

- `UMoverCoverComponent` - cover detection, wall tracking, corner/gap probing, widget management, event delegates
- `UCoverWalkingMode` - cover movement mode with shimmy, depth correction, edge peeking, corner and gap driving
- `UCoverEntryTransition` / `UCoverExitTransition` - network-safe mode transitions with server-side validation
- `FCoverModeInputs` - replicated input struct with compressed NetSerialize
- `UCoverDebugSubsystem` - editor cover surface visualizer
- `IA_Cover`, `IA_CoverCorner`, `IMC_Cover` - preconfigured Enhanced Input actions and mapping context
- Full Doxygen documentation with setup guide and screenshots

The plugin is compatible with any Mover-based character with no base class requirements. Tested on UE 5.8, packaged for 5.5 through 5.8.

---

Available on FAB: [Mover Cover System](https://www.fab.com/listings/331cddd0-1505-4750-8321-f480416f3f55)

Support: [phantomwiregames@gmail.com](mailto:phantomwiregames@gmail.com) · [Discord](https://discord.com/invite/szPYYvDnxp) · [Twitter](https://x.com/I_AmShalashaska)
