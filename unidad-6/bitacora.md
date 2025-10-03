# Evidencias de la unidad 6
_______________________________________________________________________________________________________________________________________________________________________________
# Set y Seek
## Actividad 1
En esta actividad propondré que te encuentres de nuevo el trabajo de [Tyler Hobbs](https://youtu.be/8tTGJvijoDw?si=7KWuEhMTIjH41JOj) y específicamente que mires [su artículo sobre campos de flujo](https://www.tylerxhobbs.com/words/flow-fields).
- Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.
####
<img width="323" height="581" alt="image" src="https://github.com/user-attachments/assets/a467207e-e5c4-4401-a0ff-ca987519ae40" />

Los colores y el movimiento dan una sensación de un día caluroso, pero al mismo tiempo, expresa cierta relajación.
<img width="1150" height="578" alt="image" src="https://github.com/user-attachments/assets/cd3a5290-cfd8-45d8-8e5e-2896acf45398" />

Los trazos a pesar de verse con cierto orden, dan aires de trazos de práctica cuando uno calienta al dibujar. Intentan simular una simetría y orden pero se terminan deformando al final. Y no se ven exactamente ni pulidos ni acabados.
####
- ¿Qué te inspira de su trabajo?
####
Parece ser alguien muy participe del ángulo y la asimetridad en sus obras, aunque hay algunas que intentan ser bastante simétricas, me parece interesante el acercamiento que tiene en las otras que no.

## Actividad 2
En esta actividad quiero que investigues alrededor de estas dos preguntas:
####
1. ¿Qué es una fuerza de dirección (steering force)?
####
Una steering force es una fuerza que no se aplica de manera directa como la gravedad o el viento, sino que surge de la diferencia entre la velocidad deseada y la velocidad actual del objeto.
####
2. ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?
####
Tiene dos diferencias principales con las anteriores fuerzas que hemos tratado: La steering force es relativa al agente, depende de su posición, velocidad y de dónde quiere ir; no es una fuerza física "real" como la gravedad, sino una fuerza de comportamiento, calculada artificialmente para simular decisiones.
####
3. ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?
####
Concretamente, la steering force es un mecanismo matemático que permite implementar las reglas de comportamiento propuestas por Reynolds tales como el flocking behaviour.

## Actividad 3
Vamos a analizar el primer algoritmo clave: los campos de flujo (Flow Fields), basándonos en el ejemplo del libro “The Nature of Code”. Entenderemos cómo una cuadrícula de vectores dirige el movimiento de los agentes.
- Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [Capítulo 5, sección “Flow Fields”](https://natureofcode.com/autonomous-agents/#flow-fields) (y ejemplos de código asociados).
- El código fuente del ejemplo de Flow Fields de TNoC.
####
Ver los [pasos](https://juanferfranco.github.io/simulacion-2025-20/units/unit6/).
####
1. Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
####
- **Separación:** evita choques (mantén distancia).
- **Alineación:** sigue la dirección del grupo (muévete como ellos).
- **Cohesión:** mantente dentro del grupo (no te quedes atrás).
####
La combinación balanceada de estas tres reglas hace que emerja el movimiento fluido y realista de un enjambre.
####
2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
####
Un agente usa su campo de percepción como un sensor: mide dónde están y cómo se mueven sus vecinos, traduce esa información en vectores de corrección (separarse, alinearse, unirse) y, después de ponderar y limitar, obtiene una fuerza de dirección final que determina hacia dónde y cómo se moverá en el siguiente instante.
####
3. Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
####
- **Radio de percepción:** define qué vecinos se consideran (puede variar por regla).
- **Pesos de las reglas:** ajustan la influencia relativa de separación, alineación y cohesión.
- **Maxspeed:** velocidad máxima de cada boid.
- **Maxforce:** fuerza máxima de cambio de dirección.
####
4. Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
####
...

## Actividad 4
Ahora analizaremos el segundo algoritmo: el comportamiento de enjambre (Flocking), famoso por simular el movimiento coordinado de pájaros o peces. Nos basaremos nuevamente en “The Nature of Code” para entender las tres reglas básicas que lo gobiernan.
- Libro “The Nature of Code” (TNoC) de Daniel Shiffman: [capítulo 5, sección “Flocking”](https://natureofcode.com/autonomous-agents/#flocking) (y ejemplos de código asociados).
- El código fuente del ejemplo de Flocking de TNoC.
####
Ver los [pasos](https://juanferfranco.github.io/simulacion-2025-20/units/unit6/).
####
1. Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
####
...
####
Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
####
...
####
Describe la modificación que realizaste al código y *explica detalladamente* el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
####
...

# Apply
## Actividad 5
Ahora que entiendes los algoritmos, es tu turno de crear. Aplicarás **uno** de ellos (flow fields o flocking) para generar una pieza de arte interactivo que permita visualizar un tema musical de tu elección. La interacción del usuario debe influir en el comportamiento de los agentes.
####
**Condiciones**
- Elige un tema musical que te inspire.
- Diseña una pieza de arte generativo que utilice el algoritmo de flow fields y/o flocking.
- Vas a visualizar el tema musical y además vas a “tocar” las visuales, es decir, tu pieza de arte debe ser interactiva y debe permitir la interpretación en tiempo real de las visuales como si fuera un instrumento más que acompaña de manera coherente el tema musical.
####
1. Documenta todo el proceso de diseño y creación en tu bitácora, incluyendo bocetos y decisiones de diseño.
####
Para inciar, decidí elegir uno de los chase themes de Forsaken como tema musical, siendo más específicamente el de la skin 1 Eggs ([https://youtu.be/MCicc4EQhyc?si=G3dgBHSmxiK6ioJp](https://youtu.be/MCicc4EQhyc?si=G3dgBHSmxiK6ioJp)). Para ello, me inspiré de la lírica y el aspecto de la skin para generar mi obra generativa. Segundo, hice un sketch de cómo quería que se viera:
<img width="657" height="658" alt="image" src="https://github.com/user-attachments/assets/2c315008-a610-488b-9723-4f48c9bcc830" />

<img width="649" height="655" alt="image" src="https://github.com/user-attachments/assets/e64cbd2f-e9d2-4c7d-b661-77c55f1837ad" />

Van a usarse flowfields que tomarán el ritmo de la música para definir la dirección de los "huevos", así como habrá momentos en que apareceran estrellas y equis. De igual forma, la paleta se limitará a solo tres colores: Negro, blanco y amarillo. Con respectivos cambios de tono en el caso del amarillo. También simplifiqué los sketchs en dos fases, que representan antes y después de que aparezca el amarillo.

Además, 

####
2. El código fuente completo de tu sketch en p5.js.
`Versión 1`
``` js
let debug = false;

// Flowfield object
let flowfield;
// Un array de Eggs
let eggs = [];

// Audio
let song;
let amp;

function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/1eggs.ogg');
  console.log("Cargando música");
}

function setup() { console.log("Música cargada");
  createCanvas(600, 600);

  flowfield = new FlowField(20);

  // Configuración de amplitud (intensidad del audio)
  amp = new p5.Amplitude();

  // Reproduce la canción automáticamente
  song.loop();
  
  console.log("Todo cargado");
}

function draw() {
  background(0); // fondo negro

  if (debug) flowfield.show();

  // Intensidad del audio entre 0 y 1
  let level = amp.getLevel();
  // Mapear la intensidad al número de huevos
  let numEggs = int(map(level, 0, 0.3, 50, 200, true));

  // Ajustar la cantidad de huevos en el array sin sobrecargar
  while (eggs.length < numEggs) {
    eggs.push(
      new Egg(
        random(width),
        random(height),
        random(2, 5),
        random(0.1, 0.5)
      )
    );
  }
  while (eggs.length > numEggs) {
    eggs.pop();
  }

  // Actualizar y mostrar cada huevo
  for (let i = 0; i < eggs.length; i++) {
    eggs[i].follow(flowfield);
    eggs[i].run();
  }
}

function keyPressed() {
  if (key == " ") {
    debug = !debug;
  }
}

function mousePressed() {
  flowfield.init();
}

// ------------------------- CLASE FLOWFIELD -----------------------------

class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = width / this.resolution;
    this.rows = height / this.resolution;
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(random(10000));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }

  show() {
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let w = width / this.cols;
        let h = height / this.rows;
        let v = this.field[i][j].copy();
        v.setMag(w * 0.5);
        let x = i * w + w / 2;
        let y = j * h + h / 2;
        stroke(100);
        strokeWeight(1);
        line(x, y, x + v.x, y + v.y);
      }
    }
  }

  lookup(position) {
    let column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
    let row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

// ------------------------- CLASE EGG -----------------------------

class Egg {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(0, 0);
    this.r = 6; // tamaño base del huevo
    this.maxspeed = ms;
    this.maxforce = mf;
  }

  run() {
    this.update();
    this.borders();
    this.show();
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());

    // Dibujo optimizado de un huevo con ellipse escalada
    noStroke();
    fill(255); // huevos blancos
    ellipse(0, 0, this.r * 2, this.r * 2.6);

    pop();
  }
}
```
`Versión 2`
``` js
let debug = false;

// Flowfield object
let flowfield;
// Arrays de objetos
let eggs = [];
let mis = [];
let stars = [];
let xs = [];

// Audio
let song;
let amp;
let fft;

// Paleta amarilla
let palette = [];
let lerpIndex = 0;
let lerpSpeed = 0.01;

// Fondos dinámicos
let bgColor;
let targetBgColor;
let levyTimer = 0;

// Cooldown para MI
let lastMiSpawn = 0;

// Fondo amarillo loop
let yellowActive = false;
let yellowTimer = 0;

function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/1eggs.ogg');
}

function setup() {
  createCanvas(600, 600);
  flowfield = new FlowField(20);

  // Audio
  amp = new p5.Amplitude();
  fft = new p5.FFT();

  // Paleta amarilla
  palette = [
    color(255, 204, 0),
    color(255, 221, 51),
    color(255, 238, 102),
    color(230, 184, 0)
  ];

  bgColor = color(0);
  targetBgColor = color(0);

  song.loop();
}

function draw() {
  // Fondo con transición
  bgColor = lerpColor(bgColor, targetBgColor, 0.05);
  background(bgColor);

  let level = amp.getLevel();

  // ---------------- Eggs
  let numEggs = int(map(level, 0, 0.3, 50, 200, true));
  while (eggs.length < numEggs) {
    eggs.push(new Egg(random(width), random(height), random(2, 5), random(0.1, 0.5)));
  }
  while (eggs.length > numEggs) {
    eggs.pop();
  }

  // ---------------- FFT análisis
  let spectrum = fft.analyze();
  let bassEnergy = fft.getEnergy("bass"); // graves

  // Generación de MI con cooldown y límite
  if (bassEnergy > 180 && frameCount - lastMiSpawn > 120) { 
    for (let i = 0; i < 10; i++) {
      mis.push(new MI(random(width), random(height), random(2, 5), random(0.1, 0.5)));
    }
    lastMiSpawn = frameCount;
  }

  // ---------------- Salto de Lévy (verde)
  if (random(1) < 0.001) {
    targetBgColor = color(95, 235, 30);
    levyTimer = frameCount;
  }
  if (frameCount - levyTimer > 30) {
    targetBgColor = color(0);
  }

  // ---------------- Fondo amarillo loop
  if (yellowActive) {
    if (frameCount - yellowTimer > 300) { // cada 5 segundos (300 frames)
      targetBgColor = color(224, 203, 16); // amarillo
      yellowTimer = frameCount;
    }
    if (frameCount - yellowTimer > 30) {
      targetBgColor = color(0);
    }
  }

  // ---------------- Dibujar objetos
  for (let e of eggs) {
    e.follow(flowfield);
    e.run();
  }
  for (let m of mis) {
    m.follow(flowfield);
    m.run();
  }
  for (let s of stars) {
    s.follow(flowfield);
    s.run(level);
  }
  for (let x of xs) {
    x.follow(flowfield);
    x.run(level);
  }
}

function keyPressed() {
  if (key == " ") {
    yellowActive = !yellowActive; // toggle amarillo automático
    yellowTimer = frameCount;
  }
  if (key == "c") {
    stars.push(new Star(random(width), random(height), random(2, 4), random(0.05, 0.3)));
  }
  if (key == "x") {
    xs.push(new X(random(width), random(height), random(2, 4), random(0.05, 0.3)));
  }
}

function mousePressed() {
  flowfield.init();
}

// ------------------------- CLASE FLOWFIELD -----------------------------
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = width / this.resolution;
    this.rows = height / this.resolution;
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(random(10000));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }

  lookup(position) {
    let column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
    let row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

// ------------------------- CLASE EGG -----------------------------
class Egg {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(0, 0);
    this.r = 6;
    this.maxspeed = ms;
    this.maxforce = mf;
  }

  run() {
    this.update();
    this.borders();
    this.show();
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    noStroke();
    fill(255);
    ellipse(0, 0, this.r * 2, this.r * 2.6);
    pop();
  }
}

// ------------------------- CLASE MI -----------------------------
class MI extends Egg {
  show() {
    push();
    strokeWeight(2);
    // Probabilidad de color verde (como Lévy)
    if (random(1) < 0.05) {
      stroke(95, 235, 30);
    } else {
      stroke(255);
    }
    let dir = this.velocity.copy().setMag(15);
    line(this.position.x, this.position.y, this.position.x + dir.x, this.position.y + dir.y);
    pop();
  }
}

// ------------------------- CLASE STAR -----------------------------
class Star extends Egg {
  constructor(x, y, ms, mf) {
    super(x, y, ms, mf);
    this.cIndex = 0;
    this.nextIndex = 1;
    this.t = 0;
  }

  run(level) {
    this.update();
    this.borders();
    this.show(level);
  }

  show(level) {
    let c1 = palette[this.cIndex];
    let c2 = palette[this.nextIndex];
    let col = lerpColor(c1, c2, this.t);
    this.t += map(level, 0, 0.5, 0.005, 0.05);
    if (this.t >= 1) {
      this.t = 0;
      this.cIndex = this.nextIndex;
      this.nextIndex = (this.nextIndex + 1) % palette.length;
    }

    push();
    translate(this.position.x, this.position.y);
    fill(col);
    noStroke();
    beginShape();
    curveVertex(0, -this.r * 3);
    curveVertex(this.r * 2, -this.r);
    curveVertex(this.r * 3, 0);
    curveVertex(this.r * 2, this.r);
    curveVertex(0, this.r * 3);
    curveVertex(-this.r * 2, this.r);
    curveVertex(-this.r * 3, 0);
    curveVertex(-this.r * 2, -this.r);
    endShape(CLOSE);
    pop();
  }
}

// ------------------------- CLASE X -----------------------------
class X extends Star {
  constructor(x, y, ms, mf) {
    super(x, y, ms, mf);
    this.r = abs(int(randomGaussian(5, 8))); 
    if (this.r < 6) this.r = 6; 
  }

  show(level) {
    let c1 = palette[this.cIndex];
    let c2 = palette[this.nextIndex];
    let col = lerpColor(c1, c2, this.t);
    this.t += map(level, 0, 0.5, 0.005, 0.05);
    if (this.t >= 1) {
      this.t = 0;
      this.cIndex = this.nextIndex;
      this.nextIndex = (this.nextIndex + 1) % palette.length;
    }

    push();
    translate(this.position.x, this.position.y);
    stroke(col);
    strokeWeight(4);
    line(-this.r, -this.r, this.r, this.r);
    line(this.r, -this.r, -this.r, this.r);
    pop();
  }
}
```
`Versión 3`
``` js

```
`Versión 4`
``` js

```
3. Un enlace a tu sketch en el editor de p5.js.
####
[https://editor.p5js.org/catflyx/sketches/D4P5z0aOp](https://editor.p5js.org/catflyx/sketches/D4P5z0aOp)
####
4. Capturas de pantalla mostrando tu pieza en acción.
####

# Autoevaluación
**Nota:** 5
####
Realicé todas las actividades con los requisitos pedidos, y como se ve acá realicé la autoevaluación en conjunto.

