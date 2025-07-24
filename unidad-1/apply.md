# Unidad 1
`🛠 Fase: Apply`
_________________________________________________________________________________________________________________________________________________________________________________________
# Aplicación
Vas a aplicar los conceptos con los que experimentaste en la fase de investigación para crear una aplicación de arte generativo interactivo en tiempo real.

## Actividad 08
### Creación de obra generativa
Vas a crear una obra generativa interactiva en tiempo real utilizando los conceptos de aleatoriedad que has aprendido en esta unidad.
###
Tu obra debe:
- Usar al menos tres conceptos estudiados en esta unidad COMBINADOS de manera creativa y coherente.
- Tu obra de ser interactiva y generativa en tiempo real. Puedes usar el mouse, el teclado o cualquier otro sensor de entrada para interactuar con la obra.
# Concepto
Hacer que lo que llamamos `walker` se mueva usando el ruido perlin, dejando una estela en la cual varía de tamaño. Además, hacer que tenga una progabilidad de que "salte", y que al hacer este salto cambie de color, así como que cambie de forma en el primer trazo.
# Código
`versión 1`
###
Bases del jump, así como el cambio de forma cada que lo haga.
###
``` js
let walker;

let jump;

function setup() {
  createCanvas(640, 540);
  walker = new Walker();
  jump = false;
  background('rgb(9,4,13)');
}

function draw() {
if (jump){
      walker.show2(); jump = false;
      } else{
      walker.show();
      }
  
  walker.step();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
}

  show() {
    noStroke();
  fill(random(200), random(200), random(200), 50) //fill(0, 10);
  circle(this.x, this.y, random(20, 60));
}
  show2() {
    noStroke();
  fill(random(200), random(200), random(200), 80) //fill(0, 10);
    // triangle(200, 100, 300, 150, 250, 250);
    
// Tamaño aleatorio para el lado del triángulo
  let size = random(80, 120);
  
  // Altura del triángulo equilátero
  let h = size * sqrt(7) / 2;

  // Definir los vértices relativos a (this.x, this.y) centrados
  let x1 = this.x;
  let y1 = this.y - (2 / 3) * h;

  let x2 = this.x - size / 2;
  let y2 = this.y + (1 / 3) * h;

  let x3 = this.x + size / 2;
  let y3 = this.y + (1 / 3) * h;

  triangle(x1, y1, x2, y2, x3, y3);
}

  step() {
    let r = random(1);
    
    
    if (r < 0.01) {
  this.x = random(-100, 100);
  this.y = random(-100, 100);
      jump = true;

    } else { // Si no hace el salto
  const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
  }
}
```
`versión 2`
###
Tamaño del triángulo más variada, cambio de color al salto, arreglos estéticos pequeños.
###
``` js
let walker;

let jump;

let r, g, b;

function setup() {
  createCanvas(640, 540);
  walker = new Walker();
  jump = false;
  
  r = random(200); g = random(200); b = random(200);
  
  background('rgb(9,4,13)');
}

function draw() {
if (jump){
      walker.show2(); 
  r = random(200); g = random(200); b = random(200);
  jump = false;
      } else{
      walker.show();
      }
  
  walker.step();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
}

  show() {
    noStroke();
  fill(r, g, b, 30) //fill(0, 10);
  circle(this.x, this.y, random(20, 60));
}
  show2() {
    noStroke();
  fill(r, g, b, 100) //fill(0, 10);
    // triangle(200, 100, 300, 150, 250, 250);
    
// Tamaño aleatorio para el lado del triángulo
  let size = random(40, 120);
  
  // Altura del triángulo equilátero
  let h = size * random(sqrt(1) / 2, sqrt(9) / 2);

  // Definir los vértices relativos a (this.x, this.y) centrados
  let x1 = this.x;
  let y1 = this.y - (2 / 3) * h;

  let x2 = this.x - size / 2;
  let y2 = this.y + (1 / 3) * h;

  let x3 = this.x + size / 2;
  let y3 = this.y + (1 / 3) * h;

  triangle(x1, y1, x2, y2, x3, y3);
}

  step() {
    let r = random(1);
    
    
    if (r < 0.01) {
  this.x = random(-100, 100);
  this.y = random(-100, 100);
      jump = true;

    } else { // Si no hace el salto
  const choice = floor(random(4));
    if (choice == 0) {
      this.x++; this.x++;
    } else if (choice == 1) {
      this.x--; this.x--;
    } else if (choice == 2) {
      this.y++; this.y++;
    } else {
      this.y--; this.y--;
    }
  }
  }
}
```
`versión 3`
###
Movimiento usando noise, modificación a la selección del color.
###
``` js
let walker;

let jump;

let r, g, b; let t = 0; let t2 = 0;

function setup() {
  createCanvas(640, 540);
  walker = new Walker();
  jump = false;
  
  r = random(200); g = random(200); b = random(200);
  
  background('rgb(9,4,13)');
}

function draw() 
{ t += random(-100, 400); t2 += random(-20, 200);
 
if (jump){
  r = random(random(100, 150)); 
  g = random(random(50, 200)); 
  b = random(random(170, 250));
  
      walker.show2(); 
  
  jump = false;
      } else{
      walker.show();
      }
  
  walker.step();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
}

  show() {
    noStroke();
  fill(r, g, b, 30) //fill(0, 10);
  circle(this.x, this.y, random(20, 60));
}
  show2() {
    noStroke();
  fill(r, g, b, 100) //fill(0, 10);
    // triangle(200, 100, 300, 150, 250, 250);
    
// Tamaño aleatorio para el lado del triángulo
  let size = random(40, 120);
  
  // Altura del triángulo equilátero
  let h = size * random(sqrt(1) / 2, sqrt(9) / 2);

  // Definir los vértices relativos a (this.x, this.y) centrados
  let x1 = this.x;
  let y1 = this.y - (2 / 3) * h;

  let x2 = this.x - size / 2;
  let y2 = this.y + (1 / 3) * h;

  let x3 = this.x + size / 2;
  let y3 = this.y + (1 / 3) * h;

  triangle(x1, y1, x2, y2, x3, y3);
}

  step() {
    let r = random(1);
    
    
    if (r < 0.01) {
  this.x = random(0, 640);
  this.y = random(-100, 100);
      jump = true;

    } else { // Si no hace el salto
  const choice = floor(random(2));
    if (choice == 0) {
      this.x += noise(t) * 6; this.y += noise(t2) * 2;
    } else if (choice == 1) {
      this.y += noise(t2) * 8; this.x += noise(t) * 2;
    }
  }
  }
}
```
`versión 4`
###
....
###
``` js

```
`versión 5`
###
....
###
``` js

```
###
...
###
https://editor.p5js.org/catflyx/sketches/HnPJAllcJ
###
Selecciona una captura de pantalla
