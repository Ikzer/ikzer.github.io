---
layout: post
title: "GameRunner: The Minimal JavaScript Game Framework"
description: "Let's finish our first minigame!"
date: 2022-10-01 18:30:00 -0000
tag: JavaScript GameDev
serie: Game Runner
---
# GameRunner: The Minimal JavaScript Game Framework

GameRunner is a minimal JavaScript game framework that provides a simple and easy-to-use API for creating 2D games. It is based on the Phaser game engine, but it is stripped down to its bare essentials.

## Features

* **Simple API:** GameRunner provides a simple and easy-to-use API that makes it easy to create 2D games.
* **Efficient:** GameRunner is highly efficient, making it ideal for games that need to run on mobile devices.
* **Extendable:** GameRunner is extensible, allowing you to add your own features and functionality.

## Getting Started

To get started with GameRunner, you will need to install the following dependencies:

``npm install gamerunner``

Once you have installed the dependencies, you can create a new game by creating a new HTML file and adding the following code:

```
html
<!DOCTYPE html>
<html lang="en">
<head>
<title>My Game</title>
</head>
<body>
<canvas id="game" width="640" height="480"></canvas>
<script src="gamerunner.js"></script>
<script>
// Create a new game instance
const game = new GameRunner({
canvas: document.querySelector("#game"),
});

// Start the game
game.start();

// Update the game loop
game.onUpdate(() => {
  // ...
});

// Render the game
game.onDraw(() => {
  // ...
});
</script>
</body>
</html>
```

This code will create a new game instance with a canvas width of 640 pixels and a height of 480 pixels. It will also start the game loop, which will run every frame.

## Update and Draw Callbacks

The game loop is called twice per frame, once for the ``update()`` callback and once for the ``draw()`` callback. The ``update()`` callback is called to update the game state, and the ``draw()`` callback is called to render the game to the canvas.

## Rendering Entities

GameRunner uses entities to represent objects in the game world. To create an entity, you can use the createEntity() method.

``````
javascript
// Create a new entity
const entity = game.createEntity();

// Set the entity's position
entity.position.x = 100;
entity.position.y = 100;

// Set the entity's size
entity.size.x = 200;
entity.size.y = 200;

// Set the entity's sprite
entity.sprite = new Sprite("assets/sprite.png");
``````

To render an entity, you can use the `drawEntity()` method.

```
javascript
// Render the entity
game.drawEntity(entity);
``````

## Events

GameRunner supports events that can be used to listen for game-related events. To listen for an event, you can use the `on()` method.

````
javascript
// Listen for the "click" event
game.on("click", (event) => {
  // ...
});
````

## Conclusion

GameRunner is a simple and easy-to-use JavaScript game framework that makes it easy to create 2D games. It is based on the Phaser game engine, but it is stripped down to its bare essentials.
