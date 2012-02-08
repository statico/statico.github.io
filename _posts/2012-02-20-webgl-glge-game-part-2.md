---
layout: post
title: Building a WebGL game with GLGE, Part 2
draft: true
---

If you're wondering what the `<script type="text/html">` is all about, good! By specifying a non-JavaScript MIME type you can use `<script>` tags to store useful information such as XML, which is useful because GLGE lets you build scenes with XML.



In GLGE XML that looks like this:

(XXX - XML animation)

The number of animation frames is a little arbitrary since you can adjust the frame rate of any animation dynamically. It's probably a good idea to have an animation FPS greater-than-or-equal to the global FPS so they aren't choppy.

Attaching the animation to the ducks is easy, but limited:

(XXX - revised duck objects)

It feels more natural if all of the ducks are bobbing independently -- unless, of course, it's a synchronized bobbing competition. Let's switch from setting the objects' animation in XML to doing so in JavaScript and, at the same time, offsetting the animation's current frame by a random number of frames:

(XXX - javascript animation assignment)


It's a lot concise to specify animation

spinning ducks

animation in XML

random keyframe

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
