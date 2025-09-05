# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Los ángulos, amplitudes y dirección.

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> El uso de fuerzas y resistencia de fluídos.

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> La función Lerp(), marco 101, el uso de vectores en general y aceleración.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> El random(), randomGaussian() y el la forma original de mover.

## ¿Cómo resolviste la interacción?
> Hice que hubiesen dos paletas: Una fría y una cálida, y que se pinte el fondo y lo demás según la seleccionada; esto con la letra f. De igual forma, el usuario es el que agrega las "burbujas" en donde se usará la resistencia de fluídos; esto con la letra j.

## Enlace a la obra en el editor de p5.js

[[Aquí está mi obra](URL)](https://editor.p5js.org/catflyx/sketches/6OjCSGqxz)

## Código de la obra 

``` js
// ----------------- VARIABLES GLOBALES -------------------
let mover;
let oscillators = [];
let liquids = [];

// Paletas de colores
let paletteCool = [
  [50, 100, 200],
  [80, 180, 200],
  [120, 90, 255],
  [0, 200, 255],
  [180, 80, 200]
];

let paletteWarm = [
  [255, 150, 50],
  [255, 100, 0],
  [255, 50, 80],
  [255, 200, 50],
  [200, 50, 0]
];

let currentPalette;
let useWarm = false;

// Timer para cambio de color automático
let lastColorChange = 0;
let colorInterval = 200; // inter de lo que no es el fondo

// Colores de transición del fondo
let startBgColor, targetBgColor;

// Colores de transición de los elementos
let startElemColor, targetElemColor;

function setup() {
  createCanvas(640, 480);
  mover = new Mover();

  // Creamos varios osciladores
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator());
  }

  // Paleta inicial = fría
  currentPalette = paletteCool;

  // Colores iniciales del fondo (paleta contraria)
  let oppositePalette = useWarm ? paletteCool : paletteWarm;
  startBgColor = color(random(oppositePalette));
  targetBgColor = color(random(oppositePalette));

  // Colores iniciales de los elementos (paleta activa)
  startElemColor = color(random(currentPalette));
  targetElemColor = color(random(currentPalette));

  lastColorChange = millis();
}

function draw() {
  // Progreso de interpolación (0 → 1)
  let t = (millis() - lastColorChange) / colorInterval;
  t = constrain(t, 0, 1);

  // Colores interpolados
  let bgC = lerpColor(startBgColor, targetBgColor, t);
  bgC.setAlpha(5);
  background(bgC);

  let elemC = lerpColor(startElemColor, targetElemColor, t);

  // Si terminó el intervalo → elegir nuevos colores
  if (millis() - lastColorChange > colorInterval) {
    // Fondo (paleta contraria)
    let oppositePalette = useWarm ? paletteCool : paletteWarm;
    startBgColor = targetBgColor;
    targetBgColor = color(random(oppositePalette));

    // Elementos (paleta activa)
    currentPalette = useWarm ? paletteWarm : paletteCool;
    startElemColor = targetElemColor;
    targetElemColor = color(random(currentPalette));

    lastColorChange = millis();
  }

  // Mostrar líquidos (solo burbujas creadas por el usuario)
  for (let l of liquids) {
    l.show(elemC);
  }

  // Movimiento del centro (Mover)
  mover.update();
  mover.checkEdges();
  mover.show(elemC);

  // Osciladores conectados al mover
  for (let osc of oscillators) {
    osc.update();
    osc.show(mover.position, elemC);
  }
}

// ---------------- CLASE MOVER -----------------
class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 4;
  }

  update() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    dir.normalize();
    dir.mult(0.5);
    this.acceleration = dir;

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }

  show(elemC) {
    let angle = this.velocity.heading();
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);

    noStroke();
    fill(elemC);

    // Dibujar flecha como triángulo alargado sin base
    beginShape();
    vertex(-8, -6);
    vertex(-8, 6);
    vertex(25, 0);
    endShape(CLOSE);
    pop();
  }

  checkEdges() {
    if (this.position.x > width) this.position.x = 0;
    else if (this.position.x < 0) this.position.x = width;

    if (this.position.y > height) this.position.y = 0;
    else if (this.position.y < 0) this.position.y = height;
  }
}

// ---------------- CLASE OSCILLATOR -----------------
class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));
    this.amplitude = createVector(random(20, width / 4), random(20, height / 4));
  }

  update() {
    this.angle.add(this.angleVelocity);
  }

  show(center, elemC) {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(center.x, center.y);

    noStroke();
    fill(red(elemC), green(elemC), blue(elemC), 180);

    circle(x, y, 24);
    pop();
  }
}

// ---------------- CLASE LIQUID -----------------
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  show(elemC) {
    // Pequeña probabilidad de moverse aleatoriamente
    if (random(1) < 0.02) { // 2% de probabilidad cada frame
      this.x += random(randomGaussian(-2, -5), randomGaussian(2, 5));
      this.y += random(randomGaussian(-2, -5), randomGaussian(2, 5));
    }

    noStroke();
    fill(red(elemC), green(elemC), blue(elemC), 6); // Más transparente
    ellipse(this.x, this.y, this.w, this.h);
  }
}

// ---------------- INTERACTIVIDAD -----------------
  
function keyPressed() {
  if (key === 'j' || key === 'J') {
    // Crear burbujas de resistencia (líquidos pequeños circulares)
    let x = random(width);
    let y = random(height);
    let r = random(40, 100);
    liquids.push(new Liquid(x, y, r, r, 0.1));
  }

  if (key === 'f' || key === 'F') {
    useWarm = !useWarm; // Alternar entre frío y cálido
    currentPalette = useWarm ? paletteWarm : paletteCool;
  }
}
```

## Captura de pantalla representativa
<img width="718" height="540" alt="image" src="https://github.com/user-attachments/assets/3472ead1-9476-428b-b2f2-0500ff851303" />








