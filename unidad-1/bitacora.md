# Unidad 1

## Bitácora de proceso de aprendizaje

### La aleatoriedad en el arte generativo (Actividad 01)
La aleatoriedad engendra a un arte vivo, una obra que se extiende como raíces bajo la tierra o se expande como células vistas al microscopio, despertando emoción sin obedecer a una intención; el artista solo guía la forma y pierde control sobre su propia obra.

<img width="690" height="388" alt="image" src="https://github.com/user-attachments/assets/983bdedb-ebdc-43ba-942c-af72e593c5df" />

*Ian Cheng por ejemplo, crea simulaciones generativas que existen como ecosistemas vivos, donde el artista define las reglas iniciales y luego cede el control al comportamiento impredecible del sistema.*

### Caminatas aleatorias (Actividad 02)
> ¿Qué pienso que va a suceder si cambio valores?

Antes de realizar modificaciones al código, analicé su estructura y porque no, para entender un poco mejor la base lógica de este.
``` javascript
let walker; 

function setup() {
// Aqui se crea el canvas, inicializa el walker en el centro de él y se define el color de fondo
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step(); // Define en que dirección se movera
  walker.show(); // Mostrará en pantalla los pasos dados, se puede incluir varios al tiempo
}
```
Ahora bien, entrando a la clase importante y que modificare
``` javascript
class Walker {
  constructor() {
  // Posición inicial en el centro del canvas
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
  // Define su color y el tipo de trazo, en este caso como un punto (puede tomar cualquier forma)
    stroke(0);
    point(this.x, this.y);
  }

  step() {
  // Aqui se dan las diferentes desiciones aleatorias entre 0 y 3
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++; // mov derecha
    } else if (choice == 1) {
      this.x--; // mov izquierda
    } else if (choice == 2) {
      this.y++; // mov abajo
    } else {
      this.y--; // mov arriba
    }
  }
}

```

Si modifico el trazo y utilizo line() (no se si es posible pero hay que intentarlo), espero que el walker genere una línea duh. Además, considero que si cambio los valores de choice para que el movimiento se realice de dos en dos, el walker debería desplazarse simultáneamente en ambos ejes. Finalmente, si modifico la primera decisión de x-- a x++, el movimiento en esa condición debería dirigirse únicamente hacia valores positivos del eje X.

> Que sucedio realmente :)

En general, ocurre lo esperado. Sin embargo, el trazo con line() no funciono en el primer intento, lo cual es más a un skill issue de mi parte que a un problema del concepto, pues este trazo a diferencia de point() necesita que le asignen tres valores.

> ¿Por que si ocurrio o no?

Omitiendo lo del line(), en general el código es lo suficientemente básico y claro como para anticipar qué sucede al modificar ciertos valores o funciones, ya que cada parte cumple una función especifica y facil de identificar dentro de la lógica general de walker.

### Distribuciones de probabilidad (Actividad 03)
> ¿Que diferencia hay entre la distribución uniforme y una no uniforme de números aleatorios?

Una distribución uniforme, debe contar con la misma probabilidad para todas las opciones posibles; la no uniforme por el contrario, otorga mayor probabilidad a ciertos valores sobre otros, generalmente concentrándose alrededor de una media.

Si quiero favorecer el movimiento hacia la derecha con una distribución no uniforme del codigo anterior, debo modificar la forma en que se elige la dirección del *walker*, por ejemplo: 
``` javascript
step() {
  const prevX = this.x;
  const prevY = this.y;

  // Con esto se centra en valores positivos
  const r = randomGaussian(1, 0.8);

  if (r > 0.5) {
    this.x++; // derecha
  } else if (r < -0.5) {
    this.x--; // izquierda
  } else if (random(1) < 0.5) {
    this.y++;
  } else {
    this.y--;
  }

  line(prevX, prevY, this.x, this.y);
}
```
_Notas extras: randomGaussian() es la forma en como se puede representar una distribución normal, los valores se concentran cerca de la media y los extremos ocurren con menos frecuencia, visualmente puede verse como una campana._

### Distribución Normal (Actividad 04)
Para este ejercicio modifique el código _The Nature of Code_ ejemplo 0.4, que ya utiliza distribución gausiana, pues queria replicar lo ya visto en clase pero con rayas verticales :)
``` javascript
// The Nature of Code Ejemplo 0.4
// Daniel Shiffman

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  // Media: el centro del canvas
  let mean = width / 2;

  // Qué tan dispersos están los valores
  let dis = 60;

  // Valor aleatorio (que ya sabemos que el randomGaussian hace alegoria a una distribución normal)
  let x = randomGaussian() * dis + mean;

  stroke(0, 30);
  line(x, 0, x, height);
}
```
_Notas extras: la disperción indica que tan lejos pueden estar los valores de la media, si los valores son pequeños, la mayoria de los valores estaran muy concentrados en la media, por ejemplo si se cambia dis=20. Por otro lado, un valor equilibrado se podria considerar entre 50-80 en el que se aprecia la forma de campana y un valor grande seria como 150 donde todo se vuelve mas caótico._


