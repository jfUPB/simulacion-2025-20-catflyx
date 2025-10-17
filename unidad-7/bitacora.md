# Evidencias de la unidad 7
_______________________________________________________________________________________________________________________________________________________________________________
# Set y Seek
## Actividad 1
Vamos a explorar cómo la forma visual de las letras y palabras puede comunicar su significado intrínseco, inspirándonos en el trabajo del diseñador Ji Lee. Concepto “Word as Image” de Ji Lee: observa el trabajo de Ji Lee [“Word as Image”](https://pleaseenjoy.com/#/word-as-image/). Analiza cómo manipula la tipografía para ilustrar el significado de la palabra.
####
1. Tu análisis de 3-4 ejemplos de Ji Lee, explicando cómo logran la conexión palabra-imagen.
####
- **Zipper:** El utilizar un cierre para dibujar la E es bastante ingenioso, por no decir que visualmente claro y atractivo.
- **Exit:** El implementar literalmente la I como una puerta de salida y hacer que la X corra hacia esta, ayuda a conveer bastante el  signficado de la palabra.
- **Eclipse:** Hacer uso de la C como una luna eclipsada y aún mejor, oscureces el fondo en torno se cubre completamente, es simplemente una forma brillante de mostrar el significado de la misma.
####
Tus propias ideas (descripción o boceto simple) para representar visualmente 2-3 palabras distintas de forma estática.
####
- **Ciempiés:** Sería reemplazar la m con literalmente un ciempiés.
<img width="1222" height="548" alt="image" src="https://github.com/user-attachments/assets/7eaab008-0a34-40c8-9c02-95527ce487a1" />

- **Bichos:** En un primer caso, que la i sea una hormiga y la c una hoja cortada que esta lleva. Si la b llega a ser mayúscula, esta sería un caracol.
<img width="937" height="504" alt="image" src="https://github.com/user-attachments/assets/ebd56ef7-6680-4181-a6fb-98d9aa186319" />

## Actividad 2
Para animar nuestras palabras con física, necesitamos entender Matter.js. Investigaremos sus conceptos clave y realizaremos experimentos básicos para familiarizarnos con su uso junto a p5.js.
### Recursos
#### Obligatorios:
- Sitio web oficial de Matter.js: [https://brm.io/matter-js/ (explora los demos)](https://brm.io/matter-js/).
- Video Tutorial de Patt Vira: [p5.js Coding Tutorial | Introduction to matter.js](https://youtu.be/cLXNxn5N-2Y?si=CahhG5XPhWaUF3vD).
#### Opcionales:
Documentación de la API de Matter.js (para profundizar).
Otros tutoriales o ejemplos que encuentres.
### Pasos:
1. **Visualiza y lee:** mira el video de Patt Vira completo. Explora los ejemplos básicos en el sitio web de Matter.js.
2. **Identifica conceptos clave:** mientras exploras, presta atención a estos conceptos fundamentales: `Engine`, `World`, `Bodies` (y sus tipos: rectángulos, círculos, polígonos), `Constraint`, `MouseConstraint`, `Runner/Events`.
3. **Experimenta con código:** Intenta replicar en p5.js al menos dos experimentos básicos mostrados en el video de Patt Vira o en los ejemplos del sitio web. Por ejemplo:
- Crear un mundo con gravedad y añadir algunos cuerpos simples (círculos, cajas) que caigan y colisionen.
- Crear cuerpos estáticos (como el suelo).
- Implementar `MouseConstraint` para poder interactuar con los cuerpos usando el mouse.
- (Opcional avanzado) Crear una restricción simple (Constraint) entre dos cuerpos.
4. **Explica los conceptos:** basándote en tu experimentación y lectura, explica con tus propias palabras qué es y para qué sirve cada uno de los conceptos clave listados en el paso 2 (`Engine`, `World`, `Bodies`, `Constraint`, `MouseConstraint`).
####
**Contesta...**
####
1. Muestra el código de los dos (o más) experimentos básicos que replicaste integrando Matter.js y p5.js.
####
`Experimento 1`
``` js
// --- importar Matter.js ---
const { Engine, World, Bodies, Body, Constraint, Mouse, MouseConstraint } = Matter;

let engine, world;
let ground, plataforma, pivote, bola;
let bloques = [];
let mConstraint;

function setup() {
  createCanvas(900, 500);
  engine = Engine.create();
  world = engine.world;

  // --- suelo ---
  ground = Bodies.rectangle(width / 2, height - 10, width, 20, {
    isStatic: true,
  });
  World.add(world, ground);

  // --- plataforma (balancín) ---
  plataforma = Bodies.rectangle(width / 2, height - 80, 300, 20, {
    friction: 0.5,
    restitution: 0.2,
  });
  World.add(world, plataforma);

  // --- pivote central ---
  pivote = Constraint.create({
    pointA: { x: width / 2, y: height - 80 },
    bodyB: plataforma,
    length: 0,
    stiffness: 1,
  });
  World.add(world, pivote);

  // --- bloques en el extremo izquierdo ---
  const startX = width / 2 - 100;
  const startY = height - 120;
  for (let i = 0; i < 5; i++) {
    let color = i % 2 == 0 ? "#ffb347" : "#0d3b66";
    let b = Bodies.rectangle(startX - i * 25, startY - i * 20, 30, 30, {
      restitution: 0.4,
      friction: 0.3,
    });
    b.customColor = color;
    bloques.push(b);
    World.add(world, b);
  }

  // --- esfera grande en el extremo derecho ---
  bola = Bodies.circle(width / 2 + 130, height - 200, 40, {
    restitution: 0.4,
    density: 0.02,
  });
  bola.customColor = "#0d3b66";
  World.add(world, bola);

  // --- MouseConstraint para arrastrar ---
  const canvasMouse = Mouse.create(canvas.elt);
  const opciones = {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false }, // no mostrar línea
    },
  };
  mConstraint = MouseConstraint.create(engine, opciones);
  World.add(world, mConstraint);
}

function draw() {
  background(255);
  Engine.update(engine);

  // --- suelo ---
  noStroke();
  fill(230);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 20);

  // --- plataforma ---
  push();
  translate(plataforma.position.x, plataforma.position.y);
  rotate(plataforma.angle);
  fill("#f6cf57");
  rectMode(CENTER);
  rect(0, 0, 300, 20);
  pop();

  // --- bloques ---
  for (let b of bloques) {
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    fill(b.customColor);
    rectMode(CENTER);
    rect(0, 0, 30, 30);
    pop();
  }

  // --- bola ---
  push();
  translate(bola.position.x, bola.position.y);
  fill(bola.customColor);
  ellipse(0, 0, 80);
  pop();

  // --- debug visual: resaltar cuerpo agarrado ---
  if (mConstraint.body) {
    const pos = mConstraint.body.position;
    push();
    stroke(255, 0, 0);
    strokeWeight(2);
    noFill();
    ellipse(pos.x, pos.y, 100);
    pop();
  }
}
```
`Experimento 2`
``` js
// --- importar módulos de Matter.js ---
const { Engine, World, Bodies, Body, Constraint, Composites, Composite, Mouse, MouseConstraint } = Matter;

let engine, world;
let trampolin = [];
let constraints = [];
let cubos = [];
let mConstraint;

function setup() {
  createCanvas(900, 500);
  engine = Engine.create();
  world = engine.world;

  // --- parámetros del trampolín ---
  const xInicio = 200;
  const yBase = 400;
  const segmentos = 10;
  const anchoSegmento = 50;
  const altura = 20;

  // --- crear segmentos del trampolín ---
  for (let i = 0; i < segmentos; i++) {
    let x = xInicio + i * anchoSegmento;
    let s = Bodies.rectangle(x, yBase, anchoSegmento, altura, {
      restitution: 0.2,
      friction: 0.4,
      density: 0.002,
    });
    trampolin.push(s);
    World.add(world, s);

    // conectar con el anterior (constraint tipo resorte)
    if (i > 0) {
      let prev = trampolin[i - 1];
      let c = Constraint.create({
        bodyA: prev,
        pointA: { x: anchoSegmento / 2, y: 0 },
        bodyB: s,
        pointB: { x: -anchoSegmento / 2, y: 0 },
        stiffness: 0.5,
        damping: 0.1,
      });
      constraints.push(c);
      World.add(world, c);
    }
  }

  // --- anclar extremos (como en la imagen) ---
  const izquierda = Constraint.create({
    pointA: { x: xInicio - 25, y: yBase },
    bodyB: trampolin[0],
    pointB: { x: -anchoSegmento / 2, y: 0 },
    stiffness: 1,
  });
  const derecha = Constraint.create({
    pointA: { x: xInicio + segmentos * anchoSegmento + 25, y: yBase },
    bodyB: trampolin[segmentos - 1],
    pointB: { x: anchoSegmento / 2, y: 0 },
    stiffness: 1,
  });
  World.add(world, [izquierda, derecha]);

  // --- cubos que caen ---
  for (let i = 0; i < 10; i++) {
    let c = Bodies.rectangle(350 + random(-100, 100), 100 - i * 35, 40, 40, {
      restitution: 0.4,
      friction: 0.3,
      density: 0.002,
    });
    c.customColor = random(["#ff6f59", "#f7cb15", "#f7b267", "#0d3b66", "#fdfcdc"]);
    cubos.push(c);
    World.add(world, c);
  }

  // --- mouse interactivo ---
  const canvasMouse = Mouse.create(canvas.elt);
  const opciones = {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false },
    },
  };
  mConstraint = MouseConstraint.create(engine, opciones);
  World.add(world, mConstraint);
}

function draw() {
  background(255);
  Engine.update(engine);

  // --- trampolín ---
  stroke(20);
  strokeWeight(3);
  noFill();
  beginShape();
  for (let s of trampolin) {
    vertex(s.position.x, s.position.y);
  }
  endShape();

  // --- dibujar cada segmento ---
  fill("#000820");
  noStroke();
  for (let s of trampolin) {
    push();
    translate(s.position.x, s.position.y);
    rotate(s.angle);
    rectMode(CENTER);
    rect(0, 0, 50, 20);
    pop();
  }

  // --- cubos ---
  for (let c of cubos) {
    push();
    translate(c.position.x, c.position.y);
    rotate(c.angle);
    fill(c.customColor);
    rectMode(CENTER);
    rect(0, 0, 40, 40);
    pop();
  }

  // --- debug: cuerpo agarrado ---
  if (mConstraint.body) {
    const pos = mConstraint.body.position;
    push();
    noFill();
    stroke(255, 0, 0);
    ellipse(pos.x, pos.y, 80);
    pop();
  }
}
```
####
2. Incluye una captura de pantalla o ENLACE a un GIF (no olvides, enlace) de cada experimento funcionando.
####
![Trampolin](https://github.com/user-attachments/assets/248310b7-daf6-4c8f-bb48-8f8b958cb0d9)

![Palanca](https://github.com/user-attachments/assets/eba140dd-f6b3-4844-8fc5-fbf897aa9dd2)

####
3. Proporciona tu explicación clara y concisa de los conceptos clave (`Engine`, `World`, `Bodies`, `Constraint`, `MouseConstraint`).
####
- **Engine: **Es el motor de física que calcula todas las fuerzas, colisiones y movimientos. Actualiza las posiciones y velocidades de los cuerpos en cada frame.
####
Se crea con `Matter.Engine.create()` y se actualiza con `Matter.Engine.update(engine)` en `draw()` o `update()`.
####
- **World: **Es el contenedor de todos los cuerpos físicos (objetos, muros, partículas, etc.). Pertenece al engine: engine.world
####
Aquí se “viven” los objetos que la simulación debe tener en cuenta.
####
- **Bodies: **Son los objetos físicos del mundo: círculos, rectángulos, polígonos o figuras personalizadas. Se crean con funciones como:
``` js
Bodies.circle(x, y, radius)`
Bodies.rectangle(x, y, width, height)`
```
####
Cada cuerpo tiene propiedades físicas:
- mass (masa)
- friction (fricción)
- restitution (rebote)
- isStatic (si se mueve o no)
####
- **Constraint: **Son conectores entre cuerpos, como resortes o barras rígidas. Permiten simular uniones flexibles, cuerdas, huesos o patas, y se crean con:
``` js
Constraint.create({
  bodyA: body1,
  bodyB: body2,
  stiffness: 0.9,
  length: 100
})
```
- **MouseConstraint: **Permite interactuar con el mundo físico usando el mouse. Detecta clics y arrastres sobre los cuerpos. Se añade al mundo con:
``` js
const mouse = Mouse.create(canvas.elt);
const mouseConstraint = MouseConstraint.create(engine, { mouse });
World.add(world, mouseConstraint);
```
####
4. Menciona brevemente cualquier dificultad encontrada al configurar o usar Matter.js inicialmente.
####
Entender cómo hacer funcionar los bodies principalmente, así como los constraints.

# Apply
## Actividad 3
**Animando la tipografía semántica**
####
¡Es hora de aplicar todo! Elige una palabra y, usando p5.js y Matter.js, crea una animación donde la palabra “actúe” o se comporte físicamente de una manera que refleje su significado, inspirándote en el concepto “Word as Image”.
####
1. Indica claramente la palabra elegida.
####
La palabra que elegí es "ciempiés", pues son mi animal favorito y me parece que su anatomía podría demostrar una implementación compleja e interesante.
####
2. Explica tu **idea conceptual**: ¿Cómo la animación física representa el significado de la palabra?
####
En la animación, decidí que la palabra completa sea un ciempiés, de tal forma que la C son sus dos "antenas" traseras, y la S las delanteras. Las patas por otro lado, solo ocuparían la parte inferior de las letras. Respecto a qué haría, la palabra entera se movería hacia el mouse con un movimiento similar a un ciempiés.
<img width="1529" height="512" alt="image" src="https://github.com/user-attachments/assets/a42e9e62-8f8d-4770-be57-28f6707543a8" />

Este sería un sketch de cómo se vería la palabra.
####
Al final, las patas tuvieron que tomar una ruta diferente, pero sigue manteniendo la idea.
####
3. Describe brevemente los aspectos técnicos clave de tu implementación: ¿Cómo formaste las letras con Matter.js? ¿Qué propiedades físicas fueron importantes? ¿Usaste restricciones?
####
Las letras fueron un desafío, pues quería un aspecto orgánico y ciertamente suave. Para hacerlas, me decidí por curvas bezier para cada letra y así formas lo más parecido a mi idea. Por otro lado, estas mismas se les aplicó un constraint de matter para luego hacer el movimiento de ellas conjuntamente. Esto dió problemas volviendose locas las letras, pero con repelsiones se pudo arreglar. Por último, tuve que asegurarme que solo la S siguiera el mouse y las demás letras se movieran de forma coherente en consecuencia; así como agregue una variable que midiera la velocidad en la que se hace esto por razones estéticas. 
####
Diría que lo explicado fue lo más difícil de lograr correctamente, las patitas salió con bastante rapidez a comparación, así como que se quedasen totalmente estáticas al hacer click con el mouse.
####
4. Incluye el código completo de tu sketch final.
####
`Versión 1`
``` js
// ===========================================================
// "ciempiés" — letras minúsculas 2D, articuladas, sin mostrar uniones
// ===========================================================

const { Engine, Bodies, Body, Composite, Constraint } = Matter;

let engine, world;
let letras = [];
let conexiones = [];

function setup() {
  createCanvas(900, 400);
  engine = Engine.create();
  world = engine.world;

  // Sin gravedad — flotan suspendidas
  engine.world.gravity.y = 0;

  const baseY = height / 2;

  // ---- Crear letras (posición base) ----
  letras.push(new LetraC(70, baseY));
  letras.push(new LetraI(180, baseY));
  letras.push(new LetraE(240, baseY));
  letras.push(new LetraM(320, baseY));
  letras.push(new LetraP(410, baseY));
  letras.push(new LetraI(480, baseY));
  letras.push(new LetraE(530, baseY));
  letras.push(new LetraS(610, baseY));

  // ---- Conectar letras con constraints (invisibles) ----
  for (let i = 0; i < letras.length - 1; i++) {
    let bodyA = letras[i].body;
    let bodyB = letras[i + 1].body;
    let c = Constraint.create({
      bodyA,
      pointA: { x: 25, y: 0 },
      bodyB,
      pointB: { x: -25, y: 0 },
      length: 15,
      stiffness: 0.3,
      render: { visible: false } // las uniones no se dibujan
    });
    Composite.add(world, c);
    conexiones.push(c);
  }
}

function draw() {
  background(255);
  Engine.update(engine);

  // Mostrar letras
  for (let l of letras) l.display();
}

// ===========================================================
// LETRAS SUAVES Y CONSISTENTES — estilo caligráfico limpio
// ===========================================================

class LetraBase {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  estilo() {
    noFill();
    stroke(20);
    strokeWeight(10);
    strokeCap(ROUND);
    strokeJoin(ROUND);
  }
}

// ---- c invertida y fluida ----
class LetraC extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(0.8, 0.8);
    this.estilo();
    scale(-1, 1); // invertir horizontalmente
    beginShape();
    vertex(-30, -25);
    bezierVertex(-120, -10, -120, 10, -30, 40);
    endShape();
    pop();
  }
}

// ---- i minúscula (sin punto) ----
class LetraI extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(0.8, 0.8);
    this.estilo();
    line(0, -25, 0, 45);
    pop();
  }
}

// ---- e orgánica con cola descendente (según referencia) ----
class LetraE extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(0.8, 0.8);
    this.estilo();
    noFill();
    stroke(20);
    strokeWeight(8);
    strokeCap(ROUND);

    beginShape();
    vertex(-20, 10);                   // inicio parte izquierda
    bezierVertex(-10, -15, 25, -15, 20, 5);  // bucle superior
    bezierVertex(15, 15, -10, 15, -10, 5);    // cierre del bucle
    bezierVertex(-30, 30, 30, 35, 10, 50);     // cola descendente suave
    endShape();

    pop();
  }
}

// ---- m fluida ----
class LetraM extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(0.8, 0.8);
    this.estilo();
    beginShape();
    vertex(-25, 45);
    bezierVertex(-20, -15, 0, -20, 10, 5);
    bezierVertex(15, -20, 45, -20, 40, 45);
    endShape();
    pop();
  }
}

// ---- p fluida ----
class LetraP extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(0.8, 0.8);
    this.estilo();
    beginShape();
    vertex(-10, 45);
    bezierVertex(-10, -20, 10, -20, 20, -10);
    bezierVertex(25, 0, 10, 10, -5, 5);
    endShape();
    pop();
  }
}

// ---- s alargada invertida (como tu dibujo) ----
class LetraS extends LetraBase {
  display() {
    push();
    translate(this.x, this.y);
    scale(-0.8, 0.8); // voltea horizontalmente
    noFill();
    stroke(20);
    strokeWeight(8);
    strokeCap(ROUND);

    beginShape();
    vertex(-40, -35);                   // inicio arriba derecha (invertido)
    bezierVertex(-10, -25, 25, -25, 30, -5);  // parte superior larga
    bezierVertex(35, 10, 10, 20, -10, 25);    // parte inferior
    bezierVertex(-25, 30, -10, 35, 10, 40);   // cola descendente
    endShape();

    pop();
  }
}
```
`Versión 2`
``` js
// ===========================================================
// Ciempiés de letras — versión estable y suave
// ===========================================================

let letras = [];
let activo = true; // movimiento activo o congelado
let seguimientoVelocidad = 0.1; // rapidez al seguir el mouse
let distanciaDeseada = 70; // separación entre letras
let fuerzaRepulsion = 0.0025; // evita que se amontonen
let delaySuavizado = 0.25; // retraso en el seguimiento

function setup() {
  createCanvas(900, 400);
  const baseY = height / 2;

  // Crear letras manualmente (sin Matter.js)
  letras.push(new LetraC(70, baseY));
  letras.push(new LetraI(180, baseY));
  letras.push(new LetraE(240, baseY));
  letras.push(new LetraM(320, baseY));
  letras.push(new LetraP(410, baseY));
  letras.push(new LetraI(480, baseY));
  letras.push(new LetraE(530, baseY));
  letras.push(new LetraS(-150, baseY)); // cabeza (última)
}

function draw() {
  background(255);

  if (activo) moverLetras();

  for (let l of letras) l.display();
}

function moverLetras() {
  let cabeza = letras[letras.length - 1];
  let target = createVector(mouseX, mouseY);

  // --- cabeza sigue al mouse ---
  let dir = p5.Vector.sub(target, cabeza.pos);
  dir.mult(seguimientoVelocidad);
  cabeza.pos.add(dir);

  // rotación hacia mouse
  cabeza.ang = atan2(dir.y, dir.x);

  // --- resto de letras ---
  for (let i = letras.length - 2; i >= 0; i--) {
    let siguiente = letras[i + 1];
    let actual = letras[i];

    let dirSeguir = p5.Vector.sub(siguiente.pos, actual.pos);
    let dist = dirSeguir.mag();
    dirSeguir.normalize();

    // objetivo: mantener distancia deseada
    let delta = dist - distanciaDeseada;
    actual.pos.add(dirSeguir.mult(delta * delaySuavizado));

    // rotación hacia la siguiente
    actual.ang = atan2(
      siguiente.pos.y - actual.pos.y,
      siguiente.pos.x - actual.pos.x
    );
  }

  // --- repulsión suave (solo si están muy cerca) ---
  for (let i = 0; i < letras.length; i++) {
    for (let j = i + 1; j < letras.length; j++) {
      let a = letras[i];
      let b = letras[j];
      let diff = p5.Vector.sub(a.pos, b.pos);
      let dist = diff.mag();
      let minDist = 45;
      if (dist < minDist) {
        diff.normalize();
        let fuerza = (minDist - dist) * fuerzaRepulsion;
        a.pos.add(diff.mult(fuerza * 400));
        b.pos.sub(diff.mult(fuerza * 400));
      }
    }
  }
}

// 🖱️ Click → alternar movimiento
function mousePressed() {
  activo = !activo;
}

// ===========================================================
// LETRAS SUAVES (sin física, solo dibujo)
// ===========================================================

class LetraBase {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.ang = 0;
  }

  estilo() {
    noFill();
    stroke(20);
    strokeWeight(8);
    strokeCap(ROUND);
    strokeJoin(ROUND);
  }
}

class LetraC extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-30, -25);
    bezierVertex(-70, -10, -70, 10, -30, 40);
    endShape();
    pop();
  }
}

class LetraI extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    line(0, -25, 0, 45);
    pop();
  }
}

class LetraE extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-20, 10);
    bezierVertex(-10, -15, 25, -15, 20, 5);
    bezierVertex(15, 15, -10, 15, -10, 5);
    bezierVertex(-25, 30, 25, 35, 10, 50);
    endShape();
    pop();
  }
}

class LetraM extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-25, 45);
    bezierVertex(-20, -15, 0, -20, 10, 5);
    bezierVertex(15, -20, 45, -20, 40, 45);
    endShape();
    pop();
  }
}

class LetraP extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-10, 45);
    bezierVertex(-10, -20, 10, -20, 20, -10);
    bezierVertex(25, 0, 10, 10, -5, 5);
    endShape();
    pop();
  }
}

class LetraS extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    scale(-0.8, 0.8);
    beginShape();
    vertex(-40, -35);
    bezierVertex(-10, -25, 25, -25, 30, -5);
    bezierVertex(35, 10, 10, 20, -10, 25);
    bezierVertex(-25, 30, -10, 35, 10, 40);
    endShape();
    pop();
  }
}
```
`Versión 3`
``` js
// ===========================================================
// Ciempiés de letras — versión estable y suave con patas animadas
// ===========================================================

let letras = [];
let activo = true; // movimiento activo o congelado
let seguimientoVelocidad = 0.1; // rapidez al seguir el mouse
let distanciaDeseada = 70; // separación entre letras
let fuerzaRepulsion = 0.0025; // evita que se amontonen
let delaySuavizado = 0.25; // retraso en el seguimiento
let tiempo = 0; // contador para animación de patas

function setup() {
  createCanvas(900, 400);
  const baseY = height / 2;

  // Crear letras manualmente (sin Matter.js)
  letras.push(new LetraC(30, baseY));
  letras.push(new LetraI(180, baseY));
  letras.push(new LetraE(240, baseY));
  letras.push(new LetraM(320, baseY));
  letras.push(new LetraP(410, baseY));
  letras.push(new LetraI(480, baseY));
  letras.push(new LetraE(530, baseY));
  letras.push(new LetraS(-150, baseY)); // cabeza (última)
}

function draw() {
  background(255);
  tiempo += 0.02;

  if (activo) moverLetras();

  for (let l of letras) l.display();
}

function moverLetras() {
  let cabeza = letras[letras.length - 1];
  let target = createVector(mouseX, mouseY);

  // --- cabeza sigue al mouse ---
  let dir = p5.Vector.sub(target, cabeza.pos);
  dir.mult(seguimientoVelocidad);
  cabeza.pos.add(dir);

  // rotación hacia mouse
  cabeza.ang = atan2(dir.y, dir.x);

  // --- resto de letras ---
  for (let i = letras.length - 2; i >= 0; i--) {
    let siguiente = letras[i + 1];
    let actual = letras[i];

    let dirSeguir = p5.Vector.sub(siguiente.pos, actual.pos);
    let dist = dirSeguir.mag();
    dirSeguir.normalize();

    // objetivo: mantener distancia deseada
    let delta = dist - distanciaDeseada;
    actual.pos.add(dirSeguir.mult(delta * delaySuavizado));

    // rotación hacia la siguiente
    actual.ang = atan2(
      siguiente.pos.y - actual.pos.y,
      siguiente.pos.x - actual.pos.x
    );
  }

  // --- repulsión suave (solo si están muy cerca) ---
  for (let i = 0; i < letras.length; i++) {
    for (let j = i + 1; j < letras.length; j++) {
      let a = letras[i];
      let b = letras[j];
      let diff = p5.Vector.sub(a.pos, b.pos);
      let dist = diff.mag();
      let minDist = 45;
      if (dist < minDist) {
        diff.normalize();
        let fuerza = (minDist - dist) * fuerzaRepulsion;
        a.pos.add(diff.mult(fuerza * 400));
        b.pos.sub(diff.mult(fuerza * 400));
      }
    }
  }
}

// 🖱️ Click → alternar movimiento
function mousePressed() {
  activo = !activo;
}

// ===========================================================
// LETRAS SUAVES (sin física, solo dibujo)
// ===========================================================

class LetraBase {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.ang = 0;
  }

  estilo() {
    noFill();
    stroke(20);
    strokeWeight(8);
    strokeCap(ROUND);
    strokeJoin(ROUND);
  }
}

// ---- C original restaurada ----
class LetraC extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    scale(0.8, 0.8);
    this.estilo();
    scale(-1, 1); // invertir horizontalmente
    beginShape();
    vertex(-30, -25);
    bezierVertex(-170, -10, -170, 10, -30, 40);
    endShape();
    pop();
  }
}

class LetraI extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    line(0, -25, 0, 45);

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 6 : 0;

    line(0, 45, -10 + osc, 60);
    line(0, 45, 10 - osc, 60);
    pop();
  }
}

class LetraE extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-20, 10);
    bezierVertex(-10, -15, 25, -15, 20, 5);
    bezierVertex(15, 15, -10, 15, -10, 5);
    bezierVertex(-25, 30, 25, 35, 10, 50);
    endShape();

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(5, 50, 15 + osc, 65);
    line(-5, 50, -15 - osc, 65);
    pop();
  }
}

class LetraM extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-25, 45);
    bezierVertex(-20, -15, 0, -20, 10, 5);
    bezierVertex(15, -20, 45, -20, 40, 45);
    endShape();

    // patas
    let fase = this.pos.x * 0.15;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(-20, 45, -30 + osc, 60);
    line(0, 45, 0 + osc, 60);
    line(25, 45, 35 - osc, 60);
    pop();
  }
}

class LetraP extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-10, 45);
    bezierVertex(-10, -20, 10, -20, 20, -10);
    bezierVertex(25, 0, 10, 10, -5, 5);
    endShape();

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(-10, 45, -20 + osc, 60);
    line(5, 45, 15 - osc, 60);
    pop();
  }
}

class LetraS extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    scale(-0.8, 0.8);
    beginShape();
    vertex(-40, -35);
    bezierVertex(-10, -25, 25, -25, 30, -5);
    bezierVertex(35, 10, 10, 20, -10, 25);
    bezierVertex(-25, 30, -10, 35, 10, 40);
    endShape();
    pop();
  }
}
```
`Versión 4`
``` js
// ===========================================================
// Ciempiés de letras — versión estable y suave con patas animadas
// ===========================================================

let letras = [];
let activo = false; // movimiento activo o congelado
let seguimientoVelocidad = 0.1; // rapidez al seguir el mouse
let distanciaDeseada = 70; // separación entre letras
let fuerzaRepulsion = 0.0025; // evita que se amontonen
let delaySuavizado = 0.25; // retraso en el seguimiento
let tiempo = 0; // contador para animación de patas

function setup() {
  createCanvas(900, 400);
  const baseY = 800;  // fuera del canvas (abajo)
  const baseX = 1100; // fuera del canvas (derecha)

  // Crear letras con separación ajustada (C más separada de la I)
  letras.push(new LetraC(baseX, baseY));        // C
  letras.push(new LetraI(baseX - 240, baseY));  // I — más separada
  letras.push(new LetraE(baseX - 310, baseY));  // E
  letras.push(new LetraM(baseX - 390, baseY));  // M
  letras.push(new LetraP(baseX - 480, baseY));  // P
  letras.push(new LetraI(baseX - 550, baseY));  // I
  letras.push(new LetraE(baseX - 600, baseY));  // E
  letras.push(new LetraS(baseX + 100, baseY));  // S (cabeza, adelante)
}

function draw() {
  background(255);
  tiempo += 0.02;

  if (activo) moverLetras();

  for (let l of letras) l.display();
}

function moverLetras() {
  let cabeza = letras[letras.length - 1];
  let target = createVector(mouseX, mouseY);

  // --- cabeza sigue al mouse ---
  let dir = p5.Vector.sub(target, cabeza.pos);
  dir.mult(seguimientoVelocidad);
  cabeza.pos.add(dir);

  // rotación hacia mouse
  cabeza.ang = atan2(dir.y, dir.x);

  // --- resto de letras ---
  for (let i = letras.length - 2; i >= 0; i--) {
    let siguiente = letras[i + 1];
    let actual = letras[i];

    let dirSeguir = p5.Vector.sub(siguiente.pos, actual.pos);
    let dist = dirSeguir.mag();
    dirSeguir.normalize();

    // mantener distancia deseada
    let delta = dist - distanciaDeseada;
    actual.pos.add(dirSeguir.mult(delta * delaySuavizado));

    // rotación hacia la siguiente
    actual.ang = atan2(
      siguiente.pos.y - actual.pos.y,
      siguiente.pos.x - actual.pos.x
    );
  }

  // --- repulsión suave (solo si están muy cerca) ---
  for (let i = 0; i < letras.length; i++) {
    for (let j = i + 1; j < letras.length; j++) {
      let a = letras[i];
      let b = letras[j];
      let diff = p5.Vector.sub(a.pos, b.pos);
      let dist = diff.mag();
      let minDist = 45;
      if (dist < minDist) {
        diff.normalize();
        let fuerza = (minDist - dist) * fuerzaRepulsion;
        a.pos.add(diff.mult(fuerza * 400));
        b.pos.sub(diff.mult(fuerza * 400));
      }
    }
  }
}

// 🖱️ Click → alternar movimiento
function mousePressed() {
  activo = !activo;
}

// ===========================================================
// LETRAS SUAVES (sin física, solo dibujo)
// ===========================================================

class LetraBase {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.ang = 0;
  }

  estilo() {
    noFill();
    stroke(20);
    strokeWeight(8);
    strokeCap(ROUND);
    strokeJoin(ROUND);
  }
}

// ---- C original restaurada ----
class LetraC extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    scale(0.8, 0.8);
    this.estilo();
    scale(-1, 1); // invertir horizontalmente
    beginShape();
    vertex(-30, -25);
    bezierVertex(-170, -10, -170, 10, -30, 40);
    endShape();
    pop();
  }
}

class LetraI extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    line(0, -25, 0, 45);

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 6 : 0;

    line(0, 45, -10 + osc, 60);
    line(0, 45, 10 - osc, 60);
    pop();
  }
}

class LetraE extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-20, 10);
    bezierVertex(-10, -15, 25, -15, 20, 5);
    bezierVertex(15, 15, -10, 15, -10, 5);
    bezierVertex(-25, 30, 25, 35, 10, 50);
    endShape();

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(5, 50, 15 + osc, 65);
    line(-5, 50, -15 - osc, 65);
    pop();
  }
}

class LetraM extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-25, 45);
    bezierVertex(-20, -15, 0, -20, 10, 5);
    bezierVertex(15, -20, 45, -20, 40, 45);
    endShape();

    // patas
    let fase = this.pos.x * 0.15;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(-20, 45, -30 + osc, 60);
    line(0, 45, 0 + osc, 60);
    line(25, 45, 35 - osc, 60);
    pop();
  }
}

class LetraP extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    beginShape();
    vertex(-10, 45);
    bezierVertex(-10, -20, 10, -20, 20, -10);
    bezierVertex(25, 0, 10, 10, -5, 5);
    endShape();

    // patas
    let fase = this.pos.x * 0.1;
    let osc = activo ? sin(tiempo * 5 + fase) * 8 : 0;

    line(-10, 45, -20 + osc, 60);
    line(5, 45, 15 - osc, 60);
    pop();
  }
}

class LetraS extends LetraBase {
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.ang);
    this.estilo();
    scale(-0.8, 0.8);
    beginShape();
    vertex(-40, -35);
    bezierVertex(-10, -25, 25, -25, 30, -5);
    bezierVertex(35, 10, 10, 20, -10, 25);
    bezierVertex(-25, 30, -10, 35, 10, 40);
    endShape();
    pop();
  }
}
```
[https://editor.p5js.org/catflyx/sketches/KEwTswZok](https://editor.p5js.org/catflyx/sketches/KEwTswZok)
####
5. Inserta una captura de pantalla estática Y un enlace a un GIF animado (¡Esencial!) que muestre tu tipografía semántica animada en acción.
####
<img width="920" height="398" alt="image" src="https://github.com/user-attachments/assets/48aaef9f-ca01-4ffd-aa41-a26a10fe8b84" />

![ciempies](https://github.com/user-attachments/assets/6f4e97e3-6218-49d5-8081-202610000ac4)

# Autoevaluación
**Nota:** 5
####
Cumplí con las 3 actividades completas, siguiendo cada uno de los puntos que se exigían. Además, están puestas las evidencias de las mismas como es debido. Por último, estoy haciendo la autoevaluación.







