---
title: "Building Easy Icon Font: Fonts, COLR Glyphs, and the Bug That Made Every Icon Render Twice"
excerpt: "How an in-editor Unreal tool for baking PNG/SVG icons into real font glyphs turned into mono-vector outlines, multi-layer COLR/CPAL color glyphs, a double-exposure rendering bug, and shipping across six engine versions."
layout: single
permalink: /blog/easy-icon-font/
author_profile: false
read_time: true
related: true
categories:
  - devlog
tags:
  - easy-icon-font
  - phantomwire
  - unreal-engine
  - tools-programming
---

I didn't set out to build a font engineering tool. I set out to stop tabbing over to an external website every time I needed a new icon in my UI font. One afternoon of that friction was enough - so instead of opening yet another browser tab, I spent a day building an in-editor tool for Unreal that does the conversion itself. That day turned into a proper plugin, a custom Slate editor window, a Python font-building backend, and - because nothing in engine tooling is ever as simple as it first looks - a multi-day chase after a bug that made color icons render as ghostly double exposures of themselves.

---

## Why fonts for icons at all

The pitch is simple: an icon baked into a font behaves like a character. It inherits the color of the surrounding text for free. It scales to any size with zero blur, because it's a vector outline, not a bitmap. And it drops straight into rich text - `<icon id="icon_bell"/>` sits inline with a sentence the same way a letter does, no manual placement code required.

The honest tradeoff is that a texture atlas is simpler, more predictable in memory, and doesn't need any of the font-table machinery a glyph does. I don't think icon fonts are a universal answer - for a fixed icon grid like a HUD or inventory screen, an atlas is probably the better call. But for icons that need to sit inline with text at arbitrary sizes, or that need to stay pixel-sharp at every zoom level a UI might throw at them, a font earns its keep. So the plugin ended up supporting both: build a glyph when it makes sense, fall back to a texture when it doesn't.

## Three ways to bake an icon

Not every icon source is the same kind of problem, so the tool auto-detects which of three representations actually fits:

- **Mono-vector** - a single flat-color SVG, extruded directly into a real `glyf` outline. Infinitely scalable, tinted by whatever text color it's displayed with, exactly like a letter.
- **Color-vector** - flat, limited-palette raster art (or multi-fill SVG), quantized down to a small palette, traced region by region, and rebuilt as a multi-layer COLR/CPAL glyph. Still a true vector outline underneath - the color is just baked in rather than inherited.
- **Texture fallback** - anything that doesn't vectorize cleanly (gradients, photographic detail) just becomes a plain `UTexture2D`, no font involved.

The mono-vector path was the easy part - parse the SVG path data, walk the contours, hand them to a `TTGlyphPen`, done. Getting even-odd SVG fill rules to survive the trip into TrueType's nonzero winding rule needed some care (a shape with a hole, like a letter "O" traced from a gear icon, needs its inner contour wound the opposite direction from its outer one, or the hole just fills solid) - but that was a solvable, well-understood problem.

Color-vector was where things got interesting.

## Proving COLR actually worked before building on it

COLR/CPAL - the OpenType extension that lets a single glyph be built from multiple colored outline layers stacked on top of each other - isn't something every font renderer supports. Rather than build a whole quantize-and-trace pipeline on a hopeful assumption, I hand-built a tiny two-layer test font first: a red square behind a blue triangle, nothing else. Dropped it straight into a UMG Text Block in the editor.

It rendered in color, correctly layered. That one test was worth more than any amount of documentation-reading - it confirmed Slate's FreeType build genuinely reads COLR data, not just `glyf`, before I sank real time into the segmentation and tracing logic on top of it.

## The bug that made icons render twice

Once the real pipeline was built - PIL for quantization, `scikit-image` for contour tracing, `fontTools` for assembling the layers - I fed it a badge icon: a navy ring, a blue circle inside it, a gold star on top. Three flat colors, nested regions, a reasonable real-world test case.

What came back looked like a double exposure. The ring showed up as an uneven crescent instead of a clean circle. The star had a ghost copy of itself offset diagonally, like the whole icon had been photographed through a slightly misaligned second lens.

