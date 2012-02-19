---
layout: post
title: Diferença entre TDD e BDD
comments: true
---

TDD  um acronimo para [Test-Driven Development](http://en.wikipedia.org/wiki/Test-driven_development), que significa desenvolvimento orientado a testes, e BDD  um acronimo para [Behavior Driven Development](http://en.wikipedia.org/wiki/Behavior_driven_development), que significa desenvolvimento orientando a comportamentos.  

TDD e BDD são técnicas de desenvolvimento que priorizam os testes de código, integração contínua e desenvolvimento Ágil.  Essas técnicas são para desenvolvimento orientado a testes. Mas tem uma pequena diferença, em TDD você escreve os testes e os valida de forma que eles funcionem. Já em BDD, você escreve como deve se comportar seu problema.  Além disso, em BDD é mais humano do que os testes.

Existe um framework em ruby chamado [RSpec](http://rspec.info/) que você deve ser bom em inglês, já que você praticamente "fala" com o framework e diz como vai se comportar as coisas. Em php, foi criado um framework chamado [PHPSpec](http://www.phpspec.org/), que uma "versão" do RSpec para PHP.

BDD foi originalmente criado para suprir a necessidade que começou a ser criada em TDD. E também, por que escrever orientado a testes  é mais chato, principalmente para quem não tem experiência com testes, ou tem muita experiência.

Quando se programa em TDD, com o passar do tempo, o seus testes se tornam o comportamento que você quer na sua aplicao, algumas pessoas consideram o BDD uma evolução natural do TDD.  Independente de usar técnicas ou não,  é necessário que a aplicação seja testada. E testes devem ser automáticos, manuais, documentados e validados.

Até a próxima!