![Grabacindepantalla2026-01-26204734-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/0f443d00-772e-4ab9-97ed-27854c2b244b)

_Link: https://editor.p5js.org/LuisaRoech/sketches/DzN2wrTj1_

### Distribución personalizada: Lévy flight (Actividad 05)
_El salto de Lévy_ es como se escucha (literal), dar saltos aleatorios dentro de una acción continua, como una caminata. La mayor parte del tiempo el movimiento ocurre de manera normal, con pasos cortos o recorridos dentro de una misma zona, pero BUM, de forma inesperada se produce un salto más grande que cambia la dirección o la posición de manera abrupta.

Según _The Nature of Code_, este tipo de comportamiento es interesante porque rompe con la idea de un movimiento aleatorio uniforme: no todo ocurre al mismo ritmo ni con la misma intensidad.

> ¿Qué resultados espero modificando el código para aplicar el Lévy flight?

Planeo usar el código ya modificado del anterior punto, la idea es mantener la distribución normal que concentra los valores alrededor de un promedio, con Lévy flight romperia esa regularidad cambiando el tamaño de los circulos (que puse en elipses) y color, si son pequeños tendran colores claros y si es grande tendra colores oscuros.

En resumen, espero que la mayoría de las figuras se acumulen cerca del centro, con tamaños pequeños, mientras que de forma ocasional aparezcan círculos mucho más grandes que interrumpen el patrón visual.

``` javascript
// The Nature of Code (modificado)

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  
  let mean = width / 2;
  let dis = 60;

  // Posición base
  let x = randomGaussian() * dis + mean;
  let y = height / 2;

  // Tamaño controlado por Lévy flight
  let size = levy();

  // Color cambia según el tamaño del salto
  // Saltos grandes → más oscuros
  let color = map(size, 2, 80, 40, 150);

  noStroke();
  fill(0, color);

  // Cambio de forma: círculos pequeños y grandes
  ellipse(x, y, size, size);
}


function levy() {
  let r = random(1);

  if (r < 0.01) {
    return random(40, 80); // poco frecuente
  } else {
    return random(2, 8); // frecuente
  }
}
```

