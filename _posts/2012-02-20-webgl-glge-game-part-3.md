---
layout: post
title: Building a WebGL game with GLGE, Part 3
draft: true
---

### Music

searching for "free game music" -- first hit, picked Canopy

imagined the game being "cute"
ducks bobbing in time with the music
used a keyboard BPM meter to time the music
120 BPM? perfect!


The current version of GLGE will let you wait until it has loaded the COLLADA data but doesn't block on loading assets referenced _inside_ of the `.dae`. This could lead to models being drawn without textures (they'll appear as black silhouettes) before the textures are loaded. Currently you'll have to create your own preloader if you want to get around this -- more on that later.

### Burnout Management

Part of this exercise was to also test my project-management skills. [The last game I finished][friend-rescue] was so much work that I didn't write another one for years. In building this game I needed to stay within my limit span of attention and avoid getting burnt out. While developing I made sure I was focused on _playability_ and _fun_ as opposed to spending hours fixing every flippant bug that appeared. If something was "good enough," like the accuracy of the aiming target, I kept it as is and decided to move on. A finished buggy game is better than an unplayable, perfect duct-collecting simulation.

One advantage of using an HTML5 canvas within a web page is that your GUI can be build using good ol' HTML. Many game developers seem to build menu systems and GUIs from scratch. With CSS3 directives like [`box-shadow`][box-shadow] and [`border-image`][border-image] you can create some nifty-looking GUIs without building tons of infrastructure. Translucent `<div>`s can be overlayed on top of the canvas, too. I saved a lot of time by using absolutely-positioned `<div>`s for my modal menu dialogs.

  [box-shadow]: http://caniuse.com/#search=box-shadow
  [border-image]: http://www.css3files.com/border/
