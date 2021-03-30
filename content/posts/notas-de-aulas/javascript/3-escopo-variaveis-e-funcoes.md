---
title: "Notas de Aulas de JavaScript: 3. Escopo, Variáveis e Funções"
date: 2021-02-24T20:50:30-03:00
lastmod: 2021-03-30T15:00:00-03:00
draft: false
tags: ["js", "escopo", "variáveis", "funções"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Escopo, Variáveis e Funções

```js
valor1 = 1; // escopo global
var valor2 = 12; // escopo de função
let valor3 = 123; // escopo de bloco
const valor4 = 1234; // escopo de bloco
                     // não é possível alterar o valor de uma constante
```


### Escopo global

```js
valor1 = 1;
function func1() {
  valor1 = 2;
  {
    valor1 = 3;
    console.log("func1 >> {} >> valor1:", valor1); // 3
  }
  console.log("func1 >> valor1:", valor1); // 3
}
func1();
console.log("valor1:", valor1); // 3
```


### Escopo de função

```js
var val2 = 1;
function func2() {
  var val2 = 2;
  {
    var val2 = 3;
    console.log("func2 >> {} >> val2:", val2); // 3
  }
  console.log("func2 >> val2:", val2); // 3
}
func2();
console.log("val2:", val2); // 1
```


### Escopo de bloco

```js
let val3 = 1;
const con3 = 1;
function func3() {
  let val3 = 2;
  const con3 = 2;
  {
    let val3 = 3;
    const con3 = 3;
    console.log("func3 >> {} >> val3, con3:", val3, con3); // 3 3
  }
  console.log("func3 >> val3:", val3, con3); // 2 2
}
func3();
console.log("val3, con3:", val3, con3); // 1 1
```


## Funções

### Declaração de funções, argumentos e retorno

```js
// uma função que ao ser executada imprime uma mensagem
function ola() {
  console.log("Olá! Tudo bem?");
}
ola(); // imprime "Olá! Tudo bem?"

// uma função que recebe um valor como parâmetro
// e imprime uma mensagem com esse valor
function ola_v2(nome) {
  console.log("Olá", nome);
}
ola_v2("Fulano"); // imprime "Olá Fulano"

// uma função que recebe dois valores como parâmetros
// e retorna o valor da soma desses dois valores
function soma(num1, num2) {
  return num1 + num2;
}
resp = soma(12, 21); // resp vale 33
```


### Funções anônimas

```js
// a variável soma_v2 recebe como valor uma função
// que recebe dois valores como parâmetros
// e que retorna como valor a soma desses dois parâmetros
soma_v2 = function(num1, num2) {
  return num1 + num2;
}
resp = soma_v2(1, 10); // resp vale 11
```


### *Arrow functions*

```js
// exemplo similar ao anterior
// a declaração é feita com a sintaxe conhecida como *arrow function*, =>
soma_v3 = (num1, num2) => {
  return num1 + num2;
}
resp = soma_v3(2, 20); // resp vale 22

// exemplo similar ao anterior
// arrow functions sem as chaves {} retornam o valor de sua expressão
soma_v4 = (num1, num2) => num1 + num2;
resp = soma_v4(5, 10); // resp vale 15

// arrow function com apenas um argumento dispensam parênteses
dobro = num => 2 * num;
resp = dobro(8); // resp vale 16
```

**Observação:** *Arrow functions* não possuem `this` e não devem ser usadas como métodos.


### Métodos

```js
let pessoa = {
  nome: "Fulano",
  sobrenome: "Silva",
  // nomeCompleto: function(){
  //   return `${this.nome} ${this.sobrenome}`;
  // }
  nomeCompleto(){
    return `${this.nome} ${this.sobrenome}`;
  }
};
console.log(pessoa.nomeCompleto());
```

Veja mais sobre métodos em: [9. Programação Orientada a Objetos](../9-programacao-orientada-a-objetos)


### Funções autoexecutáveis

```js
(function(){ console.log("Mensagem da função."); })();

(function(){ console.log("Mensagem da função."); }());

(()=>{ console.log("Mensagem da função."); })();

(()=>{ console.log("Mensagem da função."); }());
```


### Preservar o escopo global de um documento HTML

``` HTML
<!DOCTYPE html>
<html>
<head></head>
<body>
  <button type="button" id="btnAumentar">Aumentar</button>
  <button type="button" id="btnDiminuir">Diminuir</button>
  <button type="button" id="btnMostrar">Mostrar</button>
<script>
(function(){
  let numero = 0;
  const aumentar = function() {
    numero = numero + 10;
    mostrar();
  }
  const diminuir = function() {
    numero = numero - 10;
    mostrar();
  }
  const mostrar = function() {
    console.log(numero);
  }
  document.getElementById("btnAumentar").onclick = aumentar;
  document.getElementById("btnDiminuir").onclick = diminuir;
  document.getElementById("btnMostrar").onclick = mostrar;
  mostrar();
})();
</script>
</body>
</html>
```