![Grabacindepantalla2026-01-27210944-ezgif com-crop](https://github.com/user-attachments/assets/c3bd0c9f-d69c-490d-a44a-71da9769f538)

_Link: https://editor.p5js.org/LuisaRoech/sketches/RmaRa8OSf_

### Ruido Perlin (Actividad 06)
Hacer uso de este ruido permite generar comportamientos muy orgánicos y menos caóticos de lo que puede llegar a ser con Lévy Flight. A diferencia de estos saltos abruptos, el ruido Perlin introduce variaciones suaves y graduales, lo que hace que el movimiento se sienta más natural y predecible sin dejar de ser dinámico.

Este concepto se vuelve especialmente interesante al conocer su origen: el ruido Perlin comenzó a aplicarse a gran escala en el contexto del cine, específicamente en la película **Tron** (1982), dirigida por Steven Lisberger. El ruido fue desarrollado por Ken Perlin, quien buscaba una forma de generar texturas y movimientos digitales que no se vieran artificiales ni repetitivos. En esa epoca el azar (random()) se veia demasiado caótico, ruidoso y poco creíble por lo que el ruido perlin fue el resultado para encontrar un mundo digital que no se viera falso y sin vida, imitando la naturaleza, no completamente aleatoria, pero tampoco perfectamente ordenada.

<img width="720" height="480" alt="image" src="https://github.com/user-attachments/assets/ca80a90a-6b16-4a7f-b918-2afa2a2fcea7" />

> ¿Qué resultados espero?

Asi que, para implementar el ruido perlin planearia hacerlo en dos dimensiones (x,y) y en lugar de saltar aleatoriamente, el punto se estaria desplazando suavemente por el canvas, pues el noise() cambia de manera gradual.

``` javascript
//coordenadas
let tx = 0;
let ty = 1000;

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  let x = noise(tx) * width;
  let y = noise(ty) * height;

  noStroke();
  fill(0, 20);
  circle(x, y, 8);

  //Si incremento muy poco, el mov será lento y suave, si es mucho será erratico y si no se agrega el punto se queda quieto.
  tx += 0.01;
  ty += 0.01;
}
``` 
_Nota extra: el ruido Perlin se usa directamente con la función noise()_

![20260129-2329-32 0347503-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/8facbd63-8614-4db6-80c1-383b7bd80f76)

_Link: https://editor.p5js.org/LuisaRoech/sketches/pA37z0pI0_

## Bitácora de aplicación 

### Creación de obra generativa (Actividad 07)

> Antes de, que ideas tengo sobre lo que quiero hacer

Me gustaria experimentar con el perlin noise, distribución normal y caminatas aleatorias. Tratar de hacer formas distintas, tal vez que la caminata se mueva alrededor de un circulo invisible.

Tipos de interacción: teclado, tiempo (segundos/minutos) pero como?

> Concepto de la obra

Las formas circulares siempre se me han hecho muy enigmáticas. Si se piensa desde la idea de crear una obra generativa e interactiva, siempre me han llamado más la atención aquellas piezas que no dependen directamente del espectador, sino que actúan por sí solas. Obras que cambian a partir de los valores que las componen, sin necesidad de una acción externa constante.

Usar el tiempo como forma de interacción puede parecer algo simple, incluso soso, pero al mismo tiempo resulta interesante porque es una interacción inevitable: ocurre aunque nadie esté mirando. El tiempo no se controla, solo se observa cómo afecta al sistema, y eso lo convierte en un elemento silencioso pero fundamental dentro de la obra.

``` javascript
let walkers = [];
let lastSecond;
let currentColor;

function setup() {
  createCanvas(640, 240);
  background(255);

  currentColor = color(0, 80); // color inicial
}

function draw() {

  let s = second();

  if (s !== lastSecond) {
    walkers.push(new Walker(random(width), random(height)));
    lastSecond = s;
  }
  
  // cada walker calcula su movimiento y lo dibuja
  for (let i = walkers.length - 1; i >= 0; i--) {
    walkers[i].step();
    walkers[i].show();
    
  // cuando vive demasiado se elimina
    if (walkers[i].life > walkers[i].maxLife) {
      walkers.splice(i, 1);
    }
  }
}

function keyPressed() {
  if (key === 'a') currentColor = color(0, 80);
  if (key === 's') currentColor = color(50, 80, 200, 80);
  if (key === 'd') currentColor = color(200, 60, 60, 80);
  if (key === 'f') currentColor = color(20, 150, 90, 80);
}

class Walker {
  constructor(cx, cy) {
    this.cx = cx;
    this.cy = cy;
    
    this.radius = random(40, 100);
    this.angle = random(TWO_PI);
    this.speed = random(0.004, 0.008);
    
    // perlin & limite de vida
    this.noiseOffset = random(1000);

    this.life = 0;
    this.maxLife = floor(random(900, 1500)); 
  }

  step() {
    this.life++;
    this.angle += this.speed;

    let chaos = map(this.life, 0, this.maxLife, 0.0005, 0.01);
    this.angle += random(-chaos, chaos);

    // perlin
    this.noiseOffset += 0.001;
    let n = noise(this.noiseOffset);
    this.angle += map(n, 0, 1, -chaos, chaos);
  }

  show() {
    // dibujo lento para mantener su forma
    if (frameCount % 2 !== 0) return;

    stroke(currentColor);

    let sd = map(this.life, 0, this.maxLife, 2, 8);
    let r = this.radius + randomGaussian(0, sd);

    let x = this.cx + cos(this.angle) * r;
    let y = this.cy + sin(this.angle) * r;

    point(x, y);
  }
}
```
![20260130-0350-32 5645754-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/f900d905-f0c5-49fd-a017-b8cf00aec03a)

_Link: https://editor.p5js.org/LuisaRoech/sketches/cd05p7uYo_

nota: El espectador no controla el sistema directamente; la obra responde al tiempo real como una entidad autónoma.

## Bitácora de reflexión

1. Diferencia entre random() y noise()

random() genera valores totalmente impredecibles, útil para variaciones rápidas o caóticas, mientras que noise() produce cambios suaves y continuos, mas orgánicos.
   
2. Qué es una distribución de probabilidad y diferencia visual

Una distribución de probabilidad describe qué tan probable es que ocurran ciertos valores.
Una caminata con distribución uniforme se dispersa de forma pareja y errática, mientras que una normal se concentra alrededor de un valor central y genera formas más estables.

3. Papel de la aleatoriedad en el arte generativo

Permite que la obra evolucione, y que genere resultados que inclusive la mente humana no podria concebir.
 
4. Concepto usado en mi obra (Actividad 07)

Utilicé ruido Perlin para introducir inestabilidad progresiva en el movimiento circular, porque permite deformaciones suaves sin romper abruptamente la forma.

5. Qué es una caminata y qué es un Lévy flight

Una caminata es un movimiento paso a paso determinado por reglas y azar.
Un Lévy flight se caracteriza por muchos pasos cortos y, ocasionalmente, saltos largos e inesperados.


![cat-spinning](https://github.com/user-attachments/assets/65233b8f-923b-4db5-8065-7b2a7baddcef)













