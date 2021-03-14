---
title: "Notas de Aulas de JavaScript: 9. Programação Orientada a Objetos"
date: 2021-03-07T15:30:10-03:00
lastmod: 2021-03-13T21:50:10-03:00
draft: false
tags: ["js", "objetos", "prototype", "class", "herança", "monkeypatch"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Programação orientada a objetos

JavaScript é uma linguagem de programação baseada em protótipos (*prototype-based language*), onde propriedades e métodos podem ser compartilhadas entre objetos.
Algumas características interessantes sobre objetos em JavaScript:

* Quase todos objetos JavaScript são instâncias de `Object`.
* Um objeto típico herda as propriedades e métodos de `Object.prototype`.
* Propriedades e métodos de `prototype` podem ser sobrescritas.
* As alterações em `prototype` podem ser vistas por todos os objetos descendentes.
* O construtor `Object` cria um objeto *wrapper* para um valor dado.
* O construtor `Object` cria um objeto vazio para os valores `null` e `undefined`.

Um erro comum é acreditar que números inteiros (literais) não podem ser usados como objetos, pois o ponto `.` faz com que o interpretador o interprete como um número de ponto flutuante.

```js
123.toString(); // produz um erro de sintaxe

(123).toString(); // "123"
123.0.toString(); // "123"
123..toString(); // "123"
123 .toString(); // "123"
```

Veja mais em [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object).


### Implementação com `prototype`

Veremos um exemplo que faz uso do `prototype` para compartilhar propriedades e métodos entre objetos descendentes, em outras palavras, herança. Para isso, criaremos três **funções construtoras**, a primeira, *Coordenada2D*, que será usada como base para as outras duas, *Retangulo* e *Circulo*.

```js
function Coordenada2D(x, y) {
  this.tipo = "Coordenada 2D";
  this.x = x;
  this.y = y;
}
function Retangulo(x, y, base, altura) {
  Coordenada2D.call(this, x, y);
  this.tipo = "Retângulo";
  this.base = base;
  this.altura = altura;
}
function Circulo(x, y, raio) {
  Coordenada2D.call(this, x, y);
  this.tipo = "Círculo";
  this.raio = raio;
}
```

Na sequência, são criadas as relações de herança entre o `prototype` de *Coordenada2D* e o `prototype` de *Retangulo* e *Circulo*.

```js
Retangulo.prototype = Object.create(Coordenada2D.prototype);
Circulo.prototype = Object.create(Coordenada2D.prototype);
```

Após a definição da relação de herança, definiremos os métodos `mostrarPosicao` de *Coordenada2D* e os métodos `mostrarAtributos` de *Retangulo* e de *Circulo*.

```js
Coordenada2D.prototype.mostrarPosicao = function() {
  return `${this.tipo} na posição (${this.x}, ${this.y})`;
};
Retangulo.prototype.mostrarAtributos = function() {
  return `${this.mostrarPosicao()} com base ${this.base} e altura ${this.altura}`;
};
Circulo.prototype.mostrarAtributos = function() {
  return `${this.mostrarPosicao()} com raio ${this.raio}`;
};
```

```js
let r = new Retangulo(-10, 10, 200, 400);
r.mostrarPosicao(); // "Retângulo na posição (10, -10)"
r.mostrarAtributos(); // "Retângulo na posição (10, -10) com base 200 e altura 400"

let c = new Circulo(0, 0, 200);
c.mostrarPosicao(); // "Círculo na posição (0, 0)"
c.mostrarAtributos(); // "Círculo na posição (0, 0) com raio 200"
```


Inspecionando os objetos `r` e `c`, temos:

    Retangulo {tipo: "Retângulo", x: 10, y: -10, base: 200, altura: 400}
      altura: 400
      base: 200
      tipo: "Retângulo"
      x: 10
      y: -10
      __proto__: Coordenada2D
        mostrarAtributos: ƒ ()
        __proto__:
          mostrarPosicao: ƒ ()
          constructor: ƒ Coordenada2D(x, y)
          __proto__: Object

    Circulo {tipo: "Círculo", x: 0, y: 0, raio: 200}
      raio: 200
      tipo: "Círculo"
      x: 0
      y: 0
      __proto__: Coordenada2D
        mostrarAtributos: ƒ ()
        __proto__:
          mostrarPosicao: ƒ ()
          constructor: ƒ Coordenada2D(x, y)
          __proto__: Object


### Implementação com `class`

Entendemos classes como *templates* para criar objetos.
Em JavaScript, classes são funções construtoras com uma propriedade `prototype`.

A partir da especificação ECMAScript 2015 (ES6), JavaScript passou a contar com implementação de objetos a partir de classes.
Essa sintaxe é descrita como um *açúcar sintático*, uma forma mais "simples" de implementar objetos com `prototype`.

A seguir, replicamos o código apresentado previamente implementado com `prototype`, agora com `class`.

```js
class Coordenada2D {
  constructor(x, y) {
    this.tipo = "Coordenada 2D";
    this.x = x;
    this.y = y;
  }

  mostrarPosicao() {
    return `${this.tipo} na posição (${this.x}, ${this.y})`;
  }
}

class Retangulo extends Coordenada2D {
  constructor(x, y, base, altura) {
    super(x, y);

    this.tipo = "Retângulo";
    this.base = base;
    this.altura = altura;
  }

  mostrarAtributos() {
    return `${this.mostrarPosicao()} com base ${this.base} e altura ${this.altura}`;
  }
}

class Circulo extends Coordenada2D {
  constructor(x, y, raio) {
    super(x, y);

    this.tipo = "Círculo";
    this.raio = raio;
  }

  mostrarAtributos() {
    return `${this.mostrarPosicao()} com raio ${this.raio}`;
  }
}

let r = new Retangulo(-10, 10, 200, 400);
r.mostrarPosicao(); // "Retângulo na posição (10, -10)"
r.mostrarAtributos(); // "Retângulo na posição (10, -10) com base 200 e altura 400"

let c = new Circulo(0, 0, 200);
c.mostrarPosicao(); // "Círculo na posição (0, 0)"
c.mostrarAtributos(); // "Círculo na posição (0, 0) com raio 200"
```


### *Monkey patch* (Bônus! ⭐)

*Monkey patch* é uma técnica usada para estender ou modificar o comportamento de componentes de um sistema de software em tempo de execução.

> **🤚 Cuidado:** essa é uma técnica que deve ser usada com cautela.

Como exemplo, estenderemos o objeto `Array` com um novo método que retorna somente os elementos que forem pares, e modificaremos o método `pop` de forma que ele remova **todos** os elementos de um `Array`.

```js
// Adicionar o método somentePares() ao objeto Array
Array.prototype.somentePares = function() {
  return this.filter(item => item % 2 === 0);
};

const numeros = [1, 1, 2, 3, 5, 8, 13, 21];
numeros.somentePares(); // [2, 8]

// Substituir o método pop do objeto Array
Array.prototype.pop = function() {
  while (this.length > 0) this.shift();
};

numeros.pop(); // agora numeros é igual a []
```
