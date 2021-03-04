---
title: "Notas de Aulas de JavaScript: 7. Módulos"
date: 2021-02-28T21:13:15-03:00
draft: false
tags: ["js", "modules", "commonjs"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Módulos CommonJS

Os padrão de módulos *CommonJS* foi (e ainda é) utilizado pela plataforma *Node.js* (usada no *back-end*), portanto ainda é bastante comum a sua utilização em projetos com JavaScript, uma vez que o padrão ES6 é relativamente novo, sendo lançado em 2015.

```js
// index.js
const calculadora = require("./modulo1.js");
const { VERSAO, mensagem } = require("./modulo2.js");

console.log("Versão do módulo:", VERSAO);
console.log("2+3", calculadora.somar(2, 3));
console.log("3-2", calculadora.subtrair(3, 2));
mensagem();
```

```js
// modulo1.js
module.exports = {
  somar(a, b) {
    return a + b;
  },
  subtrair(a, b) {
    return a - b;
  }
};
```

```js
// modulo2.js
exports.VERSAO = "1.0";

exports.mensagem = function(){
  console.log("Olá! Tudo bem?");
}
```


## Módulos ES6

O sistema de módulos introduzido com o ECMAScript 2015 (ES6) possibilita programação modular em arquivos JavaScript com o uso das diretivas `import` e `export`, sem a necessidade de vincular todos os arquivos de script no documento HTML.

Para que um arquivo JavaScript seja carregado como módulo, é necessário adicionar o atributo `type="module"` à tag `<script>` no documento HTML.

Mais informações sobre o funcionamento dos módulos disponíveis em https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Modules.

```html
<!-- index.html -->
<script type="module" src="script.js"></script>
```

```js
// script.js
import calculadora, { VERSAO, mensagem } from "./modulo.js";

console.log("Versão do módulo:", VERSAO);
console.log("2+3", calculadora.somar(2, 3));
console.log("3-2", calculadora.subtrair(3, 2));
mensagem();
```

```js
// modulo.js
export const VERSAO = "1.0";

export function mensagem(){
  alert("Olá! Tudo bem?");
}

export default {
  somar(a, b) {
    return a + b;
  },
  subtrair(a, b) {
    return a - b;
  }
};
```

