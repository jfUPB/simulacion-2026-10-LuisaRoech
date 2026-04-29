# Unidad 7

## Bitácora de proceso de aprendizaje

### Ji Lee y la tipografía semántica (Actividad 01)

> ### _Obras Ji lee_

<img width="481" height="483" alt="image" src="https://github.com/user-attachments/assets/6dee13e9-6ca0-43b5-a039-bcf217502f26" />

```
Esta obra al inicialmente verla, me llamo mucho la atención, tiene la forma más directa y atractiva,
asi como se ve la n es alargada para esbozar una forma simplificada de tunel. Es lo primero que visualizas,
antes que la palabra completa, por el % de negro que contiene, su ubicación y tamaño por lo que ya sabes de
que tratará la palabra antes de leerla.
```

<img width="478" height="482" alt="image" src="https://github.com/user-attachments/assets/cf58bc88-a806-46f2-bdf8-d778f2b794cf" />

```
A diferencia de la anterior, es más implicita, no lees inmediatamente la palabra pero la sientes,
ese entorno oscuro en el que puedes focalizarte solo en una cosa, algo molesto que interrumpe la tranquilidad de todo,
una luz a la que no puedes quitarle la mirada, es un significado general pero que puede integrarse muy bien
en base a la experiencia de cada persona.
```

<img width="476" height="477" alt="image" src="https://github.com/user-attachments/assets/2e5a627c-0c58-42ba-bf50-03507cd609aa" />

```
Me gusta por la simple palabra escogida, como amante de garfield me gusta mucho como lo resolvio,
usando la G para representar la silueta de los ojos tan caracteristicos del show, además manejar dos tipografias distintas
crea un resultado elegante pero disruptivo, sketchy si pudiera darle una palabra.
```

> ### _Mis palabras_

- Guayabas: Es una palabra un tanto significativa para mi y tonta al mismo tiempo, por su silueta debe tomar una forma lo mas redonda posible, e incluso irregular, colores rosas, verdes, blancos. Puede que florezca o forme una flor. Algo simple pero caracteristico como su sabor. 
  
- Amor: Es una palabra ambigua, puede definirse de muchas cosas, es una definición que puede ser tanto global como personal. Si tuviera que darle una forma, seria lo más amplia posible, algo que cambie de letra, color, una forma que se rompa, que se reconstruya, que en ocasiones no pueda ser legible, o pueda considerarse fea, pero que tambien sea bella y tranquila.
  
- Cruz: Me gusta las alegorias o referencias a la religión, la cruz es el peso, el sacrificio, equilibrio e incluso una intersección, colores palidos, tipografia limpia, ordenada, algo que pueda generar opiniones opuestas.
  
- DOLOR: Una nueva palabra al pensar en todo lo que me gustaria representar la palabra guayaba, algo que se expande, se deshace y deja una mancha que no puede ser borrada.

> [!NOTE]
> _La que más me llama la atención de hacer es guayabas, pues al ser la palabra más directa y menos global, quiero ver que tanto le podria exprimir._


### Exploración de Matter.js (Actividad 02)

_NOTA: Youtube para profundizar matter.js: Patt Vira_

> ### _Conceptos_

- `Engine` es de los conceptos más importantes y más amplio de todos, aqui se calcula todo tipo de regla, como las fuerzas que se le pueden aplicar, su comportamiento y actualiza la simulación en cada momento

- `World` es el espacio donde viven todos los objetos y donde ocurre la simulación

- `Bodies` es la forma física de los objetos del mundo. Estos pueden reacción a fuerzas y moverse.

- `Constraint` define las reglas básicas de comportamiento hacia algo. Cómo uno o varios objetos están conectados o limitados en su movimiento.

- `MouseConstraint` este definiria el comportamiento basico del mouse, leyendo su ubicación y que funciones puede realizar, como agarrar un objeto, arrastrarlo, algo que crea un resorte invisible con un objeto y te permite interactuar fisicamente con el.

> ### _Experimentos_

<img width="748" height="528" alt="image" src="https://github.com/user-attachments/assets/ea7f5abb-1b7c-4fe8-81fd-1adcc59a7389" />

