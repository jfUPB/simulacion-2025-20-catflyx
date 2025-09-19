# Evidencias de la unidad 5
_______________________________________________________________________________________________________________________________________________________________________________
## Actividad 2
### Revisa y repasa algunos conceptos
Dale una mirada al [capítulo sobre sistemas de partículas](https://natureofcode.com/particles/) del texto guía del curso. Explora libremente, pero te pediré que revises especialmente los siguientes conceptos:
- Revisa detalladamente el ejemplo 4.2: [an Array of Particles](https://natureofcode.com/particles/#example-42-an-array-of-particles).
- Analiza el ejemplo 4.4: [a System of Systems](https://natureofcode.com/particles/#example-44-a-system-of-systems).
- Analiza el ejemplo 4.5: [a Particle System with Inheritance and Polymorphism](https://natureofcode.com/particles/#example-45-a-particle-system-with-inheritance-and-polymorphism).
- Analiza el ejemplo 4.6: [a Particle System with Forces](https://natureofcode.com/particles/#example-46-a-particle-system-with-forces).
- Analiza el ejemplo 4.7: [a Particle System with a Repeller](https://natureofcode.com/particles/#example-47-a-particle-system-with-a-repeller).
####
🧐🧪✍️ En tu bitácora responde a esta pregunta para cada una de las simulaciones: **¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**
####
Cada partícula "nace" con un tiempo de vida predeterminado. En la función `update()`, se le resta un valor a este tiempo de vida. Otra función revisa si dicho tiempo llegó a cero, y una vez lo hace, se elimina la partícula. De esta forma el sistema mantiene a raya la cantidad de partículas activas.
####
Además te pediré que hagas los siguientes experimentos y los reportes en tu bitácora:
1. Vas a modificar *cada una de las simulaciones anteriores*, incluye en cada una al menos *un concepto de las unidades anteriores*, pero **no repitas concepto**, la idea es que repases al menos uno de cada unidad.
####
`an Array of Particles`
####
**Concepto:** Salto de lévy y randomGaussian() (U1)
``` js
let particles = [];

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(100);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 1);
    }
  }
}

// -------------------- CLASE PARTICLE --------------------

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}
```
`a System of Systems`
####
**Concepto:** lerpColor() y Fuerza de viento (U2 y U3)
``` js
let emitters = [];

function setup() {
  createCanvas(640, 240);
  let text = createP("click to add particle systems");
}

function draw() {
  background(150);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    // Looping through backwards to delete
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        // Remove the particle
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```
`a Particle System with Inheritance and Polymorphism`
####
**Concepto:** LerpColor() y movimiento de un péndulo (U2 y U4)
``` js
let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
}

function draw() {
  background(150);
  emitter.addParticle();
  emitter.run();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE CONFETTI --------------------
// Child class constructor
class Confetti extends Particle {
  // Override the show method
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    fill(127, this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```
`a Particle System with Forces`
####
**Concepto:** Resorte, gravedad y salto de Lévy (U4, U3 y U1)
``` js
let emitter;

function setup() {
  createCanvas(680, 480);
  emitter = new Emitter(width / 2, 50);
}

function draw() {
  background(150,30);

  // Apply gravity force to all Particles
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  emitter.addParticle();
  emitter.run();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0.0);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.lifespan = 255.0;
    this.mass = 1; // Let's do something better here!
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}


// -------------------- CLASE EMITTER --------------------
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Using a for of loop to apply the force to all particles
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  run() {
    //{!7} Can’t use the enhanced loop because checking for particles to delete.
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
`a Particle System with a Repeller`
####
**Concepto:** Motion 101 repelente con random() y Motion 101 líquido con random() (U1 y U3)
``` js
// One ParticleSystem
let emitter;

//{!1} One repeller
let repeller;

function setup() {
  createCanvas(640 , 240);
  emitter = new Emitter(width / 2, 60);
  repeller = new Repeller(width / 2, 250);
}

function draw() {
  background(150);
  emitter.addParticle();
  // We’re applying a universal gravity.
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  //{!1} Applying the repeller
  emitter.applyRepeller(repeller);
  emitter.run();

  repeller.show();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE EMITTER --------------------
//{!1} The Emitter manages all the particles.
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Applying a force as a p5.Vector
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    //{!4} Calculating a force for each Particle based on a Repeller
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE REPELLER --------------------
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    //{!1} How strong is the repeller?
    this.power = 150;
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, 32);
  }

  repel(particle) {
    //{!6 .code-wide} This is the same repel algorithm we used in Chapter 2: forces based on gravitational attraction.
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    let strength = (-1 * this.power) / (distance * distance);
    force.setMag(strength);
    return force;
  }
}
```
####
2. Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica *cómo* lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
####
Cada partícula "nace" con un tiempo de vida predeterminado. En la función `update()`, se le resta un valor a este tiempo de vida. Otra función revisa si dicho tiempo llegó a cero, y una vez lo hace, se elimina la partícula. De esta forma el sistema mantiene a raya la cantidad de partículas activas. Además de lo mencionado en la anterior pregunta, este se puede variar usando tanto `random()` como `randomGaussian()`, para que este tiempo de vida varíe, pero siga siendo controlado.
####
3. Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
####
- **simulación 1:** Cuando se crea una partícula, tiene dos nuevos factores. Tiene un salto de lévy que define su vector de velocidad usando `random()`, así como cada partícula tiene un tiempo de vida diferente de tal forma que, `randomGaussian(255.0, 100.0);` controla la longevidad de la partícula.
- **simulación 2:** Cuando las partículas se crean, estas tienen dos colores predeterminados entre los cuales interpolar usando `lerpColor()`. Además, ahora hay una fuerza de viento constante que obliga a las partículas a moverse. Esta fuerza se puede modificar con `z` y `x`.
- **simulación 3:** Hice que se creara un péndulo del cual se generarían las partículas, además de incentivar mejor lo del "confetti" con colores saturados tanto en el fondo como en la estrella que marca el final del péndulo y el origen de las partículas. Usando `lerpColor()` únicamente para el fondo. Y se puede mover el péndulo con el mouse.
- **simulación 4:** Usa un resorte unido por cuadrados para definir la gravedad con la que caen las partículas. De igual forma, tiene un salto de lévy con el cual puede que la gravedad aumente drásticamente.
- **simulación 5:** El repeller original ahora sigue al mouse en el eje x, además de que existen dos nuevos círculos. Uno funciona como otro repeller y el otro es una burbuja donde hay resistencia de líquido. Ambos se mueven usando `random()` por el lienzo.
####
4. Incluye un enlace a tu código en el editor de p5.js.
####
- [simulación 1](https://editor.p5js.org/catflyx/sketches/dqRgkfwgr)
- [simulación 2](https://editor.p5js.org/catflyx/sketches/3x9ZNX4k3)
- [simulación 3](https://editor.p5js.org/catflyx/sketches/0LyksPnSl)
- [simulación 4](https://editor.p5js.org/catflyx/sketches/F8vJfifzl)
- [simulación 5](https://editor.p5js.org/catflyx/sketches/3TCY6TjF_)
####
5. Incluye el código fuente de cada una de las simulaciones.
####
Códigos modificados...
####
`an Array of Particles`
``` js
let particles = []; let r;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(100);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 1);
    }
  }
}

