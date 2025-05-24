---
title: "Notas de Aulas de JavaScript: 8. Programação Assíncrona"
date: 2021-03-05T20:50:10-03:00
draft: false
tags: ["js", "callback", "promise", "async", "await"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Programação assíncrona

No modelo de execução **síncrono**, as instruções de um programa são executadas em sequência, uma após a outra.
Nesse modelo, quando uma instrução demorada é executada, o fluxo de execução fica bloqueado, aguardando seu término para que as próximas instruções possam ser executadas.

No modelo de execução **assíncrono**, é possível executar mais de uma instrução ao mesmo tempo.
Assim, uma instrução demorada poderia ser executada "em segundo plano", enquanto o restante das instruções continuam normalmente com seu fluxo de execução.

Algumas linguagens de programação usam *threads* para executar código assíncrono.
Cada *thread* funciona como um novo fluxo de execução em um programa.

Entretanto, JavaSript é uma linguagem de programação *single threaded*, mesmo em computadores com múltiplos *cores*, um programa JavaScript é executado em uma única *thread*.

Isso não significa que não seja possível executar instruções de forma assíncrona com JavaScript.
JavaScript utiliza uma técnica conhecida como *event loop*, que permite o modelo de execução assíncrono em uma única *thread*.

Mais informações sobre o funcionamento do *event loop* em <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/EventLoop>.


### Um primeiro exemplo

No seguinte trecho de código, a função `exemplo` executa uma instrução síncrona, seguida de uma instrução assíncrona com um `callback` a partir da função `setTimeout`, uma instrução assíncrona com uma `Promise`, e finalmente, uma última instrução síncrona. Observe o resultado:

```js
function exemplo(){
  console.log("Síncrono: Início");

  setTimeout(() => {
    console.log("Assíncrono (Callback)");
  });

  Promise.resolve().then(() => {
    console.log("Assíncrono (Promise)");
  });

  console.log("Síncrono: Fim");
}

exemplo();
// Síncrono: Início
// Síncrono: Fim
// Assíncrono (Promise)
// Assíncrono (Callback)
```


### Callbacks

Um *callback* é uma função passada como argumento para outra função.
Dessa forma, a função que a recebeu pode executá-la ou passá-la adiante.

No exemplo a seguir são declaradas duas funções: `clicouNoDocumento`, usada no evento `click` do documento, e `mostrarMensagem` que é passada para `setTimeout` para ser executada somente após 10 segundos. 

```js
function clicouNoDocumento(){
  console.log("Usuário clicou no documento");
}
function mostrarMensagem(){
  alert("Olá! Tudo bem?");
}

// Exemplo 1: chamar a função clicouNoDocumento quando o documento receber um clique
document.addEventListener('click', clicouNoDocumento);

// Exemplo 2: executar a função mostrarMensagem após 10 segundos
setTimeout(mostrarMensagem, 10000);
```


#### Sequência de *callbacks* executados de forma assíncrona

No próximo exemplo, uma sequência de instruções `setTimeout` disparam `callbacks` que enviam mensagem com `console.log`, entretanto, devido ao tempo programado em `setTimeout`, e pelo fato da execução acontecer de forma assíncrona, a ordem do resultado acontece da última para a primeira instrução.

```js
function sequenciaDeChanadas_V1(){
  setTimeout(function(){
    console.log("Mensagem 1");
  }, 1000);
  setTimeout(function(){
    console.log("Mensagem 2");
  }, 800);
  setTimeout(function(){
    console.log("Mensagem 3");
  }, 600);
  setTimeout(function(){
    console.log("Mensagem 4");
  }, 400);
  setTimeout(function(){
    console.log("Mensagem 5");
  }, 200);
}

sequenciaDeChanadas_V1();
// Mensagem 5
// Mensagem 4
// Mensagem 3
// Mensagem 2
// Mensagem 1
```


#### Sequência de *callbacks* executados de forma síncrona

No próximo exemplo, as mesmas instruções são escritas de forma encadeada, de forma que as instruções sejam executadas de forma síncrona.

Essa cadeia de instruções começa aumentar a complexidade de compreensão do código, sendo conhecida por alguns como *callback hell* ou *indentação hadouken* 😁.

```js
function sequenciaDeChamadas_V2(){
    setTimeout(function(){
        console.log("Mensagem 1");
        setTimeout(function(){
            console.log("Mensagem 2");
            setTimeout(function(){
                console.log("Mensagem 3");
                setTimeout(function(){
                    console.log("Mensagem 4");
                    setTimeout(function(){
                        console.log("Mensagem 5");
                    }, 200);
                }, 400);
            }, 600);
        }, 800);
    }, 1000);
}

sequenciaDeChamadas_V2();
// Mensagem 1
// Mensagem 2
// Mensagem 3
// Mensagem 4
// Mensagem 5
```

Mesmo com o tempo definido de forma decrescente em `setTimeout`, cada instrução só inicia após o término da instrução anterior.


### Promises

Antes de falar de Promises, vejamos um exemplo de uma requisição AJAX usando o objeto `XMLHttpRequest` para obter dados da *{JSON} Placeholder*, uma API *fake* para testes e prototipação. A URL usada no exemplo é <https://jsonplaceholder.typicode.com/users/1>, produzindo a seguinte saída no formato *JSON*.

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874",
    "geo": {
      "lat": "-37.3159",
      "lng": "81.1496"
    }
  },
  "phone": "1-770-736-8031 x56442",
  "website": "hildegard.org",
  "company": {
    "name": "Romaguera-Crona",
    "catchPhrase": "Multi-layered client-server neural-net",
    "bs": "harness real-time e-markets"
  }
}
```

A requisição será feita a partir da função `getUser(id, callback, error)`, que dado um `id` irá obter o objeto `JSON` correspondente ao resultado da requisição, que ao terminar, executará uma função `callback` para lidar com o resultado.

```js
function getUser(id, callback){
  const xhr = new XMLHttpRequest();
  xhr.onload = function(){
    const json = JSON.parse(this.responseText);
    callback(json);
  };
  const url = `https://jsonplaceholder.typicode.com/users/${id}`;
  xhr.open("GET", url);
  xhr.send();
}
getUser(1, user => { console.log(user); });
```

Para ilustrar o uso de `Promise`, usaremos a [`Fetch API`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), para realizar a mesma requisição assíncrona feita no exemplo anterior.

A `Fetch API` oferece uma interface para buscar dados que retorna objetos do tipo `Promise`, que é um tipo de objeto usado para o processamento assíncrono. Observe:

```js
// Usando callback como parâmetro
function getUser(id, callback){
  const URL = `https://jsonplaceholder.typicode.com/users/${id}`;
  let user;
  fetch(URL)
    .then(response => response.json())
    .then(user => { callback(user); });
}
getUser(1, user => { console.log(user); });

