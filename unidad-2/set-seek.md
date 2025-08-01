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
- ¿Qué resultado esperas obtener en el programa anterior?
###
Se crea un vector position, el cual se imprime en la consola. Posteriormente, se usa la función `playingVector();`, para modificar los componentes en `x` y `y` de `position`, y se vuelve a imprimir en la consola. El programa solo se ejecuta una vez por el `noLoop();`.
###
- ¿Qué resultado obtuviste?
###
Sucecidió lo planteado arriba, solo que el `noLoop();` lo que hace es que se detenga la ejecución de específicamente la función `draw()`, no de todo el programa.
###
- Recuerda los conceptos de *paso por valor* y *paso por referencia* en programación. Muestra ejemplos de este concepto en javascript.
###
Estos conceptos se pueden resumir en:
###
**Paso por valor:** Al entregarse una variable a una función para cambiar su valor, este cambio solo se toma en cuenta *dentro de la función*. Es decir, una vez esta se termine de ejecutar, el valor que tenía la variable seguirá siendo el mismo. Ejemplo:
###
``` js
let num;

function setup() {
    createCanvas(400, 400);
  
    num = 5;
    console.log("fuera de función 1:", num);
  
    cambiarValor(num); 
  console.log("fuera de función 2:", num);
    
    noLoop();
}

function cambiarValor(x) {
  x += 10;
  console.log("dentro de función:", x);
}

```
**Paso por referencia:** Al entregarse una variable a una función para cambiar su valor, este cambio si se toma en cuenta una vez acabe la función. Es decir, una vez esta se termine de ejecutar, el valor que tenía la variable se habrá modificado. En el caso de Javascript, las variables "primitivas" no se pueden pasar por referencia, por lo que es necesario usar objetos, arrays o funciones. Ejemplo:
``` js
let nums;

function setup() {
    createCanvas(400, 400);
  
    nums = [1, 2, 3];
    console.log("fuera de función 1:", nums);
  
    cambiarValor(nums); 
  console.log("fuera de función 2:", nums);
    
    noLoop();
}

function cambiarValor(x) {
  x[0] += 10;
  console.log("dentro de función:", x);
}
```
###
Al final se imprimen los mismos números en todos los casos.
###
- ¿Qué tipo de paso se está realizando en el código?
``` js
//Modificación superficial para verificar

let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9);
    console.log("1: ", position.toString());
    playingVector(position);
    console.log("3: ", position.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
  
  console.log("2: ", position.toString());
}

function draw() {
    background(220);
    console.log("Only once");
}
```
Paso por referencia, pues al imprimir los datos fuera de la función, estos siguen manteniendo los cambios.
<img width="449" height="111" alt="image" src="https://github.com/user-attachments/assets/8f345e2f-99f8-47db-8f74-fe95c931c327" />
###
- ¿Qué aprendiste?
###
Recordé que eran los pasos por referencia y por valor textualmente, y aprendí a modificar los componentes de una variable que almacena la dirección de un vector.

## Actividad 04
### Explora posibilidades
Vamos a darle una mirada a la clase p5.Vector [aquí](https://p5js.org/reference/p5/p5.Vector/).
###
- ¿Para qué sirve el método `mag()`? Nota que hay otro método llamado `magSq()`. ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
###
El método `mag()` sirve para calcular la magnitud de un vector en dos dimensiones, es decir, solo se usan los componentes `x` y `y`. Por otro lado, `magSq()` hace esto mismo, solo que calcula la magnitud del vector al *cuadrado*. A la hora de la verdad, entendería que el `magSq()` se usa para situaciones más específicas, por lo que diría que el `mag()` es el más eficiente y versatil en usos.
###
- ¿Para qué sirve el método `normalize()`?
###
Escala los componentes del objeto del vector para que su magnitud sea igual a 1.
###
- Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método `dot()`? ¿Qué le responderías en un frase?
###
Que calcula el producto punto entre dos vectores.
###
- El método `dot()` tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
###
En la **instancia**, lo que hace es que realiza el producto punto a partir de uno de los dos vectores, osea, desde un objeto vector. Por ejemplo, al usarlo de esta forma `v1.dot(v2);`, se realizará el producto punto desde el vector 1 (v1), con entre v1 y v2. Por otro lado, en la **estática**, se forma un tercer vector usando el producto punto entre v1 y v2, sin que se tome referencia directa de ninguno de estos. Por ejemplo, en `p5.Vector.dot(v1, v2);`, se realizará lo dicho, siendo que como ahora están ambos dentros del parentesis, se calculará el producto punto con estos dos para hacer el tercero.
###
- Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del 
producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
###
La interpretación geométrica es algo similar a un triangulo, pues, se forma un vector perpendicular al plano que se forma a partir de los dos vectores; siendo que, su magnitud es el área del paralelogramo formado por los dos vectores originales, y su orientación apunta afuera del plano creado por estos.
###
- ¿Para qué te puede servir el método `dist()`?
###
Sirve para calcular la distancia entre dos puntos que son representados con vectores, lo cual puede llevar a usarse para movimientos de cierta forma, coordinados, entre ambos vectores.
###
- ¿Para qué sirven los métodos `normalize()` y `limit()`?
###
`normalize()` escala los componentes del objeto del vector para que su magnitud sea igual a 1, y `limit()` limita la magnitud de un vector usando un valor máximo que este establece.

## Actividad 05
### ¿Interpolamos?
Vas a tomar como inspiración este ejemplo de la referencia de p5.js:
``` js
function setup() {
    createCanvas(100, 100);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(30, 0);
    let v2 = createVector(0, 30);
    let v3 = p5.Vector.lerp(v1, v2, 0.5);
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, 'purple');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
Hay que modificarlo para generar el resultado indicado:
``` js

``` 
- Analiza cómo funciona el método `lerp()`.
- Nota que además de la interpolación lineal de vectores, también puedes hacer interpolación lineal de colores con el método `lerpColor()`.
###
Dedica un tiempo a estudiar cómo se dibuja una flecha en el método `drawArrow()`.
###
- El código que genera el resultado que te pedí.
Flecha interpolada **|✓|**, Colores interpolados **|X|**, Vector verde **|X|**.
- ¿Cómo funciona `lerp()` y `lerpColor()`.
###
...
###
- ¿Cómo se dibuja una flecha usando `drawArrow()`?
###
...

## Actividad 06






## Actividad 07




