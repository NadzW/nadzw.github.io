---
layout: project
permalink: /portfolio/voxel-3d-map-ui/
title: "Voxel 3D Map UI"
role: "Systems & Rendering Programmer (Solo)"
company: "PhantomWire Games"
date: August 2026
engine: "Unreal Engine 5"
banner: /assets/images/card-voxel.webp
header:
  teaser: /assets/images/card-voxel.webp
  og_image: /assets/images/card-voxel.webp
skills:
  - C++ Systems Programming
  - Custom HLSL Shaders
  - GPU Scene Capture & Rendering
  - Point Cloud Processing
  - Data-Driven Asset Pipelines
  - Editor Tooling & Build Pipelines
summary: >
  <p>A fully procedural 3D voxel map system, built after Kojima Productions' GDC talk on Death
  Stranding 2's 3D UI Voxel Map. The level is captured, converted into voxel data, and rendered back out as an
  interactive map - with no hand-placed artist assets, and no manual re-authoring when the level changes.</p>

  <figure>
    <a href="/assets/images/blogs/voxel/map-overview.webp" class="image-popup">
      <img src="/assets/images/blogs/voxel/map-overview.webp" alt="My COLDLIGHT voxel map showing an elevated overview of the captured canyon terrain against a black background" width="2316" height="1316" loading="lazy" decoding="async">
    </a>
    <figcaption>My voxel map running in COLDLIGHT, viewed from an elevated camera angle. The banner above shows a closer view of the same map.</figcaption>
  </figure>

  <p>The pipeline captures the level from a grid of hemisphere and slice views, culls geometry nothing can ever
  see, and packs what's left into a hierarchy of voxel blocks. Those blocks bake into a portable Data Asset, then
  render back out through a custom ray-box HLSL shader doing per-block voxel stepping and distance-based LOD
  blending, all driven by an isolated scene capture with a free-orbiting camera feeding a UMG map widget.</p>

  <figure>
    <a href="/assets/images/blogs/voxel/billboard-wireframe.webp" class="image-popup">
      <img src="/assets/images/blogs/voxel/billboard-wireframe.webp" alt="My Unreal Editor wireframe view of the voxel map, annotated to explain the billboarded planes and custom voxel shader" width="2560" height="1380" loading="lazy" decoding="async">
    </a>
    <figcaption>A wireframe view of my renderer. The voxel detail is resolved by a custom shader on billboarded planes rather than a separate cube mesh for every voxel.</figcaption>
  </figure>

  <p>Getting hundreds of thousands of voxel instances to render smoothly (and to survive an editor session
  without freezing Unreal's selection or undo systems) took as much problem-solving as the rendering itself.
  Custom editor tooling (a full "Build Voxel 3D World" pipeline registered through Unreal's own build-menu system) 
  turns the whole capture-to-render process into a single click.</p>

  <figure>
    <a href="/assets/images/blogs/voxel/canyon-voxel-detail.webp" class="image-popup">
      <img src="/assets/images/blogs/voxel/canyon-voxel-detail.webp" alt="A close development view of voxel detail across the canyon rock faces in my Unreal Engine project" width="631" height="566" loading="lazy" decoding="async">
    </a>
    <figcaption>A development view of voxel detail across the canyon's rock faces.</figcaption>
  </figure>

  <div class="blog-card">
    <h3>📘 Full Devlog</h3>
    <p>Hemisphere capture, occluder culling gone wrong on cliff faces, a 673K-instance editor freeze, and the
    UE-objects-that-don't-survive-a-save bugs that took a whole session to run down.</p>
    <a href="/blog/voxel-3d-ui-map/" class="button-link">Read the Devlog</a>
  </div>
---
