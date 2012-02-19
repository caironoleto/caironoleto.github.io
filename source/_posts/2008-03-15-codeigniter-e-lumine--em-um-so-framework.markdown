---
layout: post
title: "CodeIgniter e Lumine, em um só framework"
comments: true
---

Olá a todos, é mais uma vez com muita emoção que venho por aqui dizer que consegui fazer a integração do Framework Lumine ao CodeIgniter.

[CodeIgniter](http://www.codeigniter.com) é um ótimo framework MVC OpenSource para PHP.

Assim que comecei a utilização dele, fiz uso do [Lumine](http://www.hufersil.com.br/lumine) para o mapeamento Objeto Relacional. Só que ele não estava integrado ao CodeIgniter, para sua utilização nos controllers, eu tinha que chamar um arquivo, por require, e só assim criaria o objeto de configuração e assim inicializaria a sua utilização.

Após o lançamento do Lumine 1.02-beta, resolvi estudar a integração do Lumine no framework CodeIgniter, se fazendo do uso do autoload, load, e outras facilidades que só o CodeIgniter traz.

Após algumas horas de adaptações, criação, edição, consegui fazer a integração total do Lumine ao CodeIgniter. Ainda está em fase de testes, mas já pode ser utilizado.

Agora o arquivo de configuração do Lumine não existe mais, no lugar é usado o arquivo database dentro da pasta config do CodeIgniter, alterando os valores conforme o banco de dados.

Outra coisa que tambem descobri, é o suporte do Lumine a apenas o MySQL e ao PostgreSQL. Em breve, com um pouco mais de tempo, integrarei outros bancos de dados ao Lumine, fazendo-o mais completo a cada passo.

Espero que mais essa contribuição seja de mais valor a comunidade PHP. Em breve disponibilizarei aqui o Framework completo e integrado.

Até a proxima!