// Usando retorno da Promise e await na chamada
function getUser(id){
  const URL = `https://jsonplaceholder.typicode.com/users/${id}`;
  return fetch(URL).then(response => response.json());
}
console.log(await getUser(1));
```


### Sintaxe async / await

Uma instrução `async function` define uma função assíncrona, que retorna um objeto `AsyncFunction`, e o operador `await` é utilizado para esperar por uma `Promise`, e deve ser usado dentro de uma `async function`.

Agora, realizaremos o mesmo processo dos exemplos anteriores com a sintaxe `async` / `await`.

```js
async function getUser(id){
  const URL = `https://jsonplaceholder.typicode.com/users/${id}`;
  const response = await fetch(URL);
  return await response.json();
}
console.log(await getUser(1));
```


### `Promise.all`

O método `Promise.all` retorna uma único objeto `Promise` que resolve quando todos os objetos `Promises` de seu argumento forem resolvidos. Esses objetos são executados ao mesmo tempo, e quando todos terminarem, seus resultados serão retornados.

A *JSON Placeholder API* possui 6 *resource endpoists* para testes, `posts`, `comments`, `albums`, `photos`, `todos` e `users`.
No próximo exemplo faremos uma requisição para cada um dos *resource endpoints* em paralelo através de `Promise.all`.

```js
const base_url = "https://jsonplaceholder.typicode.com";
const resource_list = ["posts", "comments", "albums", "photos", "todos", "users"];

async function getResource(resource){
  const url = `${base_url}/${resource}`;
  const response = await fetch(url);
  return await response.json();
}

async function getAllResources(){
  promises = [];
  resource_list.forEach(resource => {
    promises.push(getResource(resource));
  });
  return Promise.all(promises);
}

const [posts, comments, albums, photos, todos, users] = await getAllResources();
```
