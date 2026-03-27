# Unidad 5

## Bitácora de proceso de aprendizaje

### Anatomía de una partícula (Actividad 01)

Analizando el ejemplo 4.2, `Update()` es quien abarca el marco motion 101 de este sistema, actualizando la particula en cada frame. Dentro de este, algo interesante es que se reduce de forma gradual la vida de la particula, quien inicialmente esta dada por `this.lifespan = 255.0;`, lo que indica un desvanecimiento progresivo en lugar de una desaparición instantánea.

``` Js
 // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }
```

El constructor define las propiedades principales de la particula y su clase es quien las crea, despues en `sketch.js ` se recorren para evaluar su estado (si estan vivas o muertas). Ahora bien, si las partículas no se eliminaran, el sistema acumularía cada vez más instancias, aumentando el uso de memoria y afectando el rendimiento (frame rate).

Aunque dentro de este código existe una estructura interesante al momento de leer la vida de una particula, en vez de leerla de la forma tradicional esta se lee al reves usando `let i = particles.length - 1;` (lo clave esta en el -1). Esto se debe a que se emplea `particles.splice(i, 1);` para eliminar partículas del arreglo; si se recorriera de forma tradicional (de inicio a fin), los índices cambiarían al eliminar elementos, provocando que algunas partículas no se procesen correctamente dentro del `for`.

### Del array al sistema: la abstracción del emisor (Actividad 02)

Del ejemplo 4.4 lo importante y diferenciador del ejemplo 4.2 (el anterior) esta en el `emitter.js`. Quien ahora se encarga de agregar un origen que generará las particula en base donde se ha presionado con el mouse, al hacer click las crea y el chequeo `for` que se encarga de leer el estado de una particula ahora se encuentra en esta. 
Pero ¿por qué separar esto en una nueva clase? Principalmente para que sea modular, cada emisor funciona de forma independiente, se puede tener múltiples sistemas sin duplicar la lógica, puede servir en distintos contextos y separa "quién emite" de "qué se emite".

¿Quién crea qué?

- Emitters → los crea el _sketch_ (por ejemplo en `setup()` o eventos)
- Partículas → las crea cada Emitter

> Diagrama

```
[ Sketch ]
     ↓
[ Emitters ]  →  [Emitter 1] → [Partículas]
              →  [Emitter 2] → [Partículas]
              →  [Emitter 3] → [Partículas]
```

> El sistema está organizado como: una entidad principal contiene múltiples emisores, y cada emisor gestiona su propia colección de partículas. Los niveles de colección son 1. Array de emitters y 2. Array de particulas dentro de cada emitter.

### Heterogeneidad: herencia y polimorfismo (Actividad 03)

Del ejemplo 4.5 se incluye dos tipos distintos de particulas, en la que `confetti.js` hereda de la otra particula `particle.js`, comparten el mismo constructor, las mismas fuerzas, el ciclo de vida y la misma estructura del motion 101, lo unico que se diferencia la una de la otra es en el `show()` que permite que tengan una forma distinta, una esta dada por cuadros y la otra por circulos.

Cosas interesantes explicadas es del porque el `emitter.js` no necesita saber que tipo de particula esta gestionando. estas funciones en general son interesantes:

``` Js
 addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

La razón principal es para hacer el sistema más flexible y extensible. En `run()` se ejecuta las dos particulas agregadas anteriormente en `addParticle()`, por lo que no es necesario tocar esta función al momento de implementar nuevas cosas, pues esta independientemente si se agregan mas o menos ejecutara todas las que estan listadas en `addParticle()`, que justo con esta tambien podria realizarse lo mismo agregrando un objeto que tome `particle.js` (si suponemos que todas las particulas van a heredar de esta como lo hizo confetti) y se ejecute de forma general. Lo unico malo es que es un proceso más lento y menos eficiente que agregar una por una, ya que se llama una por una para ejecutarla y no todas al tiempo.

> En resumen: El Emitter trata a todas las partículas como “entidades genéricas” sin importar cómo se vean.

Ahora si quiero agregar una nueva particula, solo es necesario crear un nuevo archivo .js que herede de particle, llamar las funciones que quiero retormar de esta y crear otras, despues simplemente este nuevo archivo debe ser llamado en el emitter en `addParticle()`. _NO MODIFICAR: el emiiter, sketch y la lógica de vida o actualización base._

###  Fuerzas y partículas (Actividad 04)

- Ejemplo 4.6: La gravedad se define en el _sketch_ como un vector constante y el sistema (el emitter) la aplica usando `applyForce()` sobre cada particula. Siendo global, pues es la misma para todas las partículas y no depende de su posición
  
- Ejemplo 4.7: por otro lado en este la gravedad se mantiene constante que ¨vive¨ fuera, como condicion general y se incluye un repeller que depende de la distancia que "vive" en una entidad específica siendo una fuente localizada de fuerza.

_Nota: El repeller se basa en una fuerza inversamente proporcional a la distancia (tipo ley de atracción/repulsión, similar a gravedad o Coulomb) = A menor distancia, mayor fuerza._

Y de estos dos ejemplos `particle.js` no cambia, la partícula no "sabe" de dónde vienen las fuerzas, solo sabe cómo aplicarlas. 

_Tabla_

| Aspecto | 4.2 | 4.4 | 4.5 | 4.6 | 4.7 |
|--------|-----|-----|-----|-----|-----|
| ¿Quién crea partículas? | _Sketch_ | _Emitter_ | _Emitter_ | _Emitter_ | _Emitter_ |
| ¿Hay clase Emitter? | NO | SI | SI | SI | SI |
| ¿Hay herencia? | NO | NO | SI | NO | NO |
| ¿Hay fuerzas externas? | NO | NO | NO | SI | SI |
| ¿Hay interacción? | NO | SI(?) | NO | NO | SI |
| ¿Cómo mueren? | _Lifespan_ | _Lifespan_ | _Lifespan_ | _Lifespan_ | _Lifespan_ |

**MODIFICACIÓN QUIRÚRGICA**

Para este ejercicio decidi tomar el **(b)** Cambiar las fuerzas sin cambiar la estructura ni la visualización. Reemplazando la gravedad por una que oscile:

``` Js
let g = map(sin(frameCount * 0.05), -1, 1, 0.05, 0.2);
let gravity = createVector(0, g);
emitter.applyForce(gravity);
```

> código (solo _sketch_ que fue el que modifique)

``` Js
// One ParticleSystem
let emitter;

//{!1} One repeller
let repeller;

function setup() {
  createCanvas(640 , 240);
  emitter = new Emitter(width / 2, 60);
  repeller = new Repeller(width / 2, 250);
}

function draw() {
  background(255);
  emitter.addParticle();
  let g = map(sin(frameCount * 0.05), -1, 1, 0.05, 0.2);
let gravity = createVector(0, g);
emitter.applyForce(gravity);
  //{!1} Applying the repeller
  emitter.applyRepeller(repeller);
  emitter.run();

  repeller.show();
}
```

## Bitácora de aplicación 

### Ciclo de vida (Actividad 05)

> concepto

Siempre me resulta natural comenzar a desarrollar una obra a partir de algo reciente que me haya impactado. En este caso, dejo de lado mi interés en las aves para centrarme en este poema, el cual explora una relación profundamente ambigua entre madre e hijo.

```
Mother, mother... Mother of me,

I know I know I should not miss you so, but mother of me, I do. Your
pained breaths that rasp'd and reverberated in your rusted iron tomb...
The blood of your breast that nourish'd me and warmed me in its caress,
when corpse and cruelty were all I witnessed...

Mother, mother... Mother of me,

I know I know you would hate me so, and mother of me, I do too. But I
would not feel, not think, not dream, were it not for you in my rusted
iron womb... Your tortured love brought me to this war, that I could
take the heart of another, and need you no more.

Mother, mother... Mother of me,

