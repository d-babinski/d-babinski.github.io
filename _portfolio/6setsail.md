---
title: "Set Sail"
excerpt: "Boss fight centetered game focused on melee combat and proper timing."
header:
  image: /assets/images/setsail_banner.png
  teaser: /assets/images/setsail_profile.png
sidebar:
  - title: "Role"
    image: /assets/images/setsail_profile.png
    image_alt: "logo"
    text: "Game Developer"
  - title: "Responsibilities"
    text: "Programming, art, audio, game design"

gallery:
  - url: /assets/images/rozgrywka.png
    image_path: assets/images/rozgrywka.png
    alt: "gameplay"
  - url: /assets/images/6_map.png
    image_path: assets/images/6_map.png
    alt: "6_map"
  - url: /assets/images/setsail_marko.gif
    image_path: assets/images/setsail_marko.gif
    alt: "marko1"
  - url: /assets/images/setsail_marko2.gif
    image_path: assets/images/setsail_marko2.gif
    alt: "marko2"
  - url: /assets/images/setsail_marko3.png
    image_path: assets/images/setsail_marko3.png
    alt: "marko3"

---
 
This game was made as part of my engineering thesis and is focused on generating the levels in turn based mobile game. The game was well received by reviewers and actively debugged by some of my friends (there was not only theorycrafting about the game but speedrunning aswell).
I've used perlin noise as a basis to do generation and I've set some rules so player could always reach it's destination on every map. The game contains simple combat (first player moves then enemies move) in which defense and offense stats are tested. There also items and stat-testing events aswell as gold for stat improvement.

{% include gallery caption="Gifs showing the way gameplay works (click to maximize)" %}

As per usual, what has been done:
- procedural map generation on various sizes
- fights worked well, were hard but not impossible
- item progression with procedural items
- event's giving various benefits (health, gold, items)
- mobile controls worked pretty well
- game was really fun to play

What went wrong:
- game required at least few tries to go through first 2 levels
- stats were convoluted, there was no info of what theyre actually doing
- there was no artistic approach
- game didn't scale too well for all screens