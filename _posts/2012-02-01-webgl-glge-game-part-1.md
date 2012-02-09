---
layout: post
title: Building a WebGL game with GLGE, Part 1
draft: true
---

[![ducks](images/ducks-splash.png)][ducksgame]

*([Play the game][ducksgame] or [view the complete source][duckssource].)*

How difficult is it to build a WebGL-based browser game without any prior 3D experience? Very. Concepts such as normals and matrix transformations can be intimidating to someone who hasn't recently passed a linear algebra exam, and a huge amount of knowledge is required just to get something moving on the screen. Luckily, frameworks such as [GLGE][glge] abstract a lot of the hard stuff away and let budding game developers develop games more quickly.

This is a three-part monologue on how I built a WebGL game with very little experience in programming 2D graphics. In four days I was able to turn an idea into a game with cute ducks that my friends' 7-year-old daughter enjoyed. (She scored 15 seconds.)

I have passing familiarity with tools like Infini-D, Blender and Flash, so when I think of a 3D scene I imagine objects and groups of objects at coordinates in space, not meshes and normals and transformation matrices. Part of this experience was learning how to translate those artistic ideas into OpenGL concepts, and GLGE luckily helps a lot with doing that.

### The origin of the idea: COLLADA models

I started by spending some time with GLGE, which includes [a handful of useful examples][glgeexamples] including a demonstration of a water-like texture and another using [COLLADA][collada] models. The water demo was really cool, but once I saw the [rubber duck COLLADA model][duckdae] it seemed obvious that the two needed to go together -- "Why don't I put the ducks _in_ the water and make them bob around a little?"

![GLGE COLLADA demo](images/glge-collada.png)

COLLADA, which I hadn't heard of before, is an open specification for 3D models which can include textures and animations. Most 3D modeling software can import and export COLLADA (extension `.dae`), including Blender and Google SketchUp. GLGE lets you import COLLADA documents almost seamlessly.

The COLLADA demo includes a cartoonish aeroplane with a spinning propeller animation. While imagining ducks bobbing in the water the idea of catching ducks with the plane popped into my head. I also recalled playing flight simulators where the plane had a limited turning radius and navigating through obstacles was a challenge. Thus, catching ducks with a kinda-hard-to-maneuver plane seemed like it could be a simple, somewhat-fun game.

### Getting started with GLGE

[GLGE][glge] calls itself "WebGL for the lazy." It provides a collection of classes to make management of the scene, camera and object much easier than if you were to write raw WebGL constructs. It also optimizes your scene by reusing meshes and materials whenever it can.

Meshes and materials? Some of the terms might seem alien if you've never worked with OpenGL before, so here's a primer. A *mesh* is a collection of triangles which make up the shape of an object. A triangle is merely a list of three points, where each point is a three-tuple (or *vector*) of X, Y and Z coordinates in space. A *material* is applied to a mesh to give it a texture. A *scene* is what you're looking at at any given moment -- a tree of objects and object groups (the *scene graph*). You look at a scene through a viewport, whose direction, width and height is defined by the scene's *camera*.

