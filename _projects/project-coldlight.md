---
layout: project
permalink: /portfolio/project-coldlight/
title: "Project Coldlight Interface Design"
role: "UI/UX & Motion Designer (Solo)"
company: "PhantomWire Games"
date: July 2026
engine: "HTML/CSS/JS Prototype — Target: Unreal Engine 5 (UMG/CommonUI)"
banner: /assets/images/card-coldlight.png
images:
video:
link:
skills:
  - UI/UX Design & Interaction Design
  - Motion Design (draw-on line-work, decode/scramble text FX)
  - Design Systems (state-driven theming via design tokens)
  - Procedural UI Construction (data-driven radial geometry)
  - Gamepad-First Radial Interaction Design
  - Rapid Interactive Prototyping ahead of UMG implementation
summary: >
  <p>COLDLIGHT is a short, linear single-player stealth game (Splinter Cell-style) currently in development in
  Unreal Engine 5. This project is an interactive HUD concept exploring a fully diegetic, <i>Blacklist</i>-inspired
  interface built from monochrome line-work and projected typography.</p>

  <h2>What Is It?</h2>
  <p>The concept is built around a handful of interlocking systems:<br>
  - <b>World-projected objective display</b>: decode/scramble text animations with a layered information hierarchy -
  primary objective, secondary objective, and mission telemetry.<br>
  - <b>Character-anchored readouts</b>: tethered directly to the operative, surfacing ammo and equipment state.<br>
  - <b>World-space markers</b>: for the objective, exfil route, and contextual interaction prompts.<br>
  - <b>Edge-of-frame hostile bearing indicators</b>: for tracking threats outside the player's view.<br>
  - <b>Radial equipment wheel</b>: procedural segment layout with a rim that re-traces on every open.</p>

  <p>The whole HUD re-themes through three awareness states - concealed, caution, and compromised - via a single
  palette swap, and every element can be toggled independently with its own reveal/hide animation. This doubles as
  a tool for testing how minimal the final HUD can go, and the whole thing is composited over in-engine-style
  gameplay framing to validate legibility against a real scene.</p>

  <h2>Try It</h2>
  <p>The prototypes below are fully interactive. The menu flow covers the full six-screen sequence - title,
  options, mission select, intel briefing, loadout/ready-up, and mission debrief. The HUD panel on the right (or
  keyboard shortcuts <b>1-5</b> for individual elements, <b>O</b> for the objective display, and <b>Q</b> for the
  equipment wheel) switches between awareness states and toggles HUD elements on and off.</p>

  <div style="position: relative; width: 100%; max-width: 960px; aspect-ratio: 16 / 9; margin: 1.5em auto; border: 1px solid rgba(255,255,255,0.15);">
    <iframe id="coldlight-menu-frame" src="/projects/Coldlight/coldlight-menu-concept.html" title="COLDLIGHT interactive menu prototype" style="width: 100%; height: 100%; border: 0; display: block;" loading="lazy" allowfullscreen></iframe>
    <button id="coldlight-menu-fullscreen-btn" onclick="coldlightToggleFullscreen('coldlight-menu-frame', 'coldlight-menu-fullscreen-btn')" style="position: absolute; top: 10px; right: 10px; z-index: 2; font-family: 'Share Tech Mono', monospace; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: #eef5f8; background: rgba(3,6,9,0.78); border: 1px solid rgba(230,238,242,0.38); padding: 6px 12px; cursor: pointer;">⛶ Fullscreen</button>
  </div>

  <p style="text-align: center; font-style: italic;">Interactive menu flow concept</p>

  <div style="position: relative; width: 100%; max-width: 960px; aspect-ratio: 16 / 9; margin: 1.5em auto; border: 1px solid rgba(255,255,255,0.15);">
    <iframe id="coldlight-hud-frame" src="/projects/Coldlight/coldlight-hud-ingame.html" title="COLDLIGHT interactive HUD prototype" style="width: 100%; height: 100%; border: 0; display: block;" loading="lazy" allowfullscreen></iframe>
    <button id="coldlight-hud-fullscreen-btn" onclick="coldlightToggleFullscreen('coldlight-hud-frame', 'coldlight-hud-fullscreen-btn')" style="position: absolute; top: 10px; right: 10px; z-index: 2; font-family: 'Share Tech Mono', monospace; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: #eef5f8; background: rgba(3,6,9,0.78); border: 1px solid rgba(230,238,242,0.38); padding: 6px 12px; cursor: pointer;">⛶ Fullscreen</button>
  </div>

  <script>
    function coldlightToggleFullscreen(frameId, btnId) {
      var frame = document.getElementById(frameId);
      if (!document.fullscreenElement) {
        (frame.requestFullscreen || frame.webkitRequestFullscreen).call(frame);
      } else {
        (document.exitFullscreen || document.webkitExitFullscreen).call(document);
      }
    }
    document.addEventListener('fullscreenchange', function () {
      ['coldlight-menu-fullscreen-btn', 'coldlight-hud-fullscreen-btn'].forEach(function (id) {
        var btn = document.getElementById(id);
        if (!btn) return;
        var frame = btn.previousElementSibling;
        btn.textContent = (document.fullscreenElement === frame) ? '⛶ Exit Fullscreen' : '⛶ Fullscreen';
      });
    });
  </script>

  <p style="text-align: center; font-style: italic;">Interactive HUD concept (Splinter Cell: Blacklist gameplay used as background)</p>

  <h2>Wireframes</h2>
  <p>A set of annotated UI wireframes documenting the complete interface architecture: the in-game HUD across its
  two core states (default play and equipment wheel open) and the full six-screen menu flow (title, options,
  mission select, intel briefing, loadout/ready-up, and mission debrief). Each frame maps element placement,
  anchoring logic, and interaction behaviour - which elements are persistent versus toggleable, how world-space
  markers tether to their targets, the slot-then-item assignment model in the loadout, the hold-to-commence pattern
  shared between menus and HUD, and the sequencing of animated reveals (decode text, count-up telemetry, badge
  stamping). Produced alongside the interactive prototype above as system documentation, the wireframes separate
  structural decisions from visual treatment so each can be critiqued and iterated independently, and served as the
  implementation reference for the UMG build. Delivered as an eight-page annotated PDF using standard wireframe
  conventions (solid for persistent containers, dashed for contextual elements, numbered behavioural callouts).</p>

  <div style="width: 100%; max-width: 960px; margin: 1.5em auto;">
    <embed src="/projects/Coldlight/coldlight-wireframes.pdf" type="application/pdf" style="width: 100%; height: 80vh; border: 1px solid rgba(255,255,255,0.15); display: block;" />
    <p style="text-align: center; margin-top: 0.5em;">
      <a href="/projects/Coldlight/coldlight-wireframes.pdf" target="_blank" class="button-link">Open / Download PDF</a>
    </p>
  </div>

  <h2>Engine, Date &amp; Role</h2>
  <p>Built as an interactive HTML/CSS/JS motion prototype in 2026, as pre-production UI design for an Unreal Engine
  5 project (UMG/CommonUI target, C++ and Blueprint on a GASP ALS base). Solo project under PhantomWire Games.</p>

  <h2>Skills Used</h2>
  <p>UI/UX design and engineering, motion design (draw-on line animations, text decode/scramble effects,
  orchestrated reveal/hide sequences), design systems thinking (state-driven theming through design tokens, mapping
  one-to-one to CommonUI style assets), procedural UI construction (radial menu geometry generated from data),
  interaction design for gamepad-first radial selection, and rapid interactive prototyping ahead of UMG
  implementation.</p>
---
