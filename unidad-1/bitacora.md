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
  // Posición inicial en el centro del canvas
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
  // Define su color y el tipo de trazo en este caso como un punto (puede tomar cualquier forma)
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

Omitiendo lo del line(), en general el codigo es lo suficientemente básico y claro como para anticipar qué sucede al modificar ciertos valores o funciones, ya que cada parte cumple una función especifica y facil de identificar dentro de la lógica general de walker.

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

