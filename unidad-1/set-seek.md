# Unidad 1
`🔎 Fase: Set + Seek`
_________________________________________________________________________________________________________________________________________________________________________________________
## Actividad 01
- https://www.youtube.com/watch?v=d2LC6Am9bZI
- https://www.youtube.com/watch?v=_8DMEHxOLQE
- https://www.youtube.com/watch?v=zBYVm2wYzDU
- https://www.youtube.com/watch?v=8tTGJvijoDw
- https://www.youtube.com/watch?v=5ETrXegVj_g
###
| Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.
####
Es dejar que la maquina se salga de curso y que rompa un poco las reglas que se le hayan asignado; que más que dejar que genere errores, permitir que cometa fluctuaciones calculadas.

## Actividad 02
[Link](https://editor.p5js.org/juanferfranco/sketches/GuYP9nwMk)
###
| Luego de ver el trabajo de Sofía piensa y escribe en TUS PROPIAS palabras:
###
- Cuál es el papel de la aleatoriedad en su obra.
###
La forma en que se dispersan las partículas al recibir las ondas sonoras.
###
- Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.
###
1. En un caso, se podría utilizar en pantallas verdes para ciertas partes de la animación. Es decir, según la pantalla (se pueden poner de diferentes colores para separar las secciones), poner un arte generativo diferente en esta o texturas.
2. Otro caso, un sistema que permita deformar ciertas partes de la animación según un algoritmo. (Un deformamiento tridimensional o simplemente de extension o achicamiento de la imágen)
3. Un último caso, combinar arte generativo con una animación pre hecha, de tal forma que el arte generativo siga alguna especie de ruta que la ayude a conectarse con la animación.

## Actividad 03
Analicemos juntos el código del ejemplo Example 0.1: A Traditional Random Walk del [texto guía](https://natureofcode.com/random/#example-01-a-traditional-random-walk).
###
| Realiza el siguiente experimento y reporta los resultados en tu bitácora:
###
- Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
###
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
Modificado: [https://editor.p5js.org/](https://editor.p5js.org/natureofcode/sketches/5C69XyrlsR)
``` js
let walker;

function setup() {
  createCanvas(640, 540);
  walker = new Walker();
  walker2 = new Walker();
  background('rgb(179,61,104)');
}

function draw() {
  background(random(200),10);
  walker.show();
  walker.step();
  walker2.show();
  walker2.step();
  //
  walker.show();
  walker.step();
  walker2.show();
  walker2.step();
  //
  walker.show();
  walker.step();
  walker2.show();
  walker2.step();
  //
  walker.show();
  walker.step();
  walker2.show();
  walker2.step();
  //
  walker.show();
  walker.step();
  walker2.show();
  walker2.step();
  //
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    square(this.x, this.y, 15);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++; this.x++;
    } else if (choice == 1) {
      this.x--; this.x--;
    } else if (choice == 2) {
      this.y++; this.y++;
    } else {
      this.y--; this.y--;
    }
  }
  
}
```
###
- Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
###
`background(random(200),10);` En esta parte, quería intentar que pareciera que el fondo cambia de tonalidad cada que se ejecute la función draw(). Así, como que lo hiciera con difuminado para que el walker pareciera que dejara una estela.
###
` walker.show();
  walker.step();
  walker2.show();
  walker2.step();` Que se dibujara uno encima del otro varias veces antes que se volviera a ejecutar la función draw().
###
`this.x++; this.x++; this.y++; this.y++;` Dupliqué estos comandos para intentar que se movieran más rápido y fuera más intersante la simulación.
###
- Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
###
Sucedió gran parte de lo que predije, aunque obtuve resultados que no esperaba, principalmente en la función draw().
###
- Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
###
Más o menos si, aunque a la hora de ver cómo los walker 1 y 2 dejaban una estela ocurrió algo muchos más interesante de lo que planié, recordándome a la estructura de un bismuto. Pondría mis errores en que desconozco bastante este lenguaje de programación.

## Actividad 04
# Distribuciones de probabilidad
Analicemos juntos y detenidamente [este](https://p5js.org/reference/p5/randomGaussian/) ejemplo.
###
- En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
###
En una distribución uniforme, en este caso de números aleatorios, sería cuando cada número tiene la misma probabilidad de ser seleccionado. Es decir, como las caras de un dado, en donde cada número tiene la probabilidad de 1/6 de ser el seleccionado. Por otro lado, en una no uniforme, pueden existir diferentes porcentajes para que aparezca cada número. Por ejemplo, el 2 tiene un 10%, y el 3 un 15%.
###
- Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.
``` js
let walker;

function setup() {
  createCanvas(640, 540);
  walker = new Walker();
  background('rgb(179,61,104)');
}

function draw() {
  background(200,70);
  walker.show();
  walker.step();
  //
  walker.show();
  walker.step();
  //
  walker.show();
  walker.step();
  //
  walker.show();
  walker.step();
  //
  walker.show();
  walker.step();
  //
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    circle(this.x, this.y, 15);
  }

  step() {
    const choice = floor(random((randomGaussian(1.5, 0.5)), 4));
    // const choice = floor(random(randomGaussian(1.5, 0.5), 4)); Tiende a la izquierda
    if (choice == 0) {
      this.x++; this.x++;
    } else if (choice == 1) {
      this.x--; this.x--;
    } else if (choice == 2) {
      this.y++; this.y++;
    } else {
      this.y--; this.y--;
    }
  }
  
}
```
Solo logré que "tendiese" a la izquierda, a pesar que en teoría debería ir a la derecha.

## Actividad 05
# Distribución Normal
Analicemos juntos y detenidamente [este](https://natureofcode.com/random/#example-04-a-gaussian-distribution) ejemplo.
###
Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.
###
- Crea un nuevo sketch en p5.js que represente una distribución normal.
- Copia el código en tu bitácora.
``` js
function setup() {
  createCanvas(700, 300); background('rgb(206,201,198)');
}

function draw() {
  // background(220);
    let x = random(620); let y = random(620);

  noStroke();
  fill(random(200), random(200), random(200), 10) //fill(0, 10);
  circle(x, y, random(30)); // (x,y,z)
}
```
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
###
https://editor.p5js.org/catflyx/sketches/-8zjcepeD
###
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
###
<img width="1919" height="587" alt="image" src="https://github.com/user-attachments/assets/e53cb4ed-92ab-4360-9509-8de55a254401" />

## Actividad 06
# Distribución personalizada: Lévy flight
Analicemos juntos y detenidamente el concepto de [Lévy flight](https://natureofcode.com/random/#a-custom-distribution-of-random-numbers).
- Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
###
Ejemplo elegido:
``` js

```
- Explica por qué usaste esta técnica y qué resultados esberabas obtener.
###
...
###
- Copia el código en tu bitácora.
``` js

```
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
###
...
###
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
###
...

## Actividad 07
# Ruido Perlin
Analicemos junto el concepto de ruido Perlin analizando la figura 0.4: “A graph of Perlin noise values over time (left) and of random noise values over time (right)”.

###
...
###

## Actividad 08
###
...
###

## Actividad 09
###
...
###

## Actividad 10
###
...
###

## Actividad 11
###
...
###


