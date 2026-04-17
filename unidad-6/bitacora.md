# Unidad 6

## Bitácora de proceso de aprendizaje

<img width="300" height="294" alt="Captura de pantalla 2026-04-16 173147-Photoroom (1)" src="https://github.com/user-attachments/assets/5201e062-fef6-4450-868a-e2e91cb87c6a" />

_The Blob's Groovy Dance | Mesmerizing Physarum_

### Un referente para pensar sistemas visuales (Actividad 01)

<img width="1778" height="702" alt="image" src="https://github.com/user-attachments/assets/b16fcacf-d57e-4d73-b61c-17d645c9f91f" />

_One One Overflow - Tyler Hobbs_

Esta obra esta compuesta por bandas horizontales que ocupan todo el plano, sin jerarquía evidente. No se encuentra un foco en especial, solo tu mirada se desplaza lateralmente. Esto rompe con la composición tradicional, estas lineas generan una corriente pero hay zonas que generan casi ruido visual por la acumulación de elementos extrema y otras ocurre lo opuesto. El ojo busca descanso, pero nunca lo encuentra del todo. El azul funciona como base estructural con acentos cálidos como los rojos, amarillos, rosas funcionando como interferencias o eventos. El ritmo se contruye con la repetición de patrones lineales y punteados pero constantemente interrumpido.

<img width="1313" height="672" alt="image" src="https://github.com/user-attachments/assets/1b8bb85f-ce6a-4fcb-b873-48873a034c7e" />

_Strength of Night - Tyler Hobbs_

En esta la composición suele estar mas agrupada como "masas" suspendidas en el espacio. Genera algo más escultórico pues no recorres la imagen, la enfrentas. Su densidad es mucho mas controlada creando un contraste entre presencia y asuencia, teniendo un movimiento orgánico. Predomina todos oscuros como los negros, azules y grises con ligeras variaciones. Tiene un ritmo lento, se construye más por acumulación progresiva.

### Agentes autónomos y steering forces (Actividad 02)

Un agente autónomo es una entidad dentro de un sistema que toma decisiones por sí misma según reglas internas y lo que percibe del entorno, sin que cada paso esté controlado directamente; en ese contexto, una `steering force` es una fuerza calculada dinámicamente que orienta su movimiento (por ejemplo, acercarse, evitar o seguir algo), a diferencia de una fuerza externa como la gravedad que actúa siempre igual y no depende de “intención” o comportamiento. La diferencia clave es que la steering force responde a objetivos y contexto, mientras que la gravedad es constante y predecible. Estas ideas son útiles no solo para simular movimiento sino para diseñar comportamiento visual porque permiten crear sistemas que parecen vivos, donde las formas no solo se mueven sino que reaccionan, se organizan y generan patrones complejos a partir de reglas simples, lo que da lugar a composiciones más orgánicas y menos rígidas.

### Flow Fields (Actividad 03)

Es un campo de flujo está construido como una grilla donde cada celda contiene un vector que indica una dirección (y a veces intensidad), y puede generarse a partir de ruido, funciones matemáticas o reglas; cada uno de esos vectores representa “hacia dónde debería moverse algo” en ese punto del espacio. Un agente, al moverse, usa su posición para ubicar en qué celda está y leer ese vector, y luego lo transforma en una decisión aplicando una `steering force` que ajusta su velocidad teniendo en cuenta límites como **maxspeed** y **maxforce**. Parámetros clave del sistema son la **resolución** del campo (qué tan detallado es), la cantidad de agentes (densidad visual), y esos límites de velocidad y fuerza que afectan qué tan suave o abrupto es el movimiento. Por ejemplo, si reduces la resolución, el movimiento se vuelve más brusco y segmentado; si la aumentas, todo fluye más orgánicamente. En general, este algoritmo produce movimientos continuos, tipo corrientes o fluidos, con trayectorias suaves que pueden parecer viento, agua o energía; visualmente sugiere algo vivo, envolvente, incluso hipnótico, y encajaría muy bien con música ambiental, electrónica suave o piezas más atmosféricas donde el ritmo no es rígido sino expansivo.

### Flocking (Actividad 04)

Un tema del cual ya me adelante de entregas pasadas, el `flocking` contiee reglas base que son bastante intuitivas: 

