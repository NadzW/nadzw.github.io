---
layout: project
permalink: /portfolio/payday3-modkit/
canonical_url: https://nadeemwali.com/portfolio/payday3-modkit/
title: "PAYDAY 3 Modkit & Modloader"
excerpt: "Rebuilding PAYDAY 3's modding ecosystem with a custom Unreal Engine 5 editor, integrated development tools and an in-game modloader."
role: "Engine Programmer, Tools Programmer, UI Engineer"
company: "Community Development"
date: 2026-08-01
end_date: Present
engine: "Unreal Engine 5.5.4"
classes: pd3-case-study
banner: /assets/images/banner-pd3-modkit.webp
header:
  teaser: /assets/images/card-pd3-modkit.webp
  og_image: /assets/images/banner-pd3-modkit.webp
summary: >
  Rebuilding PAYDAY 3's modding ecosystem with a custom Unreal Engine 5 editor, integrated development tools and an in-game modloader.
---

<dl class="pd3-facts">
  <div><dt>My role</dt><dd>Engine programming, editor tools, runtime systems and technical UI</dd></div>
  <div><dt>Project</dt><dd>PAYDAY 3 Modding, community-developed tooling</dd></div>
  <div><dt>Technologies</dt><dd>UE 5.5.4, C++, Blueprints, Slate, UMG, C# and Unreal Automation Tool</dd></div>
  <div><dt>Period and status</dt><dd>2026 to present. Active development; PDML in public early access.</dd></div>
</dl>

<div class="pd3-links">
  <a class="button-link" href="{{ '/blog/rebuilding-payday3-modding/' | relative_url }}">Read the development blog</a>
  <a class="button-link" href="https://payday3-modding.github.io/Documentation/payday3-modding/index.html">Explore the documentation</a>
</div>

## From restoring tools to rebuilding the workflow

PAYDAY 3's August 2026 move from Unreal Engine 4.27 to UE5 disrupted the community's established modding environment. I had already developed VanillaHUD+, HUDCore and other mods, and helped maintain the earlier UE4 project. Recreating that environment was the starting point. Building a more capable development workflow became the goal.

I developed the custom UE5 compatibility editor, the Modkit project, CrimeForge and PDML, working across C++ engine compatibility, editor extensions, Blueprint systems and interface design. I also integrated and extended the Suzie-based native class workflow, built templates and documentation, and supported creators through setup and troubleshooting. Other developers, testers and documentation contributors have helped shape and validate the ecosystem.

{% include pd3-toolchain.html %}

## A custom editor that understands installed game content

The custom Unreal Engine 5.5.4 editor mounts supported retail containers, including IoStore content, from the user's installed PAYDAY 3. With the game path, AES key, current `AssetRegistry.bin` and reflected native types configured, supported assets appear at their original `/Game/...` paths in the Content Browser.

My compatibility work bridges cooked package loading, editor assumptions, dependency resolution and game-specific serialization. It includes registry preservation during refreshes, cooked Blueprint references, supported levels, thumbnails, material instances, skeletons, animations and fonts. Modders can inspect supported content in context instead of manually reconstructing every dependency before starting.

{% include pd3-figure.html path="/assets/images/blogs/pd3-modkit/editor-heist.webp" width="2537" height="1351" alt="A PAYDAY 3 heist opened in the custom Unreal Editor, with level geometry and game-specific World Settings visible" caption="A supported retail heist opened for inspection in the custom editor. Screenshot from the community announcement." %}

<div class="pd3-callout" markdown="1">
**Installed content stays external.** This is a community development environment, not PAYDAY 3's original source project. Users need their own installed game and local configuration. Mounted base-game assets generally remain read-only references; modders author their own content and supported replacements separately.
</div>

### Reconstruction with explicit limits

Supported assets can be converted to uncooked copies for replacement workflows. Simpler types can be reconstructed substantially. Complex Blueprints and widgets provide a partial starting point: recoverable defaults, parent classes, components, references and widget structure. Original Blueprint graphs and native function implementations are not automatically recovered.

Widget reconstruction is especially relevant to my UI engineering work. A replacement must respect game-specific parent types, properties and named bindings. Recovering a widget tree is only part of that problem; binding metadata, animation data and tick behaviour also affect compatibility. This remains active development.

{% include pd3-figure.html path="/assets/images/blogs/pd3-modkit/widget.webp" width="2560" height="1440" alt="A reconstructed PAYDAY 3 widget displayed in Unreal's Widget Blueprint editor" caption="Recovering useful widget structure without the original graph source. Reconstruction depth varies by asset and editor build." %}

The environment also supports demonstrated custom material, mesh and animation workflows. The team's material baking work helps interpret layered colour and mask systems, with remaining visual limitations. Modified Wwise integration supports compatible audio workflows when users supply the matching third-party SDK files.

## Suzie: resolving the missing native types

Cooked assets reference PAYDAY-specific native classes unavailable in a standard Unreal project. I integrated and extended a workflow built on the existing JMAP and Suzie projects to expose those reflected types in the editor.

`GenerateJmap.bat` gathers metadata from a locally running game. Suzie reads the JMAP in `Content/DynamicClasses/` at startup, then generates and registers compatible class representations. That enables supported Data Assets and Blueprints to resolve game types, including the `SBZ` family and `PD3HeistDataAsset`. Reflection supplies structure, not the game's original C++ implementation.

