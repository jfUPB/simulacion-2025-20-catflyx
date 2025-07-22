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
###

## Actividad 04
###
###

## Actividad 05
###
###

## Actividad 06
###
###

## Actividad 07
###
###

## Actividad 08
###
###

## Actividad 09
###
###

## Actividad 10
###
###

## Actividad 11
###
###


