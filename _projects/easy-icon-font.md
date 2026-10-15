---
layout: project
permalink: /portfolio/easy-icon-font/
title: "Easy Icon Font"
role: "Tools Programmer (Solo)"
company: "PhantomWire Games"
date: August 2026
engine: "Unreal Engine 5.1 - 5.8"
banner: /assets/images/banner-eif.png
skills:
  - C++ Editor Tooling (Slate)
  - Python Scripting & Automation
  - Font Engineering (TrueType/OpenType, COLR/CPAL)
  - UMG & Rich Text Systems
  - Cross-Engine-Version Compatibility (UE 5.1-5.8)
  - Doxygen Documentation
summary: >
  <p>Easy Icon Font is a C++ and Python editor plugin for Unreal Engine that converts PNG, SVG, or WEBP icons
  directly into font glyphs - no external website or converter tool required. It auto-detects the best
  representation per icon: single-color SVGs become real vector glyph outlines, flat multi-color art gets
  quantized and traced into full-color COLR/CPAL glyphs, and anything that doesn't vectorize cleanly falls back
  to a plain texture automatically.</p>

  <a href="https://www.fab.com/listings/cda48a12-12f6-453a-9de9-16a1131f2e22" target="_blank" rel="noopener" class="button-link">View on Fab</a>

  <p>Icons built this way behave like real characters: they inherit text color, scale to any size with zero
  blur, and drop straight into rich text via a custom decorator - <code>&lt;icon id="icon_bell" size="32"
  color="#FF3B30"/&gt;</code> renders inline with the surrounding sentence. The whole pipeline runs from a
  custom Slate editor window with a live preview bar, one-click dependency setup, and automatic DataTable
  syncing, so adding or removing an icon is a single click rather than a manual asset-wrangling process.</p>

  <a href="/assets/images/blogs/eif/glyph-overview.jpg" data-lightbox="eif-glyph-overview">
  <img src="/assets/images/blogs/eif/glyph-overview.jpg"></img>
  </a>
  <p style="text-align: center; font-style: italic;">Same icons, rasterized as font glyphs instead of a texture atlas - a fraction of the storage.</p>

  <iframe width="640" height="360" src="https://www.youtube.com/embed/iJ8ofo_JZPM" title="Easy Icon Font overview" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

  <p>The plugin ships with a ready-to-use icon widget and rich text decorator pre-wired out of the box, targets
  Unreal Engine 5.1 through 5.8 with version-guarded code paths for engine API differences across that range,
  and includes full Doxygen documentation covering setup, the widget/rich-text reference, and troubleshooting.</p>

  <p>More clips and screenshots are posted on <a href="https://x.com/I_AmShalashaska/status/2085828262098337888?s=20" target="_blank" rel="noopener">X / Twitter</a>.</p>

  <div class="blog-card">
    <h3>📘 Full Devlog</h3>
    <p>Font glyphs vs textures, proving COLR/CPAL actually works, the bearing bug that made icons render as ghostly double exposures, and shipping across six Unreal engine versions.</p>
    <a href="/blog/easy-icon-font/" class="button-link">Read the Devlog</a>
  </div>
---