### _Experimento #1_

Este se podria decir que es el más básico de todos, aqui explore la gravedad, colisiones y el agarre de objetos. Entonces, las cajas caen por gravedad rebotan contra el suelo y con un `MouseConstraint` se pueden agarrar

``` Js

let Engine = Matter.Engine;
let World = Matter.World;
let Bodies = Matter.Bodies;
let Mouse = Matter.Mouse;
let MouseConstraint = Matter.MouseConstraint;

let engine, world;
let boxes = [];
let ground;
let mConstraint;

function setup() {
  createCanvas(600, 400);

  engine = Engine.create();
  world = engine.world;

  // suelo
  ground = Bodies.rectangle(300, 380, 600, 40, { isStatic: true });
  World.add(world, ground);

  // cajas iniciales
  for (let i = 0; i < 5; i++) {
    let box = Bodies.rectangle(random(100, 500), 0, 40, 40);
    boxes.push(box);
  }
  World.add(world, boxes);

  // mouse
  let canvasMouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse
  });
  World.add(world, mConstraint);
}

function draw() {
  background(240);
  Engine.update(engine);

  // cajas
  fill(200, 100, 100);
  for (let box of boxes) {
    push();
    translate(box.position.x, box.position.y);
    rotate(box.angle);
    rectMode(CENTER);
    rect(0, 0, 40, 40);
    pop();
  }

  // suelo
  fill(100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 40);
}
```
_Link: https://editor.p5js.org/LuisaRoech/sketches/z9y6h7VMG_

<img width="745" height="528" alt="image" src="https://github.com/user-attachments/assets/0e1133e9-530a-471a-a240-b55468433d5e" />

### _Experimento #2_

Retomando el ejemplo del péndulo, en este varias partículas están unidas tipo cadena, comportandose como una cuerda en el que puedes arrastrarla y ver como responde esta. Principalmente se experimenta la conexión, tensión y elasticidad (stiffness).

``` Js
let Engine = Matter.Engine;
let World = Matter.World;
let Bodies = Matter.Bodies;
let Constraint = Matter.Constraint;
let Mouse = Matter.Mouse;
let MouseConstraint = Matter.MouseConstraint;

let engine, world;
let particles = [];
let constraints = [];
let mConstraint;

function setup() {
  createCanvas(600, 400);

  engine = Engine.create();
  world = engine.world;

  // crear cadena
  let prev = null;
  for (let i = 0; i < 8; i++) {
    let p = Bodies.circle(200 + i * 40, 100, 10);
    particles.push(p);
    World.add(world, p);

    if (prev) {
      let c = Constraint.create({
        bodyA: prev,
        bodyB: p,
        length: 40,
        stiffness: 0.5
      });
      constraints.push(c);
      World.add(world, c);
    }
    prev = p;
  }

  // fijar el primero (como colgado)
  let fixed = Constraint.create({
    pointA: { x: 200, y: 50 },
    bodyB: particles[0],
    length: 0,
    stiffness: 1
  });
  World.add(world, fixed);

  // mouse
  let canvasMouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse
  });
  World.add(world, mConstraint);
}

function draw() {
  background(240);
  Engine.update(engine);

  // partículas
  fill(100, 150, 200);
  for (let p of particles) {
    circle(p.position.x, p.position.y, 20);
  }

  // conexiones
  stroke(0);
  for (let c of constraints) {
    line(
      c.bodyA.position.x,
      c.bodyA.position.y,
      c.bodyB.position.x,
      c.bodyB.position.y
    );
  }
}
```

_Link: https://editor.p5js.org/LuisaRoech/sketches/UdvXVnde4_

> [!NOTE]
> Entonces, ¿Qué me interesa más...? Si pienso en la palabra "guayaba" (DOLOR) seria en el peso/caída (cosas orgánicas, fruta madura) y la conexión (crecimiento, raíces, floración).

### Exploración de audio en p5.js (Actividad 03)

### _Experimento #1_

En este quitando la generalidad de la actividad, decidi enfocarlo a palabras guiadas a la principal (si, palabras dentro de la palabra), entonces para este primer experimento decidi manejar "Pulpa". La idea no es escalar un cículo, sino que la forma se vuelva blanda y reaccione como masa.

