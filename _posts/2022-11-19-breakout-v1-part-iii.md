---
layout: post
title: "Breakout V1 Part III: The Blocks"
description: "Building the blocks and adding the actual gameplay."
date: 2022-11-19 18:30:00 -0000
tag: JavaScript GameDev
serie: Breakout V1
---
<img style="float: left; width: 25%;" src="/images/breakout1.png">
Let's continue building the Breakout game, the game in which you destroy blocks by bouncing a ball against them. We left the game in a basic state, with a ball bouncing endlessly on the walls of the canvas, and a minimal library of funtions to help refactor code. This time we will talk about the blocks.

## The Paddle

First o all we have to add the paddle that we will control, where the ball should bounce to not lose the game. First we define its dimensions:

``````
var paddlex;
var paddleh;
var paddlew;
 
function init_paddle() {
 paddlex = WIDTH / 2;
 paddleh = 10;
 paddlew = 75;
}
``````

`paddlex` is the center point of the bar, it will be useful later. Then we modify the `paint()` to draw the bar and make the ball bounce on the superior edge of it (not below), and that it only bounces below if it's touching the paddle:

``````
function paint() {
 clear();
 circle(x, y, 10);
 
// Draw the paddle
 rect(paddlex, HEIGHT-paddleh, paddlew, paddleh);
 
 // Check boundaries to bounce
 if (x + dx > WIDTH || x + dx < 0)
 dx = -dx;
 
if (y + dy < 0)
 dy = -dy;
 else if (y + dy > HEIGHT) {
 if (x > paddlex && x < paddlex + paddlew)
 
// If it's inside the paddle we change direction
 dy = -dy;
 else
 // If its outside the bar, we stop the animation and end the game
 clearInterval(game_loop);
 }
 
 x += dx;
 y += dy;
}
 
var game_loop = init();
init_paddle();
``````

FOr starters, on line 6 we draw the rectangle from the middle point `paddlex` with the dimensions we defined previously.

Between lines 11 and 21 we introduce the "magic" so the ball bounce on the paddle and stops if it falls outside. On line 8 we see the condition to bounce on the left and right walls:

``if (x + dx > WIDTH || x + dx < 0)``

If the new `x` position is bigger than the `WIDTH` or less than 0 we change the direction.

We did the same on `y` before:

``if (y + dy > HEIGHT || y + dy < 0)``

[breakoutv1-2] But this time we only want it to bounce if it touches te top wall, so we remove the condition for `(y + dy > HEIGHT)` on line 9, leaving just `(y + dy < 0)` to make it bounce, and introducing another condition for the bottom boundaries since we need to make more checks before. 

If the ball its about to fall from the bottom wall, we check it its position on `x` its inside our paddle `(x < paddlex + paddlew)`. If its inside that interval, we make it bounce changing the direction of `dy`, and if it's not it means it's outside the paddle so we lost the game and we have to stop the animation.

To stop the animation we need two things. First, with `clearInterval(game_loop)` we stop the `setInterval()` that we create on `init()`. But to clear that particular interval we need to pass it to `clearInterval`, in this case using the `game_loop` variable. As we set up `init()` to return the `setInterval()`, we store it:

`var game_loop = init()`

And that's what we pass to `clearInterval()`.

## Movement

We have two of the main elements on stage. Let's make them interactive. As I said, we will use the keyboard as well as the mouse to move the paddle, which moves only in one dimension (on the X axis), which makes things a lot easier.

This time the movement needs to be constant, unlike last time, so it's not enough if a key has been pressed: we need to know if it's still pressed to keep the paddle moving. To do that we have to follow a few stpes, and we will use a little jQuery magic to ease things out:

 * Know if a key has been pressed
 * Move the paddle while it's pressed
 * Know if a key stopped being pressed
 * Stop the paddle when it stops being pressed

First of all, we store the state of the pulsation in two variables (left and right), assuming they are not pressed:
````
rightDown = false; 
leftDown = false;
````
And to not repeat code everywhere, we create two functions, `onKeyDown()` and `onKeyUp()` to change the state of said variables when needed (toggle true and false), and then bind the functions to the browser events via jQuery:

``````
// Activate leftDown or rightDown if hte left or right arrow keys are pressed
function onKeyDown(evt) {
 if (evt.keyCode == 39) rightDown = true;
 else if (evt.keyCode == 37) leftDown = true;
}
// And deactivate them when not
function onKeyUp(evt) {
 if (evt.keyCode == 39) rightDown = false;
 else if (evt.keyCode == 37) leftDown = false;
}
 
// Bind these functions to their corresponding events
// that will trigger them when they are pressed
$(document).keydown(onKeyDown);
$(document).keyup(onKeyUp);
``````
Once we have detected the key pulsation, we need to move the paddle accordingly (on the `paint()` function):

``````
function paint() {
 clear();
 circle(x, y, 10);
 
 // Move the paddle if left or right are pressed
 if (rightDown) paddlex += 5;
 else if (leftDown) paddlex -= 5;
 
// Draw the paddle
 rect(paddlex, HEIGHT-paddleh, paddlew, paddleh);
 
 if (x + dx > WIDTH || x + dx < 0)
 dx = -dx;
 
if (y + dy < 0)
 dy = -dy;
 else if (y + dy > HEIGHT) {
 if (x > paddlex && x < paddlex + paddlew)
 dy = -dy;
 else
 clearInterval(game_loop);
 }
 
 x += dx;
 y += dy;
}
 
var game_loop = init();
``````
Here in the lines 5 to 7 we update the `x` position of the paddle by checking the right and left movements variables before drawing its rectangle, then draw it normally.

I'm refactoring some code now to introduce the `init_paddle()` inside of `init()`, since we just need to initialize the position of `paddlex` once, as the others are constants and can be defined as globals.

For the mouse movement we need to work a little bit. First, we need to know if the cursor is inside the canvas, for which we use the variables `canvasMinX` and `canvasMaxX`, that gives us the left and right boundaries positions on the screen. We declare them as globals and initialize them on `init()`:

``````
canvasMinX = $("#canvas").offset().left;
canvasMaxX = canvasMinX + WIDTH;
``````
For `canvasMinX` we get the pòsition of the left wall of the canvas in relative to the left of the window via the `offset()` method of jQuery, that gives us the positoin of an object relative to the document. The position of the right will be left plus the width.

The, as we did with the keyboard, we create the helper `onMouseMove()` function:
``````
// Move the paddle if the mouse is inside the canvas:
function onMouseMove(evt) {
 if (evt.pageX > canvasMinX && evt.pageX < canvasMaxX) {
 paddlex = evt.pageX - canvasMinX;
 }
}
 
// We bind the mouse movement function to the browser event
$(document).mousemove(onMouseMove);
``````
We pass this function to the event manager `evt`, it will check the position of the mouse on X (`evt.pageX`) and if it's inside the canvas we change the position of the bar to where the mouse is (only on X). Finally we bind it via jQuery like before.

## Summary

With this we have the basic interaction and a working ball and paddle. Next comes the blocks.

## Index

1. [Initialization](2022/10/15/breakout-v1-part-i)
2. [Refactor and Movement](/2022/11/05/breakout-v1-part-ii)
3. **The Paddle**
4. [The Blocks](/2022/12/03/breakout-v1-part-iv)
5. [Finishing Touches]()