---
title: "Notas de Aulas de JavaScript: 1. Introdução"
date: 2021-02-28T21:13:09-03:00
draft: false
tags: ["js", "história"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Breve história

JavaScript foi criada em 1995 por *Brendan Eich* enquanto ele trabalhava para a *Netscape* implementando a linguagem no navegador da empresa, o *Netscape Navigator*.

Inicialmente, o nome da linguagem passou por algumas mudanças, como *Mocha* e *LiveScript* até chegar no seu nome atual, o *JavaScript*.

Atualmente, a linguagem é chamada, em sua padronização, de *ECMAScript*, e não *JavaScript*.
Essa diferença se dá pelo fato de que *JavaScript* é uma marca registrada pela então *Sun*, posteriormente adquirida pela *Oracle*.


## Padronização

Existem dois padrões para JavaScript, o ECMA-262, seu padrão principal, e o ISO/IEC 16262, um padrão secundário.

Atualmente, a linguagem de programação *ECMAScript* é padronizada pelo *Ecma International Commitee 39*, o TC39.

O TC39 é um comitê composto por membros, especialmente empresas concorrentes, como *Apple*, *Facebook*, *Google*, *Microsoft*, entre outras, trabalhando juntos pelo bem da linguagem.

Todas as reuniões do TC39 estão documentadas em um repositório do GitHub: <https://github.com/tc39/notes>.


## JavaScript no front-end

* O código JavaScript é inserido em um documento HTML, através de um elemento `<script>`;
* O código pode estar em um arquivo separado do documento HTML e ser referenciado a partir do atributo `src="nome-do-arquivo.js"`;
* O código também pode ser definido como conteúdo do próprio elemento `<script>`;
* Uma outra forma é importar o código como um módulo;
* Recomenda-se o uso das "ferramentas do desenvolvedor" (F12) em um navegador moderno (Chrome, Firefox).


## JavaScript no back-end

* Node.js é uma *runtime* de JavaScript assíncrona orientada a eventos;
* Node.js permite execução de código JavaScript no *back-end*;
* Npm é o gerenciador de dependência oficial do Node.js;
* Yarn é um popular gerenciador de dependências concorrente do Npm, criado pela Facebook;
* Deno é uma *runtime* de JavaScript e TypeScript projetada para ser simples e segura;
* Deno é um projeto recente e foi criado por Ryan Dahl, o mesmo criador do Node.js;


## Ferramentas de desenvolvimento

* Visual Studio Code
  * Code Runner
  * ESLint
  * Quokka
  * Debugger for Chrome
  * Debugger for Firefox
* Navegador
  * Firefox Developer Edition
  * Chrome Dev
  * Ferramentas do Desenvolvedor
* Node.js
