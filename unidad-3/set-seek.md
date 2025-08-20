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
Repasemos rápido de qué se trata:
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
Usamos la función `update()` para actualizar los vectores que almacenan el `position` y `velocity`, usando las funciones `.add()` para sumarlos, y otras para tareas que ayuden a controlar el `mover` en el canvas.

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
mover.applyForce(wind); //Viento
```
y luego:
``` js
mover.applyForce(gravity); //Gravedad
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
No estamos sumando las fuerzas de `gravity` y `wind` antes de igualar la aceleración a estas. O al menos, no se ve explícitamente en este planteamiento.
###
2. ¿Qué solución propones?
###
antes de `this.acceleration = force;`, sumar las fuerzas de `gravity` y `wind`, para que `force` = `gravity` + `wind`.
###
3. ¿Cómo lo implementarías en p5.js?
###
Si son variables, sería sumarlas antes en la misma función `applyForce(force)`. Aunque se necesitaría que esta acepte más de dos variables.

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
Ahora en vez de igualar la aceleración a `force`, se le suma con la función `.add()`.
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
La función `.mult(0)`
###
- ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
###
¿Para que sea igual a 0?
###
- ¿Por qué se multiplica por cero *justo al final* de `update()`?
###
Para que la sumatoria de fuerzas sea igual a cero también, o hacer la velocidad constante.
###
*Con ayuda del profe:* Esto se hace para "refrescar" la información de la aceleración, pues, una vez se haya calculado la velocidad con esta y en consecuencia la posición, hay que calcular una nueva aceleración para el siguiente frame. Es por esto que la volvemos cero multiplicandola por cero.

## Actividad 07
### En mi mundo los pixeles si tienen masa
Como en tu mundo los pixeles tienen masa, entonces, ¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?
###
Con la fórmula de sumatoria de fuerzas igua a masa * aceleración, pero moviendo las variables. Esto quedaría de tal forma que: aceleración es igual a sumatoria de fuerzas sobre masa.
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
¿No se sumaron las fuerzas antes de dividirlas?
###
**(Recuerda, ¿Cuándo se pasa algo a un función por valor y cuándo por referencia? En este caso, force es objeto de la clase p5.Vector, es decir, es un objeto que se pasa por referencia. ¿Qué implica esto?)**
###
Tocaba hacer una "copia" de force para dividirlo.
``` js
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }
}
```
O por ejemplo en este caso del libro, crear un nuevo vector e igualargo a una variable que almacene el resultado, y que este último sea el que se use en la aceleración. O hacer un vector que alaceme estos valores, para luego refrescarlo al final de la función.
###
Pero en conclusión, es hacerlo por **valor**.

## Actividad 08
### Paso por valor y paso por referencia
En el siguiente código:
``` js
let friction = this.velocity.copy();
let friction = this.velocity;
```
1. ¿Cuál es la diferencia entre las dos líneas?
###
En la primera se iguala la fricción a una copia de `velocity` (por valor), mientras que en la segunda se le pasa el vector original (por referencia).
###
2. ¿Qué podría salir mal con `let friction = this.velocity;`?
###
Se puede estar modificando el vector de `velocity` inconscientemente.
###
3. De nuevo, toca repasar. ¿Cuál es la diferencia entre copiar por **VALOR** y por **REFERENCIA**?
###
Por **referencia** se pasa la dirección del vector directamente, es decir, se utiliza el vector y puede modificarse en consecuencia. Mientras que en **valor**, se usa una copia del vector, evitando cualquier cambio indeseado en el vector original.
###
4. En el fragmento de código ¿Cuándo es por **VALOR** y cuándo por **REFERENCIA**.
###
`let friction = this.velocity.copy();` es por valor, `let friction = this.velocity;` es por referencia.