// -------------------- CLASE PARTICLE --------------------

class Particle {
  constructor(x, y) {
    r = random(1);
    
    if (r < 0.01) {
      this.velocity = createVector(random(-5, 5), random(-2, 0));
      console.log("Salto");
      
    } else{
      this.velocity = createVector(random(-1, 1), random(-1, 0));
      
    }
    
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.lifespan = randomGaussian(255.0, 100.0);
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}
```
`a System of Systems`
``` js
let emitters = [];

let c1, c2, c3, bg;
let inter, back; 
let a, b;

// Fuerza de viento inicial
let windStrength = 0.05; 

function setup() {
  createCanvas(640, 240);
  let text = createP("Click para agregar sistemas de partículas. Usa 'z' y 'x' para modificar el viento.");
  
  inter = 0; 
  back = false;
  
  c1 = color(52, 153, 138); // cyan
  c2 = color(52, 106, 153); // azul
  c3 = color(127, 41, 85);  // negro
  
  bg = color(196, 70, 45); // fondo
}

function draw() {
  background(bg);
  
  if (inter >= 1){ 
    back = true;
  } else if (inter <= 0){
    back = false;
  }
  
  if (back){
    inter -= 0.02;
  } else {
    inter += 0.02;
  }
  
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }

  // Mostrar valor de viento en pantalla
  fill(255);
  noStroke();
  textSize(14);
  text("Viento: " + nf(windStrength, 1, 2), 10, height - 10);
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    let d = random(1);
    
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    
    if (d < 0.01) {
      b = c1; a = c2;
    } else {
      a = c1; b = c2;
    }
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);

    // viento constante
    let wind = createVector(windStrength, 0);
    this.applyForce(wind);

    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {    
    stroke(c3, this.lifespan);
    strokeWeight(4);
    fill(lerpColor(a, b, inter), this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CONTROL DE VIENTO --------------------
function keyPressed() {
  if (key === 'z' || key === 'Z') {
    windStrength = max(0, windStrength - 0.01); // nunca negativo
  } else if (key === 'x' || key === 'X') {
    windStrength += 0.01;
  }
}
```
`a Particle System with Inheritance and Polymorphism`
``` js
let pendulum;
let emitter;
let bgC, inter, back;

function setup() {
  createCanvas(640, 240);
  pendulum = new Pendulum(width / 2, 0, 170);
  emitter = new Emitter(0, 0); // posición se actualizará al bob
  
  inter = 0; back = true;
}

function draw() {
  bgC = lerpColor(color(245, 39, 70), color(39, 87, 245), inter);
  bgC.setAlpha(30); // ajusta la transparencia
  
  background(bgC);

  // Actualizar péndulo
  pendulum.update();
  pendulum.show();

  // Sincronizar emisor con la posición del bob del péndulo
  emitter.origin.set(pendulum.bob.x, pendulum.bob.y);

  // Emitir partículas desde el bob
  emitter.addParticle();
  emitter.run();

  pendulum.drag(); // interacción con mouse
  
    if (inter >= 1){ //venga y vuelva
        back = true;
      } else if (inter <= 0){
        back = false;
      }
  
  if (back){
        inter -= 0.01;
      } else {
        inter += 0.01;
      }  
}

function mousePressed() {
  pendulum.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum.stopDragging();
}

// -------------------- CLASE PENDULUM --------------------
class Pendulum {
  constructor(x, y, r) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 24.0;
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;

      this.angleVelocity *= this.damping;
    }
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0);
    this.bob.add(this.pivot);

    stroke(0);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    // Dibujar la estrella deformada en vez del círculo
    push();
    translate(this.bob.x, this.bob.y);
    fill(color(228, 255, 0), 180);
    stroke(0);
    strokeWeight(2);
    irregularStar(0, 0, this.ballr * 0.5, this.ballr, 8); // estrella con 8 puntas
    pop();
  }

  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}

