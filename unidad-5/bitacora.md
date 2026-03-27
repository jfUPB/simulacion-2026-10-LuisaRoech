# Unidad 5

## Bitácora de proceso de aprendizaje

### Anatomía de una partícula (Actividad 01)

Analizando el ejemplo 4.2, `Update()` es quien abarca el marco motion 101 de este sistema, actualizando la particula en cada frame. Dentro de este, algo interesante es que se reduce de forma gradual la vida de la particula, quien inicialmente esta dada por `this.lifespan = 255.0;`, lo que indica un desvanecimiento progresivo en lugar de una desaparición instantánea.

``` Js
 // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }
```

El constructor define las propiedades principales de la particula y su clase es quien las crea, despues en `sketch.js ` se recorren para evaluar su estado (si estan vivas o muertas). Ahora bien, si las partículas no se eliminaran, el sistema acumularía cada vez más instancias, aumentando el uso de memoria y afectando el rendimiento (frame rate).

Aunque dentro de este código existe una estructura interesante al momento de leer la vida de una particula, en vez de leerla de la forma tradicional esta se lee al reves usando `let i = particles.length - 1;` (lo clave esta en el -1). Esto se debe a que se emplea `particles.splice(i, 1);` para eliminar partículas del arreglo; si se recorriera de forma tradicional (de inicio a fin), los índices cambiarían al eliminar elementos, provocando que algunas partículas no se procesen correctamente dentro del `for`.

### Del array al sistema: la abstracción del emisor (Actividad 02)

Del ejemplo 4.4 lo importante y diferenciador del ejemplo 4.2 (el anterior) esta en el `emitter.js`. Quien ahora se encarga de agregar un origen que generará las particula en base donde se ha presionado con el mouse, al hacer click las crea y el chequeo `for` que se encarga de leer el estado de una particula ahora se encuentra en esta. 
Pero ¿por qué separar esto en una nueva clase? Principalmente para que sea modular, cada emisor funciona de forma independiente, se puede tener múltiples sistemas sin duplicar la lógica, puede servir en distintos contextos y separa "quién emite" de "qué se emite".

¿Quién crea qué?

- Emitters → los crea el _sketch_ (por ejemplo en `setup()` o eventos)
- Partículas → las crea cada Emitter

> Diagrama

```
[ Sketch ]
     ↓
[ Emitters ]  →  [Emitter 1] → [Partículas]
              →  [Emitter 2] → [Partículas]
              →  [Emitter 3] → [Partículas]
```

> El sistema está organizado como: una entidad principal contiene múltiples emisores, y cada emisor gestiona su propia colección de partículas. Los niveles de colección son 1. Array de emitters y 2. Array de particulas dentro de cada emitter.

### Heterogeneidad: herencia y polimorfismo (Actividad 03)

Del ejemplo 4.5 se incluye dos tipos distintos de particulas, en la que `confetti.js` hereda de la otra particula `particle.js`, comparten el mismo constructor, las mismas fuerzas, el ciclo de vida y la misma estructura del motion 101, lo unico que se diferencia la una de la otra es en el `show()` que permite que tengan una forma distinta, una esta dada por cuadros y la otra por circulos.

Cosas interesantes explicadas es del porque el `emitter.js` no necesita saber que tipo de particula esta gestionando. estas funciones en general son interesantes:

``` Js
 addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```

La razón principal es para hacer el sistema más flexible y extensible. En `run()` se ejecuta las dos particulas agregadas anteriormente en `addParticle()`, por lo que no es necesario tocar esta función al momento de implementar nuevas cosas, pues esta independientemente si se agregan mas o menos ejecutara todas las que estan listadas en `addParticle()`, que justo con esta tambien podria realizarse lo mismo agregrando un objeto que tome `particle.js` (si suponemos que todas las particulas van a heredar de esta como lo hizo confetti) y se ejecute de forma general. Lo unico malo es que es un proceso más lento y menos eficiente que agregar una por una, ya que se llama una por una para ejecutarla y no todas al tiempo.

> En resumen: El Emitter trata a todas las partículas como “entidades genéricas” sin importar cómo se vean.

Ahora si quiero agregar una nueva particula, solo es necesario crear un nuevo archivo .js que herede de particle, llamar las funciones que quiero retormar de esta y crear otras, despues simplemente este nuevo archivo debe ser llamado en el emitter en `addParticle()`. _NO MODIFICAR: el emiiter, sketch y la lógica de vida o actualización base._

###  Fuerzas y partículas (Actividad 04)

- Ejemplo 4.6: La gravedad se define en el _sketch_ como un vector constante y el sistema (el emitter) la aplica usando `applyForce()` sobre cada particula. Siendo global, pues es la misma para todas las partículas y no depende de su posición
  
- Ejemplo 4.7: por otro lado en este la gravedad se mantiene constante que ¨vive¨ fuera, como condicion general y se incluye un repeller que depende de la distancia que "vive" en una entidad específica siendo una fuente localizada de fuerza.

_Nota: El repeller se basa en una fuerza inversamente proporcional a la distancia (tipo ley de atracción/repulsión, similar a gravedad o Coulomb) = A menor distancia, mayor fuerza._

Y de estos dos ejemplos `particle.js` no cambia, la partícula no "sabe" de dónde vienen las fuerzas, solo sabe cómo aplicarlas. 

_Tabla_

