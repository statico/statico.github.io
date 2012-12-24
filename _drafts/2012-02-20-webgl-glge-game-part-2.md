---
layout: post
title: Building a WebGL game with GLGE, Part 2
draft: true
---

(XXX - pic?)

([Play *Ducks*][ducksgame] or [view the complete source][duckssource].)

In [part one][part1] of this series I covered GLGE basics by demonstrating how to set up a scene with four bobbing ducks. In this part I'll describe how I built the logic for the game and mapped it to a 3D view.

Keep in mind that part of building *Ducks* was an excercise in burn out management. [The last game I finished][friend-rescue] was so much work that I didn't write another one for years. To build the game I needed to stay within my limit span of attention and avoid getting burnt out, so I made sure I was focused on _play ability_ and _fun_ instead of fixing every bug which appeared. If something was "good enough" I kept it as is and decided to move on. A finished buggy game is better than an unplayable, perfect, duct-collecting simulation.

### Basic Duck Logic

game is played on a plane, 20x10

ducks and the ship have a radius of 1

each frame, ship tries to move towards the target

each frame a simple collision detection algorithm checks the ship's radius and each duck on the plane. if there's a collision, the duck is removed.

the game ends when all of the ducks are gone

### A Game with a Backbone

A lot of game frameworks I've seen either couple the game logic with the sprites and animation very tightly or use game entities as storage for game data. This is akin to building a webapp by storing data in the DOM. Instead, I chose to separate the game logic from its presentation. This seems to have been a good decision; the result is that the logic and GLGE layers are clearly separated.

I chose [Backbone][backbone], a JavaScript Model-View-Controller framework, to build the game logic. Backbone is normally used to build rich web applications but I thought that the framework's `Model` class and pubsub event system would be really useful for the game. (I was right.) Also, I was already familiar with the framework from using it for my day job.

Was Backbone the right choice? For my game demo, sure. Would it be the right choice for a performance-sensitive game with a larger codebase? Probably not since performance optimization wasn't Backbone's primary goal. For *Ducks* it was good enough.

Backbone is normally used to browser applications and is thus coupled with the DOM. Despite not using its DOM features the pattern of separating the application into models, views and a controller was still relevant. For *Ducks*, the models were the data objects updated behind the scenes, the view was the WebGL presentation, and the controller glued everything together.

### Models

All of the game logic models inherit from `Backbone.Model`, which provides a system for defining object properties and letting observers subscribe to changes to any or all of the properties. For example, if you had a `Cannon` model with a property of `bullets`, you could bind a callback to whenever the number of bullets in the cannon changed in case you needed to update the ammo count on the screen.

The three models in *Ducks* -- or "actors" or "entities" or whatever you'd like to call them -- are the player's plan, the target reticle controlled by the user, and the ducks that the player needs to collect.

You can view [the entire `logic.js` file on GitHub][logic.js], but I'll discuss the different models below.

(XXX - pic of the ship and target))

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

(XXX - pic of ducks model)

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

Easy, right? And that's not even the fun part. It's much more exciting to put these models into action by binding events in a view.

### A 3D View

Once the logic has been written, displaying the game is a matter of rendering a 3D scene and placing objects according to the models' data. Luckily, as seen in [part one][part1], creating scenes in GLGE is pretty easy.

With *Ducks* very little of the scene is described in [the XML][scene.xml]. There are animations, the camera definition, and a ground plane with a water texture. The code in [view.js][view.js] does most of the heavy lifting.

{% highlight xml %}
<?xml version="1.0"?>
<glge>
  ...

  <scene id="mainscene" camera="#maincamera"
         ambient_color="#fff" background_color="#22738a">
    <light id="mainlight" loc_y="5" type="L_POINT" />
    <object id="ground" mesh="#groundmesh" material="#water"
            scaleX="60" scaleY="30" loc_z=".2" />
  </scene>

</glge>
{% endhighlight %}

Before the scene is created, the duck, target and plane models are loaded:

{% highlight javascript %}
var duck = new GLGE.Collada();
duck.setDocument('assets/duck.dae');

var target = new GLGE.Collada();
target.setDocument('assets/target.dae');

var ship = new GLGE.Collada();
ship.setDocument('assets/seymourplane_triangulate.dae');
{% endhighlight %}

Later on, after all of the assets are loaded, the scene is created with the ship and the target:

{% highlight javascript %}
// Standard GLGE initialization.
var renderer = new GLGE.Renderer(canvas[0]);
scene = doc.getElement('mainscene');
renderer.setScene(scene);

...

// Initialize the player's ship.
ship.setLocZ(3);
ship.setRotX(Math.PI / 2);
ship.setRotY(Math.PI / 2);
ship.setScale(0.3);
ship.setFrameRate(60);
scene.addChild(ship);

// Initialize the aiming target.
target.setLocZ(0.4);
target.setScale(0.001);
target.setRotX(Math.PI / 2);
target.setRotY(Math.PI / 2);
scene.addChild(target);
{% endhighlight %}




example 1, create a scene, but don't create the ducks just yet

use backbone views - bind to 'add' to create ducks. bind to remove too. when removing, show animation.

(XXX - game.ducks.bind callbacks)

while we're binding events, set up callbacks for game mode changes. again, could have been smarter with state transitions, but meh.

(XXX - JS for game.bind)



### a controller

controller is a backbone "View". sure, could just extend Events, but meh. emits events like "start" and "game over"

modes: menu mode, play mode, score mode

menu and score mores are "demo" modes -- stuff is happening but no user input needed. turned out to be an important choice because it demonstrates how to play the game before they play -- no tutorial needed.

smarter system would be to create and destroy scenes by defining transitions between modes. ex: score->play or menu->play means resetting the game, but play->menu does nothing.

controller has its own game loop. probably not a good idea, but meh.

each loop iteration does collision detection for ducks & ship. backbone's collections give you an each() method.

(XXX - tick() code)

### a menu

create a menu - big advantage of webgl/html5 is that you can use html to make the GUI

(XXX - menu XML and css)

  [underscore-collections]: http://documentcloud.github.com/underscore/#collections 