- **separación** hace que cada agente evite estar demasiado cerca de los demás para no colisionar;
- **alineación** hace que intente moverse en la misma dirección que sus vecinos; y
- **cohesión** lo empuja a mantenerse cerca del grupo, evitando que se disperse.

Estas reglas suelen estar controladas por parámetros como el radio de percepción (qué tan lejos “ve” a otros agentes) y los pesos o intensidades de cada fuerza. Si, por ejemplo, aumentas mucho el peso de separación, el sistema se vuelve más disperso y tenso, casi nervioso; si subes cohesión, el grupo se compacta y se siente más sólido; y si dominas con alineación, aparece un movimiento más fluido y coordinado, como una bandada real. Lo interesante es que de estas reglas simples emerge un comportamiento colectivo que puede ser compacto o disperso, estable o caótico, dependiendo del balance: bien ajustado se siente fluido, mal balanceado puede parecer errático o incluso fragmentado. Visualmente, el flocking produce una atmósfera orgánica, casi biológica, como si estuvieras viendo algo vivo que respira y se adapta; funciona muy bien en relación con música cuando no sigue el ritmo de forma literal, sino como una capa que reacciona o acompaña, por ejemplo en piezas electrónicas, ambient o incluso pasajes más suaves donde el movimiento colectivo puede amplificar la sensación de flujo o tensión.


### Comparar algoritmos como herramientas de diseño  (Actividad 05)

Los `flow fields` y el `flocking` producen movimientos que, aunque pueden parecer similares a primera vista, nacen de lógicas distintas: en los flow fields el movimiento es continuo, como corrientes invisibles que arrastran a los agentes en trayectorias suaves y envolventes, mientras que en el flocking el movimiento es colectivo y reactivo, basado en la interacción entre vecinos, lo que genera agrupaciones, rupturas y reorganizaciones constantes. Esto hace que los flow fields ofrezcan un mayor **control visual global**, pero menor nivel de emergencia, mientras que el flocking tiene menos control directo sobre la forma final pero un **alto nivel de comportamiento emergente**, donde el sistema sorprende con dinámicas complejas. En términos de atmósfera, los flow fields suelen sentirse más fluidos, meditativos o naturales (como viento o agua), mientras que el flocking tiende a lo orgánico y social, a veces estable y armonioso, otras veces tenso o caótico. En relación con música, los flow fields funcionan muy bien como capas continuas que acompañan piezas ambientales o progresivas, mientras que el flocking puede dialogar mejor con cambios rítmicos o energéticos, respondiendo de forma más “viva”. Como ventajas, los flow fields permiten diseñar direcciones claras y composiciones más controladas, pero pueden volverse predecibles; el flocking, en cambio, genera riqueza y sorpresa, aunque puede ser difícil de controlar y ajustar visualmente.

Si tuviera que elegir según emoción: para una canción **contemplativa** usaría flow fields por su suavidad y continuidad; para algo **agresivo**, flocking con parámetros extremos (alta separación o cambios bruscos) para generar tensión; para una pieza **melancólica**, flow fields más lentos y con baja variación, casi como deriva; y para algo **eufórico**, flocking bien balanceado, donde la coordinación y expansión del grupo transmitan energía colectiva.


## Bitácora de aplicación 

> Concepto visual.

Algo que alguna vez fue habitado, un lugar que sigue comportandose como hogar pero ya no hay personas que residan en el. Quiero manejar sentimientos y tensiones en la obra, intimidad y abandono, orden humano y reorganización natural, presencia y ausencia.

Visualmente pienso en un lugar con objetos ligeramente desplazados, patrones alterados, simetrías imperfectas. Como una mesa puesta pero con migas que se vuelven rutas de hormigas (este seria el sistema de particulas). Quiero que sea emocional porque es lo que me genera la canción escogida, quiero incluir una luz encendida, un vaso a medio tomar, ropa doblada, un calendario detenido, etc. Nada explícito, nada dramático. Algo orgánico donde las hormigas parecen ser los nuevos dueños de este hogar que siguen corrientes invisibles (memoria o hábitos) y tu como espectador solo puedes perturbar momentaneamente esa calma.

> Relación entre la visual y la canción.