// -------------------- FUNCIÓN ESTRELLA DEFORME --------------------
function irregularStar(x, y, radius1, radius2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    // radio interno con variación
    let r1 = radius1 + random(-4, 4);
    let sx = x + cos(a) * r1;
    let sy = y + sin(a) * r1;
    vertex(sx, sy);

    // radio externo con variación
    let r2 = radius2 + random(-6, 6);
    let sx2 = x + cos(a + halfAngle) * r2;
    let sy2 = y + sin(a + halfAngle) * r2;
    vertex(sx2, sy2);
  }
  endShape(CLOSE);
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(color(248, 255, 171), this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE CONFETTI --------------------
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    rectMode(CENTER);
    fill(color(255, 210, 171), this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```
`a Particle System with Forces`
``` js
let bob;
let spring;

function setup() {
  createCanvas(680, 480);
  spring = new Spring(width / 2, 50, 100);
  bob = new Emitter(width / 2, 200); // Bob ahora es un emisor
}

function draw() {
  background(150, 30);

  // Distancia actual del resorte
  let stretch = p5.Vector.dist(bob.origin, spring.anchor) - spring.restLength;

  // Gravedad base
  let gravityStrength = 0.1;

  // Si está muy estirado → gravedad lunar (más lenta)
  if (abs(stretch) > 50) {
    gravityStrength = 0.001;
  }

  // Salto de Lévy → pequeña probabilidad de aumentar gravedad (hacia abajo, nunca arriba)
  if (random(1) < 0.001) {
    gravityStrength = 1; console.log("Salto de lévy");
  }

  // Aplicar gravedad hacia abajo
  let gravity = createVector(0, gravityStrength);
  bob.applyForce(gravity);

  // Actualizar emisor (si no está arrastrando)
  if (!bob.dragging) {
    bob.update();
    spring.connect(bob);
    spring.constrainLength(bob, 30, 250);
  } else {
    // Si está arrastrando, la física se pausa
    bob.drag(mouseX, mouseY);
  }

  // Dibujar todo
  spring.showLine(bob);
  bob.addParticle();
  bob.run();
  spring.show();
}

// -------------------- INTERACCIÓN MOUSE --------------------
function mousePressed() {
  bob.clicked(mouseX, mouseY);
}

function mouseReleased() {
  bob.stopDragging();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0.0);
    //this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.velocity = createVector(random(-1, 1), random(0, 2));
    this.lifespan = 255.0;
    this.mass = 1;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE EMITTER (el Bob modificado) --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = 24;
    this.damping = 0.98;

    // Interactividad
    this.dragging = false;
    this.dragOffset = createVector();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.mult(this.damping);
    this.origin.add(this.velocity);
    this.acceleration.mult(0);
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  // -------------------- INTERACCIÓN --------------------
  clicked(mx, my) {
    let d = dist(mx, my, this.origin.x, this.origin.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.origin.x - mx;
      this.dragOffset.y = this.origin.y - my;
    }
  }

  stopDragging() {
    this.dragging = false;
    this.velocity.mult(0); // Al soltar, resetea la velocidad
  }

  drag(mx, my) {
    if (this.dragging) {
      this.origin.x = mx + this.dragOffset.x;
      this.origin.y = my + this.dragOffset.y;
    }
  }
}

// -------------------- CLASE SPRING --------------------
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2;
  }

  connect(bob) {
    let force = p5.Vector.sub(bob.origin, this.anchor);
    let currentLength = force.mag();
    let stretch = currentLength - this.restLength;

    force.setMag(-1 * this.k * stretch);
    bob.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {
    let direction = p5.Vector.sub(bob.origin, this.anchor);
    let length = direction.mag();

    if (length < minlen) {
      direction.setMag(minlen);
      bob.origin = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    } else if (length > maxlen) {
      direction.setMag(maxlen);
      bob.origin = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  showLine(bob) {
    // Dibujar la cuerda como 5 cuadrados
    let dir = p5.Vector.sub(bob.origin, this.anchor);
    for (let i = 0; i < 5; i++) {
      let pos = p5.Vector.add(this.anchor, p5.Vector.mult(dir, i / 5));
      push();
      rectMode(CENTER);
      fill(80);
      noStroke();
      square(pos.x, pos.y, 10);
      pop();
    }
  }
}
```
`a Particle System with a Repeller`
``` js
// One ParticleSystem
let emitter;

// Repellers
let repeller, autoRepeller;

// Bubbles
let bubble;

let cont;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 60);

  // Repeller fijo (seguirá al mouse en X)
  repeller = new Repeller(width / 2, 250);

  // Nuevo repeller que se mueve solo
  autoRepeller = new AutoRepeller(100, 150);

  // Nueva burbuja (tipo liquid) que se mueve sola
  bubble = new Bubble(300, 120, 80);

  cont = 0;
}

