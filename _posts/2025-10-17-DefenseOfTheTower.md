---
title: Defense of The Tower [Code Available!]
date: 2025-11-02 20:11:35 +0100
categories: [Code Available, PC]
tags: [practice, citybuilder, code]     # TAG names should always be lowercase
description: '[Solo Developer]<br/>Example project to showcase my approach to gamedev'
media_subpath: /assets/dott/
pin: true
image:
  path: dott.png
---

Link to repository --> [Github](https://github.com/d-babinski/Defense-of-the-Tower)

## What is Defense of the Tower?

Defense of the Tower is a simple tower defense/clicker where you put cannons and gather resources to survive as long as virtually possible.
Since project main focus is showcasing the code - heavy focus was put on performance and clear and concise structure of the code while utilising cache of the CPU.

Here below is a short video of core gameplay of the game:
{% include embed/youtube.html id='EsQXaWYZb5U' %}

You can also play the game directly in browser on itch.io:
<iframe frameborder="0" src="https://itch.io/embed/4456390" width="552" height="167"><a href="https://flushedale.itch.io/defense-of-the-tower">Defense of the Tower by FlushedAle</a></iframe>

Most of game code (around 99%) happens in 'Scripts/Main.cs' and thats for several reasons:
- This way whole thought process of design and connections between systems is loud and clear at a glance
- Since code is procedural - it reads like a book, 10-15 mins and you understand everything
- I worked solo on this project so there was no conflict on source control side
- We can talk about where I would put boundaries to split code into smaller parts if I had to

I try to take advantage of CPU caching by designing core data structures as small as possible and keeping them in continuous arrays. This way if memory gets copied over we get several degrees faster execution of the code. I also try to reduce overhead on unnecessary abstraction before it becomes necessary. I don't split data structures into smaller divisive chunks (ie. splitting idea of enemy ship and enemy cannon into different things) because of cost being really low with benefit of clear program flow. I try to create different gameplay function by handling the same data structure differently depending on context and gameplay needs instead of binding function and data together before it's necessary.

IF you have any feedback I'm than happy to receive it at: dominik.i.babinski@gmail.com


