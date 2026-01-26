# Unidad 1

## Bitácora de proceso de aprendizaje

### La aleatoriedad en el arte generativo (Actividad 01)
La aleatoriedad engendra a un arte vivo, una obra que se extiende como raíces bajo la tierra o se expande como células vistas al microscopio, despertando emoción sin obedecer a una intención; el artista solo guía la forma y pierde control sobre su propia obra.

<img width="690" height="388" alt="image" src="https://github.com/user-attachments/assets/983bdedb-ebdc-43ba-942c-af72e593c5df" />

*Ian Cheng por ejemplo, crea simulaciones generativas que existen como ecosistemas vivos, donde el artista define las reglas iniciales y luego cede el control al comportamiento impredecible del sistema.*

### Caminatas aleatorias (Actividad 02)
> ¿Qué pienso que va a suceder si cambio valores?

Antes de realizar modificaciones al código, analicé su estructura y formule hipótesis sobre como los cambios afectarian al comportamiento del *walker*. Y porque no para entender un poco mejor la base de...
``` javascript
let walker; 

function setup() {
// Aqui se crea el canvas, inicializa el walker en el centro de el y se define el color de fondo
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step(); // Define en que dirección se movera
  walker.show(); // Mostrara en pantalla los pasos dados, se puede incluir varios al tiempo
}
```

Ahora bien, entrando a la clase importante y que modificare
``` javascript
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
Esta clase especifica y randomiza las desiciones de los pasos, el constructor permite en que punto comienza la acción, con show() en esta sección podemos especificar el color, grosor y se traza a partir de puntos (puede ser un circulo, por ejemplo) y step() cuenta con diferentes desiciones, floor(random(4)) redondeara valores desde 0 hasta 4, y choices puede ser que se trate de en que eje se movera.

Si modifico el trazo como line (no se si es posible pero hay que intentarlo) generara una linea duh, y si cambio los numeros de choices en 2 y en 2 deberia moverse al tiempo en ambos ejes y la primera desicion de x-- la cambio a x++ deberia ir solo en valores positivos al tomar esa desicion.

> Que sucedio realmente >:D

Lo esperado, en realidad el trazo de line no paso, pero fue mas skill issue mio de mi parte

> ¿Por que creo que si ocurrio o no?

Profundizando mas en lo que no salio fue debido a que debia darle un punto mas al declarar line, ya que esta no funciona al igual que point que solo cuenta con dos valores, ahora bien porque si ocurrio lo que espere

### Distribuciones de probabilidad (Actividad 03)
> ¿Que diferencia hay entre la distribución uniforme y una no uniforme de números aleatorios?
Una uniforme, debe contar con la misma probabilidad todas las opciones de que salgan, la no uniforme por el contrario, debe tener mas probabilidad de que un valor salga mas que el otro por medio de la media

si quiero favorecer el movimiento a la derecha con una distribución no uniforme ...

### Distribución Normal (Actividad 04)

### Distribución personalizada: Lévy flight (Actividad 05)
El salto de _Lévy_ es como se escucha, es dar saltos aleatorios en una acción determinada, como la caminata, dar circulos y repentinamente ir en otra direccion, o trazar los puntos en una zona apartada de donde estaba

### Ruido Perlin (Actividad 06)
Hacer ruido permite tener comportamientos muy organicos
<img width="553" height="266" alt="image" src="https://github.com/user-attachments/assets/86e67f5f-2cbb-4f25-a329-0037bc1970c8" />


## Bitácora de aplicación 



## Bitácora de reflexión












