---
title:  "On the aseprite to unity animation workflow"
layout: posts
---
For a long time I've struggled with how unity handles sprite-based animations. The usual process is actually reaaally bad. Considering you're creating your animations in aseprite, first you have to export it to .png, then if you're lucky: you have some tools to cut the spritesheet for you, if not, you have to set it all up manually. Then you create a clip and manually set up timings for all frames. It takes more or less 10 mins per clip per change to get from the ground up. Longer, if you're not very familiar with it. 

Other than that you either have to hold all your .ase files somewhere for the whole project duration cause otherwise you won't have access to layer data.

This. Is. Terrible.

But there are ways to avoid this whole madness. There actually pretty nice tools that not only let you work with .ase files directly. It times it exactly as you do it in aseprite and creates seperate animation from each tag. Meaning - no more clip creation and you can even eddit assets at runtime.

The asset is free, and it's the best asset I own and I cannot really recommend it enough. It might not work on very new versions of unity, as it wasn't really updated since 2022. Still, it's totally worth the hussle especially if you work with hundreds of different animation clips like I do and you want your project to be hermetized instead of keeping a second folder with .ase sprites to track.