function draw() {
  cont++;
  background(150);

  // Spawner
  if (cont >= 8) {
    emitter.addParticle();
    cont = 0;
  }

  // Repeller fijo sigue al mouse en X
  repeller.position.x = mouseX;

  // Fuerzas
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  // Aplicar repelentes
  emitter.applyRepeller(repeller);
  emitter.applyRepeller(autoRepeller);

  emitter.run();

  // Mostrar objetos
  repeller.show();
  autoRepeller.update();
  autoRepeller.show();

  bubble.update();
  bubble.show();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 350.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- CLASE REPELLER --------------------
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.power = 150;
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(200, 100, 100);
    circle(this.position.x, this.position.y, 30);
  }

  repel(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    let strength = (-1 * this.power) / (distance * distance);
    force.setMag(strength);
    return force;
  }
}

// -------------------- CLASE AUTOREPELLER --------------------
class AutoRepeller extends Repeller {
  constructor(x, y) {
    super(x, y);
    this.velocity = p5.Vector.random2D().mult(2);
    this.power = 50;
  }

  update() {
    this.position.add(this.velocity);

    // Rebote en bordes
    if (this.position.x < 16 || this.position.x > width - 16) {
      this.velocity.x *= -1;
    }
    if (this.position.y < 16 || this.position.y > height - 16) {
      this.velocity.y *= -1;
    }
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(100, 200, 100);
    circle(this.position.x, this.position.y, 15);
  }
}

