---
layout: project
permalink: /portfolio/mover-cover-system/
title: "Mover Cover System"
role: "Gameplay Programmer (Solo)"
company: "PhantomWire Games"
date: July 2026
engine: "Unreal Engine 5.7+"
banner: /assets/images/card-mcs.jpg
skills:
  - C++ Gameplay Programming
  - Unreal Engine Mover Plugin
  - Network Replication & Prediction
  - Movement Mode Architecture
  - Editor Tooling
  - Doxygen Documentation
summary: >
  <p>Mover Cover System is a C++ plugin built on top of Unreal Engine's experimental Mover plugin. It adds a full
  cover gameplay system to any Mover-based pawn through a single actor component - no custom character base class
  required.</p>

  <a href="https://www.fab.com/portal/listings/0d59ddc9-6df8-4a97-b17c-5c58423d8a88" target="_blank" rel="noopener" class="button-link">View on Fab</a>

  <p>The system handles wall detection and classification (full and half cover), shimmy movement, edge detection,
  peeking and aiming around wall edges, transitions around outside corners, and gap movement between disconnected
  cover surfaces. Everything is replicated over the network using Mover's built-in prediction system, backed by
  custom NetSerialize implementations and server-side validation to keep cover state authoritative and
  rollback-safe.</p>

  <iframe width="640" height="360" src="https://www.youtube.com/embed/u2bslgKu33A" title="Mover Cover System multiplayer demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

  <p>Alongside the runtime system, the plugin ships with editor tooling - a subsystem and toolbar extension that
  visualizes detected cover surfaces directly in the viewport using material-based highlighting - plus
  Blueprint-exposed APIs so designers can hook into the system without touching C++, and full Doxygen
  documentation.</p>

  <p>More gameplay clips and screenshots are posted on <a href="https://x.com/I_AmShalashaska" target="_blank" rel="noopener">X / Twitter</a>.</p>

  <div class="blog-card">
    <h3>📘 Full Devlog</h3>
    <p>Wall classification, corner and gap detection, a nasty quaternion bug, and compressing NetSerialize to fit Mover's bit budget.</p>
    <a href="/blog/mover-cover-system/" class="button-link">Read the Devlog</a>
  </div>
---
