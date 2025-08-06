# Unidad 2
`🛠 Fase: Apply`
_________________________________________________________________________________________________________________________________________________________________________________________
## Actividad 08
### Creación de obra generativa
Vas a crear una obra generativa interactivo en tiempo real que utilice los conceptos de motion 101, vectores y algunos algoritmos de la unidad anterior. Vas a probar un algorimo para calcular la aceleración diferente a los que analizaste en esta unidad.
### Debe tener:
- El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.
El código de la aplicación.
# Concepto
- Describe el concepto de tu obra generativa.
###
Hacer un "pincel" usando el `mover`, de tal forma que este siga el mouse con una aceleración definida por la distancia; los cuales serán dos. De igual forma, hacer que al presionar ciertas teclas este pueda cambiar su modo de aceleración, sin dejar de seguir el mouse en ciertos modos. Ya usando conceptos de la unidad anterior, hacer que de *saltos* aleatorios hacia el centro del canvas, así como que exista otros dos `mover` independientes los cuales tendrán un trazo usando una forma distinta, así como usará otro método de aceleración. Esta última también podrá modificarse en ciertos aspectos mientras el programa corre.
###
- ¿Cómo piensas aplicar el marco MOTION 101 y por qué?
###
Usando las variables de posición, velocidad y aceleración para favorecer el movimiento de mi objeto `mover`.
###
- ¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?
###
Principalmente, la aceleración hacia el mouse y la aleatoria. Esto para permitir tanto trazos suaves y predecibles, así como trazos más rústicos y que den un producto más "accidentado", sin serlo necesariamente. La aceleración constante también se usará, pero no tendrá mucha participación pues, no será interactiva directamente.

