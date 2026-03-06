# Unidad 4

## Bitácora de proceso de aprendizaje


### Funciones sinusoides (Actividad 06)

Una función que aparece en sonido, luz, movimiento, mareas, electricidad y vibraciones (patrones rítmicos).

_y(t) = Asen(wt + π)_

Amplitud (A)
Frecuencia()
Fase

### Repasa conceptos de las unidades anteriores (Actividad 07)

´´´ Javascript
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
´´´

### (Actividad 08)

## Bitácora de aplicación 



## Bitácora de reflexión

