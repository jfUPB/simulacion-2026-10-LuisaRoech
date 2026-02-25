# Unidad 3

## Bitácora de proceso de aprendizaje

### Magnetosphere (Actividad 01)

Hubo muchos momentos de la charla de Robert Hodgin en los que me sentí identificada. Uno de los temas que más resonó en mí fue el bloqueo artístico, algo muy común cuando se trata de crear. Es un estado difícil del que desprenderse; a veces parece que uno se queda atrapado en la sensación de no poder avanzar. En esos casos, comenzar poco a poco, haciendo cosas pequeñas, puede ser una forma de retomar el camino. Sin embargo, aunque suene lógico, no siempre es fácil.

La industria del entretenimiento se vuelve cada vez más demandante. Siempre habrá alguien que parezca estar por encima de ti, con más habilidades, más experiencia o más recursos. Ahora también están las inteligencias artificiales, capaces de comprender una problemática y proponer soluciones en cuestión de segundos. Frente a eso, es inevitable preguntarse: ¿cuál es entonces el propósito de lo que hago?

No se puede mejorar de forma inmediata. No se puede pasar del punto A al C sin atravesar el B. Es evidente, pero en la práctica resulta asfixiante. Vivimos en una época que exige resultados rápidos, adaptación constante y productividad permanente. Y aunque la llegada de la inteligencia artificial no es algo nuevo —siempre que surge una tecnología algunos trabajos cambian o desaparecen—, esta avanza a una velocidad que supera lo que imaginábamos. El miedo no solo es al reemplazo, sino a la incertidumbre. No hay ansiedad más profunda que la del futuro incierto, más allá incluso de la certeza de nuestra propia muerte.

Aunque prefiero no profundizar mucho en ese miedo. Más que centrarme en lo que podría reemplazarnos, quiero enfocarme en lo que plantea el video: **el propósito**.

No se trata necesariamente de hacer algo increíble, extraño, innovador o completamente distinto. Tampoco se trata de producir por producir. Se trata de hacer algo que se sienta propio. Auténtico. Algo que tenga sentido para ti, aunque no sea lo más rápido, lo más rentable o lo más visible. Crear hasta el punto en el que puedas sentir satisfacción personal, no solo validación externa.

En un mundo apresurado por obtener resultados inmediatos, elegir disfrutar el proceso se vuelve casi un acto de resistencia. Implica aceptar la frustración, el error y la lentitud como partes necesarias del camino. Implica preguntarte con honestidad qué quieres hacer de aquí en adelante, sin dejarte arrastrar únicamente por lo que el sistema exige.

Tal vez el propósito no esté en competir contra la velocidad del mundo, sino en encontrar un ritmo propio. En permitirte explorar, equivocarte y descubrir. En crear no para ganar, sino para comprenderte. Y quizás ahí, en esa decisión consciente de hacer algo que sientas verdadero, es donde todavía vale la pena.

Y aun asi, no puedo evitar en pensar que en este mundo moderno evitamos la fricción. evitamos el aburrimiento. evitamos el no saber. Queremos respuestas antes de hacernos las preguntas. Queremos resultados antes de atravesar el proceso. Y a veces siento que, más que cansancio, es esa huida constante lo que pesa.

<img width="800" height="626" alt="image" src="https://github.com/user-attachments/assets/db71115a-4ed8-4b94-bcc9-4cde13ec4298" />

_Automat – Edward Hopper_

"–Por aquí hay una pregunta

¿Qué consejo puede darle a los jóvenes artistas de Colombia?

El autor se tomó un sorbo de agua y respondió

–En este mundo es preciso pararse y elegir

El oro o los espejos

En lo personal, yo recomiendo siempre los espejos

Pues de nada sirve la plata en la cuenta

Sin la belleza misteriosa de las cosas

El auditorio estalló en aplausos de júbilo ante la respuesta del autor

Quien, sin que nadie pudiera ya oírlo

Mirando al piso

Concluyó

–Y sin embargo

De algo hay que vivir"

_No es trabajo pero cansa - Nicolas y los fumadores_

### Motion 101 (Actividad 02)

