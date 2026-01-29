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

Las formas ciculares siempre se me han hecho muy enigmaticas, si nos guiamos con la idea de forma una obra generativa e interactiva, siempre me ha fascinado las obras de las que no requieren del esperactador pero que actuen por si solas, que puedan ir cambiando por un valor que la compone, posiblemente tener como interaccion el tiempo es la forma mas sosa de dar interacción pero ...

``` javascript
let walkers = [];
let lastMinute;


function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  let m = minute();

  if (m !== lastMinute) {
    walkers.push(new Walker(random(width), random(height)));
    lastMinute = m;
  }

  for (let w of walkers) {
    w.step();
    w.show();
  }
}


class Walker {
  constructor(cx , cy) {
    this.centerX = cx;
    this.centerY = cy;
    this.radius = random(40, 100);
    this.angle = random(TWO_PI);
    this.noiseOffset = random(1000);
  }

  step() {
   
    this.angle += random(-0.03, 0.03);

    // perlin noise 
    this.noiseOffset += 0.01;
    let n = noise(this.noiseOffset);
    this.angle += map(n, 0, 1, -0.02, 0.02);
  }

  show() {
    // distribución normal 
    let r = this.radius + randomGaussian(0, 10);

    let x = this.centerX + cos(this.angle) * r;
    let y = this.centerY + sin(this.angle) * r;

    point(x, y);
  }
}
```

_Link: https://editor.p5js.org/LuisaRoech/sketches/cd05p7uYo_

## Bitácora de reflexión

![cat-spinning](https://github.com/user-attachments/assets/65233b8f-923b-4db5-8065-7b2a7baddcef)












