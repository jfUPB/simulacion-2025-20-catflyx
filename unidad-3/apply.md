# Unidad 3
`🛠 Fase: Apply`
_________________________________________________________________________________________________________________________________________________________________________________________
## Actividad 10
### El problema de los n-cuerpos
Vas a diseñar e implementar una obra generativa interactiva en tiempo real inspirada en [el problema de los n-cuerpos](https://natureofcode.com/forces/#the-n-body-problem). En el texto guía tienes una sección que te puede ayudar a entender cómo modelar este problema, pero la idea es que uses tu creatividad para crear algo diferente basado en ese concepto y en las *esculturas cinéticas de Alexander Calder*.
# Concepto
1. Diseña e implementa tu obra generativa interactiva en tiempo real.
- Debe ser interactiva.
- Debes usar al menos dos algoritmos diferentes de la unidad 1, además de random.
###
Una obra donde existan alrededor de 4-5 `movers` que orbitarán un objeto con masa superior ("sol"), el cual podrá ser llamado a la posición del mouse. También, estos movers tendrán variables que pueden ser modificadas por el usuario. De igual forma, habrán otros orbes que usarán como referencia el ejemplo Two-Body Attraction, que rondarán siempre cerca del centro del lienzo. Por último, el lienzo se pintará constantemente con opacidad.
###
2. Explica cómo modelaste el problema de los n-cuerpos en tu obra.
###
Hice que siempre se cree un cuerpo con una masa superior. Es decir, puse un rango de masa a los demás cuerpos que nunca superará el de este. A este objeto me le refiero como "sol" en algunas partes del código. De igual forma, me aseguré que no haya forma que este escape del lienzo.
###
3. Copia el enlace a tu simulación en p5.js.
###
https://editor.p5js.org/catflyx/sketches/-dtaUlNS4
###
4. Copia el código.
###
`Versión 1`
###
El fondo y las esferas cambian a una paleta preestablecida para parecerse a la de Alexander Calder. De igual forma, para evitar que los cuerpos se alejen del canvas, se crea un objeto con masa que siempre será superior.
``` js
let movers1 = []; //orbiters

let movers2 = []; let movers3 = []; //arcos

let G = 1;

let c1, //rojo
    c2, //amarillo
    c3, //azul
    c4, //naranja
    c5; //"negro"
let randC1, randC2; let SelC1, SelC2; 

let co01, co02; let co11, co12; let co21, co22; let co31, co32; let co41, co42;

let inter;  let back;

let bgC;

function setup() {
  createCanvas(840, 340); background(212);
  
  movers1[0] = new Body(width / 2, height / 2, 8); // el "sol"
  
  for (let i = 1; i < 5; i++) {
    movers1[i] = new Body(random(width), random(height), random(0.3, 2));
  }
  
  //Paleta de colores
    c1 = color(181, 73, 53); //rojo
    c2 = color(219, 181, 53); //amarillo
    c3 = color(71, 99, 158); //azul
    c4 = color(196, 129, 65); //naranja
    c5 = color(48, 40, 31); //"negro"
  
  inter = 0; back = false;
  
  //Seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
}

function draw() { 
  bgC = lerpColor(color(138, 105, 94), color(141, 35, 30), inter);
  bgC.setAlpha(20); // ajusta la transparencia
  
  background(bgC);
                 
  for (let i = 0; i < movers1.length; i++) {
    for (let j = 0; j < movers1.length; j++) {
      if (i !== j) {
        let force = movers1[j].attract(movers1[i]);
        movers1[i].applyForce(force);
      }
    }

    movers1[i].update();
    if (i == 0){
        movers1[i].show(co01, co02);
        }
    if (i == 1){
        movers1[i].show(co11, co12);
        }
    if (i == 2){
        movers1[i].show(co21, co22);
        }
    if (i == 3){
        movers1[i].show(co31, co32);
        }
    if (i == 4){
        movers1[i].show(co41, co42);
        }
  }
                 
                 
  if (inter >= 1){ //venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.003;
      } else {
        inter += 0.003;
      }                 
}

function selectColors(){
  randC1 = floor(random(1, 5)); randC2 = floor(random(1, 5));
  
  // Color 1
  if (randC1 === 1){
      SelC1 = c1;
      }
  if (randC1 === 2){
      SelC1 = c2;
      }
  if (randC1 === 3){
      SelC1 = c3;
      }
  if (randC1 === 4){
      SelC1 = c4;
      }
  if (randC1 === 5){
      SelC1 = c5;
      }
  
  // Color 2
  if (randC2 === 1){
      SelC2 = c1;
      }
  if (randC2 === 2){
      SelC2 = c2;
      }
  if (randC2 === 3){
      SelC2 = c3;
      }
  if (randC2 === 4){
      SelC2 = c4;
      }
  if (randC2 === 5){
      SelC2 = c5;
      }
}

// ------------------------ CLASE BODY ------------------------------

class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show(color1, color2) {
    noStroke(0);
    fill(lerpColor(color1, color2, inter));
    circle(this.position.x, this.position.y, this.mass * 16);
  }

  attract(other) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, other.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * other.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }
}
```
`Versión 2`
###
Correcciones menores y agregados, y ahora al hacer click en el canvas, el "sol" se mueve a esa posición.
``` js
let movers1 = []; //orbiters

let movers2 = []; let movers3 = []; //arcos

let G = 1;

let c1, //rojo
    c2, //amarillo
    c3, //azul
    c4, //naranja
    c5; //"negro"
let randC1, randC2; let SelC1, SelC2; let opacity; let c;

let co01, co02; let co11, co12; let co21, co22; let co31, co32; let co41, co42;

let inter;  let back;

let bgC;

let targetX, targetY; let solX, solY; let pressed;

function setup() {
  createCanvas(840, 340); background(212);
  
  movers1[0] = new Body(width / 2, height / 2, 8); // el "sol"
  
  for (let i = 1; i < 5; i++) {
    movers1[i] = new Body(random(width), random(height), random(0.3, 2));
  }
  
  //Paleta de colores
    c1 = color(181, 73, 53); //rojo
    c2 = color(219, 181, 53); //amarillo
    c3 = color(71, 99, 158); //azul
    c4 = color(196, 129, 65); //naranja
    c5 = color(48, 40, 31); //"negro"
  
  inter = 0; back = false; pressed = false;
  
  //Seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
  
  solX = movers1[0].position.x;
  solY = movers1[0].position.y;
}

function draw() { 
  bgC = lerpColor(color(138, 105, 94), color(141, 35, 30), inter);
  bgC.setAlpha(20); // ajusta la transparencia
  
  background(bgC);
                 
  for (let i = 0; i < movers1.length; i++) {
    for (let j = 0; j < movers1.length; j++) {
      if (i !== j) {
        let force = movers1[j].attract(movers1[i]);
        movers1[i].applyForce(force);
      }
    }

    movers1[i].update();
    if (i == 0){ opacity = 5;
        movers1[i].show(co01, co02, opacity);
        }
    if (i == 1){ opacity = 100;
        movers1[i].show(co11, co12, opacity);
        }
    if (i == 2){ opacity = 100;
        movers1[i].show(co21, co22, opacity);
        }
    if (i == 3){ opacity = 100;
        movers1[i].show(co31, co32, opacity);
        }
    if (i == 4){ opacity = 100;
        movers1[i].show(co41, co42, opacity);
        }
  }
              
  if (inter >= 1){ //venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.003;
      } else {
        inter += 0.003;
      }    
  
  if (pressed){
      solX = lerp(solX, targetX, 0.05);
      solY = lerp(solY, targetY, 0.05);
  
      movers1[0].move(solX, solY);
      }
  
}

function selectColors(){
  randC1 = floor(random(1, 5)); randC2 = floor(random(1, 5));
  
  // Color 1
  if (randC1 === 1){
      SelC1 = c1;
      }
  if (randC1 === 2){
      SelC1 = c2;
      }
  if (randC1 === 3){
      SelC1 = c3;
      }
  if (randC1 === 4){
      SelC1 = c4;
      }
  if (randC1 === 5){
      SelC1 = c5;
      }
  
  // Color 2
  if (randC2 === 1){
      SelC2 = c1;
      }
  if (randC2 === 2){
      SelC2 = c2;
      }
  if (randC2 === 3){
      SelC2 = c3;
      }
  if (randC2 === 4){
      SelC2 = c4;
      }
  if (randC2 === 5){
      SelC2 = c5;
      }
}

function mousePressed() {
  // Al hacer clic, se actualiza la posición objetivo
  targetX = mouseX;
  targetY = mouseY;
  
  pressed = true;
}

// ------------------------ CLASE BODY ------------------------------

class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }
  
  move(x, y) {
    this.position.set(x, y);
  }

  show(color1, color2, opacidad) {
    noStroke(0);
    c = lerpColor(color1, color2, inter);
    c.setAlpha(opacidad);
    fill(c);
    circle(this.position.x, this.position.y, this.mass * 16);
  }

  attract(other) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, other.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * other.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }
}
```
`Versión 3`
###
Dandoles formas más orgánicas a los círculos, y haciendo que con `c` se pueda volver a seleccionar los colores interpolados en cada orbe y el sol, y lo mismo pero con la forma usando `s`.
``` js
let movers1 = []; //orbiters

let movers2 = []; let movers3 = []; //arcos

let G = 1;

let c1, //rojo
    c2, //amarillo
    c3, //azul
    c4, //naranja
    c5; //"negro"
let randC1, randC2; let SelC1, SelC2; let opacity; let c;

let co01, co02; let co11, co12; let co21, co22; let co31, co32; let co41, co42;

let inter;  let back;

let bgC;

let targetX, targetY; let solX, solY; let pressed;

function setup() {
  createCanvas(840, 340); background(212);
  
  movers1[0] = new Body(width / 2, height / 2, 8); // el "sol"
  
  for (let i = 1; i < 5; i++) {
    movers1[i] = new Body(random(width), random(height), random(0.3, 2));
  }
  
  //Paleta de colores
    c1 = color(181, 73, 53); //rojo
    c2 = color(219, 181, 53); //amarillo
    c3 = color(71, 99, 158); //azul
    c4 = color(196, 129, 65); //naranja
    c5 = color(48, 40, 31); //"negro"
  
  inter = 0; back = false; pressed = false;
  
  //Seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
  
  solX = movers1[0].position.x;
  solY = movers1[0].position.y;
}

function draw() { 
  bgC = lerpColor(color(138, 105, 94), color(141, 35, 30), inter);
  bgC.setAlpha(20); // ajusta la transparencia
  
  background(bgC);
                 
  for (let i = 0; i < movers1.length; i++) {
    for (let j = 0; j < movers1.length; j++) {
      if (i !== j) {
        let force = movers1[j].attract(movers1[i]);
        movers1[i].applyForce(force);
      }
    }

    movers1[i].update();
    if (i == 0){ opacity = 5;
        movers1[i].show(co01, co02, opacity);
        }
    if (i == 1){ opacity = 100;
        movers1[i].show(co11, co12, opacity);
        }
    if (i == 2){ opacity = 100;
        movers1[i].show(co21, co22, opacity);
        }
    if (i == 3){ opacity = 100;
        movers1[i].show(co31, co32, opacity);
        }
    if (i == 4){ opacity = 100;
        movers1[i].show(co41, co42, opacity);
        }
  }
              
  if (inter >= 1){ //venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.003;
      } else {
        inter += 0.003;
      }    
  
  if (pressed){
      solX = lerp(solX, targetX, 0.05);
      solY = lerp(solY, targetY, 0.05);
  
      movers1[0].move(solX, solY);
      }
  
}

function selectColors(){
  randC1 = floor(random(1, 5)); randC2 = floor(random(1, 5));
  
  // Color 1
  if (randC1 === 1){
      SelC1 = c1;
      }
  if (randC1 === 2){
      SelC1 = c2;
      }
  if (randC1 === 3){
      SelC1 = c3;
      }
  if (randC1 === 4){
      SelC1 = c4;
      }
  if (randC1 === 5){
      SelC1 = c5;
      }
  
  // Color 2
  if (randC2 === 1){
      SelC2 = c1;
      }
  if (randC2 === 2){
      SelC2 = c2;
      }
  if (randC2 === 3){
      SelC2 = c3;
      }
  if (randC2 === 4){
      SelC2 = c4;
      }
  if (randC2 === 5){
      SelC2 = c5;
      }
}

function mousePressed() {
  // Al hacer clic, se actualiza la posición objetivo
  targetX = mouseX;
  targetY = mouseY;
  
  pressed = true;
}

function keyPressed(){
  if (key === 'c' || key === 'C') { //Re-seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
  }
  if (key === 's' || key === 'S'){
    for (let i = 0; i < movers1.length; i++) {
    movers1[i].generateShape(); // Regenera la forma del orbe
  }
  }
}

// ------------------------ CLASE BODY ------------------------------

class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    
    this.shapePoints = []; // Aquí se guardará la forma
    this.generateShape();  // Se genera solo una vez
  }
  
  generateShape() { //La forma generada
    
    this.shapePoints = [];
    
    let r = this.mass * 8;
    let sides = 6;

    // Guardamos puntos extra para que curveVertex funcione bien
    for (let i = -1; i <= sides + 1; i++) {
      let angle = TWO_PI / sides * i;
      let offset = random(-r * 0.3, r * 0.3);
      let rad = r + offset;
      let x = cos(angle) * rad;
      let y = sin(angle) * rad;
      this.shapePoints.push(createVector(x, y));
    }
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }
  
  move(x, y) {
    this.position.set(x, y);
  }

  show(color1, color2, opacidad) {
    noStroke(0);
    c = lerpColor(color1, color2, inter);
    c.setAlpha(opacidad);
    fill(c);
    //circle(this.position.x, this.position.y, this.mass * 16);
    
    beginShape();
    for (let v of this.shapePoints) {
      // Desplazar la figura con respecto a la posición del objeto
      curveVertex(this.position.x + v.x, this.position.y + v.y);
    }
    endShape(CLOSE);
  }

  attract(other) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, other.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * other.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }
}
```
`Versión 4`
###
Correciones de funcionamiento y estéticas, ahora se crean unos `arcs` con un salto de lévy, los cuales se les puede cambiar el color con `x`,y definen su posición con un `randomGaussian()`.
``` js
let movers1 = []; //orbiters

let arcs = []; //arcos

let G = 1;

let c1, //rojo
    c2, //amarillo
    c3, //azul
    c4, //naranja
    c5; //"negro"
let randC1, randC2; let SelC1, SelC2; let opacity; let c;

let co01, co02; let co11, co12; let co21, co22; let co31, co32; let co41, co42;

let inter;  let back;

let bgC;

let targetX, targetY; let solX, solY; let pressed; let contp;

let num; let crearArcos; let arcc;

function setup() {
  createCanvas(840, 340); background(212);
  
  movers1[0] = new Body(width / 2, height / 2, 8); // el "sol"
  
  for (let i = 1; i < 5; i++) {
    movers1[i] = new Body(random(width), random(height), random(0.3, 2));
  }
  
  //Paleta de colores
    c1 = color(181, 73, 53); //rojo
    c2 = color(219, 181, 53); //amarillo
    c3 = color(71, 99, 158); //azul
    c4 = color(196, 129, 65); //naranja
    c5 = color(48, 40, 31); //"negro"
  
  inter = 0; back = false; pressed = false;
  
  //Seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
  
  solX = movers1[0].position.x;
  solY = movers1[0].position.y;
  
   contp = 0;
  
  num = 0; crearArcos = true;
}

function draw() { 
  bgC = lerpColor(color(138, 105, 94), color(141, 35, 30), inter);
  bgC.setAlpha(3); // ajusta la transparencia
  
  background(bgC);
                 
  for (let i = 0; i < movers1.length; i++) {
    for (let j = 0; j < movers1.length; j++) {
      if (i !== j) {
        let force = movers1[j].attract(movers1[i]);
        movers1[i].applyForce(force);
      }
    }

    movers1[i].update(); movers1[0].checkEdges(); //Para evitar que salga del borde

    if (i == 0){ opacity = 5;
        movers1[i].show(co01, co02, opacity);
        }
    if (i == 1){ opacity = 100;
        movers1[i].show(co11, co12, opacity);
        }
    if (i == 2){ opacity = 100;
        movers1[i].show(co21, co22, opacity);
        }
    if (i == 3){ opacity = 100;
        movers1[i].show(co31, co32, opacity);
        }
    if (i == 4){ opacity = 100;
        movers1[i].show(co41, co42, opacity);
        }
  }
              
  if (inter >= 1){ //venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.003;
      } else {
        inter += 0.003;
      }    
  
  if (pressed){ contp += 0.01;
      solX = lerp(solX, targetX, 0.05);
      solY = lerp(solY, targetY, 0.05);
  
      movers1[0].move(solX, solY);
    
      if (contp >= 1){
          pressed = false; contp = 0;
          }
      }
  
// Creación de pares de arcos ...................
  
if (crearArcos){
    
    if (random(1) < 0.5) {
  // Crear arco 1
  arcs.push(new Arcs(randomGaussian(width / 2, 60), randomGaussian(height / 2, 60)));
  console.log("Se creó un arco 1");

  // Crear arco 2
  arcs.push(new Arcs(randomGaussian(width / 2, 60), randomGaussian(height / 2, 60)));
  console.log("Se creó un arco 2");

  // Asignar velocidad a los últimos 2 creados
  let last = arcs.length;
  arcs[last - 2].velocity = createVector(1, 0);
  arcs[last - 1].velocity = createVector(-1, 0); // si quieres que vayan en direcciones opuestas
}    
  selectColors(); arcc = SelC1; //Elegir primeramente el color
  crearArcos = false;
    }
 
  // Actualizar y mostrar en pares
for (let i = 0; i < arcs.length - 1; i += 2) {
  arcs[i].attract(arcs[i + 1]);
  arcs[i + 1].attract(arcs[i]);

  arcs[i].update();
  arcs[i + 1].update();

  arcs[i].show();
  arcs[i + 1].show();
}
  
}

function selectColors(){
  randC1 = floor(random(1, 5)); randC2 = floor(random(1, 5));
  
  // Color 1
  if (randC1 === 1){
      SelC1 = c1;
      }
  if (randC1 === 2){
      SelC1 = c2;
      }
  if (randC1 === 3){
      SelC1 = c3;
      }
  if (randC1 === 4){
      SelC1 = c4;
      }
  if (randC1 === 5){
      SelC1 = c5;
      }
  
  // Color 2
  if (randC2 === 1){
      SelC2 = c1;
      }
  if (randC2 === 2){
      SelC2 = c2;
      }
  if (randC2 === 3){
      SelC2 = c3;
      }
  if (randC2 === 4){
      SelC2 = c4;
      }
  if (randC2 === 5){
      SelC2 = c5;
      }
}

function mousePressed() {
  // Al hacer clic, se actualiza la posición objetivo
  targetX = mouseX;
  targetY = mouseY;
  
  pressed = true;
}

function keyPressed(){
  if (key === 'c' || key === 'C') { //Re-seleccionando colores para orbes
  selectColors(); co01 = SelC1; co02 = SelC2; // 0
  selectColors(); co11 = SelC1; co12 = SelC2; // 1
  selectColors(); co21 = SelC1; co22 = SelC2; // 2
  selectColors(); co31 = SelC1; co32 = SelC2; // 3
  selectColors(); co41 = SelC1; co42 = SelC2; // 4
  }
  if (key === 's' || key === 'S'){
    for (let i = 0; i < movers1.length; i++) {
    movers1[i].generateShape(); // Regenera la forma del orbe
  }
  }
  
  if (key === 'x' || key === 'X'){ //Cambiar color de arcs
      selectColors(); arcc = SelC1;
      }
}

// ------------------------ CLASE BODY ------------------------------

class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    
    this.shapePoints = []; // Aquí se guardará la forma
    this.generateShape();  // Se genera solo una vez
  }
  
  generateShape() { //La forma generada
    
    this.shapePoints = [];
    
    let r = this.mass * 8;
    let sides = 6;

    // Guardamos puntos extra para que curveVertex funcione bien
    for (let i = -1; i <= sides + 1; i++) {
      let angle = TWO_PI / sides * i;
      let offset = random(-r * 0.3, r * 0.3);
      let rad = r + offset;
      let x = cos(angle) * rad;
      let y = sin(angle) * rad;
      this.shapePoints.push(createVector(x, y));
    }
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }
  
  move(x, y) {
    this.position.set(x, y);
  }

  show(color1, color2, opacidad) {
    noStroke(0);
    c = lerpColor(color1, color2, inter);
    c.setAlpha(opacidad);
    fill(c);
    //circle(this.position.x, this.position.y, this.mass * 16);
    
    beginShape();
    for (let v of this.shapePoints) {
      // Desplazar la figura con respecto a la posición del objeto
      curveVertex(this.position.x + v.x, this.position.y + v.y);
    }
    endShape(CLOSE);
  }

  attract(other) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, other.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * other.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }
  
  checkEdges() {
  if (this.position.x < 0) {
    this.position.x = 0;
    this.velocity.x *= -1;
  } else if (this.position.x > width) {
    this.position.x = width;
    this.velocity.x *= -1;
  }

  if (this.position.y < 0) {
    this.position.y = 0;
    this.velocity.y *= -1;
  } else if (this.position.y > height) {
    this.position.y = height;
    this.velocity.y *= -1;
  }
}
  
}

// ----------------------- CLASE ARCS ------------------------------

class Arcs {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = 8;
    this.r = sqrt(this.mass) * 2;
  }
  
  attract(body) {
    let force = p5.Vector.sub(this.position, body.position);
    let d = constrain(force.mag(), 5, 25);
    let G = 1;
    let strength = (G * (this.mass * body.mass)) / (d * d);
    force.setMag(strength);
    body.applyForce(force);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }



  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.set(0, 0);
  }

  show() {
    noStroke(0);
    fill(arcc);
    circle(this.position.x, this.position.y, this.r * 4);
  }
}
```
5. Captura una imagen representativa de tu ejemplo.
<img width="943" height="383" alt="image" src="https://github.com/user-attachments/assets/c59fdf76-0286-47a1-afa7-26bee4f6585a" />







