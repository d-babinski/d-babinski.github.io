---
title: "Blightsworn"
excerpt: "Boss fight-centric game focused on melee combat and proper timing."
header:
  image: /assets/images/blightsworn_banner.png
  teaser: /assets/images/blightsworn_profile.png
sidebar:
  - title: "Role"
    image: /assets/images/blightsworn_profile.png
    image_alt: "logo"
    text: "Game Developer"
  - title: "Responsibilities"
    text: "Programming, art, audio, game design"
gallery:
  - url: /assets/images/blightsworn_100frogs.gif
    image_path: assets/images/blightsworn_100frogs.gif
    alt: "gif 1"
  - url: /assets/images/blightsworn_dodge.gif
    image_path: assets/images/blightsworn_dodge.gif
    alt: "gif 2"
  - url: /assets/images/blightsworn_multislash.gif
    image_path: assets/images/blightsworn_multislash.gif
    alt: "gif 3"
  - url: /assets/images/blightsworn_combos.gif
    image_path: assets/images/blightsworn_combos.gif
    alt: "gif 4"
---

At the time of writing this is my current passion project which has been forming since over a year before transforming from rpg with several characters based on exploration to set of fights against bosses. Big influence came from the receivance of the first similar game - ["Ghoulash"](https://fractialcopper.github.io/portfolio/5ghoulash/) also mentioned in the portfolio. Main pillars of the gameplay are movement and timing and everything else in the game is aligned with that.

{% include gallery caption="Various gifs from the development. (click to maximize)" %}

During development I have implemented several systems with intention of long term usage in different projects.

Several of the systems are: 

- Raycast based collision system instead of built-in one
    - This allows for more customised behaviour of collisions and removes the limitations of what and when can collide.
    - Raycast and similar methods is handled globally so no code is getting repeated to check collisions/damage 

- Custom global physic controller
    - Similar to raycast, physics are controlled globally so the requests can be tracked and easily debugged
    - I had idea of doing custom timer for the sake of pauses and queueing events but haven't committed yet

- Modular AI behaviour builder
    - Each mechanic and condition is based on Scriptable Objects which contain data about specific parameters
    - Enemy behaviour script is limited to minimum (mainly accessors and information) to ensure that various usages are possible

- Input and input buffering system
    - Allows for better control of the way input is handled and detection of few actions being called at once (for ex. jump into sword slash pressed one after another).


Over the last year the scope changed from few characters and swimming by ship over the vast world to one character fighting limited ammount of bosses over few small areas. It was necessary in order to make the game finishable for one person and gives good design pillars to follow when designing the core of the game. The progress can be followed on my [twitter](http://twitter.com/aleflushed).