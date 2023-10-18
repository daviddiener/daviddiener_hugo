---
title: Vortex - An Arcade Space Game for Ludum Dare 50
date: 2022-04-30
tags:
  - gamedev
cover:
  image: "cover.png"
---

I recently participated in Ludum Dare 50, a 48-hour game jam where participants create a game based on a randomly selected theme. The theme for this year's jam was "Delay the inevitable."

I decided to create an arcade space game called Vortex, where the player must survive as long as possible while an increasing number of meteors are being pulled to them by the sun's gravitation. The game is developed with Unity, using C#.

The game features a self-hosted leaderboard which I host on Render.com. The leaderboard includes a backend server and a MongoDB database to persist the scores. The Vortex frontend then submits the scores over a REST API. The backend server is written in TypeScript using the Express.js library.

I also used Unity's shader graph language to make a highly efficient background effect with fading stars for the game.

The game was a lot of fun to develop, and I'm happy with how it turned out. I hope you enjoy playing it!

{{< button 
text="You can play Vortex here" 
link="https://davidjs.itch.io/vortex" 
>}}

**Here are some screenshots of the game:**

![Image of the player ship flying through a field of meteors](vortex_1.png)
![Image of the leaderboard screen](vortex_2.png)



{{< button
text="Check out the Unity Application on GitHub" 
link="https://github.com/daviddiener/Ludum-Dare-50" 
>}}

{{< button
text="Check out the Backend Server on GitHub" 
link="https://github.com/daviddiener/Ludum-Dare-50-Backend" 
>}}


**Thanks for reading!**