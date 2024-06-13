---
title: "Little pillagers"
excerpt: "Simple proof of concept strategy game (repository included)"
date: "2024-03-31"
header:
  teaser: /assets/images/pillagers_profile.png
sidebar:
  - title: "Role"
    image: /assets/images/pillagers_profile.png
    image_alt: "logo"
    text: "Game Developer"
  - title: "Responsibilities"
    text: "Whole game"
  - title: "Links"
  - text: "[Github](https://github.com/d-babinski/LittlePillagers)
  [Playable web build](https://flushedale.itch.io/little-pillagers)"
gallery:
  - url: /assets/images/pillagers_screen1.png
    image_path: /assets/images/pillagers_screen1.png
    alt: "screenshot-1"
  - url: /assets/images/pillagers_screen2.png
    image_path: /assets/images/pillagers_screen2.png
    alt: "screenshot-2"
  - url: /assets/images/pillagers_screen3.png
    image_path: /assets/images/pillagers_screen3.png
    alt: "screenshot-3"
---

This game is a small prototype strategy game with several systems such as unit management, player skills and combat. It's main objective was to maintain modular, easy to extend codebase so it's easy to add content and mechanics without worrying too much about sideffects of adding new code.

After previously trying out godot engine I've decided that seperation of data and logic proves to be crucial in order to maintain concise, bug free code aswell as improves testability. Scriptable objects were the primary way of achieving this in Unity, reducing coupling by sharing data via intermediate objects and connecting gameplay via events. 

This proved to work very well as adding new content, and especially new attacks, player skills and units was relatively easy to do and did not generate any bugs outside of content responsibility. All this meant that testing UI is as simple as putting ui on the scene and changing number in the scriptable object its connected to (ie. IntVariable). 

This heavily impacted debuging time as all the bugs were always very close to the source meaning no week-long bug hunting.

{% include gallery caption="Example screenshots" %}

Of course not everything went perfectly. I'm not very happy with the pipeline of adding new units, which requires adding a unit template, unit type and then adding this template to unit template database (which on small scale isn't that bad but imagine the bigger scale) so there's definitely a lot to improve upon.

Also theres not much optimization used (I tend to avoid preemptive optimization if it slows time  to deliver), theres definitely a lot of potential to move some animations into shaders, use pooling for spawning etc.

One thing scriptable variables dont do very well is connecting components of entity via data, it does better with global states. Most likely dependency injection framework would do better in that matter.

You can find game and github links in the left panel.
