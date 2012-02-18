---
layout: post
title: Building a WebGL game with GLGE, Part 2
draft: true
---

(XXX - pic?)

([Play *Ducks*][ducksgame] or [view the complete source][duckssource].)

In [part one][part1] of this series I covered GLGE basics by demonstrating how to set up a scene with four bobbing ducks. In this part I'll describe how I built the logic for the game and mapped it to a 3D view.

Keep in mind that part of building *Ducks* was an excercise in burn out management. [The last game I finished][friend-rescue] was so much work that I didn't write another one for years. To build the game I needed to stay within my limit span of attention and avoid getting burnt out, so I made sure I was focused on _play ability_ and _fun_ instead of fixing every bug which appeared. If something was "good enough" I kept it as is and decided to move on. A finished buggy game is better than an unplayable, perfect duct-collecting simulation.

### A Game with a Backbone

I chose [Backbone][backbone], a JavaScript Model-View-Controller framework, to build the game logic. Backbone is normally used to build rich web applications but I thought that the framework's `Model` class and pubsub event system would be really useful for the game. (I was right.) Also, I was already familiar with the framework from using it for my day job.

Was Backbone the right choice? For my game demo, sure. Would it be the right choice for a performance-sensitive game with a larger codebase? Probably not since performance optimization wasn't Backbone's primary goal. For *Ducks* it was good enough.

Backbone is normally used to browser applications and is thus coupled with the DOM. Despite not using its DOM features the pattern of separating the application into models, views and a controller was still relevant. For *Ducks*, the models were the data objects updated behind the scenes, the view was the WebGL presentation, and the controller glued everything together.

### Models

All of the game logic models inherit from `Backbone.Model`, which provides a system for defining object properties and letting observers subscribe to changes to any or all of the properties. For example, if you had a `Cannon` model with a property of `bullets`, you could bind a callback to whenever the number of bullets in the cannon changed in case you needed to update the ammo count on the screen.

The three models in *Ducks* -- or "actors" or "entities" or whatever you'd like to call them -- are the player's plan, the target reticle controlled by the user, and the ducks that the player needs to collect.

The ship, a moving plane, has the properties you'd expect: its position (`x` and `y`) and its velocity (`vx` and `vy`). It also had a few redundant properties, `dir` and `speed`, which made hooking up the view easier. I also created a few constants so that I could tweak the plane's movement more easily.

Note: The game appears to be 3D, but the data model underneath is actually 2D. This simplified the logic a lot and made collision detection much easier.

{% highlight javascript %}
var Ship = Backbone.Model.extend({

  RADIUS: 1,
  MIN_SPEED: 0.3,
  MAX_SPEED: 0.6,
  ACCELERATION: 0.005,
  MAX_TURN_SPEED: 3,

  defaults: {
    x: -20,
    y: 0,
    vx: 0,
    vy: 0,
    speed: 0,
    dir: 0,
    targetX: 0,
    targetY: 0
  },

  ...
});
{% endhighlight %}

For simplicity I combined the target reticle's coordinates with the `Ship` model. This made things easier but probably deserved to be its own model.

Unlike the ship, ducks don't move. The model is very simple:

{% highlight javascript %}
var Duck = Backbone.Model.extend({
  RADIUS: 1,
  defaults: {
    x: 0,
    y: 0
  }
});
{% endhighlight %}

Backbone provides another concept called *collections*, which are glorified arrays used to manage a list of models. You can `add()` and `remove()` models and use pretty much any of the [Underscore.js collection methods][underscore-collection] such as `each()` and `filter()`. You can also subscribe to events on the collection and be notified when an item is added or removed, or if a property of *any* of the models in the collection has been changed.

Creating a collection of `Duck` models is just three lines of code:

{% highlight javascript %}
var DuckCollection = Backbone.Collection.extend({
  model: Duck
});
{% endhighlight %}



### a controller

controller is a backbone "View". sure, could just extend Events, but meh. emits events like "start" and "game over"

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

  [underscore-collections]: http://documentcloud.github.com/underscore/#collections 