Entonces,

- Qué dato leo: Amplitud (volumen)
  
- Qué activa: Más "peso", más deformación

_Link: https://editor.p5js.org/LuisaRoech/full/-vXhd053z_

### _Experimento #2_

La idea en este es que varias partículas que se atraen o se dispersan según el sonido, como un 
"siste,a vivo" guiado por el sonido. Aunque spoiler, el sistema de este es más inestable por lo que se debe pensar mejor.

Entonces,

- Qué dato leo: Bajos (bass) → unión y agudos (treble) → separación

- Qué activa: El sistema cambia entre compacto (masa) y disperso (fragmentado)

_Link: https://editor.p5js.org/LuisaRoech/sketches/dGRAw8L41_

> [!NOTE]
> Al ver algo expandiendose, me recordo a mi pelicula favorita, _el color de las granadas_ en el que se encontraban tomas donde una mancha roja (de la fruta) se expandia en la tela, esto podria alejarme de lo repetitivo de mis trabajos, pensar más en la textura.

### Integración inicial de palabra, física y audio (Actividad 04)

En este experimento quise alejarme de mostrar la palabra de forma directa y trabajar más desde la sensación. Construí una especie de “tela” usando grain textures, como una superficie tensa, encima se encuentra la palabra "DOLOR" que se expande. El audio responde a esa expansión, esta no es quien la afecta realmente. Entre más se expande estas manchas, el sonido crece.

<img width="997" height="490" alt="image" src="https://github.com/user-attachments/assets/a5088517-43e5-4d42-93ac-e4c66c649db7" />

_Que cosas no funcionaron: Aunque los tiempos son corregibles, se distingue muy poco la palabra, las particulas se sienten como eso, particulas en vez de una mancha de sangre que se expande el concepto no esta mal pero se siente soso._

## Bitácora de aplicación 

# GUAYABAS (DOLOR) // Ahora CUT

Un cambio significativo en palabras, **_guayabas_** contiene un significado especial para mi pero al no ser global, es dificil representar esta palabra de forma más directa más allá de sus colores, no existe una libreria mental en lo que la gente piensa al ver esta palabra/fruta, pero esta palabra me guio al  **_dolor_** que es una sensación y sentimiento que se siente como una fruta exprimida, que mancha, que se expande y deja marca, que se diluye y pierde significado con el tiempo, pero aun sigue ahi. Ahora bien, esta palabra no requiere de fisicas para poder ser representada, por lo tanto forzarla a cumplir las condiciones pierde la intención dada. Asi que llegue a **_CUT_**, una palabra que puede derivar de dolor y puede cumplir sus mismas condiciones de otra forma, en esta la fisica cobra mas sentido y es una palabra más literal que no es dificil de representar.

> Análisis significado

**_CUT_** no es solo un gesto, es una interrupción que deja rastro. Es el momento en que algo continuo se rompe y revela que nunca fue tan sólido como parecía. Cortar no borra: abre. Hace visible una interioridad que normalmente permanece oculta, como si la superficie fuera apenas una promesa de estabilidad. La palabra no desaparece al ser herida; se separa, respira, tiembla, y luego intenta recomponerse. Ese vaivén convierte el corte en un lenguaje: cada trazo no destruye el significado, lo desplaza, lo pone en duda, lo vuelve inestable pero persistente.

> Moodboard

<img width="954" height="1411" alt="DOLOR_GUAYABA" src="https://github.com/user-attachments/assets/d29664cf-1d30-4f2e-a5e5-f22302b35bbf" />

<img width="1067" height="800" alt="image" src="https://github.com/user-attachments/assets/21de5941-ca2f-4283-9bd6-a87e55090d60" />

> Bocetos

N/A

> Mapa de desiciones

<img width="2727" height="572" alt="Mapa de desiciones" src="https://github.com/user-attachments/assets/9aa512b7-54f4-4ea8-8f92-17f23074f9df" />

> Mapa de interpretación