I know I know your thoughts had left you long ago, and mother of me, I
will never truly know. But I hope it redeems my life even just a slight,
when I cried... And crushed your skull that final night.
```

La obra se construye a partir de una dependencia afectiva extrema, donde el hijo existe gracias a la madre, pero al mismo tiempo es moldeado por su sufrimiento, su deterioro y su incapacidad de brindar un amor sano. La madre es simultáneamente origen, refugio y prisión.

Esta relación no es solo de cuidado, sino de violencia, culpa y transformación. El hijo reconoce que todo lo que es proviene de ella —su capacidad de sentir, pensar y existir—, pero también que ese origen está marcado por el dolor. Esto genera una dualidad constante entre amor y rechazo, donde la necesidad de liberarse implica necesariamente destruir aquello que le dio vida.

El acto final no se presenta únicamente como violencia, sino como una forma ambigua de emancipación, redención o ruptura, en la que el hijo deja de depender de la madre al costo de perderla.

> Boceto

1 Boceto conceptual

<img width="500" height="500" alt="Boceto concep (2)" src="https://github.com/user-attachments/assets/70149833-330d-4c87-a129-240447d9d28c" />

2 Boceto visual

<img width="500" height="500" alt="Boceto visual (2)" src="https://github.com/user-attachments/assets/2a3dbbc1-b309-402f-a8bf-65dc92c070c5" />

> Mapa de desiciones

**Opción 1 Hijo**

![Mother of me - Mapa de desiciones](https://github.com/user-attachments/assets/a7a13d31-6a27-4816-8ba3-5db1853ea4e0)

**Opción 2 Madre**

![Mother of me - Mapa de desiciones (1)](https://github.com/user-attachments/assets/8313fe37-32a2-4a21-85b2-bfa92aa7370b)

_Link:https://miro.com/welcomeonboard/WTBLekZOa2YxYk9GQTRpU3lIbm13QTZVS1FBeExNc2N4NnZORkY2d283L3R4Z1I0aUpVd3RaMXlVRUt6Z0JHMzlWVWpTZnptS1EvaFBSVjVNMFBEUnZrMlRUbHB5SzlHdHcrZ1E2eWpJbWhha08yRWlzT2kyckc4WjIrOWlieDJ3VHhHVHd5UWtSM1BidUtUYmxycDRnPT0hdjE=?share_link_id=752482876973_

> Código

PARTICLE.JS
``` Js
class Particle {
  constructor(x, y, tipo = "hijo") {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.5, 2));
    this.life = 255;
    this.tipo = tipo;
  }

  update(mother) {
    let n = noise(this.pos.x * 0.01, this.pos.y * 0.01, frameCount * 0.01);
    let angle = n * TWO_PI * 2;

    let flow = p5.Vector.fromAngle(angle);
    flow.mult(0.3);

    this.vel.add(flow);

    if (this.tipo === "madre") {
      let centro = p5.Vector.sub(mother, this.pos);
      centro.mult(0.01);
      this.vel.add(centro);
      this.vel.limit(1.5);
    } else {
      this.vel.limit(2.5);
    }

    this.pos.add(this.vel);
    this.life -= 2;
  }

  draw(nutricion, tipoMuerte) {
    noStroke();

    let col;

    if (this.tipo === "madre") {
      col = color(150, 50, 50, 80);
    } else {
      col = lerpColor(
        color(50, 100, 255),
        color(255, 50, 50),
        nutricion
      );
    }

    if (tipoMuerte === "sobrecarga") {
      col = color(255, 80, 80);
    }

    fill(red(col), green(col), blue(col), this.life);
    ellipse(this.pos.x, this.pos.y, 3);
  }

  isDead() {
    return this.life < 0;
  }
}
```

SKETCH.JS
``` Js
// ==========================
// VARIABLES
// ==========================
let mother;
let child;

let nutricion = 0.5;
let muerto = false;
let tipoMuerte = "";

let cordonRoto = false;
let tiempoMuerte = 0;

// cola
let tail = [];
let tailLength = 25;

let particles = [];

// ==========================
// SETUP
// ==========================
function setup() {
  createCanvas(800, 600);
  mother = createVector(width / 2, height / 2);
  child = createVector(width / 2 + 120, height / 2);

  for (let i = 0; i < tailLength; i++) {
    tail.push(createVector(child.x, child.y));
  }
}

// ==========================
// DRAW
// ==========================
function draw() {
  background(5, 5, 10);

  mother.x = mouseX;
  mother.y = mouseY;

  if (!muerto) {
    actualizarSistema();
  } else {
    comportamientoPostMuerte();
  }
  
  aplicarLimitesSuaves();

  actualizarCola();

  dibujarCordon();
  dibujarCola();
  
  sistemaParticulas();   
  dibujarParticulas();  
  
  dibujarHijo();
  dibujarMadre();

  if (muerto) efectosMuerte();
}

