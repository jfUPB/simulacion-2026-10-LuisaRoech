# Unidad 5

## Bitácora de proceso de aprendizaje

### Anatomía de una partícula (Actividad 01)

Analizando el ejemplo 4.2, `Update()` es quien abarca el marco motion 101 de este sistema que actualiza la particula de cada frame, dentro de este algo interesante es que se reduce de forma gradual la vida de la particula, quien inicialmente esta dada por `this.lifespan = 255.0;`

``` Js
 // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }
```

El constructor es quien define todo lo principal de la particula y su clase es quien las crea, despues en `sketch.js ` se recorre estas para saber su estado (si estan vivas o muertas). Ahora bien, el no reiniciar las particulas le exigirá mucho mas a la CPU para recorrer y leer todas las particulas dadas.

Aunque dentro de este código existe una estructura interesante al momento de leer la vida de una particula, en vez de leerla de la forma tradicional esta se lee al reves usando `let i = particles.length - 1;` (lo clave esta en el -1). Principalmente se hace por el uso de `particles.splice(i, 1);` que elimina una particula del array de particulas, por lo tanto hacerlo de forma tradicional lo vuelve suceptible a no recorrer correctamente todo el for.

### Del array al sistema: la abstracción del emisor (Actividad 02)

Del ejemplo 4.4 lo importante y diferenciador del ejemplo 4.2 (el anterior) esta en el `emitter.js`, quien se encarga ahora de agregar un origen de donde se generarán las particula en base de donde se ha presionado con el mouse y el chequeo for que se encarga de leer el estado de una particula ahora se encuentra en esta, pero ¿por que separar esto en una nueva clase? Para que la lectura sea más eficiente :p 

Como ejemplo seria pensar en...


### Heterogeneidad: herencia y polimorfismo (Actividad 03)

Del ejemplo 4.5, en esta se incluye dos tipos distintos de particulas, en la que `confetti.js` hereda de la otra particula `particle.js`, comparten el mismo constructor, las mismas fuerzas y la misma estructura del motion 101, lo unico que se diferencia la una de la otra es en el `show()` que permite que tengan una forma distinta, una esta dada por cuadros y la otra por circulos.

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

La razón principal es para generar mayor eficiencia en el momento de querer expandir el proyecto y agregar mas particulas. En `run()` se ejecuta las dos particulas agregadas anteriormente en `addParticle()`, por lo que no es necesario tocar esta función al momento de implementar nuevas cosas, pues esta independientemente si se agregan mas o menos ejecutara todas las que estan listadas en `addParticle()`, que justo esta tambien podria realizarse lo mismo agregrando un objeto que tome `particle.js` (si suponemos que todas las particulas van a heredar de esta como lo hizo confetti) y se ejecute de forma general. Lo unico malo es que es un proceso más lento y menos eficiente que agregar una por una, ya que se llama una por una para ejecutarla y no todas al tiempo.

Si quiero agregar una nueva particula, solo es necesario crear un nuevo archivo .js que herede de particle, llamar las funciones que quiero retormar de esta y crear otras, despues simplemente este nuevo archivo debe ser llamado en el emitter en `addParticle()`, por ejemplo:



###  Fuerzas y partículas (Actividad 04)


## Bitácora de aplicación 


## Bitácora de reflexión
