---
layout: project
permalink: /portfolio/payday3-modding/
title: "PAYDAY3 Modding"
role: "Reverse Engineer and Modmaker"
company: "Solo"
date: September 2023
end_date: January 2025
engine: "Unreal Engine 4"
banner: /assets/images/banner-pd3.png
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
  <p>As a dedicated modder for Payday 3, I've developed and maintained over 19 mods that enhance gameplay, user interface, 
  and overall player experience. My work involves deep integration with Unreal Engine 4, utilizing tools like UE4SS and Lua 
  scripting to implement client-side modifications. These projects showcase my proficiency in programming, UI/UX design, and 
  collaborative development within the modding community.</p>
  
  <p> My mods have collectively garnered thousands of downloads and positive feedback from the Payday 3 community. 
  Notably, Advanced Photo Mode was highlighted in a <a href="https://www.reddit.com/r/paydaytheheist/comments/1cij2em/some_good_payday_3_mods_for_people_who_hop_in/">Reddit discussion</a>
  as a must-have mod for players seeking enhanced visual experiences; and VanillaHUD+ was showcased by a <a href="https://youtu.be/D1hon5I4eR4?si=tgruTT9OsPOr-QCp&t=187">popular and partnered Payday 3 youtuber</a>
  in one of their videos and was stated to be the mod that "inspired" them to make the whole video on mods that are saving the game. 
  
  <h3>VanillaHUD+</h3>
  <img src="https://storage.modworkshop.net/mods/images/cWYE3FKfnPWxKe1WMS2TnRHUKQdYKojMU2iJB27a.webp"></img>
  <p> A mod that enhances the default HUD by adding detailed crew status, ammo counters, equipment indicators, and dynamic portraits.
  This is currently one of the only Payday 3 HUD mods and is one of the most popular mods in the game (as of typing this). </p>
  
  <p>With this mod, I worked with a concept artist who created a concept for how the UI should be laid out. Working closely, and 
  relying on them for continuous feedback, I spent a lot of time on this project reverse engineering and recreating the games
  base game HUD in a dummy generated UE4 project to then be able to make our modifications to it. While recreating the base games 
  HUD, I noticed that the devs used quite a lot of canvas panels (which is generally inefficient and cause performance loss) so I 
  cleaned up the widgets that I modified and replaced unnecessary canvas panels with overlay panels or horizontal/vertical boxes, 
  etc... I've had responses from a couple of users that confirmed that their games performs a lot better with this mod compared to
  vanilla.</p>
  
  <a href="https://modworkshop.net/mod/50675">View mod</a>
  
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
  
  <h3>Advanced Photo Mode</h3>
  <img src="https://storage.modworkshop.net/mods/images/GaKZPwypfmQcxIpKrVtSl3IAyHRa5cWK7JaqPjiA.webp"></img>
  <p>Introduces a photo mode with free camera and gallery features for capturing in-game moments. </p>

  <p>This mod started off as a test to see whether Unreal Market plugins can be added to the game. This mod
  adapts and modifies <a href="https://www.fab.com/listings/b955ab23-f61e-44a6-8554-e730bc3af7f5">Advanced Photo Mode</a>
  plugin to work with the game. It is mainly used by the community for taking fun screenshots and thumbnails for videos.</p>

  <a href="https://modworkshop.net/mod/47502">View mod</a>
  
  <h3>Corpse Despawn</h3>
  <img src="https://storage.modworkshop.net/mods/images/DiyflDhr0vghNXoFXHtGLctkcSojTRTQOGihohwE.webp"></img>
  <p>Improves game performance by making corpses despawn after death, without affecting stealth gameplay. </p>

  <a href="https://modworkshop.net/mod/47526">View mod</a>
  
  <h3>Hacking Minigame (UNRELEASED)</h3>
  <iframe width="640" height="360" src="https://www.youtube.com/embed/5HgyCFJu4oY?si=XybvKn2jTe1K_T1r" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  <p>This was a solo project where I completely reworked one of the base game mechanics to make it more fun and accessible.
  In the base game, you would have to stand in a circle for a prolonged period of time to progress. In my version, 
  you would get a hacking minigame that would instantly complete the hack rather than standing in a circle. Additionally in this video you can see an unreleased HUD that I was also working ;)</p>
  
  <h3>Weapon Skin Customisation (UNRELEASED)</h3>
  <iframe width="640" height="360" src="https://www.youtube.com/embed/jGOu9gHIrWE?si=7CuvtzVy46zDa1EC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  <iframe width="640" height="360" src="https://www.youtube.com/embed/kuw0yYLlaV4?si=pIAEWSK_zfr4cLVB" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  <p>In this project I worked as the UI designer and programmer along with some of the system programming besides another modder from the community.
  This mod would allow the player to customise the indivual parts of their weapons with custom modded patterns and effects. In the videos,
  you can see a "dark matter" esque pattern I created with blender shaders (due to not being able to have modded materials in the game).</p>
  
  <h3>And many more...</h3>
  <p>I have also made many more mods and also contribute and help the community with their mods whenever I can. 
  For around a year, I also maintained and updated the main generated modding project that the modding community uses as the game
  updated. I also took part in teaching members about the different aspects of modding the game and also made templates and guides for some mods.</p>
  
  <a href="https://modworkshop.net/user/shalashaska1">View more on my ModWorkshop</a>


responsibilities:
  - Updating and maintaining community generated UE4 project
  - Reverse engineering base game functions and blueprint logic
  - Designing and implementing performant and exciting mods
---
