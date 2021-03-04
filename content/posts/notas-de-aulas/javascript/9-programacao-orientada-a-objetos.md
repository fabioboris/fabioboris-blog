---
title: "Notas de Aulas de JavaScript: 9. Programação Orientada a Objetos"
date: 2021-02-28T21:13:17-03:00
draft: true
tags: ["js", "poo", "prototype", "class", "herança"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Programação orientada a objetos

### Implementação com `prototype`

```js
// parte 1: definir os construtores
function Forma2d(x, y) {
  this.x = x;
  this.y = y;
}
function Retangulo(x, y, base, altura) {
  Forma2d.call(this, x, y);
  this.base = base;
  this.altura = altura;
}
function Circulo(x, y, raio) {
  Forma2d.call(this, x, y);
  this.raio = raio;
}

// parte 2: definir os "links" entre os protótipos dos construtores (herança)
Retangulo.prototype = Object.create(Forma2d.prototype);
Circulo.prototype = Object.create(Forma2d.prototype);

// parte 3: definir os métodos
Forma2d.prototype.desenhar = function() {
  console.log(`Desenhando uma forma 2d em (${this.x}, ${this.y}).`);
}
Retangulo.prototype.desenhar = function() {
  console.log(`Desenhando um retângulo em (${this.x}, ${this.y}) com base ${this.base} e altura ${this.altura}.`);
}
Circulo.prototype.desenhar = function() {
  console.log(`Desenhando um círculo em (${this.x}, ${this.y}) com raio ${this.raio}.`);
}

// criar os objetos
let f1 = new Forma2d(100, -100);
let r1 = new Retangulo(-10, 10, 20, 40);
let c1 = new Circulo(0, 0, 200);
```


Inspecionando o objeto `f1`:

    f1
      Forma2d {x: 100, y: -100}
        x: 0
        y: 1
        __proto__:
          desenhar: ƒ ()
          constructor: ƒ Forma2d(x, y)
          __proto__: Object


Inspecionando o objeto `r1`:

    r1
      Retangulo {x: -10, y: 10, base: 20, altura: 40}
      altura: 40
      base: 20
      x: -10
      y: 10
      __proto__: Forma2d
        __proto__:
          desenhar: ƒ ()
          constructor: ƒ Forma2d(x, y)
          __proto__: Object


### Implementação com `class`

```js
class Forma2d {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  desenhar() {
    console.log(`Desenhando uma forma 2d em (${this.x}, ${this.y}).`);
  }
}

class Retangulo extends Forma2d {
  constructor(x, y, base, altura) {
    super(x, y);

    this.base = base;
    this.altura = altura;
  }

  desenhar() {
    console.log(`Desenhando um retângulo em (${this.x}, ${this.y}) com base ${this.base} e altura ${this.altura}.`);
  }
}

class Circulo extends Forma2d {
  constructor(x, y, raio) {
    super(x, y);

    this.raio = raio;
  }

  desenhar() {
    console.log(`Desenhando um círculo em (${this.x}, ${this.y}) com raio ${this.raio}.`);
  }
}

let f1 = new Forma2d(100, -100);
let r1 = new Retangulo(-10, 10, 20, 40);
let c1 = new Circulo(0, 0, 200);
```