<img width="1045" height="1494" alt="Mapa de interpretación" src="https://github.com/user-attachments/assets/c60c10e4-3217-4bcc-ba2b-1e62a52233d6" />

> Audio & Comportamiento



> Evidencia uso de IA



> Código fuente

CUT CODE

``` Js
let Engine = Matter.Engine;
let World = Matter.World;
let Bodies = Matter.Bodies;
let Body = Matter.Body;

let engine, world;
let blocks = [];

let cutting = false;
let cutStart = null;
let cutEnd = null;

let cutData = null;

function setup() {
  let cnv = createCanvas(windowWidth, windowHeight);

  // 👇 importante: asegura foco para teclado
  cnv.mousePressed(() => {
    window.focus();
  });

  engine = Engine.create();
  world = engine.world;

  createWordCentered("CUT");
  createGround();
}

function draw() {
  background(240);

  Engine.update(engine);

  // 🧲 retorno suave
  for (let b of blocks) {
    if (!b.isStatic) {
      let home = createVector(b.home.x, b.home.y);
      let pos = createVector(b.position.x, b.position.y);

      let dir = p5.Vector.sub(home, pos);
      let d = dir.mag();

      if (d > 1) {
        dir.normalize();
        Body.applyForce(b, b.position, {
          x: dir.x * 0.0008,
          y: dir.y * 0.0008
        });
      }
    }
  }

  // 🧱 letras
  noStroke();
  fill(200, 0, 0);

  for (let b of blocks) {
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    rectMode(CENTER);
    rect(0, 0, b.w, b.h);
    pop();
  }

  // ✂️ línea guía
  if (cutting && cutStart && cutEnd) {
    stroke(180, 0, 0);
    strokeWeight(2);
    line(cutStart.x, cutStart.y, cutEnd.x, cutEnd.y);
  }

  // 🩸 herida
  if (cutData) {
    drawWound(cutData);
  }
}

// ----------------------------
// INPUT (solo corte)
// ----------------------------
function mousePressed() {
  cutting = true;
  cutStart = createVector(mouseX, mouseY);
  cutEnd = cutStart.copy();
}

function mouseDragged() {
  cutEnd = createVector(mouseX, mouseY);
}

function mouseReleased() {
  cutting = false;

  if (cutStart && cutEnd) {
    let speed = p5.Vector.dist(cutStart, cutEnd);
    applyCut(cutStart, cutEnd, speed);

    cutData = {
      start: cutStart.copy(),
      end: cutEnd.copy()
    };
  }

  cutStart = null;
  cutEnd = null;
}

// ----------------------------
// FULLSCREEN (FIX REAL)
// ----------------------------
function keyPressed() {
  // 👇 funciona incluso con layouts raros
  if (keyCode === 70) { // 70 = F
    fullscreen(!fullscreen());
  }
}

// 👇 respaldo más fiable que tecla
function doubleClicked() {
  fullscreen(!fullscreen());
}

// ----------------------------
// CREAR PALABRA CENTRADA
// ----------------------------
function createWordCentered(word) {
  blocks = [];

  let spacing = min(width, height) * 0.08;
  let totalWidth = word.length * spacing;

  let startX = width / 2 - totalWidth / 2;
  let startY = height / 2 - spacing;

  createWord(word, startX, startY, spacing);
}

function createWord(word, startX, startY, spacing) {
  for (let i = 0; i < word.length; i++) {
    createLetter(word[i], startX + i * spacing, startY, spacing);
  }
}

function createLetter(letter, x, y, spacing) {
  let pattern = [];

  if (letter === "C") {
    pattern = [
      [0,0],[1,0],[2,0],
      [0,1],
      [0,2],
      [0,3],
      [0,4],[1,4],[2,4]
    ];
  }

  if (letter === "U") {
    pattern = [
      [0,0],[2,0],
      [0,1],[2,1],
      [0,2],[2,2],
      [0,3],[2,3],
      [0,4],[1,4],[2,4]
    ];
  }

  if (letter === "T") {
    pattern = [
      [0,0],[1,0],[2,0],
              [1,1],
              [1,2],
              [1,3],
              [1,4]
    ];
  }

  let size = spacing / 4;

  for (let p of pattern) {
    let body = Bodies.rectangle(
      x + p[0] * size,
      y + p[1] * size,
      size,
      size,
      { isStatic: true }
    );

    body.w = size;
    body.h = size;
    body.home = { x: body.position.x, y: body.position.y };

    World.add(world, body);
    blocks.push(body);
  }
}

// ----------------------------
// SUELO
// ----------------------------
function createGround() {
  let ground = Bodies.rectangle(width / 2, height + 50, width, 100, {
    isStatic: true
  });
  World.add(world, ground);
}

// ----------------------------
// CORTE
// ----------------------------
function applyCut(start, end, speed = 1) {
  let cutVec = p5.Vector.sub(end, start);
  if (cutVec.mag() < 5) return;

  let dir = cutVec.copy().normalize();
  let normal = createVector(-dir.y, dir.x);

  let forceScale = map(speed, 0, 200, 0.002, 0.02);

  for (let b of blocks) {
    let pos = createVector(b.position.x, b.position.y);
    let d = distToSegment(pos, start, end);

    let radius = 30;

    if (d < radius) {
      Body.setStatic(b, false);

      let toPoint = p5.Vector.sub(pos, start);
      let side = Math.sign(p5.Vector.dot(toPoint, normal));

      let forceDir = p5.Vector.mult(normal, side);
      let strength = map(d, 0, radius, 1, 0);

      Body.applyForce(b, b.position, {
        x: forceDir.x * forceScale * strength,
        y: forceDir.y * forceScale * strength
      });
    }
  }
}

// ----------------------------
// HERIDA ORGÁNICA
// ----------------------------
function drawWound(cut) {
  let start = cut.start;
  let end = cut.end;

  let dir = p5.Vector.sub(end, start);
  let normal = createVector(-dir.y, dir.x).normalize();

  let thickness = min(width, height) * 0.03;

  noStroke();
  fill(20);

  beginShape();

  for (let i = 0; i <= 1; i += 0.05) {
    let p = p5.Vector.lerp(start, end, i);
    let offset = sin(i * PI) * thickness;
    let n = normal.copy().mult(offset);
    vertex(p.x + n.x, p.y + n.y);
  }

  for (let i = 1; i >= 0; i -= 0.05) {
    let p = p5.Vector.lerp(start, end, i);
    let offset = sin(i * PI) * thickness;
    let n = normal.copy().mult(-offset);
    vertex(p.x + n.x, p.y + n.y);
  }

  endShape(CLOSE);
}

// ----------------------------
// DISTANCIA
// ----------------------------
function distToSegment(p, v, w) {
  let l2 = dist(v.x, v.y, w.x, w.y) ** 2;
  if (l2 === 0) return dist(p.x, p.y, v.x, v.y);

  let t = ((p.x - v.x)*(w.x - v.x) + (p.y - v.y)*(w.y - v.y)) / l2;
  t = constrain(t, 0, 1);

  let projX = v.x + t*(w.x - v.x);
  let projY = v.y + t*(w.y - v.y);

  return dist(p.x, p.y, projX, projY);
}

// ----------------------------
// RESIZE
// ----------------------------
function windowResized() {
  resizeCanvas(windowWidth, windowHeight);

  engine = Engine.create();
  world = engine.world;

  blocks = [];

  createWordCentered("CUT");
  createGround();
}
```

> Link Sketch

_Link V1 (DOLOR): https://editor.p5js.org/LuisaRoech/sketches/e2N31XzRa_

_Link V2 (CUT): https://editor.p5js.org/LuisaRoech/sketches/e2N31XzRa_

> Obra

<img width="831" height="508" alt="image" src="https://github.com/user-attachments/assets/608eeb5e-8cdb-4081-b486-63b001dfa3d1" />

<img width="825" height="498" alt="image" src="https://github.com/user-attachments/assets/b60b5a36-b9c0-475a-b2cb-1f52606ba9e2" />

## Bitácora de reflexión

<img width="736" height="414" alt="image" src="https://github.com/user-attachments/assets/7b9841c6-9f70-4873-9d48-ba77206f217b" />