// -------------------- CLASE BUBBLE --------------------
class Bubble {
  constructor(x, y, r) {
    this.position = createVector(x, y);
    this.r = r;
    this.velocity = p5.Vector.random2D().mult(1.5);
  }

  update() {
    this.position.add(this.velocity);

    // Rebote en bordes
    if (this.position.x < this.r / 2 || this.position.x > width - this.r / 2) {
      this.velocity.x *= -1;
    }
    if (this.position.y < this.r / 2 || this.position.y > height - this.r / 2) {
      this.velocity.y *= -1;
    }
  }

  show() {
    noStroke();
    fill(100, 150, 255, 80);
    ellipse(this.position.x, this.position.y, this.r, this.r);
  }
}
```
####
6. Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.
####
`Simulación 1`
<img width="719" height="269" alt="image" src="https://github.com/user-attachments/assets/4278244d-9651-4643-83d0-f1a684de3d8f" />
`Simulación 2`
<img width="724" height="270" alt="image" src="https://github.com/user-attachments/assets/a44e5028-7953-407f-9f34-553e1f1391d0" />
`Simulación 3`
<img width="719" height="274" alt="image" src="https://github.com/user-attachments/assets/15d4abb1-2363-4e9e-95ad-bc188c307bd1" />
`Simulación 4`
<img width="761" height="544" alt="image" src="https://github.com/user-attachments/assets/eeb679c0-2eb3-4532-ad15-453fc23517b1" />
`Simulación 5`
<img width="732" height="291" alt="image" src="https://github.com/user-attachments/assets/154ac3e7-625e-407a-8207-17999e42d27f" />

## Actividad 3
Es hora de una nueva creación. Diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:
####
`Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.`
####
1. Es unidad incluye una novedad: DISEÑO. Debes intencionar tu obra. Esta vez te pediré que DISEÑES antes de generar código. Define un concepto, *haz bocetos*, define la interacción, etc. **¿Cuál es el concepto de tu obra? ¿Qué quieres comunicar con ella?**
####
Quiero que mi obra recuerde a esas campanas de viento que se encuentran en las casas, la lluvia, y el atardecer. Algo parecido a este boceto:
####
<img width="847" height="419" alt="image" src="https://github.com/user-attachments/assets/35364d2d-cb9b-4ee3-83e8-d71956d932ab" />
####
Quiero retratar calma, y que las interacciones lleven a una sensación de satisfacción.
####
2. Debes utilizar los conceptos de herencia y polimorfismo que revisaste en la fase de investigación.
####
3. Debes utilizar al menos un concepto de cada una de las unidades anteriores: 4 conceptos.
####
- Unidad 1: random y randomGaussian().
- Unidad 2: lerpColor().
- Unidad 3: Resistencia al aire.
- Unidad 4: Péndulos dobles y dirección con ángulos.
####
4. Debes definir cómo vas a gestionar el tiempo de vida de las partículas y la memoria.
####
Usé un `randomGaussian()` para controlar la vida de todas las párticulas. Esta podrá variar, pero nunca se saldrá de control y tampoco afectará los fps.
####
5. La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.
####
6. Incluye un enlace a tu código en el editor de p5.js.
####
[https://editor.p5js.org/catflyx/sketches/6DoG4lDGu](https://editor.p5js.org/catflyx/sketches/6DoG4lDGu)
####
7. Incluye el código fuente.
####
``` js
let pendulums = [];
let emitters = [];
let useMouseX = true; // alterna entre X o Y para dirección del aire
let inter = 0, inter2 = 0, back = false;

let palette;
let windStrength = 1; // fuerza del viento
let c1Index = 0;
let c2Index = 1;
let forward1 = true;
let forward2 = true;
let transitionSpeed = 0.003;

function setup() {
  createCanvas(800, 400);

  // 4 péndulos dobles distribuidos horizontalmente
  let spacing = width / 5;
  for (let i = 1; i <= 4; i++) {
    let baseX = spacing * i;
    let p1 = new Pendulum(baseX, 0, 120, true);  // primer péndulo con campana
    let p2 = new Pendulum(0, 0, 100, false);     // segundo péndulo con hoja
    pendulums.push({ p1, p2 });
    emitters.push(new Emitter(0, 0));
  }

  palette = [
    color(214, 154, 43),
    color(224, 79, 161),
    color(86, 65, 191),
    color(46, 150, 179),
  ];
}

