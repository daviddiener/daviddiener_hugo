---
title: GemZ - A Roman-Themed Survival Game Inspired by MineralZ
date: 2021-01-02
tags:
  - gamedev
---

<p><strong>Status: Playable Prototype</strong></p>

Did anyone else play the StarCraft 2 arcade games? One of my favorite arcade games on StarCraft 2 was MineralZ. In this game, players control a team of protoss probes who must survive against increasingly strong waves of the Zerg. I thought it would be interesting to have a similar game, but set in a different time period as well as in a 3rd person perspective. In GemZ, players take on the role of a Roman civilian who is being chased by barbarians. This game is one of the first games I ever got a full game cycle from start to end on. While it's not yet finished, it works and is already kind of fun. In this post, I'll be sharing the process of creating GemZ.

## Technical Details
GemZ was built in Unity. I used various Unity assets to speed up the development process. One of the key technical challenges was implementing the enemy AI, which required a lot of trial and error to get right. 

### Procedural Generation of the Map
The map consist of a procedurally generated field of stones and gem resources. I used a very simple approach of generation random numbers for the map generation. It first is decided on a chance of 1/20 if a coordinate will be occupied by an object. Then it is a 5/6 chance that we spawn a rock. The remaining 1/6 times a random gem rock will be spawned.

![Procedural Mace Generation](mace_generation.gif)

### Pathfinding
The enemy AIs pathfinding is based on the [A* Pathfinding Project](https://arongranberg.com/astar/) by Aron Granberg.

![Enemy AI Pathfinding](enemy_ai.gif)

GemZ is a fun and challenging game that I'm proud to have created. I hope this post has provided some insight into the design process and technical details behind the game. If you're interested in trying out GemZ, you can play it for free on my [itch.io](https://davidjs.itch.io/gem-z) website. I'm looking forward to hearing your feedback and suggestions for improving the game.

<iframe frameborder="0" src="https://itch.io/embed/1226641" width="552" height="167"><a href="https://davidjs.itch.io/gem-z">Gem Z by david.js</a></iframe>