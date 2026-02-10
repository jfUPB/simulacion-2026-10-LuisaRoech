# Unidad 2

## Bitácora de proceso de aprendizaje

### Los vectores y el arte (Actividad 01)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/da2b3ef6-8cb8-4c93-a8d3-4fb1549efa43" />
**River scars* by Robert Hodgin (flight404)**


Un arte que cambia la forma en como lo vemos, como lo sentimos y como lo interpretamos. El uso de vectores y movimiento en el arte generativo no es solo una cuestión técnica, sino una forma distinta de pensar la imagen. Un vector deja de ser una simple flecha para convertirse en una fuerza que empuja, desvía o transforma una forma en el tiempo, haciendo que la obra no sea un resultado fijo sino un proceso que está siempre cambiando. Esto modifica la manera en que vemos la imagen, porque ya no observamos algo terminado, sino algo que está ocurriendo; la sentimos de forma más física y orgánica, y la interpretamos como un sistema vivo donde las reglas importan tanto como el resultado.

### Introducción a los vectores (Actividad 02)

> ¿Cómo funciona la suma dos vectores en p5.js?

> ¿Por qué esta línea position = position + velocity; no funciona?
No es algo que soporte la aplicación

### Repasa (Actividad 03)

> ¿Qué tuviste que hacer para hacer la conversión propuesta?

Cambie los pasos, ubicacion de x y y para que fuera manejado por vectores, un vector posición y un vector de velocidad, para esto use el primer ejercicio de caminata aleatoria..

> Escribe el código que utilizaste para resolver el ejercicio.

``` Javascript
let walker;


function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.pos = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.pos.x, this.pos.y);
  }

  step() {
    const choice = floor(random(4));
    
    let step;
    
    if (choice == 0) {
       step = createVector(1, 0);
    } else if (choice == 1) {
       step = createVector(-1, 0);
    } else if (choice == 2) {
       step = createVector(0, 1);
    } else {
       step = createVector(0, -1);
    }
    
    this.pos.add(step);
  }
}
```

### (Actividad 04)

¿Qué resultado esperas obtener en el programa anterior?
¿Qué resultado obtuviste?
Recuerda los conceptos de paso por valor y paso por referencia en programación.
¿Qué tipo de paso se está realizando en el código?
¿Qué aprendiste?

### (Actividad 05)

¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
¿Para qué sirve el método normalize()?
Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
¿Para que te puede servir el método dist()?
¿Para qué sirven los métodos normalize() y limit()?

### Interpolamos? (Actividad 06)

> El código

``` Javascript
let t = 0;

function setup() {
  createCanvas(100, 100);
}

function draw() {
  background(200);

  let v0 = createVector(50, 50);
  let v1 = createVector(30, 0);   // rojo
  let v2 = createVector(0, 30);   // azul

  // Puntas
  let tipRed = p5.Vector.add(v0, v1);
  let tipBlue = p5.Vector.add(v0, v2);

  // Punto que se mueve sobre el verde
  let movingTip = p5.Vector.lerp(tipBlue, tipRed, t);

  // Vector morado = punta - origen
  let vPurple = p5.Vector.sub(movingTip, v0);

  // Color según posición
  let c = lerpColor(color(0, 0, 255), color(255, 0, 0), t);

  drawArrow(v0, v1, 'red');
  drawArrow(v0, v2, 'blue');
  drawArrow(tipBlue, p5.Vector.sub(tipRed, tipBlue), 'green');
  drawArrow(v0, vPurple, c);

  t += 0.01;
  if (t > 1) t = 0;
}

function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(3);
  fill(myColor);
  translate(base.x, base.y);
  line(0, 0, vec.x, vec.y);
  rotate(vec.heading());
  let arrowSize = 7;
  translate(vec.mag() - arrowSize, 0);
  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
  pop();
}
``` 

¿Cómo funciona lerp() y lerpColor().
¿Cómo se dibuja una flecha usando drawArrow()?


### Motion 101 (Actividad 07)

> Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.

No es una formula, es un concepto para poder pensar el movimiento en relación de la posición, velocidad y aceleración a lo largo del tiempo (calculo integral T_T + Fisica). En esencia podriamos decir que mover algo es cambiar su posición en el tiempo, y ese cambio puede describirse con vectores.
Ahora bien como se interpreta esto geométricamente, piensa en esto. Hay un punto → posición, desde ese punto sale una flecha → velocidad, esa flecha es empujada por otra → aceleración. Con cada frame la aceleración dobla o estira la flecha de velocidad y la velocidad arrastra el punto a una nueva posición (una suma de vectores encadenada).

En código esto principalmente se representa como:

``` Javascript
velocity.add(acceleration);
position.add(velocity);
```

_Nota extra: este concepto se puede buscar como Integración → Euler → Semi-Implicit o Verlet integration_

> ¿Cómo se aplica motion 101 en el ejemplo?

Dentro del _ejemplo 1.8_ el motion 101 realmente se aplica de forma literal al ejemplo dado anteriormente:

``` Javascript
    this.position = createVector(width / 2, height / 2); // el punto
    this.velocity = createVector(); // velocidad, la flecha que arrastra el punto
    this.acceleration = createVector(-0.001, 0.01); // la flecha que empuja
    
     update() {
    this.velocity.add(this.acceleration); // donde la velocidad se le suma la aceleración, pero esta aun no se mueve
    this.velocity.limit(this.topSpeed); // limita la velocidad
    this.position.add(this.velocity); // y desplaza el circulo en una dirección
    }
```

### Experimentando con la aceleración (Actividad 08)

> ¿Qué observaste cuando usas cada una de las aceleraciones propuestas?

a


## Bitácora de aplicación 



## Bitácora de reflexión







