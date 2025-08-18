---
layout: project
permalink: /portfolio/hudcore/
title: "HUDCore"
role: "Reverse Engineer, Modmaker and HUD Developer"
company: "Solo"
date: June 2025
end_date: July 2025
engine: "Unreal Engine 4"
banner: /assets/images/banner-hc.png
images: 
video:
link: 
skills:
  - Unreal Engine 4 Modding
  - Scripting & Programming
  - UI/UX Design
  - Performance Optimization
  - Community Collaboration
summary: >
  <p>One of my most recent modding contributions to PAYDAY3 has been HUDCore. HUDCore is a custom HUD loader and Template HUD project that acts as a framework 
  for modders to be able to easily make custom HUDs. It provides a modular system for building, loading, and managing HUD elements with minimal setup, allowing modders
  to focus on functionality and styling.</p>
  
  <p>Created out of the need for more consistent and flexible UI modding tools, HUDCore is the backbone of multiple community-made HUDs and dramatically simplifies UI design
  in Blueprint environments. </p>
  
  <h2>Features:</h2>
  - <b>Custom HUD Loader:</b> Easily inject custom HUDs into the game using a standardized loader.<br>
  - <b>Widget Management:</b> Build widgets using HUDCore’s API and auto-hook them into the game UI.<br>
  - <b>Update Notifications:</b> Event-based system for reacting to setting changes in real time.<br>
  - <b>Development Tools:</b> Includes default HUD templates and <a href=https://github.com/NadzW/PD3-HUDCore/wiki>documentation</a>.<br>
  
  <h2>What I Did:</h2>
  - <b>Architected the entire HUD framework:</b> This involved reverse engineering and recreating the whole of the games HUD widgets and modifying them to make them modular
  and easily customisable.<br>
  - <b>Developed real-time responsive settings system:</b> Injected custom settings page into the games menu and dynamically loads custom HUDs settings options.<br>
  - <b>Built a template HUD system:</b> Setup an easy to use project that includes a Template HUD that can easily be used by modders as a starting place along with 
  creating editor utility widgets for easily packaging and testing mods and batch renaming assets.<br>
  
  <h3>VanillaHUD+</h3>
  <img src="https://storage.modworkshop.net/mods/images/cWYE3FKfnPWxKe1WMS2TnRHUKQdYKojMU2iJB27a.webp"></img>
  <p> A mod that enhances the default HUD by adding detailed crew status, ammo counters, equipment indicators, and dynamic portraits.
  This is currently one of the only Payday 3 HUD mods and is one of the most popular mods in the game (as of typing this). </p>

  <div class="blog-card">
    <h3>📘 Related Blog Post</h3>
    <p>How VanillaHUD+ was added into the base game for everyone to use!</p>
    <a href="/blog/vanillahudplus/" class="button-link">Read the blog</a>
  </div>
  
  <p>With this mod, I worked with a concept artist who created a concept for how the UI should be laid out. Working closely, and 
  relying on them for continuous feedback, I spent a lot of time on this project reverse engineering and recreating the games
  base game HUD in a dummy generated UE4 project to then be able to make our modifications to it. While recreating the base games 
  HUD, I noticed that the devs used quite a lot of canvas panels (which is generally inefficient and cause performance loss) so I 
  cleaned up the widgets that I modified and replaced unnecessary canvas panels with overlay panels or horizontal/vertical boxes, 
  etc... I've had responses from a couple of users that confirmed that their games performs a lot better with this mod compared to
  vanilla.</p>
  
  <a href="https://modworkshop.net/mod/50675">View mod</a>

  <br>
  <br>

  <a href="https://storage.modworkshop.net/mods/images/BOitqiIVoeQpkhC85Ic19bddkDh0HD9LTCLVjSJP.webp" data-lightbox="vanillahud">
    <img src="https://storage.modworkshop.net/mods/images/BOitqiIVoeQpkhC85Ic19bddkDh0HD9LTCLVjSJP.webp"></img>
  </a>
  
  <h3>Beta HUD</h3>
  <img src="https://storage.modworkshop.net/mods/images/xouM1E31SABIrNFb366a5gCFnK7iFjrZTctCwzvQ.webp"></img>
  <p>A mod that recreates the beta version of the game's HUD, offering a nostalgic and streamlined interface.
  This is also currently one of the only Payday 3 HUD mods and is one of the most popular mods in the game (as of typing this). </p>
  
  <p>After acquiring a beta copy of the games files, I was able to reverse engineer the beta version of the HUD. Similarly to 
  VanillaHUD+, This mod required me to completely recreate the base games HUD in UE4 through reading through decompiled uassets 
  in FModel and kismet (blueprint) analysis to figure out UI logic and layout. After having the base games HUD recreated, I went
  through and made the modifications to the HUD widgets to make it as close as possible to the Beta as I could, while also adding
  newly added UI elements and optimisations. </p>
  
  <a href="https://modworkshop.net/mod/46399">View mod</a>
  
  <br>
  <br>

  <a href="https://storage.modworkshop.net/mods/images/ONlK3UJd2PzWpyfH4bstZifBTf37izUGxsXjGgGc.webp" data-lightbox="betahud">
  <img src="https://storage.modworkshop.net/mods/images/ONlK3UJd2PzWpyfH4bstZifBTf37izUGxsXjGgGc.webp"></img>
  </a>
  
  <br><br>
  
  <a href="https://storage.modworkshop.net/mods/images/nGrK9nL7JPThixJAcivzTSNHfGXasrAipYq5Dkxg.webp" data-lightbox="betahudSettings">
  <img src="https://storage.modworkshop.net/mods/images/nGrK9nL7JPThixJAcivzTSNHfGXasrAipYq5Dkxg.webp"></img>
  </a>
  
  <br>
  
  <h3>PAYDAY THE HEIST HUD</h3>
  <img src="/Assets/Images/PLACEHOLDER_PDTH.png"></img>
  <p>A mod that recreates the first game in the PAYDAY franchise, PAYDAY: The Heist, HUD. </p>

  <p>Working from reference images of the first games HUD, I attempted to recreate the HUD in UE4 to be used in PAYDAY3. While making the HUD, I made some design choices
  and decided to update some elements to match PAYDAY3 more and made it feel more modern. </p>

  <a href="https://modworkshop.net/mod/53322">View mod</a>
  
  <br>
  <br>

  <a href="/Assets/Images/PLACEHOLDER_PDTH2.png" data-lightbox="pdthhud">
  <img src="/Assets/Images/PLACEHOLDER_PDTH2.png"></img>
  </a>
  

responsibilities:
  - Updating and maintaining Template UE4 project and wiki
  - Reverse engineering base game functions, blueprint logic and HUD widgets
  - Designing and implementing modded HUDs
---
