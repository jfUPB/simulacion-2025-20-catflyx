# Unidad 3
`🔎 Fase: Set + Seek`
_________________________________________________________________________________________________________________________________________________________________________________________
### Inspitación
## Actividad 01
Iniciemos observando un video acerca del artista [Alexander Calder](https://youtu.be/FUchd9wwBoI?si=b-s6SZWgt_EEdrOP).

## Actividad 02
Te voy a presentar el estudio de diseño SOSO. Observa el proyecto [Data Structure](https://www.sosolimited.com/work/data-structure/).
###
Tomando como inspiración el proyecto de Data Structure, explora cómo las experiencias digitales pueden manifestarse en el mundo físico. Esto puede lograrse mediante la creación de instalaciones interactivas que fusionen elementos digitales y físicos, utilizando técnicas como la proyección, la realidad aumentada y la fabricación digital.
###
Por ejemplo, se podría desarrollar una instalación donde los jugadores interactúen con proyecciones en superficies físicas, creando una experiencia de juego que trascienda la pantalla tradicional. Otra posibilidad es diseñar esculturas cinéticas controladas por algoritmos generativos, donde la animación digital influya directamente en el movimiento físico de la obra.

## Actividad 03
### ¿Qué capítulo del libro guía vamos a trabajar?
Exploremos juntos el siguiente capítulo del texto guía que trabajaremos. Se trata de la [unidad 2 del libro The nature of code](https://natureofcode.com/forces/).

## Actividad 04
### Marco Motion 101
Rerpasemo rápido de qué se trata:
``` js
let mover;

function setup() {
    createCanvas(640, 240);
    mover = new Mover();
}

function draw() {
    background(255);
    mover.show();
    mover.update();
    mover.checkEdges();
}

.
.
.

update() {

    // Aquí calculo la aceleración
    .
    .
    .
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
}
.
.
.
```


## Actividad 05
### ¿Y qué relación tiene esto de las leyes de Newton con al arte generativo?
Antes de comenzar, disfruta esta simulación donde puedes experimentar de manera creativa las leyes de la atracción. Ahora si, volviendo al lío de Monte Pío (pero no tanto).
###
La ecuación vectorial de la segunda ley de Newton se expresa como:
<img width="140" height="56" alt="image" src="https://github.com/user-attachments/assets/dc93b0cd-a516-443c-be1f-e73eef1fb4dc" />
Donde:
<img width="750" height="283" alt="image" src="https://github.com/user-attachments/assets/6d72877a-3062-433a-ae2a-0d144a44a77f" />
Y lo siguiente lo dejaré a tu criterio, finalmente tu eres quien toma las decisiones en tu mundo de arte generativo ¿Qué tal si la `masa` es igual a 1? Yo no veo por qué no, finalmente, en el mundo de los pixeles el artista manda (¿O no?).
<img width="114" height="50" alt="image" src="https://github.com/user-attachments/assets/dd1d14e5-2a2f-4bca-8e08-e8e11718bd9e" />
Entonces ya tenemos la relación. En la unidad anterior tu definías en cada frame de la simulación un algoritmo para la aceleración. Ahora, la aceleración en cada frame la calcularemos como la influencia de todas las fuerzas sobre nuestros elementos gráficos.
###
Si volvemos a nuestro texto guía: "The Nature of code", verás que un elemento gráfica que se mueva en el canvas tendrá mínimo estas propiedades:
###
``` js
class Mover {
  constructor() {
    this.position = createVector();
    this.velocity = createVector();
    this.acceleration = createVector();
  }
}
```
Ahora, considera que en un frame actúan sobre este elemento dos fuerzas: `viento` y `gravedad`. Por tanto, en ese frame aplicarás las dos fuerzas:
``` js
mover.applyForce(wind);
```
y luego:
``` js
mover.applyForce(gravity);
```
Finalmente, en el método applyForce de la clase Mover tendrás algo como:
``` js
applyForce(force) {
  this.acceleration = force;
}
```
Y listo ¿Cierto? (No olvides que queremos calcular la aceleración en cada frame como la sumatoria de todas las fuerzas que actúan sobre un objeto.)
###
1. ¿Qué problema le ves a este planteamiento?
###
...
###
2. ¿Qué solución propones?
###
...
###
3. ¿Cómo lo implementarías en p5.js?
###
...

## Actividad 06
### La fuerza neta debe ser acumulativa
Ya te diste cuenta entonces que la fuerza neta es la sumatoria de todas las fuerzas que actúan sobre un objeto. Ahora, ¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?
``` js
mover.applyForce(wind);
mover.applyForce(gravity);
.
.
.
```
``` js
applyForce(force) {
  // Segunda ley de Newton, pero con acumulación de fuerza, sumando todas las fuerzas de entrada a la aceleración
  this.acceleration.add(force);
}
```
⚠️
###
Te diste cuenta qué pasó aquí con respecto a la actividad anterior? Vuelve a mirar.
###
...
###
Entonces en cada frame, la aceleración se calcula como la sumatoria de todas las fuerzas que actúan sobre un objeto:
###
``` js
mover.applyForce(wind);
mover.applyForce(gravity);
mover.update();
```
Y en el método `update()` se actualiza la `velocidad` y la posición del objeto:
``` js
update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
}
```
Pero calma. Notaste algo raro al final de `update()`?
###
...
###
- ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
###
...
###
- ¿Por qué se multiplica por cero justo al final de `update()`?
###
...

## Actividad 07
### En mi mundo los pixeles si tienen masa
Como en tu mundo los pixeles tienen masa, entonces, ¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?
###
...
``` js
mover.applyForce(wind);
mover.applyForce(gravity);
```
``` js
applyForce(force) {
    // Asume que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```
¿Y listo cierto? Pues ¡No!
###
*¿Qué ves raro?*
###
...
###
(Recuerda, ¿Cuándo se pasa algo a un función por valor y cuándo por referencia? En este caso, force es objeto de la clase p5.Vector, es decir, es un objeto que se pasa por referencia. ¿Qué implica esto?)

## Actividad 08
### Paso por valor y paso por referencia
En el siguiente código:
``` js
let friction = this.velocity.copy();
let friction = this.velocity;
```
1. ¿Cuál es la diferencia entre las dos líneas?
###
...
###
2. ¿Qué podría salir mal con `let friction = this.velocity;`?
###
...
###
3. De nuevo, toca repasar. ¿Cuál es la diferencia entre copiar por **VALOR** y por **REFERENCIA**?
###
...
###
4. En el fragmento de código ¿Cuándo es por **VALOR** y cuándo por **REFERENCIA**.
###
...

## Actividad 09
### Modelando fuerzas
En tu mundo de pixeles tu puedes crear las fuerzas o las puedes modelar.
###
Vamos a probar con lo segundo, modelar una fuerza. Ve a la sección Modeling forces del texto guía.
###
Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:
###
- Fricción.
- Resistencia del aire y de fluidos.
- Atracción gravitacional.
-------------------------------------------------------
1. Explica cómo modelaste cada fuerza.
###
...
###
Conceptualmente cómo se relaciona la fuerza con la obra generativa.
###
...
###
Copia el enlace a tu ejemplo en p5.js.
###
...
###
Copia el código.
``` js

```
Captura una imagen representativa de tu ejemplo.
###

