# Unidad 4

## Bitácora de proceso de aprendizaje

### Memo Akten (Actividad 01)

[![tumblr-p5t67a10631qav3uso1-540.gif](https://i.postimg.cc/VLQSXpky/tumblr-p5t67a10631qav3uso1-540.gif)](https://postimg.cc/XXL7b25x)

_Gloomy Sunday by Memo Akten_

El sonido, un elemento que potencia mucho las emociones que pueden generar en un arte. Esta colecion de obras en especial me produce una sensación extraña: a veces parece que la obra me lleva hacia un estado de tranquilidad o contemplación, pero de repente puede aparecer un cambio inesperado que genera cierta incomodidad. Esa oscilación entre calma e inquietud hace que la experiencia sea muy particular.

Aún me resulta difícil imaginar cuál es el flujo de pensamiento necesario para crear obras así. Me pregunto cuánto del resultado puede ser realmente decidido por el artista y cuánto se deja a la aleatoriedad o al comportamiento del propio sistema que se crea. 

Investigue un poco sobre el y suele explorar preguntas sobre **la naturaleza de la realidad, la conciencia, la relación entre humanos y máquinas y la conexión entre ciencia y espiritualidad**. _Simple Harmonic Motion (2011–)_ es una serie de obras audiovisuales generativas basadas en principios matemáticos y físicos. El proyecto investiga cómo comportamientos complejos pueden surgir a partir de patrones de movimientos armónicos simples como la de un péndulo (como todo lo que estamos viendo en esta materia).

### Conceptos fundamentales (Actividad 02)

Analizando el código...Se dibuja una linea con dos circulos en los extremos que rota alrededor, se limpia la pantalla con `background(255);`, se aumenta un ángulo con `angle += 0.1;` que se activa cuando se presiona una tecla (aunque el cambio no es muy notorio porque se aumenta de igual forma en draw y keyPressed) yy el sistema de coordenadas se mueve y rota al centro. Pero, ¿para que se traslada el origen con `translate(width / 2, height / 2);`? bueno, en p5.js el origen se encuentra en la esquina superior izquierda por lo que esta función ayuda a que no se vea extraña la ubicación y no rote en la esquina, que no es necesariamente malo pero no siempre es el objetivo. 

El motion 101 referente al segundo código introduce el modelo básico del movimiento en simulaciones físicas _posición += velocidad_ y _velocidad += aceleración_ y en `update()` se refleja esto aqui:

``` Js
// calcula dirección hacia el mouse
 this.velocity.add(this.acceleration); // la convierte en aceleración y la suma
 this.velocity.limit(this.topspeed); // limita la velocidad
 this.position.add(this.velocity); // suma velocidad a la posición
```

Ahora bien, ¿que hace el `heading()`? En `show()` aparece:

``` Js
let angle = this.velocity.heading();
```

investigando, `heading()` es una función de p5.Vector que devuelve el ángulo del vector, es decir, calcula el ángulo entre el vector de velocidad y el eje x. Entonces el rectángulo se orienta en la dirección de la velocidad, si el objeto se mueve: 

- hacia la derecha → el rectángulo apunta a la derecha
- hacia arriba → el rectángulo rota hacia arriba
- diagonal → rota en ese ángulo

Por utlimo, `push()` y `pop()`; `push()` guarda el estado de la posición del sistema, rotación, escala y estilo de dibujo, `pop()` lo restaura. Permitiendo que las transformaciones solo afecten al objeto actual

``` Js
    push();
    rectMode(CENTER); // usa (x,y) como el centro del rectángulo, para que rote mejor sobre su centro

    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
```

### Practica un poco (Actividad 03)

Partiendo del código anterior, se hará tres cambios, primero el vehículo será un triángulo. Usare `heading()` para que el triángulo apunte hacia la dirección de movimiento y por ultimo agregar las teclas de flecha para controlar la aceleración.

``` Js

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 5;
    this.r = 16;
  }

  update() {

    // reinicia aceleración 
    this.acceleration.mult(0);

    if (keyIsDown(LEFT_ARROW)) {
      this.acceleration.x = -0.2;
    }

    if (keyIsDown(RIGHT_ARROW)) {
      this.acceleration.x = 0.2; // si lo pongo en 1 por ejemplo, el vehículo se acelera mucho más fuerte
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }

  show() {

    let angle = this.velocity.heading();

    push();

    translate(this.position.x, this.position.y);
    rotate(angle);

    stroke(0);
    fill(127);

    // triángulo apuntando hacia adelante
    triangle(-15, 10, -15, -10, 15, 0);

    pop();
  }

  checkEdges() {

    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;

    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;

  }
}
``` 

### Relación con el marco motion 101 (Actividad 04)

En este código parece que varios circulos orbitan o se atraen por uno mas grande o central. Para modificarlo aun cuando tiene fuerzas acumulativas como la gravedad, fricción, viento y atracción es necesario reiniciar la aceleración como se hace aqui:

``` Js
this.acceleration.mult(0);
```

Si no se hiciera las fuerzas se seguirían acumulando en cada frame y ace

### Coordenadas polares (Actividad 05)

### Funciones sinusoides (Actividad 06)

Una función que aparece en sonido, luz, movimiento, mareas, electricidad y vibraciones (patrones rítmicos).

_y(t) = Asen(wt + π)_

Amplitud (A)
Frecuencia()
Fase

### Repasa conceptos de las unidades anteriores (Actividad 07)

``` Js
class Oscillator {
  constructor() {
    this.angle = createVector();
    
    
    this.angleVelocity = createVector(
      random(-0.05, 0.05),
      random(-0.05, 0.05)
    );
    
    this.angleAcceleration = createVector();
    
    this.amplitude = createVector(
      random(50, width / 2),
      random(50, height / 2)
    );
    
    this.tx = random(1000);
    this.ty = random(2000);

    this.speed = 0.01;
  }
  
     applyForce(f) {
     this.angleAcceleration.add(f);
  }


  update() {

    let spring = p5.Vector.mult(this.angle, -0.01);
    this.applyForce(spring);

    this.angleVelocity.add(this.angleAcceleration);
    this.angle.add(this.angleVelocity);

    this.angleAcceleration.mult(0);

    let vx = map(noise(this.tx), 0, 1, -0.05, 0.05);
    let vy = map(noise(this.ty), 0, 1, -0.05, 0.05);

    this.angle.x += vx;
    this.angle.y += vy;


    this.tx += this.speed;
    this.ty += this.speed;
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);

    stroke(0);
    strokeWeight(2);
    line(0, 0, x, y);
    fill(127);
    circle(x, y, 32);
    pop();
  }
}
```

### Ondas (Actividad 08)

Para poner las ondas en funcionamiento agregue un ángulo (a) que 
``` Js
let angle = 0;
let angleVelocity = 0.2;
let amplitude = 100;

function setup() {
  createCanvas(640, 240);
 
}

function draw(){
  background(255);
  
  stroke(0);
  strokeWeight(2);
  fill(127, 127);
  
  let a  = angle;
  
  for (let x = 0; x <= width; x += 24) {
    // 1) Calculate the y position according to amplitude and sine of the angle.
    let y = amplitude * sin(a);
    // 2) Draw a circle at the (x,y) position.
    circle(x, y + height / 2, 48);
    // 3) Increment the angle according to angular velocity.
     a += 0.3;
  }
  
   angle += angleVelocity;
}
```

### Resortes (Actividad 09)

Para incluir un sistema de dos resortes conectados en serie, el flujo de pensamiento es simple. Consistiendo en agregar un segundo resorte (`spring`) y una seguna masa (`bob`). En esto solo es necesario modificar el sketch.js. 

Para conectarlos use `spring1.connect(bob1); // Que calcula la fuerza élastica entre el ancla y bob1, ley de hooke` para el primer resorte y con el segundo resorte cambie su ancla para que sea la posición del bob1:

``` Js
spring2.anchor = bob1.position;
spring2.connect(bob2);
```

De resto para dibujarlo, mostralo, limitar su longitud y darle gravedad e incluso poder arrastrarlos fue duplicar lo ya dado en el código anterior con solo un bob (y bueno cambiar posiciones o valores ya que se encuentran en diferentes posiciones).

``` Js
let bob1;
let bob2;

let spring1;
let spring2;

function setup() {
  createCanvas(640, 240);

  // primer resorte (anclado arriba)
  spring1 = new Spring(width / 2, 10, 100);

  // segundo resorte
  spring2 = new Spring(width / 2, 110, 100);

  bob1 = new Bob(width / 2, 100);
  bob2 = new Bob(width / 2, 180);
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);

  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  bob1.update();
  bob2.update();

  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);

  // primer resorte conecta al primer bob
  spring1.connect(bob1);

  // segundo resorte usa al primer bob como ancla
  spring2.anchor = bob1.position;
  spring2.connect(bob2);

  spring1.constrainLength(bob1, 30, 200);
  spring2.constrainLength(bob2, 30, 200);

  spring1.showLine(bob1);
  spring2.showLine(bob2);

  bob1.show();
  bob2.show();

  spring1.show();
}

// mouse

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```

### Péndulo (Actividad 10)

Ahora bien, este se asimila al anterior ejercicio, no tanto la lógica general del sistema sino el concepto para realizar un sistema en el que dos péndulos esten conectados en serie. Para este tambien solo es necesario modificar sketch.js y agregar dos masas conectadas una a otra.

```
let p1;
let p2;

function setup() {
  createCanvas(windowWidth, windowHeight);


  p1 = new Pendulum(width / 2, 0, 175);


  p2 = new Pendulum(width / 2, 175, 175);
}

function draw() {
  background(255);

  p1.update();
  p2.update();


  p1.drag();
  p2.drag();


  p1.show();

  // conectar el segundo al primero
  p2.pivot = p1.bob;


  p2.show();
}

function mousePressed() {
  p1.clicked(mouseX, mouseY);
  p2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  p1.stopDragging();
  p2.stopDragging();
}
```

## Bitácora de aplicación 

### Las bandadas de aves V2 (Actividad 11)

> Concepto obra

_idea: ondas, música y aves, un flujo tipico al pensar en una combinación de interacción y movimiento. Asi que retomando el concepto de lo hecho anteriormente y probar algo nuevo, las aves podrian estar guiadas por las ondas, modificando la obra anterior, manteniendo la interacción con el mouse y que este incluya la forma en como los sonidos se reproducen, seria gracioso poner "Westminster" (una melodia que por algún motivo quedo grabada en mi mente tras escucharla como una campanada de una iglesia de cienaga) o "coody freestyle" de steve lacy, algo tranquilo, como observar el cielo durante mucho tiempo e implementar algo religioso para que refuerce el concepto de las aves como lo mas cercano al contacto de un dios._

Pero si fuera acortar esto de forma más directa sería:

```
Una pieza que explora la relación entre ondas, movimiento y sonido.
Un sistema de ondas dinámicas guía el vuelo de aves generativas,
creando patrones cambiantes que evocan la observación prolongada del cielo.
El espectador interactúa mediante el mouse, alterando el flujo de las ondas y, con ello,
el comportamiento de las aves y la activación del sonido.
La obra puede incorporar melodías generando una atmósfera contemplativa
donde las aves funcionan como símbolo de aquello que parece más cercano al contacto con lo divino.
```

> El código


_link: https://editor.p5js.org/LuisaRoech/sketches/cwpdbKWf2_

> La obra



## Bitácora de reflexión





