// ==========================
// SISTEMA
// ==========================
function actualizarSistema() {
  let d = dist(mother.x, mother.y, child.x, child.y);

  let maxDist = 300;
  let proximidad = 1 - constrain(d / maxDist, 0, 1);

  nutricion = lerp(nutricion, proximidad, 0.05);

  // fuerza hacia la madre
  let dir = p5.Vector.sub(mother, child);
  dir.mult(0.02 * nutricion);

  //  RECHAZO SI SE ACERCA A SOBRECARGA
  if (nutricion > 0.8) {
    let repulsion = p5.Vector.sub(child, mother);
    repulsion.normalize();
    repulsion.mult(0.05 * map(nutricion, 0.8, 1, 0.5, 2));

    dir.add(repulsion);
  }

  child.add(dir);

  // inestabilidad
  if (nutricion < 0.3) {
    child.x += random(-2, 2);
    child.y += random(-2, 2);
  }

  // ======================
  // MUERTES
  // ======================
  if (d > 350) muerte("distancia");

  if (nutricion <= 0.05) muerte("abandono");

  if (nutricion >= 0.98) muerte("sobrecarga");
}

// ==========================
// LIMITES SUAVES
// ==========================

function aplicarLimitesSuaves() {
  let margen = 80;
  let fuerza = 0.05;

  // izquierda
  if (child.x < margen) {
    child.x += (margen - child.x) * fuerza;
  }

  // derecha
  if (child.x > width - margen) {
    child.x -= (child.x - (width - margen)) * fuerza;
  }

  // arriba
  if (child.y < margen) {
    child.y += (margen - child.y) * fuerza;
  }

  // abajo
  if (child.y > height - margen) {
    child.y -= (child.y - (height - margen)) * fuerza;
  }
}

// ==========================
// COMPORTAMIENTO POST MUERTE
// ==========================
function comportamientoPostMuerte() {
  tiempoMuerte++;

  if (tipoMuerte === "sobrecarga") {
    //  sigue huyendo incluso muerto
    let repulsion = p5.Vector.sub(child, mother);
    repulsion.normalize();
    repulsion.mult(2);

    child.add(repulsion);

    // movimiento errático
    child.x += random(-3, 3);
    child.y += random(-3, 3);
  }

  if (tipoMuerte === "abandono") {
    child.lerp(mother, 0.03);
  }
}

// ==========================
// MUERTE
// ==========================
function muerte(tipo) {
  muerto = true;
  tipoMuerte = tipo;
  tiempoMuerte = 0;

  if (tipo === "distancia") {
    cordonRoto = true;
  }
}

// ==========================
// COLA
// ==========================
function actualizarCola() {
  tail[0].lerp(child, 0.4);

  for (let i = 1; i < tail.length; i++) {
    let prev = tail[i - 1];
    let current = tail[i];

    let dir = p5.Vector.sub(prev, current);

    // más agresiva en sobrecarga
    let fuerza = (tipoMuerte === "sobrecarga") ? 0.5 : 0.3;

    dir.mult(fuerza);
    current.add(dir);

    let ruido = (tipoMuerte === "sobrecarga") ? 3 : 1.5;

    current.x += map(noise(i, frameCount * 0.02), 0, 1, -ruido, ruido);
    current.y += map(noise(i + 100, frameCount * 0.02), 0, 1, -ruido, ruido);
  }
}

// ==========================
// DIBUJAR COLA
// ==========================
function dibujarCola() {
  noFill();

  for (let i = 0; i < tail.length - 1; i++) {
    let t = i / tail.length;

    let grosor = map(t, 0, 1, 6, 0.5);
    strokeWeight(grosor);

    let col = lerpColor(
      color(220, 220, 220, 180),
      color(80, 80, 80, 40),
      t
    );

    if (tipoMuerte === "sobrecarga") {
      col = color(255, 80, 80, 120);
    }

    stroke(col);

    line(
      tail[i].x,
      tail[i].y,
      tail[i + 1].x,
      tail[i + 1].y
    );
  }
}

// ==========================
// CORDÓN
// ==========================
function dibujarCordon() {
  if (cordonRoto) return;

  let fibras = 6;

  for (let j = 0; j < fibras; j++) {
    strokeWeight(random(1, 2));

    let col = lerpColor(color(80, 20, 20), color(200, 0, 0), nutricion);
    stroke(col);
    noFill();

    beginShape();

    let steps = 20;

    for (let i = 0; i <= steps; i++) {
      let t = i / steps;

      let x = lerp(mother.x, child.x, t);
      let y = lerp(mother.y, child.y, t);

      let offset = map(
        noise(t * 5, frameCount * 0.02 + j * 10),
        0, 1,
        -15, 15
      );

      x += offset;
      y += offset;

      vertex(x, y);
    }

    endShape();
  }
}

