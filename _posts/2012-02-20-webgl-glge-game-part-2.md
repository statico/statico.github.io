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

menu and score mores are "demo" modes -- stuff is happening but no user input needed. turned out to be an important choice because it demonstrates how to play the game before they play -- no tutorial needed.

smarter system would be to create and destroy scenes by defining transitions between modes. ex: score->play or menu->play means resetting the game, but play->menu does nothing.

controller has its own game loop. probably not a good idea, but meh.

each loop iteration does collision detection for ducks & ship. backbone's collections give you an each() method.

(XXX - tick() code)

### a view

example 1, create a scene, but don't create the ducks just yet

use backbone views - bind to 'add' to create ducks. bind to remove too. when removing, show animation.

(XXX - game.ducks.bind callbacks)

while we're binding events, set up callbacks for game mode changes. again, could have been smarter with state transitions, but meh.

(XXX - JS for game.bind)

### a menu

create a menu - big advantage of webgl/html5 is that you can use html to make the GUI

(XXX - menu XML and css)
