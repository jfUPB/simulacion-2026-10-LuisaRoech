# Unidad 5

## Bitácora de proceso de aprendizaje

### Anatomía de una partícula (Actividad 01)

Analizando el ejemplo 4.2, Update() es quien abarca el marco motion 101 de este sistema, dentro de este se reduce de forma gradual la vida de la particula, quien inicialmente esta dada por this.lifespan = 255.0;

 // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

Ahora bien, el no reiniciara las particulas y estas se eliminarán

Aunque dentro de este código existe una estructura interesante al momento de leer la vida de una particula, en vez de leerla de la forma tradicional esta se lee al reves usando let i = particles.length - 1; principalmente por el uso de particles.splice(i, 1); que elimina una particula del array de particulas, por lo tanto hacerlo de forma tradicional lo vuelve suceptible a no recorrer correctamente todo el for.

### Del array al sistema: la abstracción del emisor (Actividad 02)

## Bitácora de aplicación 


## Bitácora de reflexión
