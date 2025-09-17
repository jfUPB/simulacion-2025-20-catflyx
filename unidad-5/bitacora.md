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
...
####
Además te pediré que hagas los siguientes experimentos y los reportes en tu bitácora:
1. Vas a modificar *cada una de las simulaciones anteriores*, incluye en cada una al menos *un concepto de las unidades anteriores*, pero **no repitas concepto**, la idea es que repases al menos uno de cada unidad.
####
`an Array of Particles`
####
Concepto: ...
####
...
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
Concepto: ...
####
...
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
Concepto: ...
####
...
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
Concepto: ...
####
...
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
Concepto: ...
####
...
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
...
####
3. Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
####
- **simulación 1:** ...
- **simulación 2:** ...
- **simulación 3:** ...
- **simulación 4:** ...
- **simulación 5:** ...
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

```
`a System of Systems`
``` js

```
`a Particle System with Inheritance and Polymorphism`
``` js

```
`a Particle System with Forces`
``` js

```
`a Particle System with a Repeller`
``` js

```
####
6. Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.
####


## Actividad 3
Es hora de una nueva creación. Diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:
####
`Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.`
####
1. Es unidad incluye una novedad: DISEÑO. Debes intencionar tu obra. Esta vez te pediré que DISEÑES antes de generar código. Define un concepto, *haz bocetos*, define la interacción, etc. **¿Cuál es el concepto de tu obra? ¿Qué quieres comunicar con ella?**
####
...
####
2. Debes utilizar los conceptos de herencia y polimorfismo que revisaste en la fase de investigación.
####
...
####
3. Debes utilizar al menos un concepto de cada una de las unidades anteriores: 4 conceptos.
####
- Unidad 1: ...
- Unidad 2: ...
- Unidad 3: ...
- Unidad 4: ...
####
4. Debes definir cómo vas a gestionar el tiempo de vida de las partículas y la memoria.
####
...
####
5. La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.
####
...
####
6. Incluye un enlace a tu código en el editor de p5.js.
####
...
####
7. Incluye el código fuente.
####
`Versión 1`
``` js

```
`Versión 2`
``` js

```
`Versión 3`
``` js

```
`Versión 4`
``` js

```
8. Captura de pantallas de tu obra con las imágenes que más te gusten
####


