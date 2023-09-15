---
layout: post
title: "Breakout V1 Part I: Initialization"
description: "First part of a new classic minigame: breakout"
date: 2022-10-15 18:30:00 -0000
tag: JavaScript GameDev
serie: Breakout V1
---
<img style="float: left;" src="/images/breakoutv1-1.png">

**Breakout** is another classic among classics, it has thousands upon thousands of versions and variants in every platform ever and is great to kill some time. And it's incredibly simple: using a bar, like in Pong, and a bouncing ball, we change the direction of said bar to hit the blocks in the top, eliminating them and earning points. That's it.

## Description

We have three elements: a bar at the bottom that moves horizontally, a ball constantly bouncing in the walls and other elements, and some blocks in the upper half that get destroyed when the ball hits them. The goal of the game is eliminate all of the blocks, and the game ends if the ball touches the bottom wall below the bar before eliminating all the blocks. This time we will be able to use the mouse besides the keyboard to play.

In this first version, and like I did with Snake, we will be making only the gameplay, without any decorations.

## Requisites

Like in the previous game, there isn't any requisite, graphics, libraries or anything like that, but I'll be using HTML5 (canvas) and jQuery to simplify things, but it's not really necessary.

## Development

The first thing is create the HTML skeleton where we will make the game, and call the respective libraries. We will be using a 300x300 canvas:

``````
<!DOCTYPE HTML>
<html>
 <body>
 
 <!-- Initialize the 300x300 canvas -->
 <canvas id="canvas" width="300" height="300"></canvas>
 
 <!-- JQuery -->
 <script src="http://code.jquery.com/jquery-2.0.2.min.js" type="text/javascript"></script>
 <script src="breakout1.js" type="text/javascript"></script>
 </body>
</html>
```````

And the corresponding initializations to the canvas in Javascript:

```
// Get the reference to the canvas
var ctx = $('#canvas')[0].getContext("2d");
 
var w = $("#canvas").width(); // Store width and height
var h = $("#canvas").height();
 
// Draw the canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, w, h);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, w, h);
```

In line 2 we save a step by saving the context directly, instead of having an auxiliar variable to store the canvas like in Snake. In lines 6 to 10 we simply draw the border of the canvas and paint the background white.

## Circle

Nows lets see some new instructions to use with canvas besides filing a square:

``````
// Draw a circle
ctx.beginPath();
ctx.arc(75, 75, 10, 0, Math.PI*2, true);
ctx.fillStyle = "black";
ctx.closePath();
ctx.fill();
``````

In line 2 we see the `beginPath()` method that initializes a new figure, and `closePath()` on line 5 closes it. The instructions in between will be the ones that draw said figure. In our case we use `arc()` to draw a circle as an arc of 2*PI radius, whicih forms a full circle. 

`arc(x, y, r, sAngle, eAngle, direction)` takes six arguments:

1. `x` and `y` are the coordinates for the **center** of the circle.
2. `r` is the **radius.
3. `sAngle` is the angle in which the curve starts to draw in radians (0 is the 3 o`clock position).
4. `eAngle` is the final angle (2*PI equals to 0).
5. `direction` tells it to draw clockwise (0) or counterclockwise (1).

Then we define again the color of the figure with `fillStyle` y actually fill it with `fill()` so it's painted on the screen. If we use `stroke()` instead of `fill()`, instead of a full color circle we will obtain only the contour with the designated color.

`fillStyle()` and `fillStroke()` can receive arguments in many formats. We can use named colors, or pass it hexadecimal colors like HTML (#FFFFFF), or in `rgba(red,green,blue,alpha)` format to give it transparency.

## Summary

With this we have initialized the project, we have a first view of the game field and the ball that we will be using. Next time we will refactor and make some auxiliary functions, and start moving things!

## Index

1. **Initialization**
2. [Refactor and Movement](/2022/11/05/breakout-v1-part-ii)
3. [The Blocks]()
4. [Finishing Touches]()