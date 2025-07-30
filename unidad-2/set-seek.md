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
### Repasa
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
    //point(this.position*mouseX/width, this.position*mouseY/height);
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
### Experimenta
Dale una mirada a este código:
``` js
let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9);
    console.log(position.toString());
    playingVector(position);
    console.log(position.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```
¿Qué resultado esperas obtener en el programa anterior?
###
...
###
¿Qué resultado obtuviste?
###
...
###
Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
###
...
###
¿Qué tipo de paso se está realizando en el código?
###
...
###
¿Qué aprendiste?
###
...

## Actividad 04
### Explora posibilidades
Vamos a darle una mirada a la clase p5.Vector [aquí](https://p5js.org/reference/p5/p5.Vector/).
###
¿Para qué sirve el método `mag()`? Nota que hay otro método llamado `magSq()`. ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
###
...
###
¿Para qué sirve el método `normalize()`?
###
...
###
Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método `dot()`? ¿Qué le responderías en un frase?
###
...
###
El método `dot()` tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
###
...
###
Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del 
producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
###
...
###
¿Para que te puede servir el método `dist()`?
###
...
###
¿Para qué sirven los métodos `normalize()` y `limit()`?
###
...

## Actividad 05 - Viernes

## Actividad 06

## Actividad 07
