---
title: Odyssey - A MUD simulation environment
date: 2021-04-30
tags:
  - webdev
  - gamedev
cover:
    image: "cover.png"
---

{{< typing_effect
    id="typing-effect-1"
    textId="htmlText-1"
    speed="40"
    deleteSpeed="10"
    delay="1000"
>}}


{{< button
text="Start your Odyssey now" 
link="https://odyssey.daviddiener.de" 
>}}

## The Idea
Ever since I had a course on web development in university I never stopped tinkering around with webdev projects, especially with the MEAN stack. As I am also a game developer, naturally I tried to combine both skills and ended up with a kind of MUD/Simulator/RPG game in the form of a web application called Odyssey. It is not by any kind finished, but I intend to keep working on it in my free time.

## Uniting Web Development and Gaming
Odyssey isn't your average web application. It's a blend of a Multi-User Dungeon (MUD), a simulator, and a role-playing game (RPG), all woven into a fantasy world. Within this web-based realm, players can create multiple characters, each ready to embark on their adventure. These characters navigate a procedurally generated landscape, comprised of diverse regions and cities, where the possibilities are endless.

## Under the Hood
Odyssey is split up into two projects. A backend which hosts all the game objects and handles the game logic and a frontend which calls the backend api and displays the game data accordingly.

### Odyssey Frontend
The frontend is powered by the Angular and Phaser.js. Angular provides the sturdy foundation for the application, while Phaser.js steps in to craft the world map. This combination ensures a seamless and engaging user experience.

### Odyssey Backend
Behind the scenes, the backend relies on the express.js framework. It serves as a REST API, containing specific routes that expose every facet of the game world, from regions and cities to the characters that inhabit it. But the backend isn't all about data; it also handles the boring stuff like user authorization and authentication.

## Crafting a Procedural World
One of Odyssey's standout features is its procedurally generated map. To accomplish this, I employed a C# Simplex Noise implementation, an algorithm that generates heightmaps. This technique assigns a value between 0.0 and 1.0 to every coordinate combination of X and Y, creating a random and ever-changing landscape. These values then form the foundation for the diverse regions that players can explore.

## Embark on Your Odyssey
If you're curious to experience this enchanting project firsthand, don't hesitate to [start your Odyssey now](https://odyssey.daviddiener.de). While it may still be a work in progress, it's a testament to the endless possibilities that can emerge when web development and gaming unite. Stay tuned for more updates!