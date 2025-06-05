---
title: "Notas de Aulas de JavaScript: 2. Sintaxe Básica, Tipos e Operadores"
date: 2021-02-24T20:50:20-03:00
draft: false
tags: ["js", "tipos", "operadores"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Comentários

JavaScript possui comentários de linha e comentários de bloco.
Os comentários de linha são definidos após duas barras `//`.
Dessa forma, todo o conteúdo após as duas barras `//` é considerado comentário e não será interpretado como código.

```js
// Este é um comentário de linha
numero = 0; // Atribuir o valor 0 a variável numero
```

Já os comentários de bloco são definidos entre as sequências `/*` e `*/`.
Assim, todo o conteúdo entre as sequências `/*` e `*/` é um bloco de comentário e não será interpretado como código.

```js
/* Este é um comentário de bloco.
   Ele pode ter várias linhas.
   É bastante útil para trechos de documentação de código. */
```


## Entrada e saída básica no navegador

Os navegadores de internet possuem alguns métodos úteis para a realização de testes durante o aprendizado da linguagem, como `console.log`, `console.warn` e `console.error`, que produzem saídas no console das ferramentas do desenvolvedor, e os métodos `prompt`, `alert` e `confirm`, do objeto `window` que permitem entrada e saída básica a partir de janelas *pop-ups*.

```js
// obter uma string do usuário a partir de uma janela *pop-up*
nome = window.prompt("Digite seu nome:");

// enviar uma mensagem ao usuário por uma janela *pop-up*
window.alert("Olá " + nome);

// obter uma confirmação do usuário (Ok/Cancelar) por uma janela *pop-up*
resposta = window.confirm("Tem certeza?")

// exibir uma mensagem ao usuário no console do desenvolvedor (F12)
// ver também: console.error, console.warn, console.info, console.table, ...
console.log("Olá", nome);
```


## Tipos de dados primitivos

- **`Boolean`**: os valores `true` e `false`.
- **`null`**: indica um valor nulo.
- **`undefined`**: indica um valor indefinido.
- **`Number`**: números inteiros e reais, ex: 10.05, 0, -1, 3.14.
- **`String`**: cadeia de caracteres (texto), ex: "Um texto!".
- **`Symbol`**: (Novo no EcmaScript 6) Tipo com instâncias únicas e imutáveis, pode ser usado, por exemplo, em chaves de objetos.

```js
typeof true; // "boolean"
typeof null; // "object"
typeof undefined; // "undefined"
typeof 123; // "number" 
typeof 1.5; // "number
typeof "abc"; // "string"
typeof 'abc'; // "string"
typeof {}; // "object"
typeof []; // "object"
```


## Operações diversas

### Aritmética básica

```js
123 + 321; // adição: 123 mais 321 igual a 444
98 - 87;   // subtração: 98 menos 87 igual a 11
5 * 2;     // multiplicação: 5 vezes 2 igual a 10
12 / 4;    // divisão: 12 dividido por 4 igual a 3
5 % 2;     // resto da divisão: 5 dividido por 2 sobra 1
4 ** 3;    // potência: 4 elevado a 3 igual a 64
```


### Atribuição simples

```js
a = 1;     // atribuição: a recebe 1
b = a + 2; // atribuição: b recebe valor de a mais 2
```


### Atribuição aritmética

```js
x = 1;   // atribuição simples: x recebe 1
x += 10; // x recebe valor de x mais 10 ... x = 11
x *= 2;  // x recebe valor de x vezes 2 ... x = 22
```


### Incrementadores/decrementadores

```js
x = 0; // atribuição simples: x recebe 0
x++;   // x agora vale 1
x++;   // x agora vale 2
x--;   // x agora vale 1

y = x++; // y vale 1 e x vale 2
y = ++x; // y e x valem 3
```


### Concatenação de `Strings`

```js
nome = "Fulano";
linguagem = "Java" + "Script";              // "JavaScript"
frase = nome + " programa em " + linguagem; // "Fulano programa em JavaScript"

// usando interpolação com "template string"
outra_frase = `${nome} programa em ${linguagem}`;
```


### Uso de parênteses

```js
// media recebe a soma de 9, 8, 7 e 6 dividida por 4
// ou, media recebe a média aritmética de 9, 8, 7 e 6
media = (9 + 8 + 7 + 6) / 4;
```


### Comparação de valores

```js
a == b; // comparar se o valor de a é igual ao valor de b
a != 0; // comparar se o valor de a é diferente de 0
b > c;  // comparar se o valor de b é maior que o valor de c
c < d;  // comparar se o valor de c é menor que o valor de d
a <= 1; // comparar se o valor de a é menor ou igual a 1
9 >= b; // comparar se 9 é maior ou igual ao valor de b

a === b;   // comparar se a e b tem o mesmo valor e o mesmo tipo
a !== b;   // comparar se a e b não tem mesmo valor e mesmo tipo

1 == "1";  // true
1 != "1";  // false
1 === "1"; // false
1 !== "1"; // true
```


### Operadores lógicos

```js
// and
true && true;   // true
true && false;  // false
false && true;  // false
false && false; // false

// or
true || true;   // true
true || false;  // true
false || true;  // true
false || false; // false

// not
!true;    // false
!false;   // true

!0;       // true
!1;       // false
!"";      // true
!!"";     // false
!"asdf";  // false
!!"asdf"; // true

// busca por um valor "verdadeiro"
nome = "";
sobrenome = "";
usuario = "admin";

console.log(nome || sobrenome || usuario || "visitante"); // "admin"
```


### Objetos

Um objeto é uma lista de pares de nomes associados a valores. 
Pode-se usar um inicializador literal com `{}` ou o construtor `Object`.

```js
contato1 = { nome: "Fulano", email: "fulano@gmail.com" };
console.log("O e-mail de", contato1.nome, "é", contato1.email);

contato2 = {};
contato2.nome = "Cicrano";
contato2.email = "cicrano@live.com";
console.log(`O e-mail de ${contato2.nome} é ${contato2.email}`);

contato3 = new Object();
contato3["nome"] = "Beltrano"; // atributo como string entre colchetes []
contato3.email = "beltrano@hotmail.com";
console.log(`O e-mail de ${contato3.nome} é ${contato["email"]}`);
```


### Arrays

Tecnicamente, em JavaScript, um `Array` é considerado um objeto com características distintas.
É definido como uma lista com zero ou mais valores indexados iniciando em zero.

```js
frutas = ["Laranja", "Banana", "Abacate"];
console.log(frutas[0], "é uma ótima fruta."); // Laranja é uma ótima fruta.
```


### Conversão automática entre tipos

```js
valor = "10" + "5"; // concatenação: "105"
valor = "10" + 5;   // concatenação: "105"
valor = 10 + "5";   // concatenação: "105"
valor = "10" - 5;   // subtração: 5 ¯\_(ツ)_/¯
valor = "10" * 5;   // multiplicação: 50
```


### Conversão entre `String` e `Number`

```js
// conversão para números inteiros
a = "10";
b = "-2";
parseInt(a) + parseInt(b);

// conversão para base decimal
parseInt("1234", 8);  // octal para decimal
parseInt("10001", 2); // binário para decimal

// conversão para números reais
a = "1.5";
b = "2.75";
parseFloat(a) + parseFloat(b);

// forma alternativa de conversão de texto para números (sinal +)
a = "2.5";
b = "4";
+"1.5" + +"4" + +a;
(+a) + (+b) + (+"1.1");

// CUIDADO!!!
x = "";
parseInt(x); // NaN
+x;          // 0
```

