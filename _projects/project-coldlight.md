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
  <p>The prototype below is fully interactive - use the panel on the right (or keyboard shortcuts <b>1-5</b> for
  individual elements, <b>O</b> for the objective display, and <b>Q</b> for the equipment wheel) to switch between
  awareness states and toggle HUD elements on and off.</p>

  <div style="position: relative; width: 100%; max-width: 960px; aspect-ratio: 16 / 9; margin: 1.5em auto; border: 1px solid rgba(255,255,255,0.15);">
    <iframe id="coldlight-hud-frame" src="/projects/Coldlight/coldlight-hud-ingame.html" title="COLDLIGHT interactive HUD prototype" style="width: 100%; height: 100%; border: 0; display: block;" loading="lazy" allowfullscreen></iframe>
    <button id="coldlight-fullscreen-btn" onclick="coldlightToggleFullscreen()" style="position: absolute; top: 10px; right: 10px; z-index: 2; font-family: 'Share Tech Mono', monospace; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: #eef5f8; background: rgba(3,6,9,0.78); border: 1px solid rgba(230,238,242,0.38); padding: 6px 12px; cursor: pointer;">⛶ Fullscreen</button>
  </div>

  <script>
    function coldlightToggleFullscreen() {
      var frame = document.getElementById('coldlight-hud-frame');
      if (!document.fullscreenElement) {
        (frame.requestFullscreen || frame.webkitRequestFullscreen).call(frame);
      } else {
        (document.exitFullscreen || document.webkitExitFullscreen).call(document);
      }
    }
    document.addEventListener('fullscreenchange', function () {
      var btn = document.getElementById('coldlight-fullscreen-btn');
      if (!btn) return;
      btn.textContent = document.fullscreenElement ? '⛶ Exit Fullscreen' : '⛶ Fullscreen';
    });
  </script>

  <p style="text-align: center; font-style: italic;">Interactive HUD concept (Splinter Cell: Blacklist gameplay used as background)</p>

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