Decidí tomar _Some better_ de LEYA, parte del álbum Eyeline. Más que proponer un mensaje claro, el álbum se mueve desde el sentir: construye atmósferas con sonidos que resultan extrañamente familiares, casi íntimos, pero al mismo tiempo inquietantes. Hay una nostalgia difícil de ubicar, como si recordaras algo que nunca terminas de reconocer. Esa ambigüedad genera curiosidad constante: no sabes exactamente qué te están contando, pero sientes que hay algo ahí, insistiendo.

Desde ahí se articula la obra. Así como en las piezas generativas, no hay un significado fijo ni dictado; cada espectador proyecta sus propias vivencias y termina completando lo que falta. La música no funciona como fondo, sino como una guía sensible que moldea la experiencia sin cerrarla. En ese sentido, la obra no se interpreta: se habita.

La relación con el sistema visual parte de esa misma lógica. El sonido no ilustra, sino que estructura el comportamiento. Las variaciones de la canción afectan directamente a los agentes y al entorno:

- En los momentos más densos, donde el sonido se vuelve más cargado o saturado, las hormigas tienden a agruparse, a condensarse en trayectorias más definidas, como si el espacio se tensara.
- En los silencios o pasajes más abiertos, se dispersan, pierden dirección momentáneamente, como si el hogar respirara.
- Las texturas sonoras más ásperas o inestables introducen pequeñas perturbaciones en el sistema: desvíos, vibraciones, rutas que se rompen y se reconstruyen.
- Los sonidos más sostenidos o armónicos estabilizan el flujo, haciendo que el movimiento se vuelva más continuo, casi ritual.

De esta forma, la música deja de ser acompañamiento y se convierte en una dimensión estructural: es lo que regula el pulso del sistema, lo que define cuándo el espacio se organiza y cuándo se descompone. Igual que en la obra, no hay una narrativa explícita, pero sí una sensación persistente de que algo está ocurriendo, incluso cuando no se puede nombrar con claridad.

> Moodboard o referencias.

<img width="1450" height="1154" alt="BiteBoxAnim" src="https://github.com/user-attachments/assets/113ff0f4-ded8-46b3-962e-f7c081f2678f" />

> Dos o más bocetos.



> Mapa de decisiones.



> Mapa de interpretación.



> Justificación del algoritmo elegido.

Principalmente porque el flow field funciona como una red de vectores que define hacia dónde moverse en cada punto del espacio representandose como una memoria residual (rutinas pasadas), la incercia del habitar (el espacio sigue “sabiendo” cómo se usaba) y una organización que no desaparece, solo deja de ser humana.

No vemos esas fuerzas en la vida real, pero las sentimos:
los caminos que tomamos siempre, los lugares donde dejamos cosas, las formas en que ocupamos un hogar.

Las hormigas siguen lo que queda de quienes ya no están, no como representación de abandono, es un hogar que sigue operando bajo otra lógica.

> Explicación de la relación audio-visual.



> Evidencia del uso de IA.



> Código fuente.

// sketch

``` Js
let ants = [];
let flowField;

let cols, rows;
let scale = 20;

let paintLayer;

// globals
let attractors = [];
let disturbances = [];

let audioLevel = 0;

function setup() {
  createCanvas(windowWidth, windowHeight);

  initSystem();

  setupAudio();
}

function initSystem() {
  // crear capa persistente
  paintLayer = createGraphics(windowWidth, windowHeight);
  paintLayer.clear();

  // recalcular grid
  cols = floor(width / scale);
  rows = floor(height / scale);

  flowField = new Array(cols * rows);

  // reiniciar hormigas
  ants = [];
  for (let i = 0; i < 300; i++) {
    ants.push(new Ant(random(width), random(height)));
  }
}

function draw() {
  background(210, 190, 160);

  // desgaste pintura
  let fadeAmount = map(audioLevel, 0, 0.3, 5, 18);
  paintLayer.noStroke();
  paintLayer.fill(210, 190, 160, fadeAmount);
  paintLayer.rect(0, 0, width, height);

  image(paintLayer, 0, 0);

  updateAudio();
  updateInteraction();
  updateFlowField();

  for (let ant of ants) {
    ant.follow(flowField);
    ant.update();
    ant.edges();
    ant.paint(paintLayer);
  }

  // líneas efímeras
  stroke(140, 100, 80, 40);
  strokeWeight(1);

  for (let ant of ants) {
    line(ant.pos.x, ant.pos.y, ant.prev.x, ant.prev.y);
  }

  // hormigas evento
  if (audioLevel > 0.18 && random() < 0.2) {
    ants.push(new Ant(random(width), random(height), true));
  }
}

function mousePressed() {
  let fs = fullscreen();
  fullscreen(!fs);

  if (!song.isPlaying()) {
    song.loop();
  }
}

// 🔁 resize dinámico
function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  initSystem();
}
```