function draw() {
  drawGradientBackground();

  for (let i = 0; i < pendulums.length; i++) {
    let { p1, p2 } = pendulums[i];
    let emitter = emitters[i];

    // actualizar primer péndulo con resistencia
    p1.update();
    p1.show();

    // actualizar segundo péndulo con resistencia
    p2.pivot = p1.bob.copy();
    p2.update();
    p2.show();

    // sincronizar emisor con el bob del segundo péndulo
    emitter.origin.set(p2.bob.x, p2.bob.y);

    // emitir partículas
    emitter.addParticle();
    emitter.run();

    // arrastrar con mouse
    p1.drag();
    p2.drag();
  }

  // Mostrar qué control se usa (X o Y) y viento
  fill(255);
  noStroke();
  textSize(14);
  text("Resistencia depende de: " + (useMouseX ? "mouseX" : "mouseY"), 10, height - 25);
  text("Viento: " + windStrength.toFixed(2), 10, height - 10);
  
    if (inter2 >= 1){ //venga y vuelva
        back = true;
      } else if (inter2 <= 0){
        back = false;
      }
  
  if (back){
        inter2 -= 0.003;
      } else {
        inter2 += 0.003;
      }    
}

function mousePressed() {
  for (let { p1, p2 } of pendulums) {
    p1.clicked(mouseX, mouseY);
    p2.clicked(mouseX, mouseY);
  }
}

function mouseReleased() {
  for (let { p1, p2 } of pendulums) {
    p1.stopDragging();
    p2.stopDragging();
  }
}

// -------------------- CLASE PENDULUM --------------------
class Pendulum {
  constructor(x, y, r, isBell) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;
    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 20.0;
    this.isBell = isBell; // true = campana, false = hoja
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      // aplicar resistencia del aire
      this.applyAirResistance();

      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;
      this.angleVelocity *= this.damping;
    }
  }

  applyAirResistance() {
    let airForce;
    if (useMouseX) {
      airForce = map(mouseX, 0, width, -0.002, 0.002);
    } else {
      airForce = map(mouseY, 0, height, -0.002, 0.002);
    }
    this.angleAcceleration += airForce * windStrength;
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle));
    this.bob.add(this.pivot);

    stroke(color(179, 46, 126));
    strokeWeight(4);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    // formas personalizadas
    if (this.isBell) {
      drawBell(this.bob.x, this.bob.y, this.angle);
    } else {
      drawLeaf(this.bob.x, this.bob.y, this.angle);
    }
  }

  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}

// -------------------- FORMAS PERSONALIZADAS --------------------
function drawBell(x, y, angle) {
  push();
  translate(x, y);
  rotate(angle);
  fill(184, 186, 209);
  stroke(color(179, 46, 126));
  strokeWeight(4);
  rectMode(CORNER);
  // pivote arriba: arranca en ( -ancho/2 , 0 )
  rect(-8, 0, 16, 40, 6);
  pop();
}

function drawLeaf(x, y, angle) {
  push();
  translate(x, y);
  rotate(angle);
  fill(196, 47, 69);
  stroke(color(179, 46, 126));
  strokeWeight(4);
  beginShape();
  vertex(0, -30);                 // punta superior
  bezierVertex(20, -10, 30, 10, 15, 25); // lado derecho ancho
  vertex(5, 15);                  // corte interior derecho
  bezierVertex(3, 20, -3, 20, -5, 15); // hacia interior izquierdo
  vertex(-15, 25);                // corte interior izquierdo
  bezierVertex(-30, 10, -20, -10, 0, -30); // lado izquierdo ancho
  endShape(CLOSE);
  pop();
}

// -------------------- CLASE PARTICLE --------------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = randomGaussian(255.0, 100.0);
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);

    // resistencia al aire basada en mouse
    let dir;
    if (useMouseX) {
      dir = map(mouseX, 0, width, -0.1, 0.1);
      this.applyForce(createVector(dir * windStrength, 0));
    } else {
      dir = map(mouseY, 0, height, -0.1, 0.1);
      this.applyForce(createVector(0, dir * windStrength));
    }

    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(color(179, 46, 126), this.lifespan);
    strokeWeight(2);
    fill(lerpColor(color(52, 62, 201), color(52, 201, 181), inter2), this.lifespan);
    circle(this.position.x, this.position.y, random(6, 10));
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