| Aspecto | 4.2 | 4.4 | 4.5 | 4.6 | 4.7 |
|--------|-----|-----|-----|-----|-----|
| ¿Quién crea partículas? | _Sketch_ | _Emitter_ | _Emitter_ | _Emitter_ | _Emitter_ |
| ¿Hay clase Emitter? | NO | SI | SI | SI | SI |
| ¿Hay herencia? | NO | NO | SI | NO | NO |
| ¿Hay fuerzas externas? | NO | NO | NO | SI | SI |
| ¿Hay interacción? | NO | SI(?) | NO | NO | SI |
| ¿Cómo mueren? | _Lifespan_ | _Lifespan_ | _Lifespan_ | _Lifespan_ | _Lifespan_ |

**MODIFICACIÓN QUIRÚRGICA**

Para este ejercicio decidi tomar el **(b)** Cambiar las fuerzas sin cambiar la estructura ni la visualización. Reemplazando la gravedad por una que oscile:

``` Js
let g = map(sin(frameCount * 0.05), -1, 1, 0.05, 0.2);
let gravity = createVector(0, g);
emitter.applyForce(gravity);
```

> código (solo _sketch_ que fue el que modifique)

``` Js
// One ParticleSystem
let emitter;

//{!1} One repeller
let repeller;

function setup() {
  createCanvas(640 , 240);
  emitter = new Emitter(width / 2, 60);
  repeller = new Repeller(width / 2, 250);
}

function draw() {
  background(255);
  emitter.addParticle();
  let g = map(sin(frameCount * 0.05), -1, 1, 0.05, 0.2);
let gravity = createVector(0, g);
emitter.applyForce(gravity);
  //{!1} Applying the repeller
  emitter.applyRepeller(repeller);
  emitter.run();

  repeller.show();
}
```

## Bitácora de aplicación 

### Ciclo de vida (Actividad 05)

> concepto

Siempre me resulta natural comenzar a desarrollar una obra a partir de algo reciente que me haya impactado. En este caso, dejo de lado mi interés en las aves para centrarme en este poema, el cual explora una relación profundamente ambigua entre madre e hijo.

```
Mother, mother... Mother of me,

I know I know I should not miss you so, but mother of me, I do. Your
pained breaths that rasp'd and reverberated in your rusted iron tomb...
The blood of your breast that nourish'd me and warmed me in its caress,
when corpse and cruelty were all I witnessed...

Mother, mother... Mother of me,

I know I know you would hate me so, and mother of me, I do too. But I
would not feel, not think, not dream, were it not for you in my rusted
iron womb... Your tortured love brought me to this war, that I could
take the heart of another, and need you no more.

Mother, mother... Mother of me,

I know I know your thoughts had left you long ago, and mother of me, I
will never truly know. But I hope it redeems my life even just a slight,
when I cried... And crushed your skull that final night.
```

La obra se construye a partir de una dependencia afectiva extrema, donde el hijo existe gracias a la madre, pero al mismo tiempo es moldeado por su sufrimiento, su deterioro y su incapacidad de brindar un amor sano. La madre es simultáneamente origen, refugio y prisión.

Esta relación no es solo de cuidado, sino de violencia, culpa y transformación. El hijo reconoce que todo lo que es proviene de ella —su capacidad de sentir, pensar y existir—, pero también que ese origen está marcado por el dolor. Esto genera una dualidad constante entre amor y rechazo, donde la necesidad de liberarse implica necesariamente destruir aquello que le dio vida.

El acto final no se presenta únicamente como violencia, sino como una forma ambigua de emancipación, redención o ruptura, en la que el hijo deja de depender de la madre al costo de perderla.

> Boceto

Opción 1 Hijo

Opción 2 Madre

> Mapa de desiciones

**Opción 1 Hijo**

![Mother of me - Mapa de desiciones](https://github.com/user-attachments/assets/a7a13d31-6a27-4816-8ba3-5db1853ea4e0)

**Opción 2 Madre**

![Mother of me - Mapa de desiciones (1)](https://github.com/user-attachments/assets/8313fe37-32a2-4a21-85b2-bfa92aa7370b)

_Link:https://miro.com/welcomeonboard/WTBLekZOa2YxYk9GQTRpU3lIbm13QTZVS1FBeExNc2N4NnZORkY2d283L3R4Z1I0aUpVd3RaMXlVRUt6Z0JHMzlWVWpTZnptS1EvaFBSVjVNMFBEUnZrMlRUbHB5SzlHdHcrZ1E2eWpJbWhha08yRWlzT2kyckc4WjIrOWlieDJ3VHhHVHd5UWtSM1BidUtUYmxycDRnPT0hdjE=?share_link_id=752482876973_

> Código

``` Js

```
_link:_

> Obra

## Bitácora de reflexión

### (Actividad 06)

**PARTE 1** — Describe con tus propias palabras cada uno de estos 10 principios:

1. Una partícula es una entidad con estado.
2. Una partícula tiene ciclo de vida.
3. Un sistema de partículas gestiona colecciones dinámicas de elementos.
4. La creación y eliminación de partículas no es un detalle técnico menor, sino parte central del modelo.
5. Debe haber separación entre la lógica de una partícula individual y la lógica del sistema/emisor.
6. Un emisor o particle system es una abstracción importante.
7. Pueden existir sistemas de sistemas.
8. Puede haber heterogeneidad usando herencia y polimorfismo.
9. Las partículas pueden responder a fuerzas globales y locales.
10. La representación visual puede variar sin cambiar el principio algorítmico de fondo.

**PARTE 2** — Transferencia a otra herramienta

Piensa en tu pieza del Apply: si la quisieras recrear en Unity (o TouchDesigner, o Blender), ¿Qué se mantendría igual y qué cambiaría? ¿Qué partes de tu diseño son independientes de la herramienta?
