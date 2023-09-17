---
title: Odyssey - A MUD simulation environment
date: 2021-04-30
tags:
  - webdev
  - gamedev
---

## The Idea
Ever since I had a course on web development in university I never stopped tinkering around with webdev projects, especially with the MEAN stack. As I am also a game developer, naturally I tried to combine both skills and ended up with a kind of MUD/simulator/rpg game in the form of a web application called [Odyssey](https://odyssey.daviddiener.de/home). It is not by any kind finished, but I intend to keep working on it in my free time.

## The Tech
Odyssey is split up into two projects. A backend which hosts all the game objects and handles the game logic and a frontend which calls the backend api and displays the game data accordingly.

### Odyssey Frontend
The frontend is based on Angular and Phaser.js. Angular is used for the basic setup of the application, while I use Phaser.js to handle the [world map](https://odyssey.daviddiener.de/worldmap).

### Odyssey Backend
The backend is based on express.js. It serves a REST-API containing specific routes for all the objects in the game, like regions, cities and characters. It also handles the boring stuff like user authorization and authentification. 

I used a C# Simplex Noise implementation to generate a heightmap to create the procedurally generated map. The Simplex Noise algorithm provides a value between 0.0 and 1.0 for each requested X and Y coordinate combination. The values are then mapped to the regions.

[If you wonder what the project looks like, feel free to start your Odyssey now.](https://odyssey.daviddiener.de)