---
title: "Nightshift: From Getaway Driver to Elimination Circuit"
excerpt: "How Nightshift went from a Crazy Taxi-style prototype to a roguelite street racing tournament - vehicles, rendering, race framework, AI, UI, and the scope pivot that shaped it."
layout: single
permalink: /blog/nightshift-devlog/
author_profile: false
read_time: true
related: true
categories:
  - devlog
tags:
  - nightshift
  - phantomwire
  - unreal-engine
  - roguelite
---

Nightshift is a roguelite street racing tournament I'm building solo under my studio, PhantomWire Games. You start
dead last, ranked #1000 out of 1000 in an underground elimination circuit. The routes are procedurally assembled
every run, a perk draft between races lets you build your run identity, and every 100 ranks a named rival stands
between you and the next tier. Think Hades meets Need for Speed Underground.

This is a breakdown of how it got here - from a scrapped prototype to a fully playable loop.

### From Getaway Driver to Elimination Circuit

The project didn't start here. It began in September 2024 as a Crazy Taxi-style getaway driver game - spawn into a
procedural city at night, ferry criminals past police patrols before sunrise. That's also where the name Nightshift
came from. Early development was spent getting UE5's Chaos Vehicle physics to actually *feel* right (suspension
travel, tyre friction, centre of mass - it all has knock-on effects), while the open-world concept was prototyped
alongside it.

By October 2025, it was clear the open-world getaway loop wasn't tight enough - too much time navigating, not
enough time doing anything interesting. The pivot stripped the game back to the part that was actually fun in
playtesting: the racing itself. Roguelite structure went in on top - elimination tournament, perk drafts, procedural
route assembly. The name stayed. The game became something far more defined.

### Vehicles, Rendering & Art Direction

The visual identity draws from NFS Underground, Tokyo Xtreme Racer and Initial D. After testing several
post-process approaches, I landed on a cel-shaded look built around the **reverse hull technique** - the same
outline method Guilty Gear Strive uses - rendering an inflated, backface-only inverted mesh in black to get bold
cartoon outlines with no post-process edge detection. Shadow regions use a halftone pattern for a printed,
comic-panel feel.

![Nightshift cel-shaded vehicle in a garage, reverse-hull outlines and halftone shadow](/assets/images/ns-preview02.webp)

Getting custom vehicles into a Chaos Vehicle project needs a specific bone hierarchy for wheels, axles and dampers,
which is slow and error-prone to rig by hand. I built a **Blender plugin** that generates the full skeleton
automatically from wheel placement alone, producing an FBX-ready rig every time. Vehicle paint runs entirely through
a master material driven by a second UV channel mapped to flat colour regions on the body - no per-colour textures,
full customisation (base colour, metallic finish, tint, decals) through material parameters alone. That system
started life as a standalone [university project](/portfolio/vehicle-customisation/) and is being folded into
Nightshift directly.

### Race Framework & AI

The race loop runs on two `UWorldSubsystem`s. `URaceEventManager` owns a six-phase state machine -
`Inactive → PreRace → Countdown → Racing → Finish → Results` - broadcasting a delegate on every transition so any
system can subscribe without coupling to the manager itself. Race position is resolved at 20hz using checkpoint
count as the primary sort key and spline distance as the tiebreaker. `URaceCheckpointManager` handles wrong-way
detection (velocity dot product against the checkpoint's forward vector, debounced to avoid false positives when
clipping a checkpoint at an angle) and keeps only the next two checkpoints visible at a time.

Racer AI follows a spline-following foundation with two layers on top: a lightweight **vehicle avoidance** pass
that nudges racers around each other without full path replanning, and a **utility-scored pickup system** that
evaluates race position, nearby threats, vehicle health and upcoming corner curvature every 1-2 seconds per racer
to decide when to use an item - staggered so the whole field doesn't pop an EMP in the same instant.

### UI, HUD & the Perk Draft Loop

Perks run through the Gameplay Ability System as multipliers against a vehicle attribute set - speed, handling,
acceleration, drift angle - so the system stays composable instead of a pile of one-off value overrides. On the
design/UI side, I rebuilt the entire UI framework from the ground up so every menu and in-game widget shares a
common base for focus management and input handling, which fixed a run of edge-case bugs (including one where the
back-action prompt silently disappeared in certain menus).

![Nightshift perk draft screen showing three perk cards after a race](/assets/images/ns-preview04.webp)

The perk draft screen presents three cards after each race - name, rarity, icon and level, with the full
description flipping open on hover. Crossing the finish line first shows your placement as big graffiti-style spray
text rather than a plain number, before fading into a results table that correctly lists DNF racers instead of
giving them a ghost time.

![Player finishing position revealed as graffiti-style spray text](/assets/images/ns-preview05.webp)
![Nightshift results screen listing every racer's finishing time, including a DNF entry](/assets/images/ns-preview06.webp)

Selected perks save to the save game, level up if already owned, and get reapplied automatically - stats and HUD
icons included - the next time you're in a race. A `SceneCaptureComponent2D` feeding a HUD render target also
drives a working rear-view mirror.

![Active perk icons shown in the Nightshift HUD](/assets/images/ns-preview07.webp)

### Scrapping the Open World

Before the current architecture, I built a full PCG pipeline for runtime open-world city generation - road network
graphs, continuous race splines, the works. Once I properly stress-tested it in early 2026, the frame rate impact
and level-streaming load times weren't acceptable for a game that needs to feel fast and load a new run instantly.
UE5's Mass Traffic system was also evaluated for ambient vehicles and ruled out - too much integration overhead for
what it was worth at this stage.

The call: no open-world city. Nightshift uses linear, procedurally assembled race routes instead - hand-crafted
segments stitched together at runtime, closer to how F-Zero or Trackmania build a track than an open world. It's a
much better fit for a roguelite where every run needs to feel snappy, and it's the kind of scope decision that's
easy to avoid making until it's already expensive - better to find out in a stress test than six months into
building systems on top of it.

### Performance Engineering

Before building anything else on the rebuilt foundation, I put the project through a full optimisation pass.
Disabling deferred rendering for DX11 forward shading (SM5) got a packaged build to 600-900fps at native 1440p on
an RTX 3060 with a near-empty scene. Forward shading meant no Mega Lights support though, so I moved back to DX12
with hardware ray tracing enabled - about 0.5ms more frame time, but Mega Lights removes a large chunk of manual
lighting work in a dynamic city environment. That build still held 400-500fps at native 1440p on desktop, and
around 300fps at native resolution on a Steam Deck.

### Where It's At Now

Nightshift currently has a fully playable end-to-end loop: **Title Screen → Garage Hub → Race → Results → Perk
Draft → Garage Hub**. It's rough around the edges with placeholder assets throughout, but everything from here
builds on a working foundation instead of a set of isolated tech tests. Rank progression, elimination logic and
tying the tournament structure together end-to-end are next.

[Read the full devlog series on PhantomWire](https://phantomwiregames.github.io/pages/news/news.html)
