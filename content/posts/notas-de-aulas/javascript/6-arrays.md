---
title: "Notas de Aulas de JavaScript: 6. Arrays"
date: 2021-02-28T21:13:14-03:00
draft: false
tags: ["js", "arrays"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Trabalhando com arrays

### Obter o tamanho de um `Array`

```js
let cores = ["verde", "amarelo", "vermelho"];
let tamanho = cores.length;
```


### Acessar um item de um `Array`

```js
let primeira = cores[0];
let ultima = cores[cores.length - 1];
```


### Iterar um `Array`

```js
// usando forEach
// cores.forEach(function(item, indice, array) {
//   console.log(item);
// });
cores.forEach(item => {
  console.log(item);
});

// usando for-of
for (let item of cores) {
  console.log(item);
}

// usando for
for (let i = 0, n = cores.length; i < n; i++){
  console.log(cores[i]);
}
```


### Adicionar e remover itens do `Array`

```js
// adicionar um item no final
let tamanhoAposAzul = cores.push("azul");

// remover um item do final
let ultimoRemovido = cores.pop();

// adicionar um item no início
let tamanhoAposBranco = cores.unshift("branco");

// remover um item do início
let primeiroRemovido = cores.shift();

// localizar a posição de um item
// retornará -1 caso o valor não seja encontrado
let posicao = cores.indexOf("amarelo");

// adicionar 2 itens a partir da posição 2
let posicaoInserir = 2;
let itensInserir = ["roxo", "cinza"];
cores.splice(posicaoInserir, 0, ...itensInserir);

// remover 2 itens a partir da posição 2
let posicaoRemover = 2;
let quantidadeRemover = 2;
let removidos = cores.splice(posicaoRemover, quantidadeRemover);
```


### Obter um pedaço de um `Array`

```js
const numeros = [1, 1, 2, 3, 5, 8, 13, 21];
const pedaco = numeros.slice(4, 7); // [5, 8, 13]
```


### Concatenar `Arrays`
```js
const pares = [2, 4, 6];
const impares = [1, 3, 5];
let novo = pares.concat(impares); // [2, 4, 6, 1, 3, 5]
```


### Conversão entre `Array` e `String` delimitada

```js
// criar um array inicial
let frutas = ["Laranja", "Abacate", "Maçã", "Abacaxi"];

// criar uma string delimitada por hífens a partir do array
let texto = frutas.join("-"); // "Laranja-Abacate-Maçã-Abacaxi"

// modificar a string, "adicionando" novos itens
texto += "-Uva-Melancia";

// recriar o array a partir da string delimitada
frutas = texto.split('-'); // ["Laranja", "Abacate", "Maçã", "Abacaxi", "Uva", "Melancia"]
```


### Filtrar os itens de um `Array`

```js
function par(valor){
  return valor % 2 === 0;
}

let numeros = [8, 3, 12, -4, 7, -1, 13];
let pares = numeros.filter(par);

// de forma alternativa, com "arrow function"
pares = numeros.filter(num => num % 2 === 0);
```


### Filtrar os objetos de um `Array`

```js
// itens de uma compra
let itens = [
  { produto: { descrição: "Café torrado moído 500g", preço: 9.5 }, quantidade: 1 },
  { produto: { descrição: "Pão de forma integral", preço: 7.75 }, quantidade: 2 },
  { produto: { descrição: "Manteiga com sal 200g", preço: 8.0 }, quantidade: 1 },
  { produto: { descrição: "Água com gás 510ml", preço: 1.5 }, quantidade: 12 },
  { produto: { descrição: "Pipoca de microondas", preço: 2.5 }, quantidade: 4 }
];

// itens onde a quantidade for maior que 1
let mais_de_1_item = itens
  .filter((item) => item.quantidade > 1);

// itens onde o valor total (valor unitário * quantidade) for maior que 10
let valor_total_maior_que_10 = itens
  .filter((item) => item.produto.preço * item.quantidade > 10);

// itens onde a descrição do produto começa com a letra P
let descrição_começa_com_p = itens
  .filter((item) => item.produto.descrição[0].toUpperCase() === "P");
```


### Ordenar os itens de um `Array`

```js
let cores = ["verde", "amarelo", "vermelho"];

cores.sort(); // ["amarelo", "verde", "vermelho"]
```


### Inverter a ordem dos itens de um `Array`

```js
let cores = ["verde", "amarelo", "vermelho"];

cores.reverse(); // ["vermelho", "amarelo", "verde"]
```


### Ordenar os objetos de um `Array`

```js
let funcionarios = [
  { nome: "João", salario: 3000 },
  { nome: "Maria", salario: 4500 },
  { nome: "Ana", salario: 2500 },
  { nome: "Pedro", salario: 4000 },
  { nome: "Paulo", salario: 2500 }
];

// ordenar por salário
funcionarios.sort((a, b) => a.salario - b.salario);

// ordenar por salário (decrescente)
funcionarios.sort((a, b) => b.salario - a.salario);

// ordenar por nome
funcionarios.sort(function(a, b){
  if (a.nome > b.nome) return 1;
  else if (a.nome < b.nome) return -1;
  else return 0; // iguais
});
```


### Transformar os dados de um `Array` com `map`

```js
/* calcular as taxas correspondentes de um imposto fictício onde:
10% para valores inferiores a 1000
20% para superiores ou iguais a 1000 e inferiores a 5000
30% para valores superiores ou iguais a 5000 */
function calcularImposto(valor){
  if (valor < 1000) return valor * 0.1; // 10%
  else if (valor < 5000) return valor * 0.2; // 20%
  return valor * 0.3; // 30%
}

// valores para calcular
let valores = [500, 800, 1500, 2500, 5000, 5500, 8000, 10000];

// valores dos impostos
let impostos = valores.map(calcularImposto);

// valores detalhados: valor, imposto e total
let detalhes = valores.map(valor => {
  const valor_imposto = calcularImposto(valor);
  const valor_total = valor + valor_imposto;
  return {
    valor: valor,
    imposto: valor_imposto,
    total: valor_total
  };
});
```


### "Reduzir" os dados de um `Array`

O método `reduce` aplica uma função com um *acumulador* em cada valor de um `Array`, do primeiro até o último, para reduzi-los a um único valor.

A sintaxe do método `reduce` com a função `callback` é:

```js
array.reduce(callback(acumulador, valorAtual, índice, array), valorInicial);
```

Um exemplo para obter a soma dos números que compõem um `Array`:

```js
// obter a soma dos números de um array
let numeros = [1, 2, 3, 4, 5];

// let soma = numeros.reduce(function(acumulador, atual, indice, array){
//   return acumulador + atual;
// }, 0);

let soma = numeros.reduce((acum, atual) => acum + atual);
```


### "Reduzir" os dados de um `Array` de objetos

```js
// obter a soma dos salários dos seguintes funcionários
let funcionarios = [
  { nome: "João", salario: 3000 },
  { nome: "Maria", salario: 4500 },
  { nome: "Ana", salario: 2500 },
  { nome: "Pedro", salario: 4000 },
  { nome: "Paulo", salario: 2500 }
];

// somente com o método reduce
let salariosTotal = funcionarios.reduce((a, b) => {
  return { salario: a.salario + b.salario }
}).salario; // 16500

// usando os métodos map e reduce
let salariosTotal_V2 = funcionarios
  .map(item => item.salario)
  .reduce((a, b) => a + b); // 16500
```

