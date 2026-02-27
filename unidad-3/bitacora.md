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

```
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
```
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

### Las bandadas de aves (Actividad 04)

> La historia

Es gracioso, pero después de la charla sobre arte generativo, la inteligencia artificial y el como estamos aplicando esto en los trabajos, sentí que lo que he hecho hasta ahora se percibe vacío. Y eso me frustra, porque amo el arte y la creación.

Me inquieta la idea de que, al trabajar con sistemas generativos, estoy construyendo algo donde no tengo control absoluto. Una obra basada en condiciones y reglas nunca será tan específica o detallada como una pintura tradicional o una pieza completamente dirigida por la mano humana.

Entonces me pregunto: ¿qué hacer?

Quizás la respuesta no es intentar controlar todo desde el inicio, sino comenzar con algo simple, más manejable, y permitir que crezca hacia algo menos predecible. El comportamiento de las bandadas de aves puede parecer un recurso común dentro del arte generativo. Sin embargo, quiero experimentarlo por mi cuenta, no como efecto visual, sino como estructura conceptual.

Las aves, simbólicamente, cargan significados profundos en culturas del Asia del Sur y Asia Central — regiones que últimamente me interesan mucho, especialmente India. En tradiciones sufíes del sur de Asia (Pakistán y norte de India), el ave representa el alma humana en búsqueda de lo divino. El vuelo es metáfora del viaje espiritual: desplazamiento, transformación, tránsito entre estados.

En Asia Central — Mongolia, Irán, Kazajistán, Uzbekistán — el simbolismo cambia de matiz, pero mantiene su fuerza. En el tengrianismo (Tengrism), el cielo no es decorativo: es lo absoluto. Las aves, especialmente el águila — como el Golden eagle — son las únicas criaturas capaces de habitar ese dominio. Son mediadoras entre humanos y cielo, símbolos de poder, protección y legitimidad.

En ambos contextos, el vuelo implica trascendencia.
Pero la bandada introduce un significado extra, el individuo no existe aislado. La revelación no está en el ave sola, sino en el conjunto. La divinidad — o la conciencia — emerge colectivamente.

Y mientras pensaba en todo esto, recordé el nuevo álbum de Gorillaz, _The Mountain_. Más allá de si el disco está inspirado directamente en Asia Central o no, sí toma influencias del sur de Asia y se nutre de experiencias ligadas a espiritualidad, duelo, tránsito y viaje cultural.

```
Así como el águila acompaña al viajero en la estepa,
mi obra busca acompañar el tránsito entre control y pérdida de control.

En las culturas de Asia Central, especialmente en aquellas vinculadas al Tengrism, el cielo no es un fondo: es lo absoluto.
El águila históricamente en la cetrería nómada — no representa únicamente poder o libertad, sino mediación.
Es puente entre la tierra y el cielo, entre lo humano y lo infinito.
```

> ¿Como implemento interactividad?

Ya que no puedo enloquecerme con el scope de la obra (aunque me gustaria) pienso en algo simple, interacción con mouse donde los boids evitan el cursor. O lo siguen suavemente. O se dividen cuando el cursor entra.

El usuario cree que controla… pero en realidad solo altera el sistema, no lo domina.

> El codigo

```
let flock = [];
let totalBoids = 140;

let mouseStillTime = 0;
let lastMouse;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(18, 12, 8);
  lastMouse = createVector(mouseX, mouseY);

  for (let i = 0; i < totalBoids; i++) {
    flock.push(new Boid(random(width), random(height)));
  }
}

function draw() {
  // Fondo desértico nocturno con estela cálida
  fill(18, 12, 8, 40);
  noStroke();
  rect(0, 0, width, height);

  // polvo sutil
  for (let i = 0; i < 3; i++) {
    stroke(120, 90, 50, 15);
    point(random(width), random(height));
  }

  // detectar si el mouse está quieto
  let currentMouse = createVector(mouseX, mouseY);
  if (p5.Vector.dist(currentMouse, lastMouse) < 2) {
    mouseStillTime++;
  } else {
    mouseStillTime = 0;
  }
  lastMouse = currentMouse.copy();

  for (let boid of flock) {
    boid.edges();

    if (mouseStillTime > 120) {
      boid.ritualOrbit(mouseX, mouseY);
    } else {
      boid.flock(flock);
    }

    boid.update();
    boid.show();
  }
}

class Boid {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(1, 2));
    this.acceleration = createVector(0, 0);

    this.mass = 1;
    this.maxForce = 0.12;
    this.maxSpeed = 2.4;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  // Fuerza vertical sutil (Tengri / cielo absoluto)
  skyPull() {
    return createVector(0, -0.015);
  }

  ritualOrbit(mx, my) {
    let center = createVector(mx, my);
    let toCenter = p5.Vector.sub(center, this.position);
    let distToCenter = toCenter.mag();

    // mantener órbita amplia
    if (distToCenter > 200) {
      toCenter.setMag(0.05);
      this.applyForce(toCenter);
    }

    // rotación tangencial
    let tangent = createVector(-toCenter.y, toCenter.x);
    tangent.normalize();
    tangent.mult(0.08);

    this.applyForce(tangent);
    this.applyForce(this.skyPull());
  }

  flock(boids) {
    let alignment = this.align(boids);
    let cohesion = this.cohesion(boids);
    let separation = this.separation(boids);

    this.applyForce(alignment);
    this.applyForce(cohesion);
    this.applyForce(separation);
    this.applyForce(this.skyPull());
  }

  align(boids) {
    let perception = 60;
    let steering = createVector(0, 0);
    let total = 0;

    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perception) {
        steering.add(other.velocity);
        total++;
      }
    }

    if (total > 0) {
      steering.div(total);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce);
    }

    return steering;
  }

  cohesion(boids) {
    let perception = 70;
    let steering = createVector(0, 0);
    let total = 0;

    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perception) {
        steering.add(other.position);
        total++;
      }
    }

    if (total > 0) {
      steering.div(total);
      steering.sub(this.position);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce);
    }

    return steering;
  }

  separation(boids) {
    let perception = 40;
    let steering = createVector(0, 0);
    let total = 0;

    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perception) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.normalize();
        diff.div(d);
        steering.add(diff);
        total++;
      }
    }

    if (total > 0) {
      steering.div(total);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce * 1.3);
    }

    return steering;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());

    let flap = sin(frameCount * 0.4 + this.position.x * 0.05) * 4;

    // aura cálida
    noStroke();
    fill(255, 180, 90, 25);
    ellipse(0, 0, 28);

    fill(255, 210, 140, 220);
    beginShape();
    vertex(12, 0);
    vertex(-4, -10 - flap);
    vertex(-8, 0);
    vertex(-4, 10 + flap);
    endShape(CLOSE);

    pop();
  }
}
```

> La obra



## Bitácora de reflexión

### (Actividad 05)


