Keeping the JMAP and Asset Registry current makes ordinary game updates easier to maintain. Engine-level changes can still require compatibility fixes. [Read the architecture documentation](https://payday3-modding.github.io/Documentation/payday3-modding/modkit/architecture.html).

## CrimeForge: authoring, validation and packaging

I developed CrimeForge as a PAYDAY-specific Unreal Editor plugin. Its Slate interface brings searchable mod cards, metadata, configuration, Content Browser navigation, compatible legacy imports and Forge actions into one place. Each mod is an Unreal plugin with its own namespace.

The current **Blank**, **Logic Mod** and **Custom Heist** templates create the relevant plugin and registration assets. Logic mods receive a Blueprint derived from PDML's `ModActorBase`. Heists receive a level and `PD3HeistDataAsset` with their initial references connected. HUD and audio templates remain planned.

{% include pd3-figure.html path="/assets/images/blogs/pd3-modkit/crimeforge-dashboard.webp" width="659" height="435" alt="CrimeForge's editor dashboard with mod cards and creation, import and Forge controls" caption="CrimeForge makes the mod plugin the centre of the authoring and build workflow." %}

Forge validates the selected mod before cooking, checking Primary Asset Labels, Asset Manager registration, chunk configuration, explicit assets, dependencies and stale template references. Blocking errors and warnings are presented separately; recognised configuration issues can be repaired where safe.

The packaging challenge goes beyond wrapping Unreal's build button. Plugin-owned assets and replacements at original `/Game/...` paths need different handling. CrimeForge uses a plugin cook for ordinary content, adds a project replacement cook when required, and combines the selected output into one PDML-compatible plugin. Plugin-only mods skip that extra pass. Custom C# automation runs through Unreal Automation Tool, with optional copying into the game and launching for testing.

## PDML: runtime systems and player-facing UI

I developed PDML as the runtime counterpart to CrimeForge. Complete plugin mods live under `PAYDAY3/Mods/<ModName>/`, keeping their descriptors and cooked containers together. PDML provides supported registration, initialization and Blueprint APIs for logic mods, custom UI, settings and custom-heist foundations.

{% include pd3-figure.html path="/assets/images/blogs/pd3-modkit/installedmods.webp" width="2559" height="1439" alt="PDML's in-game interface showing information about installed compatible mods" caption="The early-access Mod Manager exposes installed mod information and links to download pages." %}

The in-game Mod Manager and integrated settings framework connect my runtime and UI work. Authors expose configuration through shared APIs; players adjust supported mods inside the game. This builds on my earlier HUDCore and VanillaHUD+ experience while removing repeated integration work for other creators.

## Engineering challenges and current boundaries

The hardest problems sit between systems: making cooked packages useful to an editor, registering compatible native types, preserving references during reconstruction, and packaging multiple content roots without pulling unrelated game assets into a mod.

Custom heists now have an authoring and registration foundation, but advanced AI navigation, AirNav and some game behaviours need more work. Runtime plugin mounting and Asset Registry discovery are ongoing investigations. Reliable dynamic unloading, load ordering, enable/disable controls, hot-swapping and in-game ModWorkshop downloads are not established public-release features.

## Community, documentation and recognition

The result is a publicly documented path from local setup to a forged, testable plugin mod. I help maintain Antora and AsciiDoc guides covering the editor, reflection, CrimeForge, PDML, replacements and custom heists. Community feedback helps turn individual compatibility discoveries into repeatable workflows.

The [announcement credits](https://payday3-modding.github.io/Documentation/payday3-modding/announcement.html#_contributions) recognise contributions including Columbus's material baking work, Capcake's custom-heist documentation and testing, Pinki_Ninja's documentation and testing, and wider compatibility testing. FlamingFury1000 contributed future Mod Manager concept UI; that concept is distinct from the current interface shown here.

Beyond the response from the modding community, the project has received positive acknowledgement from Starbreeze, the developers of PAYDAY 3. They have expressed appreciation for the work and an interest in supporting the effort further, although I can't share additional details at this stage. The toolchain remains community-developed.

## Project resources

- [PD3 Modkit and public tool source](https://github.com/PAYDAY3-Modding/PD3-Modkit)
- [Documentation and getting started](https://payday3-modding.github.io/Documentation/payday3-modding/index.html)
- [Custom editor and compatibility documentation](https://payday3-modding.github.io/Documentation/payday3-modding/modkit/custom-editor.html)
- [PDML early-access release on ModWorkshop](https://modworkshop.net/mod/59255)
- [PAYDAY 3 Modding organisation](https://github.com/PAYDAY3-Modding)

The [full development article]({{ '/blog/rebuilding-payday3-modding/' | relative_url }}) explains the engineering decisions in more depth. My [earlier PAYDAY 3 Modding project]({{ '/portfolio/payday3-modding/' | relative_url }}) covers the UE4-era work that preceded this toolchain.

Screenshots above come from the [public community announcement](https://payday3-modding.github.io/Documentation/payday3-modding/announcement.html). The banner and portfolio thumbnail are from the [PDML ModWorkshop page](https://modworkshop.net/mod/59255?tab=images).
