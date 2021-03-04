---
title: "Notas de Aulas de JavaScript: 5. DOM"
date: 2021-02-28T21:13:13-03:00
draft: false
tags: ["js", "dom"]
categories: ["Notas de Aulas", "JavaScript"]
---


## Manipulação do HTML DOM

O HTML DOM (*Document Object Model*) é um modelo de objetos de um documento HTML.
O DOM de um documento consiste em uma hierarquia de nós, onde cada nó pode ter, opcionalmente, um nó pai (*parent*) e nós filhos (*children*).

Um elemento do DOM é um nó que representa uma *tag* HTML.
No DOM, todo elemento (objeto do tipo *HTMLElement*) é um nó (objeto do tipo *Node*), mas nem todo nó é um elemento, pois existem outros tipos de nós na hierarquia de um documento DOM, como comentários, blocos de texto, a *tag* especial *<!DOCTYPE>*, entre outros.

``` HTML
<!DOCTYPE html>
<html>

<head>
  <style>
    .conteiner {
      /* definir tamanho e deixar no formato de círculo */
      position: absolute;
      font-size: 80pt;
      width: 200px;
      height: 200px;
      border: 2px solid black;
      border-radius: 100px;
      /* centralizar o conteiner */
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      /* centralizar o conteúdo interno */
      display: flex;
      align-items: center;
      justify-content: center;
    }
  </style>
  <script>
    // definir função autoexecutável para não sujar o escopo global
    (() => {
      let contador = 0;

      const mostrar = () => {
        const p = document.querySelector("#contador");
        p.innerText = contador;
        p.style.color = contador >= 0 ? "blue" : "red";
      };

      // iniciar o código somente após o conteúdo ser carregado
      document.addEventListener("DOMContentLoaded", () => {

        const btnAumentar = document.getElementById("btnAumentar");
        const btnDiminuir = document.getElementById("btnDiminuir")

        // criar funções dos botões
        btnAumentar.addEventListener("click", () => {
          contador++;
          mostrar();
        });
        btnDiminuir.addEventListener("click", () => {
          contador--;
          mostrar();
        });

        // disparar o evento click dos botões caso as teclas + ou - sejam usadas
        document.addEventListener("keypress", event => {
          if (event.key === "+") {
            btnAumentar.dispatchEvent(new Event("click"));
          } else if (event.key === "-") {
            btnDiminuir.dispatchEvent(new Event("click"));
          }
        });

        // criar um elemento onde o contador será exibido
        const div = document.createElement("div");
        const p = document.createElement("p");
        div.className = "conteiner";
        p.id = "contador";
        div.appendChild(p);
        document.body.appendChild(div);

        mostrar();
      });
    })();
  </script>
</head>

<body>
  <button type="button" id="btnDiminuir">Diminuir</button>
  <button type="button" id="btnAumentar">Aumentar</button>
</body>

</html>
```

