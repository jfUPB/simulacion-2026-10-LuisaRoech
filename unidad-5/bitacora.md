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


## Bitácora de aplicación 


## Bitácora de reflexión
