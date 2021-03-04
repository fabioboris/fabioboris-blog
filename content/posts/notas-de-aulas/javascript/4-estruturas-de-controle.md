---
title: "Notas de Aulas de JavaScript: 4. Estruturas de Controle"
date: 2021-02-24T20:50:40-03:00
draft: false
tags: ["js", "if", "switch", "while", "for", "for-in", "for-of", "try-catch"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Estruturas de controle

### if-else

```js
if (condição)
  instrução;

if (condição) {
  instrução1;
  instrução2;
}

if (condição) {
  instrução1;
} else {
  instrução2;
}

if (condição1) {
  instrução1;
} else if (condição2) {
  instrução2;
} else {
  instrução3;
}

// exemplo: comparar dois números inteiros fornecidos pelo usuário.
const n1 = parseInt(prompt("Por favor, digite um número inteiro."));
const n2 = parseInt(prompt("Por favor, digite mais um número inteiro."));

let resp;
if (n1 > n2) {
  resp = `${n1} é maior que ${n2}`;
} else if (n1 > n2) {
  resp = `${n1} é menor que ${n2}`;
} else {
  resp = `${n1} e ${n2} são iguais`;
}

alert(resp);
```


### switch

```js
switch (expressão) {
  case valor1:
    instrução1;
    [break;]
  case valor2:
    instrução2;
    [break;]
  default:
    instrução3;
}

// exemplo: um conceito para uma nota de 1 a 5
let num = parseInt(prompt("Por favor, digite um número inteiro de 1 a 5"));

switch(num) {
  case 1:
    console.log("Muito ruim");
    break;
  case 2:
    console.log("Ruim");
    break;
  case 3:
    console.log("Regular");
    break;
  case 4:
    console.log("Bom");
    break;
  case 5:
    console.log("Muito bom");
    break;
  default:
    console.log("Número fora do intervalo esperado");
}
```


### while

```js
// while (expressão)
//   instrução;

// exemplo: listar os números de 1 até 10
let contador = 1;
while (contador <= 10) {
  console.log(contador);
  contador++;
}
```


### do-while

```js
// do {
//   instrução;
// } while (expressão);

// exemplo: interromper o loop quando o usuário digitar SAIR
let comando;
do {
  comando = window.prompt("Para terminar, digite SAIR.");
} while (comando !== "SAIR");
```


### for

```js
// for (expressãoInicial; condição; expressãoDeIncremento)
//   instrução;

// exemplo: listar os números de 1 até 10
for (let contador = 1; contador <= 10; contador++) {
  console.log(contador);
}
```


### for-in

```js
// for (variável in objeto)
//   instrução;

// exemplo: iterar os atributos de um objeto e mostrar seus valores
let objeto = { nome: "Fulano", email: "fulano@gmail.com" };
for (let atributo in objeto) {
  console.log(`O atributo ${atributo} tem valor ${objeto[atributo]}`);
}
```


### for-of

```js
// for (variável of objeto)
//   instrução;

// exemplo: iterar os itens de um array e mostrar seus valores
let frutas = ["Laranja", "Banana", "Abacate"];
for (let item of frutas) {
  console.log(item);
}
```


### break

```js
// break [rótulo];

// exemplo: interromper o loop quando o usuário digitar SAIR
let comando;
while(true) {
  comando = window.prompt("Para terminar, digite SAIR.")
  if (comando === "SAIR") break;
}
```


### continue

```js
// continue [rótulo];

// somar apenas os números pares
let i, n, s;
for (i = 0, s = 0; i < 10; i++) {
  n = parseFloat(window.prompt("Digite um número:"));
  if (n % 2 == 1) continue; // ir para a próxima iteração
  s += n;
}
console.log("Soma:", s);
```


## Trabalhando com erros

### Gerando erros

```js
function obterNumero() {
  const str = prompt("Por favor, digite um número inteiro");
  const num = parseInt(str);
  if (isNaN(num)) {
    throw "Número inválido";
  } else {
    return num;
  }
}
```

### Verificando erros

```js
try {
  const n1 = obterNumero();
  const n2 = obterNumero();
  const resp = n1 + n2;

  // gerar um erro caso o resultado seja negativo
  if (resp < 0) throw "Resultado negativo";

  console.log(`${n1} + ${n2} = ${resp}`);
} catch(err) {
  console.error(err);
} finally {
  console.log("Fim da execução");
}
```