// -------------------- CLASE CONFETTI --------------------
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    rectMode(CENTER);
    fill(lerpColor(color(52, 201, 181), color(52, 62, 201), inter2), this.lifespan);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, random(10, 15));
    pop();
  }
}

// -------------------- CLASE EMITTER --------------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// -------------------- FONDO DEGRADADO --------------------
function drawGradientBackground() {
  inter += transitionSpeed;
  if (inter >= 1) {
    inter = 0;

    // control superior (c1): ping pong completo
    if (forward1) {
      c1Index++;
      if (c1Index >= palette.length - 1) {
        forward1 = false;
      }
    } else {
      c1Index--;
      if (c1Index <= 0) {
        forward1 = true;
      }
    }

    // control inferior (c2): solo rebota entre 1 y último
    if (forward2) {
      c2Index++;
      if (c2Index >= palette.length - 1) {
        forward2 = false;
      }
    } else {
      c2Index--;
      if (c2Index <= 1) {
        forward2 = true;
      }
    }
  }

  // easing para suavizar transición
  let t = inter * inter * (3 - 2 * inter);

  let c1 = palette[c1Index];
  let c2 = palette[c2Index];
  let interColor1 = lerpColor(c1, palette[(c1Index + 1) % palette.length], t);
  let interColor2 = lerpColor(c2, palette[(c2Index + 1) % palette.length], t);

  for (let y = 0; y < height; y++) {
    let amt = map(y, 0, height, 0, 1);
    let c = lerpColor(interColor1, interColor2, amt);
    stroke(c);
    line(0, y, width, y);
  }
}

// -------------------- TECLAS --------------------
function keyPressed() {
  if (key === 'y' || key === 'Y') {
    useMouseX = !useMouseX;
  }
  if (key === 'z' || key === 'Z') {
    windStrength = max(0, windStrength - 0.1);
  }
  if (key === 'x' || key === 'X') {
    windStrength += 0.1;
  }
}
```
8. Captura de pantallas de tu obra con las imágenes que más te gusten
####
<img width="871" height="446" alt="image" src="https://github.com/user-attachments/assets/c71bca96-14a1-438d-805c-3d7b63bd2faf" />
<img width="870" height="446" alt="image" src="https://github.com/user-attachments/assets/48936159-6c11-4406-a900-43c940a2687d" />
<img width="871" height="443" alt="image" src="https://github.com/user-attachments/assets/df2ec8c8-190d-4998-9eb8-6dda1a6f5859" />
<img width="864" height="447" alt="image" src="https://github.com/user-attachments/assets/51299dc4-4013-437d-a222-0afc9e3b993c" />

## Nota
Tu mismo vas a propoer una nota basada en la rúbrica y justificarás cada criterio de la rúbrica indicando qué evidencias presentes en la bitácora justifican la nota que propones.
####
### Criterio 1: Investigación y Experimentación (Evidencia en Actividad 2)
Se realizó todo según lo indicado en cada simulación, haciendo uso de conceptos de las 4 unidades anteriores; así como se contestarón las preguntas con los elementos y análisis requerido.
`Excelente (4.5 - 5.0)`
### Criterio 2: Intención y Diseño (Proceso de Actividad 3)
Cumple, pues la inspiración es clara, y se pueden intuir de la misma que procesos se pueden usar para llegar a esta.
`Excelente (4.5 - 5.0)`
### Criterio 3: Aplicación Técnica (Código de Actividad 3)
Cada función y proceso tiene su lugar en el código, y nada sobra para evitar el consumo innecesario de memoria.
`Excelente (4.5 - 5.0)`
### Criterio 4: Calidad de la Obra Final (Artefacto Entregado)
La obra está bien pulida y posee la interactividad necesaria dentro de la idea que esta desea transmitir, tanto para que el resultado siempre sea diverso, como para que sea coherente con la idea inicial.
`Excelente (4.5 - 5.0)`
### Final
Si cada criterio vale alrededor de 25%, la nota estaría entre `4.5 y 5.0`.
