# Unidad 2

## Bitácora de proceso de aprendizaje

### Los vectores y el arte (Actividad 01)


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

## Bitácora de aplicación 



## Bitácora de reflexión
