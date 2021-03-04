---
title: "Notas de Aulas de JavaScript: 8. Programação Assíncrona"
date: 2021-02-28T21:13:16-03:00
draft: false
tags: ["js", "callbacks", "async"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Programação assíncrona

No modelo de execução **síncrono**, as instruções de um programa são executadas em sequência, uma após a outra.
Nesse modelo, quando uma instrução demorada é executada, o fluxo de execução fica bloqueado, aguardando seu término para que as próximas instruções possam ser executadas.

No modelo de execução **assíncrono**, é possível executar mais de uma instrução ao mesmo tempo.
Assim, uma instrução demorada poderia ser executada "em segundo plano", enquanto o restante das instruções continuam normalmente com seu fluxo de execução.

Algumas linguagens de programação usam *threads* para executar código assíncrono.
Cada *thread* funciona como um novo fluxo de execução em um programa.

Entretanto, JavaSript é uma linguagem de programação *single threaded*, mesmo em computadores com múltiplos *cores*.
Dessa forma, um programa JavaScript é executado em uma única *thread*.

Isso não significa que não seja possível executar instruções de forma assíncrona com JavaScript.
JavaScript utiliza uma técnica conhecida como *event loop*, que permite o modelo de execução assíncrono em uma única *thread*.
Mais informações sobre o funcionamento do *event loop* em <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/EventLoop>.

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
Dessa forma, a função que a recebeu pode executá-la.

```js
function clicouNoDocumento(){
  console.log("Usuário clicou no documento");
}
function mostrarMensagem(){
  alert("Olá! Tudo bem?");
}

// exemplo 1: chamar a função clicouNoDocumento quando o documento receber um clique
document.addEventListener('click', clicouNoDocumento);

// exemplo 2: executar a função mostrarMensagem após 10 segundos
setTimeout(mostrarMensagem, 5000);
```


#### Sequência de *callbacks* executados de forma assíncrona

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
```


#### Sequência de *callbacks* executados de forma síncrona

```js
// código "hadouken"
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
```


### Promises

Vejamos um exemplo usando a *{JSON} Placeholder*, uma API *fake* para testes e prototipação. A URL usada no exemplo foi <https://jsonplaceholder.typicode.com/users/1>, produzindo a seguinte saída no formato *JSON*.

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

Para consultar essa API e obter o atributo `name`, usaremos a `fetch API`, que retorna um objeto do tipo `Promise`.

```js
const URL = "https://jsonplaceholder.typicode.com/users/1";
let name;
fetch(URL)
  .then(response => response.json())
  .then(json => { name = json.name; })
  .catch(error => { console.error(error); });
console.log(name);
```


### Sintaxe async / await

Agora, realizaremos o mesmo processo com a sintaxe `async` / `await`.

```js
const URL = "https://jsonplaceholder.typicode.com/users/1";
const response = await fetch(URL);
const json = await response.json();
let name = json.name;
console.log(name);
```

O processo pode ser definido em uma função assíncrona para obter o nome de um usuário, dado um id.

```js
async function getUserName(id){
  const URL = `https://jsonplaceholder.typicode.com/users/${id}`;
  const response = await fetch(URL);
  const json = await response.json();
  return json.name;
}
console.log(await getUserName(1));
```


### Paralelismo com `Promise.all`

```js
// TO-DO: melhorar exemplo
const promises = [promise1, promise2, promise3];
let results = await Promise.all(promises);
```

