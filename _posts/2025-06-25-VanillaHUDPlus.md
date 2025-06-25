---
title: "How a mod I created for PAYDAY 3 made it into the base game"
excerpt: "VanillaHUD+ was added to PAYDAY3"
layout: single
permalink: /blog/vanillahudplus/
author_profile: false
read_time: true
related: true
categories: 
  - devlog
tags: 
  - postmortem
  - vanillahudplus
  - payday3
---

When I first began working on VanillaHUD+, the goal was simple: improve Payday 3's team HUD while staying true to its 
original visual language. The vanilla HUD at launch was clean but lacked clarity in certain areas. Information like crew 
ammo, tools, placeables and overkill status was missing - making teamwork unnecessarily harder as people would be forced to 
chat or use the chat wheel to ask for extra ammo.

I wanted to address that.

### The Design Philosophy Behind VanillaHUD+

Working with an incredible concept artist from the community ([FlamingFury1000](https://modworkshop.net/user/FlamingFury1000)),
the vision behind VanillaHUD+ was to enhance readability, efficiency, and consistency, while keeping the aesthetic 
“vanilla-adjacent.” No flashy effects, no intrusive overlays—just cleaner layout logic and a focus on what matters during gameplay.

Some of the key changes I introduced:
- Compact but rich player panels that emphasized key stats like renown level, ammo count, and equipment and overkill status.
- Dynamic Heister Potraits that change whether the player is masked or unmasked
- Hostage counter
- Visual hierarchy tweaks to reduce clutter and boost legibility.
- Performance optimisation by removing overused canvas panels.

![modpreview.gif](/assets/images/blogs/vhudplus/modpreview.gif)

![maskpreview.gif](/assets/images/blogs/vhudplus/maskpreview.gif)

![hostagecounter.gif](/assets/images/blogs/vhudplus/hostagecounter.gif)

### What was introduced in patch 2.1.4

The official patch 2.1.4 arrived with changes that closely mirror many decisions from VanillaHUD+. With renown levels now showing,
and the crewmates health and armor status being extended to also show ammo and equipment. The visual style with these changes are 
almost identical with VanillaHUD+ albeit some padding differences. The new update also introduced some very nice changes to the 
defeat states and downed counter which differ from VanillaHUD+. However, the dynamic heister potraits and overkill status seen in 
VanillaHUD+ are still missing from the base game.

<a href="/assets/images/blogs/vhudplus/comparisons.jpeg" data-lightbox="hud-comparison">
  <img src="/assets/images/blogs/vhudplus/comparisons.jpeg" alt="PD3 HUD Comparison" />
</a>

### Coincidence or Community Inspiration?

I don’t work at Starbreeze. I don’t have insider access. So I can't say for certain whether my mod directly inspired these official 
changes.

But the similarities are striking.

It’s entirely possible that the Payday 3 team had similar goals, arrived at the same usability issues, and reached similar 
conclusions. Or—maybe—they saw what VanillaHUD+ offered and decided to integrate those ideas into the base game. 
Either way, it's an encouraging sign.

This is what I love about modding. It’s not just a personal sandbox. It’s a dialogue. Sometimes, that dialogue loops back
into the very games we love.

### Technical Notes
VanillaHUD+ was developed using:
- Figma
- Unreal Engine 4.27
- Custom Blueprint + UMG injections
- Widget recreations from Uasset and [Kismet Analysis](https://github.com/trumank/kismet-analyzer)

This project was not just a UI reskin - it required reverse engineering, hooking into HUD elements, and patching logic without 
breaking the core game loop. It was both a design and technical challenge.

### What I Learned

This experience deepened my skills in:
- UI/UX design for real-time games
- Working within tight engine constraints
- Respecting and extending a game’s original artistic direction
- Creating tools and mods that are maintainable post-patch

It also reminded me that modding can be more than a hobby - it can be a form of prototyping, feedback, and design leadership.

### Final Thoughts

Whether the similarities are coincidental or not, it's incredibly rewarding to see a personal project align with an official design 
direction. If nothing else, it validates the design choices I made and reaffirms my belief in the power of player-driven iteration.

If you're a developer, studio, or team interested in design-focused, technically capable talent - I'm currently open to 
opportunities. Let's build better HUDs and UI, together.

Although it is now outdated, you can still view [VanillaHUD+ mod on ModWorkshop](https://modworkshop.net/mod/50675)