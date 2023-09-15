---
layout: post
title: "Breakout V1 Part II: Refactor and Movement"
description: "Refacotring functions and starting with the gameplay."
date: 2022-11-05 18:30:00 -0000
tag: JavaScript GameDev
serie: Breakout V1
---
<img style="float: left;" src="/images/breakoutv1-1.png">

## Movement

Let's continue. For now we have drawn a static ball.  It's time to move it. FOr starters we initialize the position and create an `init()` function to have the initial state of the game and the update interval that will call the future `paint()` function that will draw the content and make the actual movements:

``````
var x = 150;
var y = 150;
var dx = 2;
var dy = 4;
var ctx;
 
function init() {
 ctx = $('#canvas')[0].getContext("2d");
 return setInterval(paint, 10);
}
``````

`x` and `y` are the initial position of the ball in the canvas, in this case it's the middle. `dx` and `dy` are the velocity in both axes, that will dictate how much the ball will move in both directions.

The `paint()` function now has to draw the ball and make it move, so it has to redraw everything in every frame:

````
function paint() {
 // Clean the canvas
 ctx.clearRect(0,0,300,300);
 // Draw the canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, w, h);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, w, h);
 
 // Draw the circle
 ctx.beginPath();
 ctx.arc(x, y, 10, 0, Math.PI*2, true);
 ctx.fillStyle = "black";
 ctx.closePath();
 ctx.fill();
 x += dx;
 y += dy;
 }
 `````
 
 Starting on line 3 we see the `clearRect()` function with which we delete all that was drawn on the canvas previously. Then we move the canvas initialization and the circle draw instructions inside `paint()`, but this time instead of using a fixed position we use the variables we created just before.
 
 On `init()` we defined a `setInterval()` that will call this function every 10ms, so in every call everything will be redrawn. To create the movement we created some velocity variables, `dx` and `dy`, so in every call  we will change de position adding this variables so, for example, if in the first call the middle is on (150,150), on the next call it will be (152,154), then (154.158) and so on. This way every time the frame is drawn the ball will be in a different position, creating the illusion of movement. Trying different values (including negatives) on `dx` and `dy` will change speed and direction.

## Refactor

We will need to repeat a lot of things constantly from now on so we need to tidy up the code now before it's too late. We will create functions for the most basic stuff: draw the frame, draw the circle, initialize the game and draw a rectangle, and of course define some variables.

Variables:

``````
var x = 150; // Initial position on X
var y = 150; // Initial position on Y
var dx = 2; // Velocity on X
var dy = 4; // Velocity on Y
var WIDTH; // Width of the frame
var HEIGHT; // Height of the frame
var ctx;
``````

Initialize everything:

``````
function init() {
 ctx = $('#canvas')[0].getContext("2d"); // Obtain the context from the canvas
 WIDTH = $("#canvas").width(); // Assign the size to the variables
 HEIGHT = $("#canvas").height();
 return setInterval(paint, 10); // Call paint() every 10 ms
}
``````

Draw a circle:

``````
// Draw a circle with its center on (x,y), radius r and a color 
 
function circle(x,y,r,color) {
 ctx.beginPath();
 ctx.arc(x, y, r, 0, Math.PI*2, true);
 ctx.fillStyle = color;
 ctx.closePath();
 ctx.fill();
}
``````

Draw a rectangle:

``````
// Draw a rectangle of width w, height h and its top left corner on (x,y)
function rect(x,y,w,h) {
 ctx.beginPath();
 ctx.rect(x,y,w,h);
 ctx.closePath();
 ctx.fill();
}
``````

Clean the frame:

``````
// Clear the canvas
function clear() {
 ctx.clearRect(0, 0, WIDTH, HEIGHT);</pre>
// Draw the canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, WIDTH, HEIGHT);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, WIDTH, HEIGHT);
}
``````

Code in action (draw and initialize):

``````
function paint() {
 clear();
 circle(x, y, 10, "black");
 
 x += dx;
 y += dy;
}
 
init();
``````

All the code before `paint()` shouldn't be too modified from now on (or at all), it's kind of a basic library to use when necessary.

Now that we have those in place and a ball that moves in a certain way and falls in a certain place, we need to contain it inside the boundaries of the canvas:

``````
function paint() {
 clear();
 circle(x, y, 10);
 
 if (x + dx > WIDTH || x + dx < 0)
 dx = -dx;
 if (y + dy > HEIGHT || y + dy < 0)
 dy = -dy;
 
 x += dx;
 y += dy;
}
``````

![Breakout V1](/images/breakoutv1-1.png) To prevent if from falling or going out of the boundaries we check if the new position will be outside of said boundaries and change the direction before we actually update it. If its current position plus `dx` or `dy` is outside (greater than `WIDTH` by the right, less than 0 by the left, greater than `HEIGHT` on top or less than 0 on bottom), we change the sign of `dx` or `dy` according so the ball goes in the opposite direction in that axis, creating the illusion of "bouncing". We can also make it accelerate if we increase `dx` or `dy`, but that will have to wait until there are levels in the future.

## Summary

We have plenty now. We created the canvas and put a ball inside. The ball moves constantly and bounces on all four walls, being this latest the key improvement for now. Next time we will create the bottom bar, control it and the blocks.

## Index

1. [Initialization](2022/10/15/breakout-v1-part-i)
2. **Refactor and Movement**
3. [The Blocks]()
4. [Finishing Touches]()