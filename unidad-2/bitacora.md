[![20260213-0330-27-7017076-(1).gif](https://i.postimg.cc/hPHkb6pN/20260213-0330-27-7017076-(1).gif)](https://postimg.cc/Xpk1nHMg)
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

`|v| = sqrt(x² + y² (+ z^2))`

Lo que sirve para saber qué tan fuerte es una velocidad, una fuerza o qué tan lejos está algo en términos vectoriales. Por otro lado, `magSq()` devuelve la magnitud al cuadrado:

`|v|^2 = x^2 + y^2 (+ z^2)`

No calcula la raíz, siendo más eficiente porque evita la raiz, que es más costosa computacionalmente. Se usa normalmente cuando se quiere comparar magnitudes.

> ¿Para qué sirve el método normalize()?

Este convierte el vector en un vector unitario (magnitud 1) manteniendo la dirección, se usa para obtener solo la dirección, aplicar fuerzas con intensidad controlada o evitar que la velocidad crezca sin límite.

> Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?

“El método dot sirve para medir qué tan alineados están dos vectores.”

> El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?

- **Instancia:** `v1.dot(v2)` → el producto punto se calcula usando el vector que llama al método.
- **Estática:** `p5.Vector.dot(v1, v2)` → recibe ambos vectores como parámetros.

> Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

El producto cruz genera un nuevo vector perpendicular al plano formado por los dos vectores originales.

- Magnitud: representa el área del paralelogramo que forman los dos vectores.
- Orientación: sigue la regla de la mano derecha (determina hacia dónde apunta el vector resultante).

En resumen: el producto cruz produce un vector perpendicular cuya longitud depende de cuánto “se abren” los vectores originales.

> ¿Para que te puede servir el método dist()?

Este calcula la distancia entre dos vectores (puntos), muy útil para detectar colisiones, saber si algo está "cerca" o generar interacción entre partículas.

> ¿Para qué sirven los métodos normalize() y limit()?

- `normalize()` → mantiene la dirección pero fija la magnitud en 1.
- `limit(max)` → restringe la magnitud para que no supere un valor máximo.

En sistemas generativos o físicos:

- `normalize()` controla dirección pura.
- `limit()` evita velocidades exageradas y mantiene estabilidad.

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

Significa _linear interpolation_, matemáticamente se hace esto:

_resultado = a + (b − a) ∗ t_

si _t = 0_ devuelve `a`, si _t = 1_ devuelve `b`, si _t = 0.5_ se da el punto exactamente a la mitad y si _t_ aumenta progresivamente se muevemente suavemente de `a` hacia `b`. En mi código funciona asi

``` Javascript
let movingTip = p5.Vector.lerp(tipBlue, tipRed, t);
```

Donde el punto se mueve suavemente desde la punta azul hasta la roja y _t_ controla cuánto ha avanzado. Ahora bien con `lerpColor()` funciona igual pero interpola colores.

``` Javascript
let c = lerpColor(color(0, 0, 255), color(255, 0, 0), t);
```

 si _t = 0_ → azul, si _t = 1_ → rojo y en el medio → mezcla progresiva (violeta, magenta, etc.)

> ¿Cómo se dibuja una flecha usando drawArrow()?

Paso a paso se aplica en esta linea:

``` Javascript
function drawArrow(base, vec, myColor) // base es el punto donde empieza la flecha, vector la dirección y magnitud y myColor el color duh.
``` 

- `translate(base.x, base.y);` Mueve el sistema de coordenadas al punto de origen.
- `line(0, 0, vec.x, vec.y);` Dibuja la línea principal del vector.
- `rotate(vec.heading());` Rota el sistema según el ángulo del vector.
- `translate(vec.mag() - arrowSize, 0);` Se mueve hasta la punta del vector.
- `triangle(...)` Dibuja la cabeza de la flecha.

Es literalmente convertir dirección + magnitud en forma visual.

### Motion 101 (Actividad 07)

> Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.

No es una formula, es un concepto para poder pensar el movimiento en relación de la posición, velocidad y aceleración a lo largo del tiempo (Cálculo integral T_T + Física). En esencia podriamos decir que mover algo es cambiar su posición en el tiempo, y ese cambio puede describirse con vectores.
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

Partiendo de la idea del libro —que lo importante es definir la aceleración y dejar que el efecto en cascada (aceleración → velocidad → posición) haga su trabajo— probé los tres comportamientos distintos. La **aceleración constante** aumenta su velocidad progresivamente en una misma dirección, esta no se mueve solo "rápido", sino que cada frame acumula más velocidad, mientras que la **aceleración aleatoria** cambia el comportamiento radicalmente, como dice su nombre, es aleatorio la forma en como se mueve el objeto y no es tan simple, pues hay inercia, lo que significa que aunque la aceleración cambie al azar, la velocidad mantiene memoria del movimiento anterior. Por último, la **aceleración hacia el mouse**, acelera en dirección al cursor. Se siente intencional, si no se normaliza el vector hacia el mouse, la aceleración depende de la distancia y el objeto se dispara cuando está lejos. Al normalizarlo, el movimiento se vuelve más controlado y consistente.

_Nota extra: limit() es MUY pero muy util para controlar la velocidad. Pues el crecimiento en estas aceleraciones se sienten como gravedad o como un objeto empujado continuamente por una fuerza fija, lo interesante es que el movimiento deja de ser lineal y pasa a ser exponencial en percepción._

## Bitácora de aplicación 

> Antes de, que ideas tengo sobre lo que quiero hacer

Inicialmente, me gustaria experimentar estos conceptos en blender, usando aceleración hacia el mouse, donde las partículas aceleran hacia un Empty que sigue el mouse, ahora bien como hay que aplicar el marco de motion 101...quiero que las particulas tengan su propio movimiento pero se acerquen cuando el mouse se encuentra cerca, pero si es demasiado cerca, se repelen.

> Concepto de la obra

La obra explora la relación entre autonomía y perturbación dentro de un sistema dinámico. Un conjunto de partículas habita un espacio en constante movimiento, impulsadas por fuerzas internas y externas. Cada entidad posee inercia, memoria y sensibilidad, respondiendo a reglas de aceleración que determinan su comportamiento.

El espectador no controla directamente las partículas; las perturba. Su presencia altera el campo, generando zonas de atracción y repulsión que modifican el equilibrio del sistema.

Cada partícula opera bajo tres fuerzas fundamentales:

- Aceleración constante/aleatoria → Representa el movimiento autónomo, la deriva natural del sistema.
- Aceleración hacia el mouse → Representa influencia, atención o atracción.
- Repulsión cercana → Representa límite, defensa o saturación.

> Los nodos / código

``` Javascript
let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  
  for (let i = 0; i < 120; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  background(10, 10, 20, 40); // leve estela
  
  for (let p of particles) {
    p.update();
    p.display();
  }
}

class Particle {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D();
    this.velocity.mult(random(0.5, 2));
    this.acceleration = createVector(0, 0);
    this.maxSpeed = 4;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    
    // aceleración aleatoria
    let randomForce = p5.Vector.random2D();
    randomForce.mult(0.05);
    this.applyForce(randomForce);
    
    
    // aceleración mouse
    let mouse = createVector(mouseX, mouseY);
    let direction = p5.Vector.sub(mouse, this.position);
    let distance = direction.mag();
    
    direction.normalize();
    
    // si está cerca
    if (distance < 200 && distance > 50) {
      let strength = map(distance, 50, 200, 0.5, 0);
      direction.mult(strength);
      this.applyForce(direction);
    }
    
    // si está demasiado cerca 
    if (distance <= 50) {
      direction.mult(-1);
      direction.mult(0.6);
      this.applyForce(direction);
    }

   
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.velocity.mult(0.98); // damping
    this.position.add(this.velocity);

    // reset 
    this.acceleration.mult(0);

    this.edges();
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  display() {
    noStroke();
    fill(150, 200, 255, 180);
    ellipse(this.position.x, this.position.y, 6);
  }
}
```
_Link: https://editor.p5js.org/LuisaRoech/sketches/fc5osgRdq_

> Mi pieza

![20260213-0330-27 7017076 (1)](https://github.com/user-attachments/assets/2246da32-7547-494f-b429-6e7f031018f6)

## Bitácora de reflexión

> Describe el concepto de tu obra generativa. Explica el concepto de tu obra generativa.

> El código de la aplicación.

> Un enlace al proyecto en el editor de p5.js.

> Selecciona capturas de pantalla representativas de tu pieza de arte generativa.


<img width="556" height="449" alt="image-removebg-preview" src="https://github.com/user-attachments/assets/acc055f6-fa8f-4d14-8ead-9de8f9b2412d" />








