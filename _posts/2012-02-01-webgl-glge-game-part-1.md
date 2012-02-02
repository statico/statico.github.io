---
layout: post
title: Building a WebGL game with GLGE, Part 1
published: false
---

![ducks](images/ducks-splash.png)

This is a three-part monologue on how I built a WebGL game with very little experience in programming 3D graphics. In four days I was able to turn an idea into a game with cute ducks that my friends' 7-year-old daughter enjoyed. (She scored 15 seconds.)

Whether you're new to OpenGL entirely or just WebGL, the place to start is probably [Learning WebGL][learningwebgl]. There's a nice tutorial worth looking at which covers the basics. This game arose out of my evaluation of the GLGE library, which wraps WebGL into a nice API and includes a few utilities. I do have plans to evaluate other libraries like Three.js and even cover "raw" WebGL game development once I'm good enough, but that will be a later discussion.

I have passing familiarity with tools like Infini-D, Blender and Flash, so when I think of a 3D scene I imagine objects and groups of objects at coordinates in space, not meshes and normals and transformation matrices. Part of this experience was learning how to translate those artistic ideas into OpenGL concepts, and GLGE luckily helps a lot with doing that.

### The Idea

I started by spending some time with GLGE, which includes [a handful of useful examples][glgeexamples] including a demonstration of a water-like texture and another using [COLLADA][collada] models. The water demo was really cool, but once I saw the [rubber duck COLLADA model][rubberduck] it seemed obvious that the two needed to go together -- "Why don't I put the ducks _in_ the water and make them bob around a little?"

![GLGE water demo](images/glge-water.png)

COLLADA, which I hadn't heard of before, is an open specification for 3D models which can include textures and animations. Most 3D modeling software can import and export COLLADA (extension `.dae`), including Blender and Google SketchUp. GLGE lets you import COLLADA documents almost seamlessly.

The COLLADA demo includes a cartoonish aeroplane with a spinning propeller animation. While imagining ducks bobbing in the water the idea of catching ducks with the plane popped into my head. I also recalled playing flight simulators where the plane had a limited turning radius and navigating through obstacles was a challenge. Thus, catching ducks with a kinda-hard-to-maneuver plane seemed like it could be a simple, somewhat-fun game.

![GLGE COLLADA demo](images/glge-collada.png)

### The Non-Programming Part

Part of this exercise was to also test my project-management skills. In building this game I needed to stay within my limit span of attention and avoid getting burnt out. While developing I made sure I was focused on _playability_ and _fun_ as opposed to spending hours fixing every flippant bug that appeared. If something was "good enough," like the accuracy of the aiming target, I kept it as is and decided to move on. A finished buggy game is better than an unplayable, perfect duct-collecting simulation.

One big time-saver was being able to use HTML5 elements to build the GUI. Instead of trying to build a 2D menu out of rectangle and text primitives you can simply overlay XXX

One advantage of using an HTML5 canvas within a web page is that your GUI can be build using good ol' HTML. With CSS3 directives like `box-shadow` and `border-pattern` (XXX cite) you can create some nify-looking GUIs without building an entire windowing system. Translucent `<div>`s can be overlayed on top of the canvas, too. I was able to create a simple modal menu system very easily.

### Getting Started with GLGE

GLGE is a framework "for the lazy WebGL programmer" (XXX cite). It provides a collection of classes to make management of the scene, camera and object much easier than if you were to write raw WebGL constructs. It also optimizes your scene by reusing meshes and materials whenever it can.

For the uninitiated, a **mesh** is a collection of triangles which make up the shape of an object. A triangle is merely a list of three points, where each point is a three-tuple (or **vector**) of X, Y and Z coordinates in space. A **material** is applied to a mesh to give it a texture. A **scene** is what you're looking at at any given moment -- a tree of objects and object groups (the **scene graph**). You look at a scene through a viewport, whose direction, width and height is defined by the scene's **camera**.

To start using GLGE you'll need a web page with a canvas and the GLGE library. The following is that with some simple initialization code:

{% highlight html %}
<!doctype html>
<html>
  <head lang="en-us">
    <title>test</title>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge;chrome=1" />
  </head>
  <body>
    <canvas id="canvas" width="1024" height="768"></canvas>
    <script>
      // ...
    </script>
  </body>
</html>
{% endhighlight %}

(XXX - describe GLGE init)

If you're wondering what the `<script type="text/html">` is all about, good! By specifying a non-JavaScript MIME type you can use `<script>` tags to store useful information such as XML, which is useful because GLGE lets you build scenes with XML.

### An XML Scene

GLGE lets you initialize your scene in two ways: XML and JavaScript. The XML method is a declarative approach using XML which lets you define objects in the scene and the camera. You can give each element an ID which you can refer to within other parts of the XML document or within JavaScript later, much like ``document.getElementById()`` in an HTML DOM.

So let's imagine a simple scene with four ducks. You're looking down at the ducks.

(XXX - drawing of scene)

Here's that scene in a GLGE XML document, which you should be able to follow:

(XXX - xml of scene with four ducks)

The duck is imported from the COLLADA XML file `duck.dae`, which references the image `duck.png` to use as a texture. The current version of GLGE will let you wait until it has loaded the COLLADA data but doesn't block on loading assets referenced _inside_ of the `.dae`. This could lead to models being drawn without textures (they'll appear as black silouhettes) before the textures are loaded. Currently you'll have to create your own preloader if you want to get around this -- more on that later.

The COLLADA file contains everything you need to display a nice duckie, including mesh and texture. If you look at the GLGE demos you'll see examples of specifying meshes and textures inside of the XML -- it's very verbose.

Each duck is placed on the corners of a square around the origin. The units used are completely arbitrary -- I wasn't able to find a clear description of what to use for units or how many or how few should be in a viewport. After looking at some other examples I chose to assume that, in the game, the ducks would be scattered across a 20 x 10 rectangle, and I chose that I'd figure out various window sizes and aspect ratios later.

In the "camera looking at the front" angle, at least according to Blender's conventions, the negative Y axis is coming towards you, the X axis goes across the screen horizontally, and the Z axis XXX

### Intro to The JavaScript API

The XML declaration is good for setting up an initial scene, but what if you want to add or remove objects dynamically or move things around? Enter the [GLGE API][api] which lets you do everything the XML document can do and much more. It is in fact possible to create a scene entirely with JavaScript and without any XML at all -- but the JavaScript ends up being much more verbose and looks like copy-and-paste boilerplate. If your scene starts the same way every time, stick with XML.

Regardless of how you created the scene you'll need JavaScript to manipulate it.

Four ducks facing the same direction is a little boring. It'd be a lot more fun if they faced random directions as if they had been placed in the water by hand.

### Animation

Animations in GLGE are wonderfully easy to use. Similar to CSS3 animation, you specify a duration and keyframes and the engine will "tween" in between the keyframes. Declaring them with XML is straightforward and allows easy reuse via the JavaScript API.

XXX - easing?

A simple bob back-and-forth is a little boring. Anyone who's ever tried to stand on a raft in an lake knows that balance isn't limited to a single axis. In addition to the lateral wobble the ducks should also bob forward and backward a little, like this:

(XXX - drawing)

In GLGE XML that looks like this:

(XXX - xml animation)

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
