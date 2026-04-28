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
> Entonces, ¿Qué me interesa más...? Si pienso en la palabra "guayaba" seria en el peso/caída (cosas orgánicas, fruta madura) y la conexión (crecimiento, raíces, floración).

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

En este experimento quise alejarme de mostrar la palabra de forma directa y trabajar más desde la sensación. Construí una especie de “tela” usando física, como una superficie tensa, y debajo hay una masa rosada que empuja constantemente, como si fuera pulpa tratando de salir. El audio afecta ese comportamiento: los bajos hacen que la presión aumente y la tela se deforme más, mientras que los agudos generan vibración e inestabilidad. La palabra “guayabas” no aparece como texto plano, sino que se revela cuando la tela se deforma, como si estuviera atrapada debajo y solo se dejara ver en ciertos momentos. Es más una presencia que una lectura directa.



## Bitácora de aplicación 

# GUAYABAS



## Bitácora de reflexión
