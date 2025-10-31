# Evidencias de la unidad 8
_______________________________________________________________________________________________________________________________________________________________________________
# Set y Seek
## Actividad 1
**Explora fragmentos de al menos 2-3 de estos enlaces:**
- Blog de Alba G. Corral: [https://blog.albagcorral.com/](https://blog.albagcorral.com/) (navega por sus proyectos).
- Sónar+D CCCB 2020: [Carles Viarnès & Alba G. Corral 360º AV Show](https://youtu.be/EMO45Y0Jazs?si=mtWqXb2IBZTwiF9K)
- Le Parody & Alba G. Corral:[ En directo en el Teatro Principal de Zaragoza](https://youtu.be/eEQPHICafbs?si=rCgcmiR0t4mJQAw7)
- Dimension N: [Alba G Corral & Makaruk - Performance at Festival des Bains Numeriques #9](https://youtu.be/r0lZ83wvgvs?si=MUkJCBFb6fm5RXzr)
####
1. Describe tus observaciones sobre la conexión sonido-imagen en al menos dos de las performances vistas.
####
- La modificación de lienzos ya "pre hechos", pero con cambios que se hacen en vivo con el programa.
- El uso de diferentes formas de noise para recorrer las formas y las ondas de sonido.
####
2. Explica qué elementos te parecieron generativos y por qué crees que cada visualización sería única.
####
Los ángulos, algunos tamaños y la dirección que tomaban las formas que se modificaban, haciendo una obra única con cada input.
####
3. Comparte tu reflexión sobre la sensación de “liveness”.
####
Es una sensación que se da por una combinación meticulosa entre el talento humano en vivo, y la capacidad de cierta forma, restringida, de la máquina para crear algo siguiendo el mismo flow del artista. Tiene su vida propia, y se modifica con un ámbito tanto predecible, pero con un resultado inesperado y que, en las manos correctas, resulta llamativo y no solo una generación de código.

## Actividad 2
1. La pieza musical elegida (con enlace/archivo si es posible).
####
Domino Effect de Forsaken: [DOMINO EFFECT | 1X1X1X1 CHASE THEME (FORSAKEN)](https://youtu.be/D9tLX2mIxkQ?si=O_iYP_cf8u1SNM96)
####
2. La descripción de tu concepto visual.
####
Quiero que de una sensación caótica y amenazadora, así como que se vea enérgitica y representativa del personaje al que pertenece la canción. De igual forma, quería separar visualmente cada capa de forma significativa.
####
3. Los inputs seleccionados y la justificación de por qué los elegiste.
####
- Ondas difuminadas para las "ondas", triangulares y que aumentan su opacidad manualmente. La opacidad es interactuable.
- "Mass infections" recorriendo la pantalla, se crean. Y costillas representadas con 3 líneas blancas. Estos usarán flowfiels y interactuan con el click.
- Se agregan "Entanglements", más rápidos que los MI. Se crean también.
- X negras lloviendo. Son partículas con vida limitada que se activan y desactivan manualmente.
- Difuminado general verde que se activa.
- Estrellas rojas que aparecen cada cierto tiempo y se van borrando gradualmente.
- Onda roja que se activa y desactiva manualmente.
####
4. ¿Qué algoritmos o técnicas planeas usar (ej: flow fields, flocking, física, partículas, etc.) y por qué? Tus bocetos y una explicación de cómo los inputs influirán en los visuales.
####
Planeo hacer uso de flowfields, partículas, ondas, entre otros. Basándome en los siguientes bocetos en orden de "capas":
<img width="498" height="374" alt="image" src="https://github.com/user-attachments/assets/610c1d8f-8df3-4d75-ba95-65c8eff932f1" />

<img width="496" height="371" alt="image" src="https://github.com/user-attachments/assets/55215ce2-2596-421d-8c56-03432402703e" />

<img width="499" height="372" alt="image" src="https://github.com/user-attachments/assets/e67d7146-1898-45d8-bbdf-0732c91ff94a" />

<img width="495" height="372" alt="image" src="https://github.com/user-attachments/assets/ed8b4146-534f-4cef-a5da-7358a4822875" />

# Apply
## Actividad 3
1. El código fuente completo de tu sketch en p5.js.
####
`Versión 1`
``` js
// ----------------- AUDIO -----------------
let song;
let amp;
let fft;

// ----------------- FLOWFIELD -----------------
let flowfield;

// ----------------- RIBS / DOMINO / ONE_X -----------------
let ribs = [];
let dominos = [];
let oneXActive = false;
let oneXIntervalId = null;
let oneXParticles = [];

// ----------------- ONDA -----------------
let waveformOpacity = 15; // Opacidad inicial (0–255)

// ----------------- EFECTOS EXTRA -----------------
let infections = []; // MASSINFECTION
let entanglements = []; // ENTANGLEMENT

// ----------------- REDWAVE -----------------
let redActive = false;
let redOpacity = 180;

function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/DE1x.ogg');
}

function setup() {
  createCanvas(800, 600);
  // dejar rastro (estela)
  background(0);

  flowfield = new FlowField(20);

  amp = new p5.Amplitude();
  fft = new p5.FFT();

  // Si el sonido no se carga por motivos de autoplay, quédalo en loop si está listoa
  song.loop();
}

function draw() {
  // fondo semitransparente para dejar estelas
  background(0);

  // ---------------- Onda puntiaguda (verde) ----------------
  let waveform = fft.waveform();
  if (!waveform || !waveform.length) waveform = new Array(1024).fill(0);

  noStroke();
  fill(60, 255, 0, waveformOpacity);

  beginShape();
  vertex(0, height);
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length - 1, 0, width);
    let sample = abs(waveform[i]);
    let y = map(sample, 0, 1, height, 0);
    let norm = map(y, 0, height, 0, 1);
    let deformFactor = pow(norm, 0.2);
    let deformY = lerp(0, y, deformFactor);
    vertex(x, deformY);
  }
  vertex(width, height);
  endShape(CLOSE);

  // ---------------- REDWAVE (solo agudos, activable con Y) ----------------
  if (redActive) {
    drawRedWave();
  }

  // ---------------- Dibujar Ribs ----------------
  for (let r of ribs) {
    r.follow(flowfield);
    r.run();
  }

  // ---------------- Dibujar MASSINFECTION ----------------
  for (let i = infections.length - 1; i >= 0; i--) {
    infections[i].update();
    infections[i].show();
    if (infections[i].isOffscreen()) infections.splice(i, 1);
  }

  // ---------------- Dibujar ENTANGLEMENT ----------------
  for (let i = entanglements.length - 1; i >= 0; i--) {
    entanglements[i].update();
    entanglements[i].show();
    if (entanglements[i].isOffscreen()) entanglements.splice(i, 1);
  }

  // ---------------- Dibujar DOMINOS ----------------
  for (let i = dominos.length - 1; i >= 0; i--) {
    dominos[i].update();
    dominos[i].show();
    if (dominos[i].isDead()) dominos.splice(i, 1);
  }

  // ---------------- Dibujar ONE_X particles ----------------
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    oneXParticles[i].update();
    oneXParticles[i].show();
    if (oneXParticles[i].isDead()) oneXParticles.splice(i, 1);
  }
}

// ----------------- Dibuja la REDWAVE usando solo agudos -----------------
function drawRedWave() {
  // obtener espectro y tomar la parte alta (agudos)
  let spectrum = fft.analyze();
  if (!spectrum || !spectrum.length) spectrum = new Array(1024).fill(0);

  // slice de agudos (ej. últimos 35% del espectro)
  let start = floor(spectrum.length * 0.65);
  let high = spectrum.slice(start);
  // reducir/normalizar a 200 muestras max
  let samples = 200;
  let step = max(1, floor(high.length / samples));
  let values = [];
  for (let i = 0; i < high.length; i += step) {
    values.push(high[i] / 255); // 0..1
  }

  noStroke();
  fill(232, 16, 16, redOpacity); // rojo
  beginShape();
  vertex(0, height);
  for (let i = 0; i < values.length; i++) {
    let x = map(i, 0, values.length - 1, 0, width);
    // valores de agudos invertidos para que 0 quede abajo
    let v = values[i];
    // efecto puntiagudo más sutil
    let y = map(v, 0, 1, height, height * 0.25);
    // acentuar picos
    let deform = lerp(height, y, pow(v, 0.8));
    vertex(x, deform);
  }
  vertex(width, height);
  endShape(CLOSE);
  
  updateOneXSystem();
}

// -------------------------------------------------
// Teclas: control de opacidad y nuevos efectos
function keyPressed() {
  if (key === 'k' || key === 'K') {
    waveformOpacity = constrain(waveformOpacity + 15, 0, 255);
  }
  if (key === 'l' || key === 'L') {
    waveformOpacity = constrain(waveformOpacity - 15, 0, 255);
  }

  // Crear MASSINFECTION
  if (key === 'm' || key === 'M') {
    for (let i = 0; i < 3; i++) {
      infections.push(new MASSINFECTION());
    }
  }

  // Crear ENTANGLEMENT
  if (key === 'n' || key === 'N') {
    for (let i = 0; i < 3; i++) {
      entanglements.push(new ENTANGLEMENT());
    }
  }

  // Crear DOMINOS
  if (key === 'd' || key === 'D') {
    for (let i = 0; i < 3; i++) {
      dominos.push(new DOMINO());
    }
  }

  // Toggle REDWAVE
  if (key === 'y' || key === 'Y') {
    redActive = !redActive;
  }

  // Toggle ONE_X (creación cada 1 segundo mientras esté activo)
  if (key === 'x' || key === 'X') {
    oneXActive = !oneXActive;
    if (oneXActive) {
      // crear inmediatamente una partícula y arrancar intervalo
      oneXParticles.push(new ONE_X());
      oneXIntervalId = setInterval(() => {
        oneXParticles.push(new ONE_X());
      }, 300);
    } else {
      // detener creación periódica
      if (oneXIntervalId !== null) {
        clearInterval(oneXIntervalId);
        oneXIntervalId = null;
      }
    }
  }

}

// -------------------------------------------------
// Click: reinicia flowfield y agrega nuevos Ribs
function mousePressed() {
  if (flowfield && typeof flowfield.init === 'function') flowfield.init();

  for (let i = 0; i < 3; i++) {
    ribs.push(new Ribs(mouseX + random(-20, 20), mouseY + random(-20, 20), random(2, 5), random(0.05, 0.5)));
  }
}

// ----------------------- CLASE FLOWFIELD --------------------------
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(floor(random(10000)));
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
    let v = this.field[column] ? this.field[column][row] : null;
    if (v && v.copy) return v.copy();
    return createVector(1, 0);
  }
}

// ----------------------- CLASE RIBS --------------------------
class Ribs {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxspeed = ms;
    this.maxforce = mf;
    this.r = random(6, 10);
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(f) {
    this.acceleration.add(f);
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

  run() {
    this.update();
    this.borders();
    this.show();
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    rectMode(CENTER);
    noStroke();
    fill(200, 200, 200, 180);
    for (let i = -1; i <= 1; i++) rect(i * 8, 0, 6, 10);
    pop();
  }
}

// ----------------------- CLASE MASSINFECTION --------------------------
class MASSINFECTION {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.speed = random(18, 20); // velocidad base aquí
    this.dir = random([1, -1]);
    this.color = color(60, 255, 0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420); // largo
    this.amplitude = random(8, 22); // pequeña amplitud para delgadez
    this.phase = random(TWO_PI);
  }

  update() {
    // ligero pulso vertical con el tiempo para 'vida'
    this.phase += 0.02;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // Dibujamos con 5 puntos (4-5 vértices), delgado
    beginShape();
    // punto inicial (muy cercano al borde)
    vertex(0 * this.dir, -this.amplitude * 0.2 + sin(this.phase) * 1);

    // segundo punto
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.6 + sin(this.phase + 0.3) * 1.2);

    // punto central alto (cresta suave)
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 1.2 + sin(this.phase + 0.6) * 1.6);

    // cuarto punto
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.5 + sin(this.phase + 0.9) * 1.2);

    // quinto punto final
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.2 + sin(this.phase + 1.2) * 1);

    // ahora la parte inferior, con 3 puntos para cerrar delgado
    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.3);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.5);
    vertex(0 * this.dir, this.amplitude * 0.2);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE ENTANGLEMENT --------------------------
class ENTANGLEMENT {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.baseSpeed = random(25, 27);
    this.speed = this.baseSpeed * 2; // doble de rápida
    this.dir = random([1, -1]);
    this.color = color(0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420);
    this.amplitude = random(6, 18);
    this.phase = random(TWO_PI);
  }

  update() {
    this.phase += 0.03;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // misma estructura delgada, menos amplitud para negro
    beginShape();
    vertex(0 * this.dir, -this.amplitude * 0.15 + sin(this.phase) * 0.8);
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.45 + sin(this.phase + 0.3) * 1.0);
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 0.9 + sin(this.phase + 0.6) * 1.4);
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.4 + sin(this.phase + 0.9) * 1.0);
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.15 + sin(this.phase + 1.2) * 0.7);

    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.25);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.45);
    vertex(0 * this.dir, this.amplitude * 0.15);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE DOMINO --------------------------
class DOMINO {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(levyStep(5), levyStep(5));
    this.lifespan = 255;
    this.fadeSpeed = random(1.5, 3);
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= this.fadeSpeed;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rectMode(CENTER);
    noStroke();
    fill(255, this.lifespan);
    rect(0, 0, 40, 80, 8);
    fill(60, 255, 0, this.lifespan);
    rect(0, -20, 40, 40, 8, 8, 0, 0);
    stroke(0, this.lifespan);
    line(-20, 0, 20, 0);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

// ------------------ PARTICULAS ONE_X ------------------
let lastOneXSpawn = 0;
let spawnInterval = 500; // cada 0.5 segundos

function updateOneXSystem() {
  // crear una nueva partícula cada 0.5s
  if (millis() - lastOneXSpawn > spawnInterval) {
    oneXParticles.push(new ONE_X());
    lastOneXSpawn = millis();
  }

  // actualizar y dibujar todas
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    let p = oneXParticles[i];
    p.update();
    p.show();
    if (p.isDead()) oneXParticles.splice(i, 1);
  }
}

// -------------- CLASE ONE_X (partículas X negras que caen) ------------------
class ONE_X {
  constructor() {
    this.pos = createVector(random(width), -10);
    this.vel = createVector(0, 2.2); // caída suave
    this.size = 20;
    this.lifespan = random(300, 500); // frames de vida
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= 1;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    textAlign(CENTER, CENTER);
    textSize(this.size);
    fill(0, map(this.lifespan, 0, 300, 0, 255));
    noStroke();
    text('X', 0, 0); // sin rotación, caen rectas
    pop();
  }

  isDead() {
    return this.lifespan <= 0 || this.pos.y > height + 20;
  }
}

// ----------------- FUNCIÓN DE SALTO DE LÉVY -----------------
function levyStep(scale = 100) {
  // distribución simple de Lévy con cola larga
  let r = random(0.0001, 1);
  let step = pow(r, -1.5) * (scale * (random() < 0.5 ? 1 : -1));
  // limitar a un paso razonable
  return constrain(step, -width * 0.6, width * 0.6);
}
```
`Versión 2`
``` js
// ----------------- AUDIO -----------------
let song;
let amp;
let fft;

// ----------------- FLOWFIELD -----------------
let flowfield;

// ----------------- RIBS / DOMINO / ONE_X -----------------
let ribs = [];
let dominos = [];
let oneXActive = false;
let oneXIntervalId = null;
let oneXParticles = [];

// ----------------- ONDA -----------------
let waveformOpacity = 15; // Opacidad inicial (0–255)

// ----------------- EFECTOS EXTRA -----------------
let infections = []; // MASSINFECTION
let entanglements = []; // ENTANGLEMENT

// ----------------- REDWAVE -----------------
let redActive = false;
let redOpacity = 180;

let hatreds = [];


function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/DE1x.ogg');
}

function setup() {
  createCanvas(800, 600);
  // dejar rastro (estela)
  background(0);

  flowfield = new FlowField(20);

  amp = new p5.Amplitude();
  fft = new p5.FFT();

  // Si el sonido no se carga por motivos de autoplay, quédalo en loop si está listoa
  song.loop();
}

function draw() {
  // fondo semitransparente para dejar estelas
  background(0);

  // HATRED
      for (let i = hatreds.length - 1; i >= 0; i--) {
    hatreds[i].update();
    hatreds[i].show();
    if (hatreds[i].isDead()) hatreds.splice(i, 1);
  }
  
  // ---------------- Onda puntiaguda (verde) ----------------
  let waveform = fft.waveform();
  if (!waveform || !waveform.length) waveform = new Array(1024).fill(0);

  noStroke();
  fill(60, 255, 0, waveformOpacity);

  beginShape();
  vertex(0, height);
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length - 1, 0, width);
    let sample = abs(waveform[i]);
    let y = map(sample, 0, 1, height, 0);
    let norm = map(y, 0, height, 0, 1);
    let deformFactor = pow(norm, 0.2);
    let deformY = lerp(0, y, deformFactor);
    vertex(x, deformY);
  }
  vertex(width, height);
  endShape(CLOSE);

  // ---------------- REDWAVE (solo agudos, activable con Y) ----------------
  if (redActive) {
    drawRedWave();
  }

  // ---------------- Dibujar Ribs ----------------
  for (let r of ribs) {
    r.follow(flowfield);
    r.run();
  }

  // ---------------- Dibujar MASSINFECTION ----------------
  for (let i = infections.length - 1; i >= 0; i--) {
    infections[i].update();
    infections[i].show();
    if (infections[i].isOffscreen()) infections.splice(i, 1);
  }

  // ---------------- Dibujar ENTANGLEMENT ----------------
  for (let i = entanglements.length - 1; i >= 0; i--) {
    entanglements[i].update();
    entanglements[i].show();
    if (entanglements[i].isOffscreen()) entanglements.splice(i, 1);
  }

  // ---------------- Dibujar DOMINOS ----------------
  for (let i = dominos.length - 1; i >= 0; i--) {
    dominos[i].update();
    dominos[i].show();
    if (dominos[i].isDead()) dominos.splice(i, 1);
  }

  // ---------------- Dibujar ONE_X particles ----------------
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    oneXParticles[i].update();
    oneXParticles[i].show();
    if (oneXParticles[i].isDead()) oneXParticles.splice(i, 1);
  }
}

// ----------------- Dibuja la REDWAVE usando solo agudos -----------------
function drawRedWave() {
  // obtener espectro y tomar la parte alta (agudos)
  let spectrum = fft.analyze();
  if (!spectrum || !spectrum.length) spectrum = new Array(1024).fill(0);

  // slice de agudos (ej. últimos 35% del espectro)
  let start = floor(spectrum.length * 0.65);
  let high = spectrum.slice(start);
  // reducir/normalizar a 200 muestras max
  let samples = 200;
  let step = max(1, floor(high.length / samples));
  let values = [];
  for (let i = 0; i < high.length; i += step) {
    values.push(high[i] / 255); // 0..1
  }

  noStroke();
  fill(232, 16, 16, redOpacity); // rojo
  beginShape();
  vertex(0, height);
  for (let i = 0; i < values.length; i++) {
    let x = map(i, 0, values.length - 1, 0, width);
    // valores de agudos invertidos para que 0 quede abajo
    let v = values[i];
    // efecto puntiagudo más sutil
    let y = map(v, 0, 1, height, height * 0.25);
    // acentuar picos
    let deform = lerp(height, y, pow(v, 0.8));
    vertex(x, deform);
  }
  vertex(width, height);
  endShape(CLOSE);
  
  updateOneXSystem();

}

// -------------------------------------------------
// Teclas: control de opacidad y nuevos efectos
function keyPressed() {
  if (key === 'k' || key === 'K') {
    waveformOpacity = constrain(waveformOpacity + 15, 0, 255);
  }
  if (key === 'l' || key === 'L') {
    waveformOpacity = constrain(waveformOpacity - 15, 0, 255);
  }

  // Crear MASSINFECTION
  if (key === 'm' || key === 'M') {
    for (let i = 0; i < 3; i++) {
      infections.push(new MASSINFECTION());
    }
  }

  // Crear ENTANGLEMENT
  if (key === 'n' || key === 'N') {
    for (let i = 0; i < 3; i++) {
      entanglements.push(new ENTANGLEMENT());
    }
  }

  // Crear DOMINOS
  if (key === 'd' || key === 'D') {
    for (let i = 0; i < 3; i++) {
      dominos.push(new DOMINO());
    }
  }

  // Toggle REDWAVE
  if (key === 'y' || key === 'Y') {
    redActive = !redActive;
  }

  // Toggle ONE_X (creación cada 1 segundo mientras esté activo)
  if (key === 'x' || key === 'X') {
    oneXActive = !oneXActive;
    if (oneXActive) {
      // crear inmediatamente una partícula y arrancar intervalo
      oneXParticles.push(new ONE_X());
      oneXIntervalId = setInterval(() => {
        oneXParticles.push(new ONE_X());
      }, 300);
    } else {
      // detener creación periódica
      if (oneXIntervalId !== null) {
        clearInterval(oneXIntervalId);
        oneXIntervalId = null;
      }
    }
  }

   if (key === 'h' || key === 'H') {
    let baseX = random(width * 0.2, width * 0.8);
    let baseY = random(height * 0.2, height * 0.8);

    // Crear 4 estrellas consecutivas
    for (let i = 0; i < 4; i++) {
      let offset = i * 25; // distancia más corta
      let delay = i * 200; // tiempo de aparición escalonado
      hatreds.push(new HATRED(baseX + offset, baseY + offset, delay));
    }
  }
  
}

// -------------------------------------------------
// Click: reinicia flowfield y agrega nuevos Ribs
function mousePressed() {
  if (flowfield && typeof flowfield.init === 'function') flowfield.init();

  for (let i = 0; i < 3; i++) {
    ribs.push(new Ribs(mouseX + random(-20, 20), mouseY + random(-20, 20), random(2, 5), random(0.05, 0.5)));
  }
}

// -------------------------- CLASE HATRED --------------------------
class HATRED {
  constructor(x, y, delay) {
    this.pos = createVector(x, y);
    this.w = 120;
    this.h = 60;
    this.color = color(232, 16, 16);
    this.birthTime = millis() + delay;
    this.fadeInTime = 400;
    this.lifespan = 2000;
    this.alpha = 0;
    this.state = "waiting";
  }

  update() {
    let now = millis();

    if (this.state === "waiting" && now >= this.birthTime) {
      this.state = "fadingIn";
      this.startFade = now;
    }

    if (this.state === "fadingIn") {
      let t = map(now - this.startFade, 0, this.fadeInTime, 0, 1, true);
      this.alpha = lerp(0, 255, t);
      if (t >= 1) {
        this.state = "fadingOut";
        this.startFade = now;
      }
    }

    if (this.state === "fadingOut") {
      let t = map(now - this.startFade, 0, this.lifespan, 0, 1, true);
      this.alpha = lerp(255, 0, t);
    }
  }

  show() {
    if (this.state === "waiting") return;

    push();
    translate(this.pos.x, this.pos.y);
    fill(red(this.color), green(this.color), blue(this.color), this.alpha);

    // Dibujo: diamante acostado (más ancho que alto)
    beginShape();
    vertex(-this.w / 2, 0);
    vertex(0, -this.h / 2);
    vertex(this.w / 2, 0);
    vertex(0, this.h / 2);
    endShape(CLOSE);
    pop();
  }

  isDead() {
    return this.state === "fadingOut" && this.alpha <= 1;
  }
}

// ----------------------- CLASE FLOWFIELD --------------------------
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(floor(random(10000)));
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
    let v = this.field[column] ? this.field[column][row] : null;
    if (v && v.copy) return v.copy();
    return createVector(1, 0);
  }
}

// ----------------------- CLASE RIBS --------------------------
class Ribs {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxspeed = ms;
    this.maxforce = mf;
    this.r = random(6, 10);
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(f) {
    this.acceleration.add(f);
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

  run() {
    this.update();
    this.borders();
    this.show();
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    rectMode(CENTER);
    noStroke();
    fill(200, 200, 200, 180);
    for (let i = -1; i <= 1; i++) rect(i * 8, 0, 6, 10);
    pop();
  }
}

// ----------------------- CLASE MASSINFECTION --------------------------
class MASSINFECTION {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.speed = random(18, 20); // velocidad base aquí
    this.dir = random([1, -1]);
    this.color = color(60, 255, 0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420); // largo
    this.amplitude = random(8, 22); // pequeña amplitud para delgadez
    this.phase = random(TWO_PI);
  }

  update() {
    // ligero pulso vertical con el tiempo para 'vida'
    this.phase += 0.02;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // Dibujamos con 5 puntos (4-5 vértices), delgado
    beginShape();
    // punto inicial (muy cercano al borde)
    vertex(0 * this.dir, -this.amplitude * 0.2 + sin(this.phase) * 1);

    // segundo punto
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.6 + sin(this.phase + 0.3) * 1.2);

    // punto central alto (cresta suave)
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 1.2 + sin(this.phase + 0.6) * 1.6);

    // cuarto punto
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.5 + sin(this.phase + 0.9) * 1.2);

    // quinto punto final
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.2 + sin(this.phase + 1.2) * 1);

    // ahora la parte inferior, con 3 puntos para cerrar delgado
    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.3);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.5);
    vertex(0 * this.dir, this.amplitude * 0.2);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE ENTANGLEMENT --------------------------
class ENTANGLEMENT {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.baseSpeed = random(25, 27);
    this.speed = this.baseSpeed * 2; // doble de rápida
    this.dir = random([1, -1]);
    this.color = color(0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420);
    this.amplitude = random(6, 18);
    this.phase = random(TWO_PI);
  }

  update() {
    this.phase += 0.03;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // misma estructura delgada, menos amplitud para negro
    beginShape();
    vertex(0 * this.dir, -this.amplitude * 0.15 + sin(this.phase) * 0.8);
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.45 + sin(this.phase + 0.3) * 1.0);
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 0.9 + sin(this.phase + 0.6) * 1.4);
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.4 + sin(this.phase + 0.9) * 1.0);
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.15 + sin(this.phase + 1.2) * 0.7);

    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.25);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.45);
    vertex(0 * this.dir, this.amplitude * 0.15);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE DOMINO --------------------------
class DOMINO {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(levyStep(5), levyStep(5));
    this.lifespan = 255;
    this.fadeSpeed = random(1.5, 3);
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= this.fadeSpeed;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rectMode(CENTER);
    noStroke();
    fill(255, this.lifespan);
    rect(0, 0, 40, 80, 8);
    fill(60, 255, 0, this.lifespan);
    rect(0, -20, 40, 40, 8, 8, 0, 0);
    stroke(0, this.lifespan);
    line(-20, 0, 20, 0);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

// ------------------ PARTICULAS ONE_X ------------------
let lastOneXSpawn = 0;
let spawnInterval = 500; // cada 0.5 segundos

function updateOneXSystem() {
  // crear una nueva partícula cada 0.5s
  if (millis() - lastOneXSpawn > spawnInterval) {
    oneXParticles.push(new ONE_X());
    lastOneXSpawn = millis();
  }

  // actualizar y dibujar todas
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    let p = oneXParticles[i];
    p.update();
    p.show();
    if (p.isDead()) oneXParticles.splice(i, 1);
  }
}

// -------------- CLASE ONE_X (partículas X negras que caen) ------------------
class ONE_X {
  constructor() {
    this.pos = createVector(random(width), -10);
    this.vel = createVector(0, 2.2); // caída suave
    this.size = 20;
    this.lifespan = random(300, 500); // frames de vida
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= 1;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    textAlign(CENTER, CENTER);
    textSize(this.size);
    fill(0, map(this.lifespan, 0, 300, 0, 255));
    noStroke();
    text('X', 0, 0); // sin rotación, caen rectas
    pop();
  }

  isDead() {
    return this.lifespan <= 0 || this.pos.y > height + 20;
  }
}

// ----------------- FUNCIÓN DE SALTO DE LÉVY -----------------
function levyStep(scale = 100) {
  // distribución simple de Lévy con cola larga
  let r = random(0.0001, 1);
  let step = pow(r, -1.5) * (scale * (random() < 0.5 ? 1 : -1));
  // limitar a un paso razonable
  return constrain(step, -width * 0.6, width * 0.6);
}
```
`Versión 3`
``` js
// ----------------- AUDIO -----------------
let song;
let amp;
let fft;

// ----------------- FLOWFIELD -----------------
let flowfield;

// ----------------- RIBS / DOMINO / ONE_X -----------------
let ribs = [];
let dominos = [];
let oneXActive = false;
let oneXIntervalId = null;
let oneXParticles = [];

// ----------------- ONDA -----------------
let waveformOpacity = 15; // Opacidad inicial (0–255)

// ----------------- EFECTOS EXTRA -----------------
let infections = []; // MASSINFECTION
let entanglements = []; // ENTANGLEMENT

// ----------------- REDWAVE -----------------
let redActive = false;
let redOpacity = 180;

let hatreds = [];

let greenFadeActive = false;
let greenFadeOpacity = 0;


function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/DE1x.ogg');
}

function setup() {
  createCanvas(800, 600);
  // dejar rastro (estela)
  background(0);

  flowfield = new FlowField(20);

  amp = new p5.Amplitude();
  fft = new p5.FFT();

  // Si el sonido no se carga por motivos de autoplay, quédalo en loop si está listoa
  song.loop();
}

function draw() {
  // fondo semitransparente para dejar estelas
  background(0);

  // HATRED
      for (let i = hatreds.length - 1; i >= 0; i--) {
    hatreds[i].update();
    hatreds[i].show();
    if (hatreds[i].isDead()) hatreds.splice(i, 1);
  }
  
  updateOneXSystem();
  
  // ---------------- Onda puntiaguda (verde) ----------------
  let waveform = fft.waveform();
  if (!waveform || !waveform.length) waveform = new Array(1024).fill(0);

  noStroke();
  fill(60, 255, 0, waveformOpacity);

  beginShape();
  vertex(0, height);
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length - 1, 0, width);
    let sample = abs(waveform[i]);
    let y = map(sample, 0, 1, height, 0);
    let norm = map(y, 0, height, 0, 1);
    let deformFactor = pow(norm, 0.2);
    let deformY = lerp(0, y, deformFactor);
    vertex(x, deformY);
  }
  vertex(width, height);
  endShape(CLOSE);

  // ---------------- REDWAVE (solo agudos, activable con Y) ----------------
  if (redActive) {
    drawRedWave();
  }

  // ---------------- Dibujar Ribs ----------------
  for (let r of ribs) {
    r.follow(flowfield);
    r.run();
  }

  // ---------------- Dibujar MASSINFECTION ----------------
  for (let i = infections.length - 1; i >= 0; i--) {
    infections[i].update();
    infections[i].show();
    if (infections[i].isOffscreen()) infections.splice(i, 1);
  }

  // ---------------- Dibujar ENTANGLEMENT ----------------
  for (let i = entanglements.length - 1; i >= 0; i--) {
    entanglements[i].update();
    entanglements[i].show();
    if (entanglements[i].isOffscreen()) entanglements.splice(i, 1);
  }

  // ---------------- Dibujar DOMINOS ----------------
  for (let i = dominos.length - 1; i >= 0; i--) {
    dominos[i].update();
    dominos[i].show();
    if (dominos[i].isDead()) dominos.splice(i, 1);
  }

  // ---------------- Dibujar ONE_X particles ----------------
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    oneXParticles[i].update();
    oneXParticles[i].show();
    if (oneXParticles[i].isDead()) oneXParticles.splice(i, 1);
  }
  
  // ----------------- DIFUMINADO VERDE -----------------
if (greenFadeActive) {
  // Aumentar opacidad suavemente hasta un máximo
  greenFadeOpacity = lerp(greenFadeOpacity, 180, 0.02);

  noStroke();
  for (let y = height; y > 0; y -= 2) {
    let alpha = map(y, 0, height, 0, greenFadeOpacity);
    fill(60, 255, 0, alpha * 0.3); // verde suave translúcido
    rect(0, y, width, 2);
  }
}
}

// ----------------- Dibuja la REDWAVE usando solo agudos -----------------
// ----------------- Dibuja la REDWAVE usando solo GRAVES -----------------
function drawRedWave() {
  // Obtener espectro y tomar la parte baja (graves)
  let spectrum = fft.analyze();
  if (!spectrum || !spectrum.length) spectrum = new Array(1024).fill(0);

  // Slice de graves (por ejemplo, primeros 35% del espectro)
  let end = floor(spectrum.length * 0.35);
  let low = spectrum.slice(0, end);

  // Reducir/normalizar a 200 muestras máximo
  let samples = 200;
  let step = max(1, floor(low.length / samples));
  let values = [];
  for (let i = 0; i < low.length; i += step) {
    values.push(low[i] / 255); // Normaliza a 0..1
  }

  // ---- Aquí continúa tu parte de dibujo de la onda ----
  noFill();
  stroke(232, 16, 16);
  strokeWeight(2);

  beginShape();
  for (let i = 0; i < values.length; i++) {
    let x = map(i, 0, values.length - 1, 0, width);
    let y = height / 2 - values[i] * 200; // amplitud proporcional a graves
    vertex(x, y);
  }
  endShape();
}

// -------------------------------------------------
// Teclas: control de opacidad y nuevos efectos
function keyPressed() {
  if (key === 'k' || key === 'K') {
    waveformOpacity = constrain(waveformOpacity + 15, 0, 255);
  }
  if (key === 'l' || key === 'L') {
    waveformOpacity = constrain(waveformOpacity - 15, 0, 255);
  }

  // Crear MASSINFECTION
  if (key === 'm' || key === 'M') {
    for (let i = 0; i < 3; i++) {
      infections.push(new MASSINFECTION());
    }
  }

  // Crear ENTANGLEMENT
  if (key === 'n' || key === 'N') {
    for (let i = 0; i < 3; i++) {
      entanglements.push(new ENTANGLEMENT());
    }
  }

  // Crear DOMINOS
  if (key === 'd' || key === 'D') {
    for (let i = 0; i < 3; i++) {
      dominos.push(new DOMINO());
    }
  }

  // Toggle REDWAVE
  if (key === 'y' || key === 'Y') {
    redActive = !redActive;
  }

  // Toggle ONE_X (creación cada 1 segundo mientras esté activo)
  if (key === 'x' || key === 'X') {
    oneXActive = !oneXActive;
    if (oneXActive) {
      // crear inmediatamente una partícula y arrancar intervalo
      oneXParticles.push(new ONE_X());
      oneXIntervalId = setInterval(() => {
        oneXParticles.push(new ONE_X());
      }, 300);
    } else {
      // detener creación periódica
      if (oneXIntervalId !== null) {
        clearInterval(oneXIntervalId);
        oneXIntervalId = null;
      }
    }
  }

   if (key === 'h' || key === 'H') {
    let baseX = random(width * 0.2, width * 0.8);
    let baseY = random(height * 0.2, height * 0.8);

    // Crear 4 estrellas consecutivas
    for (let i = 0; i < 4; i++) {
      let offset = i * 25; // distancia más corta
      let delay = i * 200; // tiempo de aparición escalonado
      hatreds.push(new HATRED(baseX + offset, baseY + offset, delay));
    }
  }
  
  if (key === 'g' || key === 'G') {
  greenFadeActive = true;
  greenFadeOpacity = 0; // reinicia al presionar
}

}

// -------------------------------------------------
// Click: reinicia flowfield y agrega nuevos Ribs
function mousePressed() {
  if (flowfield && typeof flowfield.init === 'function') flowfield.init();

  for (let i = 0; i < 3; i++) {
    ribs.push(new Ribs(mouseX + random(-20, 20), mouseY + random(-20, 20), random(2, 5), random(0.05, 0.5)));
  }
}

// -------------------------- CLASE HATRED --------------------------
class HATRED {
  constructor(x, y, delay) {
    this.pos = createVector(x, y);
    this.w = 120;
    this.h = 60;
    this.color = color(232, 16, 16);
    this.birthTime = millis() + delay;
    this.fadeInTime = 400;
    this.lifespan = 2000;
    this.alpha = 0;
    this.state = "waiting";
  }

  update() {
    let now = millis();

    if (this.state === "waiting" && now >= this.birthTime) {
      this.state = "fadingIn";
      this.startFade = now;
    }

    if (this.state === "fadingIn") {
      let t = map(now - this.startFade, 0, this.fadeInTime, 0, 1, true);
      this.alpha = lerp(0, 255, t);
      if (t >= 1) {
        this.state = "fadingOut";
        this.startFade = now;
      }
    }

    if (this.state === "fadingOut") {
      let t = map(now - this.startFade, 0, this.lifespan, 0, 1, true);
      this.alpha = lerp(255, 0, t);
    }
  }

  show() {
    if (this.state === "waiting") return;

    push();
    translate(this.pos.x, this.pos.y);
    fill(red(this.color), green(this.color), blue(this.color), this.alpha);

    // Dibujo: diamante acostado (más ancho que alto)
    beginShape();
    vertex(-this.w / 2, 0);
    vertex(0, -this.h / 2);
    vertex(this.w / 2, 0);
    vertex(0, this.h / 2);
    endShape(CLOSE);
    pop();
  }

  isDead() {
    return this.state === "fadingOut" && this.alpha <= 1;
  }
}

// ----------------------- CLASE FLOWFIELD --------------------------
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(floor(random(10000)));
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
    let v = this.field[column] ? this.field[column][row] : null;
    if (v && v.copy) return v.copy();
    return createVector(1, 0);
  }
}

// ----------------------- CLASE RIBS --------------------------
class Ribs {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxspeed = ms;
    this.maxforce = mf;
    this.r = random(6, 10);
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(f) {
    this.acceleration.add(f);
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

  run() {
    this.update();
    this.borders();
    this.show();
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    rectMode(CENTER);
    noStroke();
    fill(200, 200, 200, 180);
    for (let i = -1; i <= 1; i++) rect(i * 8, 0, 6, 10);
    pop();
  }
}

// ----------------------- CLASE MASSINFECTION --------------------------
class MASSINFECTION {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.speed = random(18, 20); // velocidad base aquí
    this.dir = random([1, -1]);
    this.color = color(60, 255, 0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420); // largo
    this.amplitude = random(8, 22); // pequeña amplitud para delgadez
    this.phase = random(TWO_PI);
  }

  update() {
    // ligero pulso vertical con el tiempo para 'vida'
    this.phase += 0.02;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // Dibujamos con 5 puntos (4-5 vértices), delgado
    beginShape();
    // punto inicial (muy cercano al borde)
    vertex(0 * this.dir, -this.amplitude * 0.2 + sin(this.phase) * 1);

    // segundo punto
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.6 + sin(this.phase + 0.3) * 1.2);

    // punto central alto (cresta suave)
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 1.2 + sin(this.phase + 0.6) * 1.6);

    // cuarto punto
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.5 + sin(this.phase + 0.9) * 1.2);

    // quinto punto final
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.2 + sin(this.phase + 1.2) * 1);

    // ahora la parte inferior, con 3 puntos para cerrar delgado
    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.3);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.5);
    vertex(0 * this.dir, this.amplitude * 0.2);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE ENTANGLEMENT --------------------------
class ENTANGLEMENT {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.baseSpeed = random(25, 27);
    this.speed = this.baseSpeed * 2; // doble de rápida
    this.dir = random([1, -1]);
    this.color = color(0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420);
    this.amplitude = random(6, 18);
    this.phase = random(TWO_PI);
  }

  update() {
    this.phase += 0.03;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // misma estructura delgada, menos amplitud para negro
    beginShape();
    vertex(0 * this.dir, -this.amplitude * 0.15 + sin(this.phase) * 0.8);
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.45 + sin(this.phase + 0.3) * 1.0);
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 0.9 + sin(this.phase + 0.6) * 1.4);
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.4 + sin(this.phase + 0.9) * 1.0);
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.15 + sin(this.phase + 1.2) * 0.7);

    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.25);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.45);
    vertex(0 * this.dir, this.amplitude * 0.15);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE DOMINO --------------------------
class DOMINO {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(levyStep(5), levyStep(5));
    this.lifespan = 255;
    this.fadeSpeed = random(1.5, 3);
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= this.fadeSpeed;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rectMode(CENTER);
    noStroke();
    fill(255, this.lifespan);
    rect(0, 0, 40, 80, 8);
    fill(60, 255, 0, this.lifespan);
    rect(0, -20, 40, 40, 8, 8, 0, 0);
    stroke(0, this.lifespan);
    line(-20, 0, 20, 0);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

// ------------------ PARTICULAS ONE_X ------------------
let lastOneXSpawn = 0;
let spawnInterval = 500; // cada 0.5 segundos

function updateOneXSystem() {
  // crear una nueva partícula cada 0.5s
  if (millis() - lastOneXSpawn > spawnInterval) {
    oneXParticles.push(new ONE_X());
    lastOneXSpawn = millis();
  }

  // actualizar y dibujar todas
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    let p = oneXParticles[i];
    p.update();
    p.show();
    if (p.isDead()) oneXParticles.splice(i, 1);
  }
}

// -------------- CLASE ONE_X (partículas X negras que caen) ------------------
class ONE_X {
  constructor() {
    this.pos = createVector(random(width), -10);
    this.vel = createVector(0, 2.2); // caída suave
    this.size = 20;
    this.lifespan = random(300, 500); // frames de vida
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= 1;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    textAlign(CENTER, CENTER);
    textSize(this.size);
    fill(0, map(this.lifespan, 0, 300, 0, 255));
    noStroke();
    text('X', 0, 0); // sin rotación, caen rectas
    pop();
  }

  isDead() {
    return this.lifespan <= 0 || this.pos.y > height + 20;
  }
}

// ----------------- FUNCIÓN DE SALTO DE LÉVY -----------------
function levyStep(scale = 100) {
  // distribución simple de Lévy con cola larga
  let r = random(0.0001, 1);
  let step = pow(r, -1.5) * (scale * (random() < 0.5 ? 1 : -1));
  // limitar a un paso razonable
  return constrain(step, -width * 0.6, width * 0.6);
}
```
`Versión 4`
``` js
// ----------------- AUDIO -----------------
let song;
let amp;
let fft;

// ----------------- FLOWFIELD -----------------
let flowfield;

// ----------------- RIBS / DOMINO / ONE_X -----------------
let ribs = [];
let dominos = [];
let oneXActive = false;
let oneXIntervalId = null;
let oneXParticles = [];

// ----------------- ONDA -----------------
let waveformOpacity = 15; // Opacidad inicial (0–255)

// ----------------- EFECTOS EXTRA -----------------
let infections = []; // MASSINFECTION
let entanglements = []; // ENTANGLEMENT

// ----------------- REDWAVE -----------------
let redActive = false;
let redOpacity = 180;

let hatreds = [];

let greenFadeActive = false;
let greenFadeOpacity = 0;


function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound('assets/DE1x.ogg');
}

function setup() {
  createCanvas(800, 600);
  // dejar rastro (estela)
  background(0);

  flowfield = new FlowField(20);

  amp = new p5.Amplitude();
  fft = new p5.FFT();

  // Si el sonido no se carga por motivos de autoplay, quédalo en loop si está listoa
  song.loop();
}

function draw() {
  // fondo semitransparente para dejar estelas
  background(0);

  // HATRED
      for (let i = hatreds.length - 1; i >= 0; i--) {
    hatreds[i].update();
    hatreds[i].show();
    if (hatreds[i].isDead()) hatreds.splice(i, 1);
  }
  
  updateOneXSystem();
  
  // ---------------- Onda puntiaguda (verde) ----------------
  let waveform = fft.waveform();
  if (!waveform || !waveform.length) waveform = new Array(1024).fill(0);

  noStroke();
  fill(60, 255, 0, waveformOpacity);

  beginShape();
  vertex(0, height);
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length - 1, 0, width);
    let sample = abs(waveform[i]);
    let y = map(sample, 0, 1, height, 0);
    let norm = map(y, 0, height, 0, 1);
    let deformFactor = pow(norm, 0.2);
    let deformY = lerp(0, y, deformFactor);
    vertex(x, deformY);
  }
  vertex(width, height);
  endShape(CLOSE);

  // ---------------- REDWAVE (solo agudos, activable con Y) ----------------
  if (redActive) {
    drawRedWave();
  }

  // ---------------- Dibujar Ribs ----------------
  for (let r of ribs) {
    r.follow(flowfield);
    r.run();
  }

  // ---------------- Dibujar MASSINFECTION ----------------
  for (let i = infections.length - 1; i >= 0; i--) {
    infections[i].update();
    infections[i].show();
    if (infections[i].isOffscreen()) infections.splice(i, 1);
  }

  // ---------------- Dibujar ENTANGLEMENT ----------------
  for (let i = entanglements.length - 1; i >= 0; i--) {
    entanglements[i].update();
    entanglements[i].show();
    if (entanglements[i].isOffscreen()) entanglements.splice(i, 1);
  }

  // ---------------- Dibujar DOMINOS ----------------
  for (let i = dominos.length - 1; i >= 0; i--) {
    dominos[i].update();
    dominos[i].show();
    if (dominos[i].isDead()) dominos.splice(i, 1);
  }

  // ---------------- Dibujar ONE_X particles ----------------
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    oneXParticles[i].update();
    oneXParticles[i].show();
    if (oneXParticles[i].isDead()) oneXParticles.splice(i, 1);
  }
  
  // ----------------- DIFUMINADO VERDE -----------------
if (greenFadeActive) {
  // Aumentar opacidad suavemente hasta un máximo
  greenFadeOpacity = lerp(greenFadeOpacity, 180, 0.02);

  noStroke();
  for (let y = height; y > 0; y -= 2) {
    let alpha = map(y, 0, height, 0, greenFadeOpacity);
    fill(60, 255, 0, alpha * 0.3); // verde suave translúcido
    rect(0, y, width, 2);
  }
}
}

// ----------------- Dibuja la REDWAVE usando solo agudos -----------------
// ----------------- Dibuja la REDWAVE usando solo GRAVES -----------------
function drawRedWave() {
  // Obtener espectro y tomar la parte baja (graves)
  let spectrum = fft.analyze();
  if (!spectrum || !spectrum.length) spectrum = new Array(1024).fill(0);

  // Slice de graves (por ejemplo, primeros 35% del espectro)
  let end = floor(spectrum.length * 0.35);
  let low = spectrum.slice(0, end);

  // Reducir/normalizar a 200 muestras máximo
  let samples = 200;
  let step = max(1, floor(low.length / samples));
  let values = [];
  for (let i = 0; i < low.length; i += step) {
    values.push(low[i] / 255); // Normaliza a 0..1
  }

  // ---- Aquí continúa tu parte de dibujo de la onda ----
  noFill();
  stroke(232, 16, 16);
  strokeWeight(2);

  beginShape();
  for (let i = 0; i < values.length; i++) {
    let x = map(i, 0, values.length - 1, 0, width);
    let y = height / 2 - values[i] * 200; // amplitud proporcional a graves
    vertex(x, y);
  }
  endShape();
}

// -------------------------------------------------
// Teclas: control de opacidad y nuevos efectos
function keyPressed() {
  if (key === 'k' || key === 'K') {
    waveformOpacity = constrain(waveformOpacity + 15, 0, 255);
  }
  if (key === 'l' || key === 'L') {
    waveformOpacity = constrain(waveformOpacity - 15, 0, 255);
  }

  // Crear MASSINFECTION
  if (key === 'm' || key === 'M') {
    for (let i = 0; i < 3; i++) {
      infections.push(new MASSINFECTION());
    }
  }

  // Crear ENTANGLEMENT
  if (key === 'n' || key === 'N') {
    for (let i = 0; i < 3; i++) {
      entanglements.push(new ENTANGLEMENT());
    }
  }

  // Crear DOMINOS
  if (key === 'd' || key === 'D') {
    for (let i = 0; i < 3; i++) {
      dominos.push(new DOMINO());
    }
  }

  // Toggle REDWAVE
  if (key === 'y' || key === 'Y') {
    redActive = !redActive;
  }

  // Toggle ONE_X (creación cada 1 segundo mientras esté activo)
  if (key === 'x' || key === 'X') {
    oneXActive = !oneXActive;
    if (oneXActive) {
      // crear inmediatamente una partícula y arrancar intervalo
      oneXParticles.push(new ONE_X());
      oneXIntervalId = setInterval(() => {
        oneXParticles.push(new ONE_X());
      }, 300);
    } else {
      // detener creación periódica
      if (oneXIntervalId !== null) {
        clearInterval(oneXIntervalId);
        oneXIntervalId = null;
      }
    }
  }

   if (key === 'h' || key === 'H') {
    let baseX = random(width * 0.2, width * 0.8);
    let baseY = random(height * 0.2, height * 0.8);

    // Crear 4 estrellas consecutivas
    for (let i = 0; i < 4; i++) {
      let offset = i * 25; // distancia más corta
      let delay = i * 200; // tiempo de aparición escalonado
      hatreds.push(new HATRED(baseX + offset, baseY + offset, delay));
    }
  }
  
  if (key === 'g' || key === 'G') {
  greenFadeActive = true;
  greenFadeOpacity = 0; // reinicia al presionar
}

}

// -------------------------------------------------
// Click: reinicia flowfield y agrega nuevos Ribs
function mousePressed() {
  if (flowfield && typeof flowfield.init === 'function') flowfield.init();

  for (let i = 0; i < 3; i++) {
    ribs.push(new Ribs(mouseX + random(-20, 20), mouseY + random(-20, 20), random(2, 5), random(0.05, 0.5)));
  }
}

// -------------------------- CLASE HATRED --------------------------
class HATRED {
  constructor(x, y, delay) {
    this.pos = createVector(x, y);
    this.w = 120;
    this.h = 60;
    this.color = color(232, 16, 16);
    this.birthTime = millis() + delay;
    this.fadeInTime = 400;
    this.lifespan = 2000;
    this.alpha = 0;
    this.state = "waiting";
  }

  update() {
    let now = millis();

    if (this.state === "waiting" && now >= this.birthTime) {
      this.state = "fadingIn";
      this.startFade = now;
    }

    if (this.state === "fadingIn") {
      let t = map(now - this.startFade, 0, this.fadeInTime, 0, 1, true);
      this.alpha = lerp(0, 255, t);
      if (t >= 1) {
        this.state = "fadingOut";
        this.startFade = now;
      }
    }

    if (this.state === "fadingOut") {
      let t = map(now - this.startFade, 0, this.lifespan, 0, 1, true);
      this.alpha = lerp(255, 0, t);
    }
  }

  show() {
    if (this.state === "waiting") return;

    push();
    translate(this.pos.x, this.pos.y);
    fill(red(this.color), green(this.color), blue(this.color), this.alpha);

    // Dibujo: diamante acostado (más ancho que alto)
    beginShape();
    vertex(-this.w / 2, 0);
    vertex(0, -this.h / 2);
    vertex(this.w / 2, 0);
    vertex(0, this.h / 2);
    endShape(CLOSE);
    pop();
  }

  isDead() {
    return this.state === "fadingOut" && this.alpha <= 1;
  }
}

// ----------------------- CLASE FLOWFIELD --------------------------
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

  init() {
    noiseSeed(floor(random(10000)));
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
    let v = this.field[column] ? this.field[column][row] : null;
    if (v && v.copy) return v.copy();
    return createVector(1, 0);
  }
}

// ----------------------- CLASE RIBS --------------------------
class Ribs {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxspeed = ms;
    this.maxforce = mf;
    this.r = random(6, 10);
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(f) {
    this.acceleration.add(f);
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

  run() {
    this.update();
    this.borders();
    this.show();
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    rectMode(CENTER);
    noStroke();
    fill(200, 200, 200, 180);
    for (let i = -1; i <= 1; i++) rect(i * 8, 0, 6, 10);
    pop();
  }
}

// ----------------------- CLASE MASSINFECTION --------------------------
class MASSINFECTION {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.speed = random(18, 20); // velocidad base aquí
    this.dir = random([1, -1]);
    this.color = color(60, 255, 0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420); // largo
    this.amplitude = random(8, 22); // pequeña amplitud para delgadez
    this.phase = random(TWO_PI);
  }

  update() {
    // ligero pulso vertical con el tiempo para 'vida'
    this.phase += 0.02;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // Dibujamos con 5 puntos (4-5 vértices), delgado
    beginShape();
    // punto inicial (muy cercano al borde)
    vertex(0 * this.dir, -this.amplitude * 0.2 + sin(this.phase) * 1);

    // segundo punto
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.6 + sin(this.phase + 0.3) * 1.2);

    // punto central alto (cresta suave)
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 1.2 + sin(this.phase + 0.6) * 1.6);

    // cuarto punto
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.5 + sin(this.phase + 0.9) * 1.2);

    // quinto punto final
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.2 + sin(this.phase + 1.2) * 1);

    // ahora la parte inferior, con 3 puntos para cerrar delgado
    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.3);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.5);
    vertex(0 * this.dir, this.amplitude * 0.2);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE ENTANGLEMENT --------------------------
class ENTANGLEMENT {
  constructor() {
    this.y = random(height * 0.2, height * 0.8);
    this.baseSpeed = random(25, 27);
    this.speed = this.baseSpeed * 2; // doble de rápida
    this.dir = random([1, -1]);
    this.color = color(0);
    this.x = this.dir === 1 ? -width * 0.3 : width * 1.3;
    this.size = random(250, 420);
    this.amplitude = random(6, 18);
    this.phase = random(TWO_PI);
  }

  update() {
    this.phase += 0.03;
    this.x += this.speed * this.dir;
  }

  show() {
    push();
    translate(this.x, this.y);
    fill(this.color);
    noStroke();

    // misma estructura delgada, menos amplitud para negro
    beginShape();
    vertex(0 * this.dir, -this.amplitude * 0.15 + sin(this.phase) * 0.8);
    vertex(this.size * 0.25 * this.dir, -this.amplitude * 0.45 + sin(this.phase + 0.3) * 1.0);
    vertex(this.size * 0.5 * this.dir, -this.amplitude * 0.9 + sin(this.phase + 0.6) * 1.4);
    vertex(this.size * 0.75 * this.dir, -this.amplitude * 0.4 + sin(this.phase + 0.9) * 1.0);
    vertex(this.size * 1.0 * this.dir, -this.amplitude * 0.15 + sin(this.phase + 1.2) * 0.7);

    vertex(this.size * 1.0 * this.dir, this.amplitude * 0.25);
    vertex(this.size * 0.5 * this.dir, this.amplitude * 0.45);
    vertex(0 * this.dir, this.amplitude * 0.15);
    endShape(CLOSE);

    pop();
  }

  isOffscreen() {
    return (this.dir === 1 && this.x > width + this.size) ||
           (this.dir === -1 && this.x < -this.size);
  }
}

// ----------------------- CLASE DOMINO --------------------------
class DOMINO {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(levyStep(5), levyStep(5));
    this.lifespan = 255;
    this.fadeSpeed = random(1.5, 3);
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= this.fadeSpeed;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rectMode(CENTER);
    noStroke();
    fill(255, this.lifespan);
    rect(0, 0, 40, 80, 8);
    fill(60, 255, 0, this.lifespan);
    rect(0, -20, 40, 40, 8, 8, 0, 0);
    stroke(0, this.lifespan);
    line(-20, 0, 20, 0);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

// ------------------ PARTICULAS ONE_X ------------------
let lastOneXSpawn = 0;
let spawnInterval = 500; // cada 0.5 segundos

function updateOneXSystem() {
  // crear una nueva partícula cada 0.5s
  if (millis() - lastOneXSpawn > spawnInterval) {
    oneXParticles.push(new ONE_X());
    lastOneXSpawn = millis();
  }

  // actualizar y dibujar todas
  for (let i = oneXParticles.length - 1; i >= 0; i--) {
    let p = oneXParticles[i];
    p.update();
    p.show();
    if (p.isDead()) oneXParticles.splice(i, 1);
  }
}

// -------------- CLASE ONE_X (partículas X negras que caen) ------------------
class ONE_X {
  constructor() {
    this.pos = createVector(random(width), -10);
    this.vel = createVector(0, 2.2); // caída suave
    this.size = 20;
    this.lifespan = random(300, 500); // frames de vida
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= 1;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    textAlign(CENTER, CENTER);
    textSize(this.size);
    fill(0, map(this.lifespan, 0, 300, 0, 255));
    noStroke();
    text('X', 0, 0); // sin rotación, caen rectas
    pop();
  }

  isDead() {
    return this.lifespan <= 0 || this.pos.y > height + 20;
  }
}

// ----------------- FUNCIÓN DE SALTO DE LÉVY -----------------
function levyStep(scale = 100) {
  // distribución simple de Lévy con cola larga
  let r = random(0.0001, 1);
  let step = pow(r, -1.5) * (scale * (random() < 0.5 ? 1 : -1));
  // limitar a un paso razonable
  return constrain(step, -width * 0.6, width * 0.6);
}
```
####
2. Un enlace a tu sketch en el editor de p5.js.
####
[https://editor.p5js.org/catflyx/sketches/5AMlGGjwO](https://editor.p5js.org/catflyx/sketches/5AMlGGjwO)
####
3. Capturas de pantalla mostrando tu pieza en acción.
####
<img width="849" height="638" alt="image" src="https://github.com/user-attachments/assets/501e5f5b-e9b2-49db-9dd8-20b3d5c28136" />

<img width="849" height="636" alt="image" src="https://github.com/user-attachments/assets/5b49e7c9-e306-448a-9b52-f8843d78d0c6" />

<img width="842" height="634" alt="image" src="https://github.com/user-attachments/assets/ca5bd77c-8366-4ecc-b5ec-d7e73de9034c" />

# Autoevaluación
**Nota:** 5
####
Completé las actividades con lo que se pedía, y satisfací las necesidades de cada una.




