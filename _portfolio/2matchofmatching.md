---
title: "Match of Matching"
excerpt: "Crossplatform 1v1 jewel matching game."
header:
  image: /assets/images/match_banner.png
  teaser: /assets/images/match_profile.png
sidebar:
  - title: "Role"
    image: /assets/images/match_profile.png
    image_alt: "logo"
    text: "Game Developer, Team Leader"
  - title: "Responsibilities"
    text: "Client programming, game design, project management"

gallery:
  - url: /assets/images/4clients.png
    image_path: assets/images/4clients.png
    alt: "4 clients at once"
  - url: /assets/images/main_screen_2.png
    image_path: assets/images/main_screen_2.png
    alt: "gameplay "
  - url: /assets/images/register.png
    image_path: assets/images/register.png
    alt: "registration menu"
  - url: /assets/images/mobile.png
    image_path: assets/images/mobile.png
    alt: "mobile client"
  - url: /assets/images/web.png
    image_path: assets/images/web.png
    alt: "web client"


---

This is a project we've decided to do during university course resolving around group projects. It has been made in around 10 weeks. The game is about rotating the elements in 2x2 square in a way to get most points possible by setting elements in rows or columns of 3 or more elements. Alongside 4 basic colors of jewels game contains blanks (elements that can work as all jewels) and bombs (that destroy jewels around them on activation) differentiating the gameplay.







{% include gallery caption="Little gallery from finished project (click to maximize)" %}


We've worked as team of 4 people which of 3 had no previous experience with game making, Unity or doing networking and servers. We've used github projects as a way to use agile workflow with assigning weekly tasks and doing reviews to each other. After designing initial rules of the game I've created quick prototype and we've set a basic communication rules with the server. We knew that we wanted to have game logic on both server and client to keep games safe from cheating and that we'll be using websocket in order to communicate. We've set a basic flask server and implemented game logic on both client and server side. During the project we've had several problems with keeping the logic between each part of an app identical. We've learned later that it would require us to do much more detailed mechanics planning very early on in order for it to go smoothly.


During the project we've managed to do:
- working client and server allowing for games to be played
- database with players and their games tracked
- matchmaking algorithm based on KNN over players' stats
- logging/registration from client
- crossplatform gameplay
- automated gameplay tests (we've tested the game for 6 straight hours this way)

What didn't work:
- throughout all the project we had trouble to keep identical states on both server and clients
- we haven't had enough time to implement music
- not all animations have been made due to time constraints
- we've had to simplify counting mechanism
- we haven't managed to host server publicly to check the game outside of localhost