Cada objeto suele tener tres vectores: posición, velocidad y aceleración. Las fuerzas (como gravedad o viento) también son vectores. Todas las fuerzas se suman, esa suma se convierte en aceleración (según F = m·a), la aceleración modifica la velocidad, y la velocidad modifica la posición en cada frame. Es un ciclo continuo que produce movimiento
 
Por ejemplo: imagina una partícula en pantalla. Le aplicas una fuerza de gravedad hacia abajo y una fuerza de viento hacia la derecha. Al sumar esos vectores, la partícula no cae recta ni se mueve solo a la derecha: se mueve en diagonal. Si agregas fricción, el movimiento se suaviza. Si aumentas la masa, la misma fuerza la afecta menos. Todo es combinación de vectores.

### Tres obras (Actividad 03)

1. Fricción.

La fricción es simplemente una fuerza opuesta a la velocidad:

``` Javascript
friction = velocity.copy()
friction.mult(-1)
friction.normalize()
friction.mult(coef)
```

En este la idea fue crear partículas lanzadas al azar que poco a poco pierden energía, como recuerdos que se apagan.

``` Javascript
 let particles = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(10, 20);

  if (mouseIsPressed) {
    particles.push(new Particle(mouseX, mouseY));
  }

  for (let p of particles) {
    p.update();
    p.display();
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(2, 5));
    this.acc = createVector(0, 0);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    let friction = this.vel.copy();
    friction.mult(-1);
    friction.normalize();
    friction.mult(0.05); // coeficiente de fricción
    this.applyForce(friction);

    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    noStroke();
    fill(200, 150);
    circle(this.pos.x, this.pos.y, 6);
  }
}
```

2. Resistencia al aire y fluidos

Para esta fuerza que depende de la velocidad, cree objetos cayendo en un “líquido invisible” en la mitad inferior del canvas (un tanto similar a los ejemplos _the nature of code_).

``` Js
let movers = [];

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 10; i++) {
    movers.push(new Mover(random(width), random(-200, 0), random(1, 4)));
  }
}

function draw() {
  background(15);

  fill(40, 60, 120, 100);
  rect(0, height/2, width, height/2); // zona de fluido

  for (let m of movers) {
    let gravity = createVector(0, 0.1 * m.mass);
    m.applyForce(gravity);

    if (m.pos.y > height/2) {
      let drag = m.vel.copy();
      let speed = drag.mag();
      drag.mult(-1);
      drag.normalize();
      drag.mult(0.02 * speed * speed);
      m.applyForce(drag);
    }

    m.update();
    m.display();
  }
}

class Mover {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.mass = m;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    fill(200);
    circle(this.pos.x, this.pos.y, this.mass * 8);
  }
}
```

3. Atracción gravitacional.

La fuerza gravitacional depende de la distancia (F ∝ (m1 * m2) / distancia²) por ello experimente con partículas orbitando un centro como si fuera un núcleo cósmico.

``` Js
let attractor;
let movers = [];

function setup() {
  createCanvas(600, 400);
  attractor = new Attractor(width/2, height/2);

  for (let i = 0; i < 20; i++) {
    movers.push(new Mover(random(width), random(height), random(1, 3)));
  }
}

function draw() {
  background(5, 10, 20, 50);

  attractor.display();

  for (let m of movers) {
    let force = attractor.attract(m);
    m.applyForce(force);
    m.update();
    m.display();
  }
}

class Attractor {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.mass = 20;
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distance = constrain(force.mag(), 5, 25);
    force.normalize();

    let G = 1;
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);

    return force;
  }

  display() {
    fill(255, 200);
    circle(this.pos.x, this.pos.y, 30);
  }
}

class Mover {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.mass = m;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    fill(180);
    circle(this.pos.x, this.pos.y, this.mass * 6);
  }
}
``` 

Todas estas obras comparten muchas cosas en común, lo unico que cambia es la forma en como calculo sus fuerzas. Eso es Motion 101 aplicado al arte generativo.

## Bitácora de aplicación 

### (Actividad 04)

> La historia
> 

## Bitácora de reflexión

### (Actividad 05)