My first theory was a seam problem - each color traced independently, so maybe the boundary between two adjacent colors was getting simplified slightly differently on each side, leaving a sliver of a gap. I added mask dilation to make adjacent layers overlap instead of almost-touch. No change. Same ghosting, pixel-for-pixel identical to before.

That result was actually the useful part - it meant the bug wasn't in the boundary between colors, since a boundary fix had done nothing. I built a small standalone renderer to composite the layers by hand and inspect the raw glyph coordinates directly, bypassing every layer of assumption in between. And there it was: each color layer's `xMin` didn't match its `hmtx` left-side-bearing. I'd hardcoded every layer's bearing to zero, but `TTGlyphPen`-built glyphs don't have their bounds populated at build time - and a font renderer uses the mismatch between a glyph's declared bearing and its actual outline position to *shift* the glyph horizontally. Every layer was getting shifted by a different, wrong amount, because every layer's shape sat at a different true position and I'd told the renderer to pretend they all started at zero.

The fix was small once found - compute the real bearing from each layer's own geometry instead of assuming zero - but finding it meant ruling out two entire categories of plausible-sounding explanations first. The dilation attempt wasn't wasted, either; it's still in the pipeline today as a genuine safety margin against seam gaps, it just wasn't *this* bug.

## Don't ship the file people are actively editing

Somewhere in testing, icons that had worked days earlier started rendering as the classic Windows "tofu" placeholder - the black box with PRIVATE USE stamped on it that shows up when a codepoint can't resolve to a real glyph. The font file itself turned out to be fine. The actual problem was upstream of the font entirely: every plugin update I'd shipped included the *live, working* font file as part of the package. Extracting a new zip over an existing install silently overwrote whatever icons had already been built - there was no warning, no conflict prompt, just a quiet reset back to the shipped version.

The fix became a small architectural rule: never ship the file a user is actively editing. The plugin now ships a blank template font that's safe to overwrite on every update, and a one-time Setup step clones it into the real working file *only if that file doesn't already exist*. Simple in hindsight, but it took losing test data to actually notice the update process was destructive in the first place.

## Supporting six engine versions is its own project

Once the plugin worked, "does it work" turned out to have a much longer tail than expected. Building across Unreal 5.1 through 5.8 surfaced a small museum of version-specific problems: an `FString` factory method that didn't exist yet in 5.1, a UPROPERTY category rule that's only enforced under Engine-module packaging (never during normal project compiles, so it stayed invisible until the first real distribution build), a `FJsonObject` internal type that changed between engine releases, an MSVC toolset UE 5.2 specifically wasn't validated against, an Android NDK mismatch on 5.1. None of these were bugs in the plugin's own logic - each one was a real API or toolchain difference that only a genuine build against that specific engine version would ever catch.

That eventually justified building a second piece of tooling alongside the plugin itself: a PowerShell script that builds, tests, and packages against every supported engine version in one pass, with a live dashboard and a final report listing the actual warning and error lines per version - not just a pass/fail count. Getting *that* script running reliably turned into its own small debugging arc, including a genuinely memorable one: `.ps1` files need a UTF-8 byte-order mark for Windows PowerShell to read certain characters correctly, and losing that one invisible marker was enough to make an otherwise-correct script throw parser errors on lines that had nothing wrong with them at all.

## Where it landed

The finished plugin is a custom Slate editor window - not a Blueprint utility widget, an actual native tool - with a live preview bar reading straight from the font file, a dependency status light, one-click Setup, and automatic DataTable syncing so the DataTable powering the icon lookup never drifts out of sync with the font itself. A rich text decorator lets icons sit inline with prose, with per-instance size and color overrides. Full Doxygen documentation covers setup, the widget and rich-text reference, and troubleshooting for the handful of failure modes real usage actually turned up.

It's on Fab now, still under active iteration - the SVG-native color-vector path (skipping the raster round-trip entirely for icons that started as multi-color SVGs) is the next thing on the list.
