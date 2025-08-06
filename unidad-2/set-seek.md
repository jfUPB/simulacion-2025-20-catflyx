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
let inter = 0; let back = false;

function setup() {
    createCanvas(500, 500);
}

function draw() {
    background('rgb(173,169,162)');

    let v0 = createVector(50, 50);
    let v1 = createVector(350, 0);
    let v2 = createVector(0, 350);
    let v3 = p5.Vector.lerp(v1, v2, inter);
    
  let o = p5.Vector.add(v0, v1); //origen
  let d = p5.Vector.add(v0, v2); //destino
  let dir = p5.Vector.sub(d, o); //dirección
  
    drawArrow(v0, v1, 'rgb(182,2,2)');
    drawArrow(v0, v2, 'rgb(51,51,177)');
    drawArrow(v0, v3, lerpColor('rgb(182,2,2)', 'rgb(51,51,177)', inter)); 
  //Interpolación color
    drawArrow(o, dir, '#4CAF50');
  
  if (inter >= 1){ //Pa#539555venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.01;
      } else {
        inter += 0.01;
      }
  console.log("interpolación: ", inter, " - Modo: ", back); //Comprobador
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
- Analiza cómo funciona el método `lerp()`.
- Nota que además de la interpolación lineal de vectores, también puedes hacer interpolación lineal de colores con el método `lerpColor()`.
###
Dedica un tiempo a estudiar cómo se dibuja una flecha en el método `drawArrow()`.
###
- El código que genera el resultado que te pedí.
###
Flecha interpolada **|✓|**, Colores interpolados **|✓|**, Vector verde **|✓|**.
###
- ¿Cómo funciona `lerp()` y `lerpColor()`.
###
`lerp()` es una función que permite *interpolar* dos componentes entre sí, siendo el caso de `lerpColor()`, interpolando dos colores (definidos en orden) y luego poniendo qué tanto se interpolarán, siendo valores de 0 a 1.
###
- ¿Cómo se dibuja una flecha usando `drawArrow()`?
###
Esta está compuesta por dos partes: La línea y la punta. Para la línea se usa un `stroke()`, para el cual se usan los componentes en `x` y `y` para ser dibujada, además de definir su rotación con el método `heading()`. La punta por otro lado, se contruye dibujando un triángulo, que toma de referencia una variable previamente definida llamada `arrowSize`.

## Actividad 06
### Motion 101
En la sección del texto guía llamada [Motion with vectors](https://natureofcode.com/vectors/#motion-with-vectors), el autor propone un marco de movimiento llamado motion 101. Así mismo, el [ejemplo 1.7](https://natureofcode.com/vectors/#example-17-motion-101-velocity) muetra cómo se aplica este marco en un ejemplo simple.
###
- Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.
###
Usar el velocity para así cambiar la posición del objeto de forma continua y por así decirlo, *smooth*, como si se usara un ruido de Perlin; usando `velocity` y `position` respectivamente para estos datos. De igual forma, su interpretación geométrica sería una función (gráfica), en donde se presenta una línea horizontal recta sin ningún tipo de ondulación, siendo que, la aceleración sería igual a 0 y la velocidad cte, creando un movimiento homogeéneo sea cual sea la dirección.
###
- ¿Cómo se aplica motion 101 en el ejemplo?
###
Este se usa para mover el objeto, en este caso llamado `mover`, para que el circulo se mueva de tal forma que elija aleatoriamente: una posición, la dirección, y la velocidad con la que se trasladará; y al llegar al límite del canvas (en cualquiera de estos), ser transportada a un punto relativamente cercano en la dirección prestablecida, para que continue con la moción anterior. Esto hasta que llegue al límite izquierdo o derecho, y se devuelva al límite opuesto para repetir el proceso. 
###
Cabe agregar, que cada que se ejecuta el programa las variables mencionadas serán diferentes (posición,dirección y velocidad), pero si solo se le deja correr, estás mantendrán los mismos valores siempre, a excepción de la posición que se va actualizando según la velocidad o si ha tocado o no el límite del canvas.

## Actividad 07
### Experimentando con la aceleración
En el libro proponen una regla (que eventualmente se rompe cuando conviene):
###
*"The goal for programming motion is to come up with an algorithm for calculating acceleration and then let the trickle-down effect work its magic."*
###
Para investigador el significado de esta frase te propone que construyas un experimento donde analices cómo se comporta un objeto en movimiento con:
###
- Aceleración constante.
- Aceleración aleatoria.
- Aceleración hacia el mouse.
###
¿Qué observaste cuando usas cada una de las aceleraciones propuestas?
###
**Aceleración constante:** Al ser `a = cte`, la velocidad va en *aumento* cada segundo. Es decir, mientras más corra el programa, más rápido el `mover` será.
###
**Aceleración aleatoria:** Tanto la velocidad y la aceleración no siguen ningún patrón, pues como la aceleración es aleatoria, la velocidad también lo es, afectando directamente a la posición. Esto crea un movimiento más errático en el `mover`.
###
**Aceleración hacia el mouse:** Aquí la aceleración se definirá por la posición del mouse, es decir, dependiendo de la distancia y dirección a la está el mouse y el `mover`, la aceleración variará. Es por esto que se ve como "desacelera", ya que al tener que cambiar de dirección abruptamente genera una especie de conbinación negativa de los vectores, dejando la velocidad en valores cercanos a cero, así como disminuyendo la aceleración por unos instantes. Esto genera un movimiento más orgánico y predecible en el `mover`.