# Código
`Versión 1`
###
Las bases del código: dos objetos circulares que sigen de cierta forma la orbita del mouse, y dos objetos pentagonales que rondan de forma aleatoria el canvas, hasta chocar con sus límites. Además, existe una probabilidad que alguno de los circulos vayan al centro, donde se volveran una suerte de estrella por algunos frames, antes de volver a seguir el mouse.
###
``` js
let orbiters = [];
let orbitIntensity = 0.1;
let bouncers = [];

function setup() {
  createCanvas(800, 600);

  // Crear dos orbitadores con direcciones opuestas
  orbiters.push(new Orbiter(color(255, 0, 0), PI, 1));   // Sentido horario
  orbiters.push(new Orbiter(color(0, 0, 255), PI / 2, -1)); // Sentido antihorario

  // Crear pentágonos
  bouncers.push(new Bouncer(createVector(0.05, 0.05)));   // Aceleración normal (hacia abajo)
  bouncers.push(new Bouncer(createVector(-0.05, -0.05))); // "Gravedad" invertida (hacia arriba)
}

function draw() {
  background(30);

  // Orbitadores
  for (let orb of orbiters) {
    orb.update();
    orb.display();
  }

  // Bouncers
  for (let b of bouncers) {
    b.update();
    b.display();
  }
}

function keyPressed() {
  if (key === 'z') {
    orbitIntensity += 0.05;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  } else if (key === 'x') {
    orbitIntensity -= 0.05;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  }
}

// ---------------------- CLASE ORBITADOR ----------------------
class Orbiter {
  constructor(c, initialAngle, direction = 1) {
    this.angle = initialAngle;
    this.radius = random(50, 150);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.pos = createVector(0, 0);
    this.color = c;
    this.direction = direction; // Sentido de la órbita
    this.isStar = false;
    this.starTimer = 0;
  }

  update() {
    // Baja probabilidad de moverse hacia el centro
    if (random(1) < 0.002 && !this.isStar) {
      this.isStar = true;
      this.starTimer = 60; // frames en forma de estrella
      this.pos.set(width / 2, height / 2);
      return;
    }

    if (this.isStar) {
      this.starTimer--;
      if (this.starTimer <= 0) {
        this.isStar = false;
      }
      return;
    }

    // Movimiento orbital con dirección opuesta y mayor velocidad
    this.angle += 0.05 * this.direction;

    // Variar la distancia con aceleración aleatoria
    let accRadius = random(-1, 1) * orbitIntensity;
    this.acceleration = createVector(0, accRadius);
    this.velocity.add(this.acceleration);
    this.radius += this.velocity.y;

    this.radius = constrain(this.radius, 40, 200);

    // Calcular nueva posición alrededor del mouse
    let offsetX = cos(this.angle) * this.radius;
    let offsetY = sin(this.angle) * this.radius;
    this.pos.set(mouseX + offsetX, mouseY + offsetY);
  }

  display() {
    fill(this.color);
    noStroke();

    if (this.isStar) {
      push();
      translate(this.pos.x, this.pos.y);
      rotate(frameCount * 0.1);
      beginShape();
      for (let i = 0; i < 10; i++) {
        let angle = TWO_PI / 10 * i;
        let r = i % 2 === 0 ? 10 : 4;
        let x = cos(angle) * r;
        let y = sin(angle) * r;
        vertex(x, y);
      }
      endShape(CLOSE);
      pop();
    } else {
      ellipse(this.pos.x, this.pos.y, 20, 20);
    }
  }
}

// ---------------------- CLASE BOUNCER (PENTÁGONOS) ----------------------
class Bouncer {
  constructor(acc) {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D().mult(2);
    this.acc = acc.copy();
    this.color = color(random(255), random(255), random(255));
    this.r = 30;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);

    // Rebotar en los bordes
    if (this.pos.x < this.r || this.pos.x > width - this.r) {
      this.vel.x *= -1;
    }
    if (this.pos.y < this.r || this.pos.y > height - this.r) {
      this.vel.y *= -1;
    }
  }

  display() {
    fill(this.color);
    noStroke();
    push();
    translate(this.pos.x, this.pos.y);
    rotate(frameCount * 0.01);
    beginShape();
    for (let i = 0; i < 5; i++) {
      let angle = TWO_PI / 5 * i;
      let x = cos(angle) * this.r;
      let y = sin(angle) * this.r;
      vertex(x, y);
    }
    endShape(CLOSE);
    pop();
  }
}

```
`Versión 2`
###
El canvas ahora no pinta el fondo cada frame, los objetos que orbitan el mouse cambian de color con un parametro cada frame, así como se puede hacer que la intensidad de su orbita aumente con `z` y desaumente con `x`. La estrella ahora define sus dimensiones con parametros aleatorios, así como dura menos en pantalla. Por otro lado, los pentagonos ahora pueden modificarse sus colores de dos formas: con `e`, se vuelven escala de grises, y con `r`, se vuelven un color aleatorio.
###
``` js
let orbiters = [];
let orbitIntensity = 0.1;
let bouncers = [];

let c1, c2; let p1, p2; let r1, g1, b1; let r2, g2, b2;

let color1, color2;

function setup() {
  createCanvas(800, 600); background(random(0, 100));

  r1 = random(200); g1 = random(200); b1 = random(200);
  r2 = random(200); g2 = random(200); b2 = random(200);
  
  // Crear dos orbitadores con direcciones opuestas
  orbiters.push(new Orbiter(color(r1, g1, b1), PI, 1));   // Sentido horario
  orbiters.push(new Orbiter(color(r2, g2, b2), PI / 2, -1)); // Sentido antihorario

  // Crear pentágonos
  bouncers.push(new Bouncer(createVector(0.05, 0.05)));   // Aceleración normal (hacia abajo)
  bouncers.push(new Bouncer(createVector(-0.05, -0.05))); // "Gravedad" invertida (hacia arriba)
}

function draw() {
  //Cambiar color con el paso del tiempo
  r1 += random(-50, 50); g1 += random(-50, 50); b1 += random(-50, 50);
  r2 += random(-50, 50); g2 += random(-50, 50); b2 += random(-50, 50);
  
  orbiters[0].color = color(r1, g1, b1); orbiters[1].color = color(r2, g2, b2);
  
  if (random(1) < 0.005) {
      r1 += random(-180, 180); g1 += random(-180, 180); b1 += random(-180, 180);
     console.log('c1 cambió de color', r1, g1, b1);
    }
  if (random(1) < 0.005) {
      r2 += random(-180, 180); g2 += random(-180, 180); b2 += random(-180, 180);
     console.log('c2 cambió de color', r2, g2, b2);
    }
  
  // Orbitadores
  for (let orb of orbiters) { 
    orb.update();
    orb.display();
  }

  // Bouncers
  for (let b of bouncers) {
    b.update();
    b.display();
  }
  
  //Situaciones a evitar
  if (r1, g1, b1 <= 10 || r2, g2, b2 <= 10){ //evita que se quede en negro
  r1 += 100; g1 += 100; b1 += 100;
  r2 += 100; g2 += 100; b2 += 100;
    console.log('negro');
      } else if (r1, g1, b1 >= 240 || r2, g2, b2 >= 240){ //evita que se quede en blanco
  r1 -= 100; g1 -= 100; b1 -= 100;
  r2 -= 100; g2 -= 100; b2 -= 100;
    console.log('blanco');
            }
  
  
}

function keyPressed() {
  if (key === 'z' || key === 'Z') { //++
    orbitIntensity += 1;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  } else if (key === 'x' || key === 'X') { //--
    orbitIntensity -= 1;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  }
  
  //Cambiar color pentagonos
  if (key === 'e' || key === 'E'){ //Grises
    bouncers[0].color = color(random(200)); bouncers[1].color = color(random(200));
      }
    if (key === 'r' || key === 'R'){ //Color
    bouncers[0].color = color(random(200), random(200), random(200));
    bouncers[1].color = color(random(200), random(200), random(200));
      }
  
  
}

// ---------------------- CLASE ORBITADOR ----------------------
class Orbiter {
  constructor(c, initialAngle, direction = 1) {
    this.angle = initialAngle;
    this.radius = random(50, 150);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.pos = createVector(0, 0);
    this.color = c;
    this.direction = direction; // Sentido de la órbita
    this.isStar = false;
    this.starTimer = 0;
  }

  update() {
    // Baja probabilidad de moverse hacia el centro
    if (random(1) < 0.002 && !this.isStar) {
      this.isStar = true;
      this.starTimer = 2; // frames en forma de estrella
      this.pos.set(width / 2, height / 2);
      return;
    }

    if (this.isStar) {
      this.starTimer--;
      if (this.starTimer <= 0) {
        this.isStar = false;
      }
      return;
    }

    // Movimiento orbital con dirección opuesta y mayor velocidad
    this.angle += 0.05 * this.direction;

    // Variar la distancia con aceleración aleatoria
    let accRadius = random(-1, 1) * orbitIntensity;
    this.acceleration = createVector(0, accRadius);
    this.velocity.add(this.acceleration);
    this.radius += this.velocity.y;

    this.radius = constrain(this.radius, 40, 200);

    // Calcular nueva posición alrededor del mouse
    let offsetX = cos(this.angle) * this.radius;
    let offsetY = sin(this.angle) * this.radius;
    this.pos.set(mouseX + offsetX, mouseY + offsetY);
  }

  display() {
    fill(this.color);
    noStroke();

    if (this.isStar) {
      push();
      translate(this.pos.x, this.pos.y);
      rotate(frameCount * 0.1);
      beginShape();
      for (let i = 0; i < 10; i++) {
        let angle = TWO_PI / 10 * i;
        let r = i % 2 === 0 ? random(60, 120) : random(16, 60);
        let x = cos(angle) * r;
        let y = sin(angle) * r;
        vertex(x, y);
      }
      endShape(CLOSE);
      pop();
    } else {
      ellipse(this.pos.x, this.pos.y, 20, 20);
    }
  }
}

// ---------------------- CLASE BOUNCER (PENTÁGONOS) ----------------------
class Bouncer {
  constructor(acc) {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D().mult(2);
    this.acc = acc.copy();
    this.color = color(random(255), random(255), random(255));
    this.r = 30;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);

    // Rebotar en los bordes
    if (this.pos.x < this.r || this.pos.x > width - this.r) {
      this.vel.x *= -1;
    }
    if (this.pos.y < this.r || this.pos.y > height - this.r) {
      this.vel.y *= -1;
    }
  }

  display() {
    fill(this.color);
    noStroke();
    push();
    translate(this.pos.x, this.pos.y);
    rotate(frameCount * 0.01);
    beginShape();
    for (let i = 0; i < 5; i++) {
      let angle = TWO_PI / 5 * i;
      let x = cos(angle) * this.r;
      let y = sin(angle) * this.r;
      vertex(x, y);
    }
    endShape(CLOSE);
    pop();
  }
}

```
`Versión 3`
###
Al hacer click, los circulos cambiarán a cuadrados, y viceversa. Ahora tienen transparencia leve. Correción de valores de color.
###
``` js
let orbiters = [];
let orbitIntensity = 0.1;
let bouncers = [];

let c1, c2; let p1, p2; let r1, g1, b1; let r2, g2, b2;

let color1, color2;

function setup() {
  createCanvas(800, 600); background(random(0, 50));

  r1 = random(200); g1 = random(200); b1 = random(200);
  r2 = random(200); g2 = random(200); b2 = random(200);
  
  // Crear dos orbitadores con direcciones opuestas
  orbiters.push(new Orbiter(color(r1, g1, b1), PI, 1));   // Sentido horario
  orbiters.push(new Orbiter(color(r2, g2, b2), PI / 2, -1)); // Sentido antihorario

  // Crear pentágonos
  bouncers.push(new Bouncer(createVector(0.05, 0.05)));   // Aceleración normal (hacia abajo)
  bouncers.push(new Bouncer(createVector(-0.05, -0.05))); // "Gravedad" invertida (hacia arriba)
}

function draw() {
  //Cambiar color con el paso del tiempo
  if (random(1) < 0.5){
  r1 += random(-50, 50); g1 += random(-50, 50); b1 += random(-50, 50);
  r2 -= random(-50, 50); g2 += random(-50, 50); b2 += random(-50, 50);
    
      } else if (random(1) > 0.5) {
         r1 -= random(-50, 50); g1 -= random(-50, 50); b1 -= random(-50, 50);
         r2 += random(-50, 50); g2 -= random(-50, 50); b2 -= random(-50, 50);
      }
  
  orbiters[0].color = color(r1, g1, b1, random(150, 200));
  orbiters[1].color = color(r2, g2, b2, random(150, 200));
  
  if (random(1) < 0.005) {
      r1 += random(-180, 180); g1 += random(-180, 180); b1 += random(-180, 180);
    orbiters[0].color = color(r1, g1, b1); orbiters[1].color = color(r2, g2, b2);
     console.log('c1 cambió de color', r1, g1, b1);
    }
  if (random(1) < 0.005) {
      r2 += random(-180, 180); g2 += random(-180, 180); b2 += random(-180, 180);
    orbiters[0].color = color(r1, g1, b1); orbiters[1].color = color(r2, g2, b2);
     console.log('c2 cambió de color', r2, g2, b2);
    }
  
  // Orbitadores
  for (let orb of orbiters) { 
    orb.update();
    orb.display();
  }

  // Bouncers
  for (let b of bouncers) {
    b.update();
    b.display();
  }
  
           //Situaciones a evitar
  
  //c1
  if (r1 <= 10){ 
    //evita que se quede en negro, negativos
  r1 += 50;
    
  } else if (r1 >= 250){  //evita que se quede en blanco, muy altos
  r1 -= 50;
  }
  
  if (g1 <= 10){ 
    //evita que se quede en negro, negativos
  g1 += 50;
    
  } else if (g1 >= 250){  //evita que se quede en blanco, muy altos
  g1 -= 50;
  }
  
  if (b1 <= 10){ 
    //evita que se quede en negro, negativos
  b1 += 50;
    
  } else if (b1 >= 250){  //evita que se quede en blanco, muy altos
  b1 -= 50;
  }
  
  //c2
  if (r2 <= 10){ 
    //evita que se quede en negro, negativos
  r2 += 50;
    
  } else if (r2 >= 250){  //evita que se quede en blanco, muy altos
  r2 -= 50;
  }
  
  if (g2 <= 10){ 
    //evita que se quede en negro, negativos
  g2 += 50;
    
  } else if (g2 >= 250){  //evita que se quede en blanco, muy altos
  g2 -= 50;
  }
  
  if (b2 <= 10){ 
    //evita que se quede en negro, negativos
  b2 += 50;
    
  } else if (b2 >= 250){  //evita que se quede en blanco, muy altos
  b2 -= 50;
  }
  
}

function keyPressed() {
  if (key === 'z' || key === 'Z') { //++
    orbitIntensity += 1;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  } else if (key === 'x' || key === 'X') { //--
    orbitIntensity -= 1;
    orbitIntensity = constrain(orbitIntensity, 0, 1);
  }
  
  //Cambiar color pentagonos
  if (key === 'e' || key === 'E'){ //Grises
    bouncers[0].color = color(random(200), 180); 
    bouncers[1].color = color(random(200), 180);
      }
    if (key === 'r' || key === 'R'){ //Color
    bouncers[0].color = color(random(200), random(200), random(200), 180);
    bouncers[1].color = color(random(200), random(200), random(200), 180);
      }
  
  
}

function mousePressed() {
  for (let orb of orbiters) {
    // Alterna entre true y false
    orb.isSquare = !orb.isSquare;
  }
}

// ---------------------- CLASE ORBITADOR ----------------------
class Orbiter {
  constructor(c, initialAngle, direction = 1) {
    this.angle = initialAngle;
    this.radius = random(50, 150);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.pos = createVector(0, 0);
    this.color = c;
    this.direction = direction; // Sentido de la órbita
    
    this.isStar = false;
    this.starTimer = 0;
    
    this.isSquare = false;
  }

  update() {
    // Baja probabilidad de moverse hacia el centro
    if (random(1) < 0.002 && !this.isStar) {
      this.isStar = true;
      this.starTimer = 2; // frames en forma de estrella
      this.pos.set(width / 2, height / 2);
      return;
    }

    if (this.isStar) {
      this.starTimer--;
      if (this.starTimer <= 0) {
        this.isStar = false;
      }
      return;
    }

    // Movimiento orbital con dirección opuesta y mayor velocidad
    this.angle += 0.05 * this.direction;

    // Variar la distancia con aceleración aleatoria
    let accRadius = random(-1, 1) * orbitIntensity;
    this.acceleration = createVector(0, accRadius);
    this.velocity.add(this.acceleration);
    this.radius += this.velocity.y;

    this.radius = constrain(this.radius, 40, 200);

    // Calcular nueva posición alrededor del mouse
    let offsetX = cos(this.angle) * this.radius;
    let offsetY = sin(this.angle) * this.radius;
    this.pos.set(mouseX + offsetX, mouseY + offsetY);
  }

  display() {
    fill(this.color);
    noStroke();

    if (this.isStar) {
      push();
      translate(this.pos.x, this.pos.y);
      rotate(frameCount * 0.1);
      beginShape();
      for (let i = 0; i < 10; i++) {
        let angle = TWO_PI / 10 * i;
        let r = i % 2 === 0 ? random(60, 120) : random(16, 60);
        let x = cos(angle) * r;
        let y = sin(angle) * r;
        vertex(x, y);
      }
      endShape(CLOSE);
      pop();
    } else {
     // Alternar entre círculo y cuadrado
    if (this.isSquare) {
      rectMode(CENTER);
      square(this.pos.x, this.pos.y, 20);
    } else {
      ellipse(this.pos.x, this.pos.y, 20, 20);
    }
  }
  }
}

//ellipse(this.pos.x, this.pos.y, 20, 20);

// ---------------------- CLASE BOUNCER (PENTÁGONOS) ----------------------
class Bouncer {
  constructor(acc) {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D().mult(2);
    this.acc = acc.copy();
    this.color = color(random(255), random(255), random(255), random(20, 60));
    this.r = 30;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);

    // Rebotar en los bordes
    if (this.pos.x < this.r || this.pos.x > width - this.r) {
      this.vel.x *= -1;
    }
    if (this.pos.y < this.r || this.pos.y > height - this.r) {
      this.vel.y *= -1;
    }
  }

  display() {
    fill(this.color);
    noStroke();
    push();
    translate(this.pos.x, this.pos.y);
    rotate(frameCount * 0.01);
    beginShape();
    for (let i = 0; i < 5; i++) {
      let angle = TWO_PI / 5 * i;
      let x = cos(angle) * this.r;
      let y = sin(angle) * this.r;
      vertex(x, y);
    }
    endShape(CLOSE);
    pop();
  }
}
```
- Un enlace al proyecto en el editor de p5.js.
###
https://editor.p5js.org/catflyx/sketches/LjO0Gb2Rg
###
- Una captura de pantalla representativa de tu pieza de arte generativo.
###
<img width="806" height="604" alt="image" src="https://github.com/user-attachments/assets/53b2fb12-adea-4f89-ae79-dd904623fb54" />







