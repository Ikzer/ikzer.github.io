---
layout: post
title: "Breakout V1 Part I: The Ball"
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

Nows lets see some new instructions to use with canvas besides filing a square:

``````
// Dibujar un círculo
ctx.beginPath();
ctx.arc(75, 75, 10, 0, Math.PI*2, true);
ctx.fillStyle = "black";
ctx.closePath();
ctx.fill();
``````

En la línea 2 vemos el método beginPath() que se encarga de inicializar una nueva forma, y closePath() en la línea 5 la termina. Las instrucciones que vayan en medio serán las que dibujen dicha forma. En nuestro caso utilizamos la forma arc() para dibujar el círculo como si se tratase de un arco o curva de radio 2*PI, o lo que viene siendo un círculo completo. arc() toma 6 argumentos: (x, y, r, sAngle, eAngle, dirección). x e y son las coordenadas del punto central del círculo, r es el radio, sAngle es el ángulo desde el que comienza a dibujarse la curva en radianes (0 es en la posición de las 3 en punto), eAngle es el ángulo final (2*PI es igual que 0) y dirección se refiere a si se dibujará en el sentido de las agujas del reloj (0 = sentido de las agujas del reloj, 1 = sentido contrario).

Luego definimos otra vez el color de relleno con fillStyle() para sobreescribir el blanco del fondo, y finalmente rellenamos con fill() para que la figura salga por pantalla. Si en lugar de fill() utilizamos el método stroke(), en lugar de un círculo relleno obtendríamos solo su contorno con el color que hayamos asignado a fillStroke() (en nuestro caso lo hemos definido como negro anteriormente).

El tema de los estilos da para mucho. A fillStyle() y fillStroke() se le puede mandar información en varios formatos. Podríamos por ejemplo pasarle información en formato hexadecimal como hace el HTML normalmente (#FFFFFF), o en rgba (red, green, blue, alpha) para darle transparencia. Eso es cosa de experimentar.

De momento tenemos dibujada una bola estática. Vamos a moverla. Para empezar, inicializamos las variables de posición y creamos una función init() para tener un estado inicial de juego y el intervalo de actualización del dibujo que llamará a la futura función paint(), encargada de dibujar el contenido y realizar los movimientos:

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


Las variables x e y corresponden a la posición inicial de la bola en el cuadro, en este caso en el centro, mientras que dx y dy son las de velocidad, en ambos ejes, que dictarán cuánto se mueve la bola en cada dirección.

A la función paint() le corresponde ahora pintar la bola y hacer que se mueva, para lo que tiene que redibujar todo en cada iteración:

````
function paint() {
 // Limpiamos el canvas
 ctx.clearRect(0,0,300,300);
 // Dibujar el canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, w, h);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, w, h);
 
 // Dibujar el círculo
 ctx.beginPath();
 ctx.arc(x, y, 10, 0, Math.PI*2, true);
 ctx.fillStyle = "black";
 ctx.closePath();
 ctx.fill();
 x += dx;
 y += dy;
 }
 `````
 
Para empezar, en la línea 3 vemos la función clearRect() con la que borramos todo lo que anteriormente había en el cuadro de 300×300 (que para nosotros se corresponde con todo el canvas). A continuación, trasladamos al interior de esta función las instrucciones que utilizamos al principio para dibujar el canvas (fondo blanco y borde negro), luego trasladamos también las instrucciones para dibujar el círculo, pero esta vez en lugar de ponerle un lugar fijo como al principio, utilizamos las posiciones x e y creadas anteriormente. En init() habíamos definido un setInterval() que llamará a esta función cada 10ms, por lo que en cada intervalo se redibujará todo. Para crear el movimiento habíamos creado las variables de velocidad dx y dy así que en cada llamada cambiaremos el valor de la posición sumándole estas variables, de manera que, por ejemplo, si en la primera llamada el centro está en (150,150), en la siguiente estará en (152,154), luego en (154,158) y así sucesivamente. Así, cada vez que se dibuja el frame, la bola está en una posición distinta, creando la sensación de animación. Probar distintos valores (incluyendo negativos) en dx y dy hará que se mueva a distintas velocidades y direcciones.

Vamos a necesitar repetir unas cuantas cosas constantemente a partir de ahora, por lo que poner un poco de orden en el código viene bastante bien. Crearemos funciones para las cosas más básicas: dibujar el frame, dibujar el círculo, inicializar el juego y dibujar un rectángulo, además de definir unas cuantas variables. El código completo quedaría así:

Variables:

``````
var x = 150; // Posición inicial en X
var y = 150; // Posición inicial en Y
var dx = 2; // Velocidad en X
var dy = 4; // Velocidad en Y
var WIDTH; // Ancho del frame
var HEIGHT; // Alto del frame
var ctx;
``````

Initialize everthing:

``````
function init() {
 ctx = $('#canvas')[0].getContext("2d"); // Obtener el canvas
 WIDTH = $("#canvas").width(); // Asignar su tamaño a las variables
 HEIGHT = $("#canvas").height();
 return setInterval(paint, 10); // Llamar a paint() cada ms
}
``````

Draw a circle:

``````
// Dibujar un círculo de centro en (x,y) y de radio r
 
function circle(x,y,r) {
 ctx.beginPath();
 ctx.arc(x, y, r, 0, Math.PI*2, true);
 ctx.closePath();
 ctx.fill();
}
``````

Draw a rectangle:

``````
// Dibujar un rectángulo de ancho w, alto h, con la esquina superior izquierda en (x,y)
 
function rect(x,y,w,h) {
 ctx.beginPath();
 ctx.rect(x,y,w,h);
 ctx.closePath();
 ctx.fill();
}
``````

Clean the frame:

``````
function clear() {
// Limpiamos el canvas
 ctx.clearRect(0, 0, WIDTH, HEIGHT);</pre>
// Dibujar el canvas
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
 circle(x, y, 10);
 
 x += dx;
 y += dy;
}
 
init();
``````

La parte de las funciones anteriores a paint() no debería modificarse mucho (o nada) en el futuro, queda como una librería básica para ir utilizando cuando sea necesario.

Bien, tenemos unas funciones definidas y una bola que se mueve en una dirección concreta y que se cae por algún sitio siempre. Nos queda contenerla dentro de los bordes del cuadro:

``````
function draw() {
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
![Breakout V1](/images/breakoutv1-1.png) Para evitar que se salga, lo que hacemos es que cuando su nueva posición se encuentre fuera de los límites del cuadro le cambiamos la dirección antes de actualizarla. Es decir, si su posición actual más la dx o dy quedan fuera de los límites (mayor que WIDTH por la derecha, menor que 0 por la izquierda, mayor que HEIGHT por abajo, menor que 0 por arriba), cambiamos el signo de dx o dy (según corresponda) para que la bola comience a ir en dirección opuesta, creando la sensación de que «rebota» en los bordes. Podríamos hacer también que cada vez que choque acelere un poco si además incrementamos dx o dy, aunque eso quedará para el tema de los niveles de dificultad (aumento de velocidad) en el futuro.

Por ahora tenemos bastante. Hemos creado el cuadro y le hemos puesto una bola que se mueve constantemente y rebota en sus cuatro lados, siendo esto último quizás lo más importante. En la próxima parte veremos cómo incluir la barra de abajo y cómo controlarla, los bloques a destruir y la manera de hacerlo.

## Index

1. **The Ball**
2. [The Blocks]()
3. [Finishing Touches]()