---
layout: post
title: Futuro do PHPSpec
comments: true
---

Olá, para quem não conhece, PHPSpec é um framework BDD para PHP.

Hoje o desenvolvedor do framework, Pádraic Brady, postou um email na lista phpspec-dev (phpspec-dev@googlegroups.com) e um artigo em seu blog sobre o futuro do PHPSpec.

A primeira e mais importante notícia, é que ele quer criar uma DSL decente (Sim, decente). Como ele citou no seu artigo, no Ruby, o BDD é sexy, mas em PHP é feito e chato. Por conta da sintaxe do PHP, a DSL acaba sendo uma coisa totalmente feia. Enquanto que no Ruby temos algo como `should score 0 for gutter game`, em PHP isso acaba virando um método:

```php
  public function itShouldScore0ForGutterGame() {
    for ($i=1; $i&lt;=20; $i++) {
      $this->_bowling->hit(0);
    }
    $this->spec($this->_bowling->score)->should->equal(0);
  }
```

Para desenvolvedores PHP isso é de fácil leitura, mas infelizmente, isso gera uma barreira enorme para pessoas que não sabem nada de PHP ou de programação, isso acaba fazendo ter um nível de explicação para o cliente ou para o interessado nesse comportamento, e isso vai diretamente contra o principio do BDD, onde todos os envolvidos tem que enteder realmente o que se está fazendo.

A outra notícia, é que ele está retomando a dianteira no desenvolvimento do Framework, desde fevereiro ele não vem dando dedicação necessária ao framework.

A primeira coisa é que ele está fazendo uma varredura nos bugs e liberar mais uma nova versão 0.2.x. Além disso ele fez uma chamada para quem quiser contribuir com alguma coisa para a versão 0.3 :).

Outra coisa importante é que anunciou que possivelmente em setembro ou outubro sairá a versão 5.3 do PHP, e pelo roadmap dele, a versão 0.5 já estará sobre o PHP 5.3 e afirmando que não fará esforços para que o framework funcione nas versões anteriores passando essa responsabilidade para a comunidade.

Agora é esperar até a DSL for sexy o suficiente para todos entender ;).

Até a próxima!
