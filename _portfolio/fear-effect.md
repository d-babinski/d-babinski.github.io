---
title: "Fear Effect Reinvented"
layout: single
date: "2024-04-15"
excerpt: "3rd person puzzle FPS, commercialy at Megapixel Studio."
header:
  teaser: /assets/images/fear_profile.png
sidebar:
  - title: "Role"
    image: /assets/images/fear_profile.png
    image_alt: "logo"
    text: "Game Developer"
  - title: "Responsibilities"
    text: "Gameplay, Systems, AI, UI, porting preparation"
gallery:
  - url: /assets/images/fear_screen1.png
    image_path: /assets/images/fear_screen1.png
    alt: "screenshot-1"
  - url: /assets/images/fear_screen2.png
    image_path: /assets/images/fear_screen2.png
    alt: "screenshot-2"
  - url: /assets/images/fear_screen3.png
    image_path: /assets/images/fear_screen3.png
    alt: "screenshot-3"
  - url: /assets/images/fear_screen4.png
    image_path: /assets/images/fear_screen4.png
    alt: "screenshot-4"
  - url: /assets/images/fear_screen5.png
    image_path: /assets/images/fear_screen5.png
    alt: "screenshot-5"
---

[Trailer](https://www.youtube.com/watch?v=UNB_Xy3Qhao)

This is a project I've worked on for over 2 years at Megapixel Studio. I've joined as second game developer after the preproduction phase in march 2021 and have worked on it till july 2023. I've participated in the whole process eventually becoming the only gameplay developer as the project lead took more of a management and optimization role. I've took part in every possible aspect of game creation beside release.

{% include gallery caption="Screens from promotion materials available on youtube" %}

## Responsibilities:

### Gameplay
- bossfight implementation, from concept to final state
- various kinds of puzzles
- implementation of enemies from scratch, with custom mechanics and routines
- special gameplay elements and systems (enemies with vehicles, special movement zones etc)
- concept and full implementation of ai for most of enemies in the game aswell as maintaining them


### UI
- HUD for the character state, weapon and player character state dependant
- Inventory system with each item being inspectable in seperate view
- Weapon system for both characters having one/two hands available and considering hands being occupied by flashlight
- Main menu and game menu implementation from scratch alongside, saves, loading, character choice, level selection, character selection, all of settings implementation, key rebinding etc.
- Adaptive key hints dependant on the device and platform that game was run on (mapping done via Rewired)


### Systems and AI
- Group AI, and squad roles for enemies aswell as math behind the decisionmaking for AI best position and action
- Design and implementation of a spatial sound system taking into account the level geometry and obstruction of the source based on the state of dynamic elements (e.g., doors and their degree of opening),
- Design and implementation of a sound system from scratch using FMOD for easier sound control and simplicity of use across all projects in the company,
- Implementation of enemy entities and their AI,
- IK system from scratch including stairs walking with smooth camera
- Impact particle and sounds system based off materials


### Misc.
- Implementation of animator controllers from scratch with blending, proper transitions and logic and IK
- Developing tools for designers for easier gameplay flow design
- Pathfinding solutions, including modification of the navmesh caused by in-game events, custom tagging, flock behavior and local avoidance.
- Maintenance and bugfixing of the project and preparing for milestones,
- Deployment and supervision of new team members.
- Basic preparation of the project for porting to the Xbox platform,
