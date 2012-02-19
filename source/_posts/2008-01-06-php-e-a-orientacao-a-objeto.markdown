---
layout: post
title: "PHP e Orientação a Objeto"
comments: true
---

Meu amigo Marcelo Lobo me perguntou agora a pouco no GTalk:

    é o seguinte: "eu tenho uma classe e quero chamar outra classe dentro dela".

Vou responder com um exemplo prático.

Na empresa onde trabalho (W7 Solutions) programamos em PHPOO (PHP Orientado a Objeto).

Em PHP, pra se criar uma classe é bastante simples:

```php
//Declaração da classe

class NomeDaClasse {
  //Declaração de Método

  public function nomeDoMetodo(parametros = "") {
  }
}
```

Em PHP, sem a utilização de algum framework, para se instanciar novos objetos de uma classe, e sua classe se encontra em outro arquivo, se tem a necessidade de se importar o arquivo para a fonte:

```php

require_once("arquivoComAClasse.php");

$objeto = new NomeDaClasse();

$objeto->nomeDoMetodo("parametro");
```

Simples assim, se utiliza da Orientação a Objetos em PHP.

Até a proxima.
