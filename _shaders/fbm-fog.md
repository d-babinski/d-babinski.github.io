---
title: "Fractal brownian motion"
excerpt: "Fog shader based on this paper: https://thebookofshaders.com/13/"
date: "2024-03-15"
header:
  teaser: /assets/images/shaders/fog/banner.png
sidebar:
  - title: "Shader Type"
    text: "Shadergraph"
  - title: "Additional tags"
    text: "fog, fbm, noise, subshader"
gallery:
---

Fractal-brownian motion fog based on extensive article in [Book of Shaders](https://thebookofshaders.com/13/). Made as subshader and then connected several times for more interesting effect.


Fog subshader:
![Shadergraph](../../assets/images/shaders/fog/fog.png)

Multiple subshaders combined:
![Shadergraph](../../assets/images/shaders/fog/fog_multiple.png)


Final result:

<video controls src="../../assets/images/shaders/fog/fog.mp4" title="Title" width=500 height=360></video>
