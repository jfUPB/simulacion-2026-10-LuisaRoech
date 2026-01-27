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

Omitiendo lo del line(), en general el codigo es lo suficientemente básico y claro como para anticipar qué sucede al modificar ciertos valores o funciones, ya que cada parte cumple una función especifica y facil de identificar dentro de la lógica general de walker.

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
Para este ejercicio modifique el codigo _The nature of code_ ejemplo 0.4, que ya utiliza distribución gausiana, pues queria replicar lo ya visto en clase pero con rayas verticales :)
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

![Grabacindepantalla2026-01-26204734-ezgif com-resize](https://github.com/user-attachments/assets/489e6265-de63-4002-bf6f-59c2a18f7c19)

_Link: https://editor.p5js.org/LuisaRoech/sketches/DzN2wrTj1_

### Distribución personalizada: Lévy flight (Actividad 05)
El salto de _Lévy_ es como se escucha (literal), es dar saltos aleatorios en una acción determinada, como la caminata, dar circulos y repentinamente ir en otra direccion, o trazar los puntos en una zona apartada de donde estaba

### Ruido Perlin (Actividad 06)
Hacer ruido permite tener comportamientos muy organicos

<img width="553" height="266" alt="image" src="https://github.com/user-attachments/assets/86e67f5f-2cbb-4f25-a329-0037bc1970c8" />


## Bitácora de aplicación 



## Bitácora de reflexión

![cat-spinning](https://github.com/user-attachments/assets/65233b8f-923b-4db5-8065-7b2a7baddcef)