## Actividad 09
### Modelando fuerzas
En tu mundo de pixeles tu puedes crear las fuerzas o las puedes modelar.
###
Vamos a probar con lo segundo, modelar una fuerza. Ve a la sección [Modeling forces](https://natureofcode.com/forces/#modeling-a-force) del texto guía.
###
Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:
###
- Fricción.
- Resistencia del aire y de fluidos.
- Atracción gravitacional.
-------------------------------------------------------
1. Explica cómo modelaste cada fuerza.
###
- *Fricción:*  Hice que al dar click en el canvas, se aplique fricción.
- *Resistencia del aire y de fluidos:*  Hice que diferentes movers tuvieran que reaccionar a varios líquidos.
- *Atracción gravitacional:* Hice que el atractor se moviera constantemente.
###
2. Conceptualmente cómo se relaciona la fuerza con la obra generativa.
###
- *Fricción:* Hace que cambie de sentido, y disminuye su velocidad al tener el "freno" de la fricción. Esto permite diferente cercaría en los aros que se se dibujan continuamente.
- *Resistencia del aire y de fluidos:*  Hace que el trazo tarde más o menos en realizarse si está dentro o no del líquido actual, el cual puede tener cualquier altura. Esto más tonos azulados, crea un efecto interesante al ejecutarse.
- *Atracción gravitacional:* Usa el movimiento "errático" del mover que orbita el atractor para crear trazos únicos e impredecibles de cierta forma.
###
3. Copia el enlace a tu ejemplo en p5.js.
###
- [Fricción](https://editor.p5js.org/catflyx/sketches/tpp5_-BYw)
- [Resistencia del aire y de fluidos](https://editor.p5js.org/catflyx/sketches/dELj-5yTb)
- [Atracción gravitacional](https://editor.p5js.org/catflyx/sketches/J8-fCUekI)
###
4. Copia el código.
### Fricción
``` js
let mover;

// contador para vectores
let cv;

//Para la fricción
let c; let friction;

let r, g, b;

function setup() {
  createCanvas(240, 640); background(255);
  // Create the Mover object.
  mover = new Mover();
  
  r = 0; g = 0; b = 0;
  
  cv = createVector(-0.001, 0.01);
}

function draw() { //background(255);
  cv.x *= 1.01; cv.y *= 1.01;
  this.acceleration = cv;
  
  if (acceleration.x < 0.1){
    acceleration.x++;
      }
  if (acceleration.y < 0.1){
    acceleration.y++;
      }  
  if (acceleration.z < 0.1){
    acceleration.z++;
      }  
  
  if (key === 'e' || key === 'E'){ //Colorear
    r = random(0, 250); g = random(0, 250); b = random(0, 250);
      }
  if (key === 'd' || key === 'D'){ //Negro otra vez
    r = 0; g = 0; b = 0;
      }
  
  // Call methods on the Mover object.
  mover.update();
  mover.checkEdges();
  mover.show();
}

function mousePressed() {
  c = 0.1;
  friction = mover.velocity.copy();
  friction.mult(-1);
  friction.setMag(c);
  
  mover.applyForce(friction);
}



// CLASE MOVER

class Mover {
  constructor() {
    // The object has two vectors: position and velocity.
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(random(-2, 2), random(-2, 2));
    
    this.acceleration = createVector(-0.001, 0.01);
    
    this.topSpeed = 10;
  }

  update() {
    this.velocity.add(this.acceleration); this.velocity.limit(this.topSpeed);
    
    this.position.add(this.velocity);
    
    this.velocity.add(this.friction);
  }

  show() {
    stroke(r, g, b);
    //strokeWeight(2);
    //fill(127);
    circle(this.position.x, this.position.y, 38);
  }
  
    applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f); //console.log("aceleración: ", acceleration);
  }

  checkEdges() {
    if (this.position.x >= (width)) {
      this.position.x = random(0, 240);
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y >= (height)) {
      this.position.y = random(0, 640);
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```
### Resistencia del aire y de fluidos
``` js
// cuerpos
let movers = [], cantidad = 0;

// Liquids
let liquid = [];

// Color pa el liquid
let r1, g1, b1; let r2, g2, b2;

function setup() {
  createCanvas(640, 720); background(255);
  reset();
  
  r1 = random(30, 150); g1 = random(18, 200); b1 = random(145, 255);
  
  r2 = random(30, 150); g2 = random(77, 200); b2 = random(145, 255);
  
  // Create liquid object
  liquid = new Liquid(0, height / 2, width, height / 2, 0.1);
}

function draw() { //background(255);

  for (let i = 0; i < movers.length; i++) {
    // Is the Mover in the liquid?
    if (liquid.contains(movers[i])) {
      // Calculate drag force
      let dragForce = liquid.calculateDrag(movers[i]);
      // Apply drag force to Mover
      movers[i].applyForce(dragForce);
    }

    // Gravity is scaled by mass here!
    let gravity = createVector(0, 0.1 * movers[i].mass);
    // Apply gravity
    movers[i].applyForce(gravity);

    // Update and display
    movers[i].update();
    movers[i].show();
    movers[i].checkEdges();
    
    if (cantidad > 9){
        cantidad = 9;
        }
  }
}

function mousePressed() {
  reset();
}

// Restart all the Mover objects randomly
function reset() {
  for (let i = 0; i < cantidad; i++) {
    movers[i] = new Mover(40 + i * 70, 0, random(0.5, 3));
  }
}

function keyPressed() { 
  liquid.show();
  r1 = random(30, 150); g1 = random(18, 200); b1 = random(145, 255);
  liquid = new Liquid(0, height - random(100, 720), width, 200, 0.1);
  
  if (key === 'g' || key === 'G') {
    cantidad++;
    console.log("Cantidad de movers: ", cantidad);
  }

                      
  if (key === 'c' || key === 'C') { //Color
    r2 = random(30, 150); g2 = random(77, 200); b2 = random(145, 255);
  }
}

// ---------------- CLASE MOVER ---------------------------

class Mover {
  constructor(x, y, mass,) {
    this.mass = mass;
    this.radius = mass * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }
  // Newton's 2nd law: F = M * A
  // or A = F / M
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // Velocity changes according to acceleration
    this.velocity.add(this.acceleration);
    // position changes by velocity
    this.position.add(this.velocity);
    // We must clear acceleration each frame
    this.acceleration.mult(0);
  }

  show() {
    fill(r2, g2, b2);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  // Bounce off bottom of window
  checkEdges() {
    if (this.position.y > height - this.radius) {
      this.velocity.y *= -0.9; // A little dampening when hitting the bottom
      this.position.y = height - this.radius;
    }
  }
}



// ---------------- CLASE LIQUID ---------------------------

class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  // Is the Mover in the Liquid?
  contains(mover) {
    let pos = mover.position;
    return (
      pos.x > this.x &&
      pos.x < this.x + this.w &&
      pos.y > this.y &&
      pos.y < this.y + this.h
    );
  }

  // Calculate drag force
  calculateDrag(mover) {
    // Magnitude is coefficient * speed squared
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    // Direction is inverse of velocity
    let dragForce = mover.velocity.copy();
    dragForce.mult(-1);

    // Scale according to magnitude
    dragForce.setMag(dragMagnitude);
    return dragForce;
  }

  show() {
    noStroke();
    fill(r1, g1, b1, 30);
    rect(this.x, this.y, this.w, this.h);
  }
}
```
### Atracción gravitacional
``` js
// A Mover and an Attractor
let mover;
let attractor;

// Gravitational constant (for global scaling)
let G = 1;

let r, val = 1;

function setup() {
  createCanvas(640, 240); background(5);
  mover = new Mover(300, 50, 0.5);
  attractor = new Attractor();
}

function draw() {
  
  let force = attractor.attract(mover);
  mover.applyForce(force);
  mover.update();

  attractor.update();
  attractor.show();
  mover.show();
}

function keyPressed(){
  if (key === 'f' || key === 'F'){
    square(random(0, 640), random(0, 240), random(2, 10)); // square(x, y, s);
  }
  
  if (key === 'x' || key === 'X'){
    val += 0.1;
  }
  if (key === 'z' || key === 'Z'){
    val -= 0.1;
  }
  if (val < 0.02){
      val = 1;
      }
  
  console.log("val: ", val);
}

// ------------------ CLASE MOVER -------------------------------------

class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(1, 0);
    this.acceleration = createVector(0, 0);
  }
  // Newton's 2nd law: F = M * A
  // or A = F / M
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // Velocity changes according to acceleration
    this.velocity.add(this.acceleration);
    // position changes by velocity
    this.position.add(this.velocity);
    // We must clear acceleration each frame
    this.acceleration.mult(0);
  }

  show() {
    fill(255, 80);
    circle(this.position.x, this.position.y, this.radius * 2);
  }
}



// ------------------ CLASE ATRACTOR -------------------------------------

class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.dragOffset = createVector(0, 0);
    this.dragging = false;
    this.rollover = false;
  }

  attract(mover) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, mover.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }

  // Method to display
  show() {
    fill(50);
    circle(this.position.x, this.position.y, this.mass * 2);
  }
  
  update() {
    r = random(val);
    
    if (r < 0.01){
          this.position.x = random(0, 640);
          this.position.y = random(240, 0);
        }
    
    this.position.x += random(0, 2);
    this.position.y += random(0, 2);
  }
}
```
5. Captura una imagen representativa de tu ejemplo.
### Fricción
<img width="276" height="719" alt="image" src="https://github.com/user-attachments/assets/280dc62b-9630-4698-be30-53edb0095d30" />

### Resistencia del aire y de fluidos
<img width="731" height="720" alt="image" src="https://github.com/user-attachments/assets/05e05190-bcc8-432d-8c2b-d43f396e8539" />

### Atracción gravitacional
<img width="721" height="280" alt="image" src="https://github.com/user-attachments/assets/b0367d09-79f9-49af-b2c8-23b287bd66d3" />