If you want to dive in to the docs and examples immediately, perform the following. Clone or download & unzip [the GLGE project from GitHub][glge-github], run `python -m SimpleHTTPServer` from within the `GLGE/` directory, and connect to [http://localhost:8000/](http://localhost:8000/). This will give you fast access to the API and, unlike using `file:///`, will let you run the examples without triggering any cross-domain security hooplah.

### Making a scene

GLGE lets you initialize your scene in two ways: XML and JavaScript. The XML method is a declarative approach using XML which lets you define objects in the scene and the camera. You can give each element an ID which you can refer to within other parts of the XML document or within JavaScript later, much like ``document.getElementById()`` in an HTML DOM.

Creating a scene with XML is pretty easy, so let's do that. Imagine a simple scene with four ducks spaced equally around the origin and the camera is pointing straight down at the ducks:

![ducks drawing](images/4ducks-sketchup.png)

Here's how that scene could be described in GLGE XML:

{% highlight xml %}
<?xml version="1.0"?>
<glge>

  <camera id="maincamera" loc_z="10" />

  <scene id="mainscene" camera="#maincamera"
         ambient_color="#fff" background_color="#999">

    <light id="mainlight" loc_y="5" type="L_POINT" />

    <collada document="duck.dae" loc_x="-1" loc_y="-1"
             rot_x="1.57" rot_y="4.71" scale=".01" />
    <collada document="duck.dae" loc_x="-1" loc_y="1"
             rot_x="1.57" rot_y="4.71" scale=".01" />
    <collada document="duck.dae" loc_x="1" loc_y="-1"
             rot_x="1.57" rot_y="4.71" scale=".01" />
    <collada document="duck.dae" loc_x="1" loc_y="1"
             rot_x="1.57" rot_y="4.71" scale=".01" />

  </scene>

</glge>
{% endhighlight %}

Straightforward, right? First, declare a camera positioned 20 units up on the Z axies, and by default the camera looks straight down the Z axis. Next, declare a scene which references the color, contains a simple light source, and puts four ducks on the ground. The ducks are models are defined in another file, `duck.dae`, and we'll get to that in a second.

The duck is imported from the [COLLADA sample XML file `duck.dae`][duckdae], which references the image [`duck.png`][duckdae] to use as a texture. The COLLADA file contains everything needed to display a nice duckie, including meshes and materials. Each duck needs to be rotated on the X and Y axes -- π/2 and 3π/2 radians, respectively. Rounding to two decimal places is good enough.

Each duck is placed on the corners of a square around the origin. The units used are completely arbitrary -- I wasn't able to find a clear description of what to use for units or how many or how few should be in a viewport. After looking at some other examples I chose to assume that, in the game, the ducks would be scattered across a 20 x 10 rectangle, and I chose that I'd figure out various window sizes and aspect ratios later.

You can save this XML in a separate file, but when starting out with GLGE you'll probably find it easier to embed the XML inside the HTML document using `<script type="text/xml">` tags. For clarify, however, I'll assume the XML is saved as a separate file, `scene.xml`. Here's a minimal page and script that will load the scene

{% highlight html %}
<!doctype html>
<html>
  <body>

    <canvas id="canvas" width="550" height="400"></canvas>

    <script src="glge-compiled-min.js"></script>

    <script>
      var doc = new GLGE.Document();

      doc.onLoad = function() {
        // Tell GLGE to use our canvas.
        var renderer = new GLGE.Renderer(document.getElementById('canvas'));

        // Set up the scene.
        var scene = doc.getElement('mainscene');
        renderer.setScene(scene);

        // Simple stupid render loop.
        setInterval(function() {
          renderer.render();
        }, 1000 / 30);
      };

      doc.load('scene.xml');
    </script>

  </body>
</html>
{% endhighlight %}

The script begins by creating a new `GLGE.Document`, which represents the scene. Setting the `onLoad` property lets you execute code once the document has been loaded, which is akin to `document.onload` in browsers. The handler creates a new `GLGE.Renderer` objects bound to the `<canvas>` element and initializes it with the `<scene id="mainscene">` defined in the XML. A simple loop calls `render()`, which abstracts away the mountains of WebGL primitives required to push polygons to your graphics hardware. For simplicity I used JavaScript's built-in `setInterval()`, but a smarter solution would be to use [window.requestAnimationFrame][raf].

Put this all together and viola, ducks! (Check out [the full source][fourducks-source] or the [live demo][fourducks].)

![4ducks-rendered](images/4ducks-rendered.png)

### Getting animated

Animations in GLGE are wonderfully easy to use. Similar to CSS3 animation, you specify a duration and keyframes and the engine will "[tween][tweening]" in between the keyframes.

Let's have the ducks bob back and forth as if they're sitting in water. But a simple bob back-and-forth is a little boring; anyone who's ever tried to stand on a raft in an lake knows that balance isn't limited to a single axis. In addition to the lateral wobble the ducks should also bob forward and backward a little. In GLGE XML, this type of animation can be declared like this:

{% highlight xml %}
<animation_vector id="bob" frames="60">

  <animation_curve channel="DRotX">
    <linear_point x="1" y="0" />
    <linear_point x="15" y="-.2" />
    <linear_point x="30" y="0" />
    <linear_point x="45" y=".2" />
    <linear_point x="60" y="0" />
  </animation_curve>

  <animation_curve channel="DRotY">
    <linear_point x="1" y="0" />
    <linear_point x="15" y="-.2" />
    <linear_point x="30" y="0" />
    <linear_point x="45" y="-.2" />
    <linear_point x="60" y="0" />
  </animation_curve>

</animation_vector>
{% endhighlight %}

Did you notice that the `<animation_vector>` has an ID of `bob`? When attaching animations to objects in the scene you simply need to refer to the ID in CSS-selector-like syntax:

{% highlight xml %}
(XXX - <collada> objects)
{% endhighlight %}

And there you have it -- bobbing ducks. Unfortunately, all of the ducks bob in perfect sync, which seems unnatural. It would be a little more fun if all of the ducks moved independently, and making that happen is a perfect task for the GLGE JavaScript API.

### Intro to The JavaScript API

The XML declaration is good for setting up an initial scene, but what if you want to add or remove objects dynamically or move things around? Enter the [GLGE API][api] which lets you do everything the XML document can do and much more. It is in fact possible to create a scene entirely with JavaScript and without any XML at all -- but the JavaScript ends up being much more verbose and looks like copy-and-paste boilerplate. If your scene starts the same way every time, stick with XML.

Regardless of how you created the scene you'll need JavaScript to manipulate it. In the case of our ducks, we created the scene and animations using XML, but to make the ducks bob independently we should use JavaScript. Yes, you could create four different `<animation_vector>`s, but that would be a lot more work.

Each GLGE animation has a fixed number of frames and loops indefinitely by default. An easy way to make the ducks XXX





  [ducksgame]: http://statico.github.com/webgl-demos/ducks/
  [duckssource]: http://github.com/statico/webgl-demos/tree/master/ducks/
  [glge]: http://www.glge.org/
  [glgeexamples]: https://github.com/supereggbert/GLGE/tree/master/examples
  [duckdae]: https://collada.org/owl/browse.php?sess=0&parent=126&expand=1&order=name&curview=0&sortname=ASC
  [raf]: http://www.html5rocks.com/en/tutorials/speed/html5/#toc-request-ani-frame
  [fourducks]: http://statico.github.com/webgl-demos/fourducks/
  [fourducks-source]: https://github.com/statico/webgl-demos/tree/master/fourducks
