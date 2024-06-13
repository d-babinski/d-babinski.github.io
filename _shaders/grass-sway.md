---
title: "Vertex grass sway"
excerpt: "Non-pixel perfect wind shader"
date: "2023-12-01"
header:
  teaser: /assets/images/shaders/swaying_grass/banner.png
sidebar:
  - title: "Shader Type"
    text: "Shadergraph"
  - title: "Additional tags"
    text: "vertex, uv, sinwave"
gallery:
---

Made in shadergraph using the general idea of displacing vertices x position of sprite by (y-0.5)^3 * sin(time+global_x) function with some tweaks. This makes it so top part is the most affected by displacement while bottom stays in place. Pivot of the sprite is set in the bottom of the grass which is center of an image.

Decided to go for modern looking non pixel-perfect motion for this one to achieve the smooth motion and calming look of the foliage.

Here's how I made this in shadergraph:

![Shadergraph](../../assets/images/shaders/swaying_grass/swaying_grass_shadergraph.png)


Here's how it looks in isolation:

<video controls src="../../assets/images/shaders/swaying_grass/single_grass.mp4" title="Title" width=500 height=360></video>

And in practice (look at the grass):

<video controls src="../../assets/images/shaders/swaying_grass/grass_in_mountains.mp4" title="Title" width=500 height=360></video>