// ants

``` Js
class Ant {
  constructor(x, y, painter = false) {
    this.pos = createVector(x, y);
    this.prev = this.pos.copy();

    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);

    this.isPainter = painter;

    this.maxSpeed = painter ? 3 : 1.5;
    this.maxForce = 0.1;
  }

  follow(field) {
    let x = floor(this.pos.x / scale);
    let y = floor(this.pos.y / scale);

    let index = x + y * cols;
    let force = field[index];

    if (force) this.applyForce(force);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.prev = this.pos.copy();

    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);

    this.acc.mult(0);
  }

  edges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  paint(layer) {
    layer.strokeCap(ROUND);

    if (this.isPainter) {
      // 🔥 NARANJA EVENTO
      layer.strokeWeight(2 + audioLevel * 4);

      layer.stroke(
        255,
        140 + random(40),
        90,
        180
      );
    } else {
      // 🟤 MARRÓN BASE
      layer.strokeWeight(1);

      layer.stroke(
        160 + random(20),
        110 + random(20),
        80,
        90
      );
    }

    layer.line(this.pos.x, this.pos.y, this.prev.x, this.prev.y);
  }
}
```

// flowField

``` Js
function updateFlowField() {
  let yoff = 0;

  for (let y = 0; y < rows; y++) {
    let xoff = 0;

    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;

      let angle = noise(xoff, yoff, frameCount * 0.001) * TWO_PI;

      let audioForce = map(audioLevel, 0, 0.3, 0.8, 2.5);
      angle *= audioForce;

      flowField[index] = p5.Vector.fromAngle(angle);

      xoff += 0.1;
    }
    yoff += 0.1;
  }
}
```

// interaction

``` Js
let lastXFrame = 0;
let cooldown = 25; // frames (~0.4s)

function updateInteraction() {

  let timeSinceLast = frameCount - lastXFrame;

  // 🎧 condición controlada
  if (
    audioLevel > 0.18 &&
    timeSinceLast > cooldown &&
    random() < 0.25
  ) {

    let size = dist(0, 0, width, height);

    let x = mouseX;
    let y = mouseY;

    // leve variación angular
    let angleOffset = random(-0.2, 0.2);

    let dx = cos(angleOffset) * size;
    let dy = sin(angleOffset) * size;

    // 🔥 color contraste
    paintLayer.stroke(255, 245, 200, 140);

    let weight = map(audioLevel, 0.18, 0.3, 2, 5);
    paintLayer.strokeWeight(weight);

    // X gigante
    paintLayer.line(x - dx, y - dy, x + dx, y + dy);
    paintLayer.line(x + dy, y - dx, x - dy, y + dx);

    // registrar evento
    lastXFrame = frameCount;
  }

  // disturbio base (suave)
  disturbances.push({
    pos: createVector(mouseX, mouseY),
    strength: 1,
    life: 50
  });

  disturbances = disturbances.filter(d => {
    d.life--;
    return d.life > 0;
  });
}
```

// audio

``` Js
let song;
let amplitude;

function preload() {
  song = loadSound('Some Better.mp3');
}

function setupAudio() {
  amplitude = new p5.Amplitude();
}

function mouseClicked() {
  if (!song.isPlaying()) {
    song.loop();
  }
}

function updateAudio() {
  audioLevel = amplitude.getLevel();
}
```

> Enlace al sketch.

_link: https://editor.p5js.org/LuisaRoech/sketches/J_E2Htck-_

> Capturas o registros de momentos importantes de la pieza.



## Bitácora de reflexión
