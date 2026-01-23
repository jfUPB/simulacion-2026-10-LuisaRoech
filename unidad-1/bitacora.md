# Unidad 1

## Bitácora de proceso de aprendizaje

### La aleatoriedad en el arte generativo (Actividad 01)
La aleatoriedad engendra a un arte vivo, una obra que se extiende como raíces bajo la tierra o se expande como células vistas al microscopio, despertando emoción sin obedecer a una intención; el artista solo guía la forma y pierde control sobre su propia obra.

<img width="690" height="388" alt="image" src="https://github.com/user-attachments/assets/983bdedb-ebdc-43ba-942c-af72e593c5df" />

*Ian Cheng por ejemplo, crea simulaciones generativas que existen como ecosistemas vivos, donde el artista define las reglas iniciales y luego cede el control al comportamiento impredecible del sistema.*

### Caminatas aleatorias (Actividad 02)
> ¿Que pienso que va a suceder si cambio valores?

Primero entendiendo el codigo sketch.js
```
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
```
Se crea un canvas que limita el espacio donde se va a mover los pasos, la variable **walker** ...
En la función **draw()**, contamos con .step(); que define en que direccion se movera y .show() que mostrara en pantalla los pasos dados, Se puede incluir varios .step() lo que mostrara diferentes pasos al tiempo, no solo uno (asumo).

Ahora bien, entrando a la clase importante y que modificare
```
class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```
Esta clase especifica y randomiza las desiciones de los pasos, el constructor permite en que punto comienza la acción, con show() en esta sección podemos especificar el color, grosor y se traza a partir de puntos (puede ser un circulo, por ejemplo) y step() cuenta con diferentes desiciones, floor(random(4)) redondeara valores desde 0 hasta 4, y choices puede ser que se trate de en que eje se movera y tal vez tal vez velocidad

Si modifico el trazo como line (no se si es posible pero hay que intentarlo) generara una linea duh, y si cambio los numeros de choices en 2 y en 2 deberia moverse al tiempo en ambos ejes y la primera desicion de x-- la cambio a x++ deberia ir solo en valores positivos al tomar esa desicion.

> Que sucedio realmente >:D
> ¿Por que creo que si ocurrio o no?
## Bitácora de aplicación 



## Bitácora de reflexión