// ==========================
// HIJO
// ==========================
function dibujarHijo() {
  push();
  translate(child.x, child.y);

  noStroke();

  let size = map(nutricion, 0, 1, 12, 25);

  if (muerto && tipoMuerte === "abandono") {
    fill(120, 120, 120, 150);
  } 
  else if (tipoMuerte === "sobrecarga") {
    fill(255, 80, 80, 150);
    size *= random(0.8, 1.4);
  } 
  else if (tipoMuerte === "distancia") {
    fill(150, 0, 0, 150);
  } 
  else {
    fill(230, 230, 230, 200);
  }

  ellipse(0, 0, size * 1.4, size);

  pop();
}

// ==========================
// MADRE
// ==========================
function dibujarMadre() {
  noStroke();

  let baseSize = 120;

  if (muerto && tipoMuerte === "abandono") {
    fill(80, 80, 80, 60);
    baseSize *= 0.8;
  } 
  else if (muerto && tipoMuerte === "distancia") {
    fill(50, 20, 20, 50);
    baseSize *= 0.6;
  } 
  else if (tipoMuerte === "sobrecarga") {
    fill(200, 50, 50, 100);
    baseSize *= 1.5;
  } 
  else {
    fill(120, 40, 40, 80);
  }

  beginShape();
  for (let a = 0; a < TWO_PI; a += 0.1) {
    let r = baseSize + noise(a, frameCount * 0.02) * 40;

    let x = mother.x + cos(a) * r;
    let y = mother.y + sin(a) * r;

    vertex(x, y);
  }
  endShape(CLOSE);
}

// ==========================
// EFECTOS
// ==========================
function efectosMuerte() {
  if (tipoMuerte === "abandono") {
    fill(0, 10);
    rect(0, 0, width, height);
  }
}

// ==========================
// DIBUJAR PARTICULAS
// ==========================
function dibujarParticulas() {
  for (let p of particles) {
    p.draw(nutricion, tipoMuerte);
  }
}

// ==========================
// PARTICULAS
// ==========================
function sistemaParticulas() {

  // ======================
  // HIJO
  // ======================
  for (let i = 0; i < 3; i++) {
    particles.push(new Particle(child.x, child.y, "hijo"));
  }

  // ======================
  // MADRE 
  // ======================
  for (let i = 0; i < 2; i++) {
    let angle = random(TWO_PI);
    let r = random(20, 80);

    let x = mother.x + cos(angle) * r;
    let y = mother.y + sin(angle) * r;

    particles.push(new Particle(x, y, "madre"));
  }

  // ======================
  // CORDÓN (opcional)
  // ======================
  if (!cordonRoto) {
    let t = random();
    let x = lerp(mother.x, child.x, t);
    let y = lerp(mother.y, child.y, t);

    particles.push(new Particle(x, y, "hijo"));
  }

  // ======================
  // UPDATE
  // ======================
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];

    p.update(mother);

    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

// ==========================
// RESET
// ==========================
function mousePressed() {
  muerto = false;
  cordonRoto = false;
  nutricion = 0.5;
  tipoMuerte = "";

  child = createVector(width / 2 + 120, height / 2);

  tail = [];
  for (let i = 0; i < tailLength; i++) {
    tail.push(createVector(child.x, child.y));
  }
}
```
_link: https://editor.p5js.org/LuisaRoech/sketches/5p-l5PqYa_

> Obra

https://github.com/user-attachments/assets/e8623f10-5b22-47ed-b29d-6d9dfdf3e431

## Bitácora de reflexión

### (Actividad 06)

**PARTE 1** — Describe con tus propias palabras cada uno de estos 10 principios:

1. Una partícula es una entidad con estado.
2. Una partícula tiene ciclo de vida.
3. Un sistema de partículas gestiona colecciones dinámicas de elementos.
4. La creación y eliminación de partículas no es un detalle técnico menor, sino parte central del modelo.
5. Debe haber separación entre la lógica de una partícula individual y la lógica del sistema/emisor.
6. Un emisor o particle system es una abstracción importante.
7. Pueden existir sistemas de sistemas.
8. Puede haber heterogeneidad usando herencia y polimorfismo.
9. Las partículas pueden responder a fuerzas globales y locales.
10. La representación visual puede variar sin cambiar el principio algorítmico de fondo.

**PARTE 2** — Transferencia a otra herramienta

Piensa en tu pieza del Apply: si la quisieras recrear en Unity (o TouchDesigner, o Blender), ¿Qué se mantendría igual y qué cambiaría? ¿Qué partes de tu diseño son independientes de la herramienta?
