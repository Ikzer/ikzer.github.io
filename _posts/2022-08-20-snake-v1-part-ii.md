---
layout: post
title: "Snake V1 Part II: The Snake"
description: "Let's start coding!"
date: 2022-08-20 10:30:00 -0000
tag: JavaScript GameDev
serie: Snake V1
---
![Snake](/images/snake1.jpg) 

It took me to the ninth post to actually start posting code here. But let's begin.

1. [HTML](#html)
2. [Canvas](#canvas)
3. [Defining the Snake](#defining-the-snake)
4. [Drawing the snake](#drawing-the-snake)
5. [Moving](#moving)
6. [Keyboard Inputs](#keyboard-inputs)
7. [Index](#index)

# HTML

To start, the first thing we need to do is create the base HTML5 that we will use, call the jQuery library and initialize the _canvas_ where we will draw the elements. So we create and ``index.html`` with this:

    ```
    !DOCTYPE HTML>
    `<html>
    `<body>
    `
    `<!-- Create 500x500 canvas -->
    <canvas id="canvas" width="500" height="500"></canvas>
    
    <!-- JQuery -->
    <script src="http://code.jquery.com/jquery-2.0.2.min.js" type="text/javascript"></script>
    
    </body>
    </html>
    ```

That's basically all the HTML we will need for this project. The rest will be JavaScript only. The ``canvas`` element while having its dimensions set will be a blank square for now, since we haven't draw anything inside, so it's pretty useless right now.

# Canvas

So let's initialize the `canvas` blank space. Within the ``<script></script>`` tags we type this:

```
$(document).ready(function(){
 // Initializa Canvas
 var canvas = $("#canvas")[0]; // The canvas variable contains our <canvas> element
 var ctx = canvas.getContext("2d"); // We initialize the 2D context of canvas in ctx
 var w = $("#canvas").width(); // We save width and height
 var h = $("#canvas").height();
 
 // Draw the canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, w, h);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, w, h);
 
});
```

La línea 1 es la inicialización de JQuery para ejecutarse cuando el documento esté listo para mostrarse, así no hay que llamarlo desde ningún sitio. En la línea 3 guardamos el canvas en una variable, sin más.

En la línea 4 ya hay algo interesante: el contexto. El método getContext() devuelve un objeto que provee de métodos y propiedades para dibujar en el canvas. El canvas en sí es eso: un lienzo, y se necesita obtener las herramientas para trabajar en él. Como argumento necesita el contexto que se solicita: en nuestro caso «2d«. ¿Significa esto que hay un contexto «3d»? Aún no como tal, pero existe el contexto «webgl» que permite trabajar con las APIs de WebGL y por tanto OpenGL-ES 2.0, pero eso se va mucho del objetivo actual.

El contexto provee de muchos métodos, tanto para obtener información de lo que hay en el interior del canvas, como para dibujar en él. En las línes 5 y 6 guardamos las dimensiones del canvas para tenerlas más accesibles. En las líneas 9 y 10 dibujamos el rectángulo que contendrá el juego. Con fillStyle podemos tanto obtener como definir el estilo (color, degradado o patrón) con el que está o estará rellenado el canvas y con fillRect() realizamos el relleno de un rectángulo con lo que hayamos definido en fillStyle, pasándole como argumentos las coordenadas de la esquina superior izquierda (x,y) y las dimensiones que tendrá (w, h). En las siguientes dos líneas hacemos lo mismo, pero esta vez con strokeStyle y strokeRect(), que en lugar del relleno definen el color de los bordes.

# Defining the Snake

Ahora toca crear la serpiente como tal, que será un array de celdas:

```
// Creamos la serpiente
 var snake_array; // Es un array de "celdas"
 
 create_snake();
 ```


Y la función para crear la serpiente:

```
// Rellenamos la serpiente
 function create_snake()
 {
  var length = 5; // Tamaño de la serpiente
  snake_array = []; //Inicializar el array vacío
  for(var i = length-1; i>=0; i--)
  {
   // Esto creará una serpiente horizontal
   // empezando en la esquina superior izquierda
   snake_array.push({x: i, y:0});
  }
 }
 ```

El número de celdas del array será el tamaño de la serpiente, y lo que hacemos es meter con el push elementos nuevos al final de ese array, constando cada uno de dos propiedades: su posición en x e y dentro de la cuadrícula. En este caso además opto por inicializarlo en la parte superior del canvas.

# Drawing the snake

En el siguiente paso toca empezar a pintar los elementos que forman parte del juego, y comenzaremos por la serpiente en sí. Llamaremos a la función paint(), que será crucial para dibujarlo todo:

``paint();``

Y la función en sí:

```
var cw = 10; // El tamaño de las celdas será de 10 px
 
function paint()
{
 
// Pintamos la serpiente
 for(var i = 0; i < snake_array.length; i++)
 {
  var c = snake_array[i];
  ctx.fillStyle = "red";
  ctx.fillRect(c.x*cw, c.y*cw, cw, cw);
  ctx.strokeStyle = "white";
  ctx.strokeRect(c.x*cw, c.y*cw, cw, cw);
 }
}
```

Por comodidad, y para su fácil acceso o modificación, guardamos el tamaño de las celdas, 10px, en una variable. En la función lo que hacemos, de momento, es un bucle en el que pasamos por todos los elementos de la serpiente uno a uno, guardándolos en la variable temporal c. Luego dibujamos, como ya vimos anteriormente con fillStyle, fillRect, strokeStyle y strokeRect, utilizando como parámetros las posiciones x e y de cada una, y lo multiplicamos por el tamaño de la celda, de manera que en cada iteración se dibuja un cuadrado azul de borde blanco de tamaño 10.

# Moving

Ahora viene el momento de mover la serpiente. La función setInterval(function, miliseconds) de JavaScript llama a la función function cada cierto tiempo, indicado en el segundo parámetro en milisegundos. Así, la llamada que haremos ejecutará la función paint() cada 60 milisegundos. Esto servirá en el futuro para crear el concepto de niveles y velocidad (que no irán en la V1):

``game_loop = setInterval(paint, 60);``

El movimiento de la serpiente se hará dentro de paint(), puesto que se tiene que ir actualizando y repintando constantemente:

```
function paint()
{
 // -- Movimiento de la serpiente --
 // La lógica es simple: quitar la cola del final y traerla al frente
 
 var nx = snake_array[0].x; // Guardamos la posición de la cabeza de la serpiente en nx y ny
 var ny = snake_array[0].y;
 
 nx++; // Incrementamos esos valores para obtener la nueva posición de la cabeza
 
 var tail = snake_array.pop(); // Sacamos la cola
 tail.x = nx; // Asignamos a la cola la posición de la cabeza
 snake_array.unshift(tail); // Metemos la cola en la primera posición
 
 ... código de pintar la serpiente
 
}
```

La idea a la hora de mover la serpiente de sitio es simple. Tiene que ganar un elemento en la cabeza y eliminar uno en la cola, así que llevaremos la cola a la cabeza. Primero obtenemos la posición de la cabeza y la incrementamos, de momento en x, para obtener la posición en la que debería terminar al moverse. A continuación quitamos la cola del array con ``pop()``, así tiene un elemento menos (el último), pero lo guardamos como tail. Le asignamos a ``tail`` la posición que debería tener la cabeza y lo metemos en la primera posición del array, convirtiéndolo en la nueva cabeza.

¿Porqué queremos «poner la cola en el lugar de la cabeza» en lugar de simplemente «eliminar la cola y dibujar una nueva serpiente en otras posiciones»? En primer lugar, realmente lo que hacemos sí es eso, o casi: eliminar la cola y dibujar una nueva cabeza, pero nos valemos del elemento cola para no tener que crear un nuevo objeto que tenga unos datos de x y de y, inicializarlo y asignarle la nueva posición. En segundo lugar, el concepto de «mover la cola a la cabeza» lo que consigue es que la serpiente se mueva como debe hacerlo, porque si la reiniciáramos en cada dibujo lo que haría es ser siempre recta (recordemos que la función ``create_snake()`` crea un array recto en una sola dirección), así que no la reiniciamos cada vez, solamente cambiamos la posición de uno de sus elementos, dejando el resto como estaban.

De esta manera vemos que la serpiente se mueve hacia la derecha eternamente… pero deja rastro. El problema es que lo único que estamos haciendo es que se dibuje una nueva celda en la posición nueva de la cabeza, porque nuestro bucle que dibuja la serpiente hace eso: dibujar, pero no «desdibuja» nada, por lo que añade elementos, pero no quita ninguno. Para solucionar esto utilizamos un truco: antes de dibujar la serpiente, volvemos a dibujar el canvas completo. El movimiento es pues «canvas blanco –> dibujar serpiente en nueva posición». Para ello, traemos el código de dibujar el canvas dentro de la función ``paint()``, antes de dibujar nada más:

```
function paint()
{
// Dibujar el canvas
 ctx.fillStyle = "white";
 ctx.fillRect(0, 0, w, h);
 ctx.strokeStyle = "black";
 ctx.strokeRect(0, 0, w, h);
 
 ...todo el código que ya teníamos en paint()
}
```

De momento se mueve, pero siempre hacia la derecha. Tenemos que añadirle restricciones de dirección. Guardaremos en una variable la dirección en que debe moverse la serpiente:

``var d = "right";  // Dirección del movimiento, por defecto a la derecha``

Y en lugar de incrementar la posición de la cabeza solo en ``x``, tendremos en cuenta su dirección:

```
// Modificamos esos valores en función de la dirección que tiene la serpiente
 if(d == "right") nx++;
 else if(d == "left") nx--;
 else if(d == "up") ny--;
 else if(d == "down") ny++;
 ```

Y también la nueva posición de la cabeza:

``tail.x = nx; tail.y = ny; // Asignamos a la cola la posición de la cabeza``

# Keyboard Inputs

Ahora la serpiente puede moverse en todas direcciones… por si sola. Toca añadir el reconocimiento de entrada por teclado, para poder interactuar con ella:

```
// Añadimos los controles del teclado
 // incluyendo una cláusula que impida ir "en sentido contrario"
 $(document).keydown(function(e){
var key = e.which;
if(key == "37" && d != "right") d = "left";
else if(key == "38" && d != "down") d = "up";
else if(key == "39" && d != "left") d = "right";
else if(key == "40" && d != "up") d = "down";
 // La serpiente ahora se puede controlar por teclado
 })
 ```

Simple: reconocemos la tecla que se ha pulsado, y según cuál sea le asignamos la dirección adecuada a la variable que almacena la dirección. Se añade la cláusula del «sentido contrario» para que no pase nada si se intenta mover mal, así no puede cambiar de sentido sobre sí misma.

De momento lo dejamos aquí. Tenemos un canvas con una serpiente azul que se mueve según le mandamos, siempre que sea dentro de los límites del plano, porque sino se nos desaparece. En la segunda parte añadiremos la «comida», las condiciones de fin de partida y la puntuación básica.

# Index

With this we're almost done. Next time we will make the fun part, with the eating and points elements of the gameplay. Enjoy!

1. [Introduction](https://ikzer.github.io/2022/08/19/snake-v1-part-i.html)
2. **The Snake**
3. The Game