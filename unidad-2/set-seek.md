# Unidad 2
`🔎 Fase: Set + Seek`
_________________________________________________________________________________________________________________________________________________________________________________________
# VECTORES
En esta unidad repasarás el concepto de vector y algunas de sus operaciones básicas y experimentarás con conceptos matemáticos y físicos, como velocidad y aceleración, para controlar el movimiento en tus proyectos interactivos.
## Actividad 01
Comencemos por el ejemplo: [Example 1.2: Bouncing Ball with Vectors!](https://natureofcode.com/vectors/#example-12-bouncing-ball-with-vectors)
###
- ¿Cómo funciona la suma dos vectores en p5.js?
###
Se suman los componentes en `x` y `y` de la posición y velocidad, y luego se *actualiza* la información del vector. En este caso, el de posición. 
###
- ¿Por qué esta línea `position = position + velocity;` no funciona?
###
Es por la biblioteca, pues esta no puede sumar las direcciones de los vectores usando este método. Aunque, se puede usando: `position.add(velocity);`, por ejemplo. En este se agregan los componentes de `velocity` a `position`, y se actualiza la información del vector.

## Actividad 02                
# Repasa
Realiza este ejercicio del libro [Exercise 1.1](https://natureofcode.com/vectors/#exercise-11)
###
- ¿Qué tuviste que hacer para hacer la conversión propuesta?
###
Cambiar los componentes `this.x` y `this.y` al de un vector `position`, en donde almacene las mismas propiedades que tenían para moverse en `x` y `y`.
###
- Muestra el código que utilizaste para resolver el ejercicio.
``` js
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
    this.position = createVector(width / 2, height / 2);
    
    //this.x = width / 2;
    //this.y = height / 2;
  }

  show() {
    stroke(0);
    //point(this.position.x, this.position.y);
    point(this.position);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      //this.x++;
      this.position.x++;
    } else if (choice == 1) {
      //this.x--;
      this.position.x--;
    } else if (choice == 2) {
      //this.y++;
      this.position.y++;
    } else {
      //this.y--;
      this.position.y--;
    }
  }
}
```

## Actividad 03

## Actividad 04

## Actividad 05

## Actividad 06

## Actividad 07
