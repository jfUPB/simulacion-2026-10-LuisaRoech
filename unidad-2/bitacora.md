# Unidad 2

## Bitácora de proceso de aprendizaje

### Los vectores y el arte (Actividad 01)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/da2b3ef6-8cb8-4c93-a8d3-4fb1549efa43" />

_**River scars** por Robert Hodgin (flight404)_


Un arte que cambia la forma en como lo vemos, como lo sentimos y como lo interpretamos. El uso de vectores y movimiento en el arte generativo no es solo una cuestión técnica, sino una forma distinta de pensar la imagen. Un vector deja de ser una simple flecha para convertirse en una fuerza que empuja, desvía o transforma una forma en el tiempo, haciendo que la obra no sea un resultado fijo sino un proceso que está siempre cambiando. Esto modifica la manera en que vemos la imagen, porque ya no observamos algo terminado, sino algo que está ocurriendo; la sentimos de forma más física y orgánica, y la interpretamos como un sistema vivo donde las reglas importan tanto como el resultado.

### Introducción a los vectores (Actividad 02)

> ¿Cómo funciona la suma dos vectores en p5.js?

En p5.js los vectores funcionan como un objeto, no un número. `position.add(velocity);` suma componente a componente lo que en cada frame se esta moviendo la posición en la dirección y magnitud que indica velocity. 

> ¿Por qué esta línea position = position + velocity; no funciona?

Pues no es algo que soporte javaScript, `position` y `velocity` no son números, son instancias de `p5.Vector` y el operador `+` no sabe como sumar vectores. En lugar de hacer una suma mátematica, JS intenta convertirlos a texto ("[object Object]") y el resultado es inválido para lo que se busca.
Por eso p5 ofrece métodos como `.add()`, `.sub()`, `.mult()`, etc., que sí saben cómo combinar vectores correctamente.

### Repasa (Actividad 03)

> ¿Qué tuviste que hacer para hacer la conversión propuesta?

Cambie los pasos, basada en las variables sueltas de x y y para que fuera manejado por vectores, un vector posición y un vector de velocidad. Es decir, en lugar de tener algo como `x++`, `y--`, etc., transformé la posición en un vector `(this.pos = createVector(width/2, height/2);)`. Eso significa que la posición ya no se maneja como dos números independientes, sino como un solo objeto que contiene dirección y magnitud. Luego, en el método `step()`, en vez de modificar directamente las coordenadas, creo un vector de desplazamiento `(step)` según la dirección aleatoria elegida. Cada posible movimiento (derecha, izquierda, arriba, abajo) se convierte en un vector unitario: `(1,0)`, `(-1,0)`, `(0,1)` o `(0,-1)`. Finalmente, `this.pos.add(step);` suma componente a componente a la posición actual.  

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

> ¿Qué resultado esperas obtener en el programa anterior?

Analizando un poco el código, esperaria que el vector cambie de `(6,9)` que es la condición inicial que tiene position a `(20,30)`, pues dentro de la función `playingVector(position)` se están modificando sus valores `x` y `y`. Y bueno, que todo esto sea visible en la consola por los varios `console.log` que hay en el.

> ¿Qué resultado obtuviste?

La consola imprime esto:

```
p5.Vector Object : [6, 9, 0]
p5.Vector Object : [20, 30, 0]
Only once
```

Es decir, el vector **sí cambia**, aunque la modificación ocurrió dentro de la funcion `playingVector();`.

> Recuerda los conceptos de paso por valor y paso por referencia en programación.

- **Paso por valor:** se envía una copia del dato. Si lo modificas dentro de la función, el original no cambia. (ejm: los primitivos como number, string, boolean se pasan por valor)

- **Paso por referencia:** se envía la referencia al objeto original. Si lo modificas dentro de la función, el original sí cambia. (ejm: los objetos como arrays, objetos, p5.Vector se pasan por referencia)

> ¿Qué tipo de paso se está realizando en el código?

Principalmente se esta usando el paso por referencia, pues `position` es un objeto `p5.Vector`. Cuando se llama `playingVector(position);` no se crea una copia del vector, sino que `v` apunta al mismo objeto en memoria que `position`.

> ¿Qué aprendiste?

Al tratar con vectores no estoy moviendo números sueltos, sino manipulando el mismo objeto en memoria. Eso significa que una función no solo “usa” el vector, sino que puede transformarlo directamente, afectando todo el sistema. Entendí que el comportamiento del programa depende mucho de si estoy compartiendo referencias o creando copias. Podria aplicarlo en trayectorias, fuerzas, composición, ritmo. Es literalmente intervenir el “organismo” de la obra.

### (Actividad 05)

> ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?

`mag()` devuelve la magnitud (longitud) del vector, es decir, qué tan largo es. Matemáticamente es:

Lo que sirve para saber qué tan fuerte es una velocidad, una fuerza o qué tan lejos está algo en términos vectoriales. Por otro lado, `magSq()` devuelve la magnitud al cuadrado, no calcula la raíz, siendo más eficiente porque evita la raiz, que es más costosa computacionalmente. Se usa normalmente cuando se quiere comparar magnitudes.

> ¿Para qué sirve el método normalize()?

Este convierte el vector en un vector unitario (magnitud 1) manteniendo la dirección, se usa para obtener solo la dirección, aplicar fuerzas con intensidad controlada o evitar que la velocidad crezca sin límite.

> Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?



> El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?


> Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te


> pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y


> la magnitud del vector resultante.


> ¿Para que te puede servir el método dist()?


> ¿Para qué sirven los métodos normalize() y limit()?

### Interpolamos? (Actividad 06)

> El código que genera el resultado que te pedí.

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

> ¿Cómo funciona lerp() y lerpColor().

a

> ¿Cómo se dibuja una flecha usando drawArrow()?

a

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

Ideas
Referencias


## Bitácora de reflexión















