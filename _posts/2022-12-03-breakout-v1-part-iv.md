
Para terminar por hoy, vamos a colocar los bloques.

Necesitamos una matriz de 2×2 (un array de 2 dimensiones) para representar los bloques y el estado en que se encuentran (golpeado o no). Esto nos crea la necesidad de almacenar seis datos:

``````
var bricks; // El array de bloques
var NROWS = 5; // Número de filas
var NCOLS; = 5 // Número de columnas
var BRICKWIDTH; // Ancho de los bloques
var BRICKHEIGHT = 10; // Altura de los bloques
var PADDING = 1; // Separación entre bloques
``````

Lo único que necesitamos inicializar en init() es el ancho, que dependerá del tamaño del canvas para que sean todos iguales:

``BRICKWIDTH = (WIDTH/NCOLS) - 1;``

Y creamos una función para crear el array y rellenarlo:

``````
// Rellenar el array de bloques
// 1 = Mostrar bloque
// 2 = Bloque golpeado, no mostrar
function initbricks() {
bricks = new Array(NROWS);
for (i=0; i < NROWS; i++) {
bricks[i] = new Array(NCOLS);
 for (j=0; j < NCOLS; j++) {
 bricks[i][j] = 1;
 }
 }
}
``````

Es simplemente un bucle anidado de dos dimensiones, iteramos sobre cada fila, y en cada fila iteramos por las columnas y asignándole el valor 1 a cada elemento, significando esto que el bloque es válido y que, cuando dibujemos, hay que ponerlo. Llamaremos a esta función una sola vez, al iniciar por primera vez el juego después de llamar a init().

Creamos también la función paintbricks() con la que dibujaremos los bloques:

``````
// Pintar los bloques
// Saltar el espacio si el valor del bloque es 0
function paintbricks() {
 for (i=0; i < NROWS; i++) {
 for (j=0; j < NCOLS; j++) {
 if (bricks[i][j] == 1) {
 rect((j * (BRICKWIDTH + PADDING)) + PADDING,
 (i * (BRICKHEIGHT + PADDING)) + PADDING,
 BRICKWIDTH, BRICKHEIGHT);
 }
 }
 }
}
``````

En esta función iteramos nuevamente sobre los dos arrays y dibujamos el rectángulo solo si el valor del bloque es 1 (no ha sido golpeado). El cuadrado que dibujamos tiene que ver con la posición del bloque en su fila y columna. La función rect() recibe 4 argumentos:

``rect(x, y, w, h)``

Siendo x e y las coordenadas del punto en el que se dibuja el cuadro. En este caso, los argumentos que le pasamos son:

* x = (j * (BRICKWIDTH + PADDING)) + PADDING: la columna correspondiente multiplicada por el ancho más la separación entre bloques, añadiéndole otra separación (así hay separación por ambos lados). De esta manera comienza a dibujarlo NCOL veces a la derecha del inicio.

* y = i * (BRICKHEIGHT + PADDING)) + PADDING: la fila correspondiente multiplicada por el alto más la separación entre bloques, añadiéndole otra separación (así hay separación por arriba y por abajo).

* w = BRICKWIDTH: el ancho del bloque.

* h = BRICKHEIGHT: el alto del bloque.

[breakoutv1-3] Ya tenemos dibujados los bloques. El toque final será hacer que la bola rebote con ellos y los haga desaparecer. Lo que viene siendo el juego en sí. Para ello, volvemos a retocar la función paint().

Tenemos que verificar si la bola se encuentra dentro del área de los bloques, y si lo está, determinar a cuál le ha dado. Primero, guardamos los tamaños de las filas y las columnas:

``````
// Verificar si le damos a un bloque
 rowheight = BRICKHEIGHT + PADDING; // Altura de una fila
 colwidth = BRICKWIDTH + PADDING; // Ancho de una columna
 ``````

Localizar la posición de la bola en relación con las filas y columnas dentro de todo el canvas, como un entero, dividiendo su posición entre la altura o ancho de fila/columna:

``````
row = Math.floor(y/rowheight); // Fila en la que se encuentra la bola
col = Math.floor(x/colwidth); // Columna en la que se encuentra la bola
``````

Comprobar la colisión:

``````
// Si coincide, rebotar la bola y marcar el bloque como golpeado
 if (y < NROWS * rowheight && row >= 0 && col >= 0 && bricks[row][col] == 1) {
 dy = -dy;
 bricks[row][col] = 0;
 }
 ``````

Vamos a desgranar lo que hace esta comprobación:

* En primer lugar, recordemos que para la altura, cuanto menor sea y más arriba estará.
* Comprueba si la posición vertical (y) es menor que la altura total que tienen todas las filas (NROWS * rowheight).
* Comprueba si se encuentra en una fila (row >= 0) o una columna (col >= 0) es decir, no está en un intermedio.
* Comprueba si el bloque al que corresponde la fila y columna en la que se encuentra es válido (bricks[row][col] == 1).
* Si todo ello es cierto (la posición está dentro del rango de las filas, está en colisión con una de ellas en concreto y no se encuentra vacía) hacemos rebotar la bola (dy = -dy) y marcamos esa posición del array de bloques como golpeado para que no se dibuje en la próxima iteración.

[breakoutv1-4] Y con esto ya tenemos la jugabilidad del Breakout terminada. Tenemos una bola en constante movimiento, una barra controlada por ratón o teclado y unos bloques que desaparecen si la bola choca con ellos. De momento lo dejo aquí, aunque haré una tercera parte en la que daremos unos pequeños retoques, corregiremos algún pequeño fallo y le daremos algo de estilo para que no quede tan feo. Aunque, técnicamente, podría quedarse así puesto que el gameplay ya está hecho.