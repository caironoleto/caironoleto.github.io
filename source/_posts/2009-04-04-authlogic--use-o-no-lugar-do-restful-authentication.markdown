---
layout: post
title: Authlogic, use-o no lugar do restful_authentication
---

Autenticação no Rails e a primeira coisa que todos pensam é: [restful_authentication](http://github.com/technoweenie/restful-authentication/tree/master). Muito bom o plugin, extremamente útil, já gera testes básicos, os models, controllers, rotas e tudo mais para começar a funcionar.

Mas, existe uma outra boa opção: [Authlogic](http://github.com/binarylogic/authlogic/tree/master).

A diferença básica entre o restful_authentication para o authlogic é que o authlogic é bem mais modular e simples do que o restful_authentication. Você só precisa autenticar na sua aplicação, ou precisa usar o OpenID, ou não precisa de tudo que o resftul_authentication ou apenas quer uma biblioteca mais simples de se adaptar e de personalizar?! Então você tem que usar o authlogic.

Ele dá mais trabalho do que o restful_authentication, primeiro ele não te dá uma suíte de testes já pronta, ele não cria os models, controllers, views ou tudo relacionado a sua autenticação. Você é que deve criar tudo isso.

"Eu vou ter mais trabalho e tu ta me dizendo que é melhor?!" Sim! É mais fácil você entender toda a lógica por trás da autenticação e é bem mais legal quando você mesmo cria os seus testes e os vê passando!

Ele usa uma lógica diferente, ao invés de usar um controller para fazer a autenticação, ele usa um model para isso. Assim deixa tudo nos seus lugares ;). O lance do model é que ele trata a autenticação como algo que deve ser persistido, no caso em um cookie.

O Authlogic te da a possibilidade de usar SHA1, SHA512, MD5, BCrypt e AES256 como opções de criptografia das senhas.
