---
layout: post
title: Building a WebGL game with GLGE, Part 2
draft: true
---

In [part one][part1] of this series I covered GLGE basics by demonstrating how to set up a scene with four bobbing ducks. In this part I'll describe how I built the logic for the game and mapped it to a 3D view.

intro to backbone

XXX - good enough

### models

models: ship, ducks.

ship has x, y, vx, vy, target coords.

ducks don't move: x, y

(XXX - basic ship and duck code)

### a controller

controller is a backbone "View". sure, could just extend Events, but meh. emits events like "start" and "gameover"

modes: menu mode, play mode, score mode

smarter system would be to create and destroy scenes by defining transitions between modes. ex: score->play or menu->play means resetting the game, but play->menu does nothing.

controller has its own game loop. probably not a good idea, but meh.

each loop iteration does collision detection for ducks & ship. backbone's collections give you an each() method.

(XXX - tick() code)

### a view
