---
layout: post
title: Cuidado com Model-View-Controller
comments: true
---

Um assunto recorrente nas listas de discussão é sobre [MVC](http://en.wikipedia.org/wiki/Model-view-controller), esse [pattern](http://en.wikipedia.org/wiki/Design_pattern_(computer_science)) que é a base para quase todos os [framewok](http://en.wikipedia.org/wiki/Frameworks) atuais. Quem usa Ruby On Rails sabe "Fat Models, Slim Controls" por que é um comportamento imposto pelo criador do framework e praticamente por todos que utilizam.

O que acontece é que em outros frameworks ([CodeIgniter](http://en.wikipedia.org/wiki/CodeIgniter) por exemplo) te deixam mais livres e as pessoas acabam fazendo as coisas certas em lugares errados. O pattern por si só diz que é necessário colocar a lógica para os models e os controllers fazerem o fluxo das informações irem de um lugar para outro, mas não é bem isso que acontece.

E como a discussão foi em uma lista de CodeIgniter, ele ainda peca por isso. O CodeIgniter usa MVC, mas o framework da a liberdade para você fazer o que quer. Eu vejo aplicações que reutilizam controllers e não fazem uso dos helpers, eu vejo aplicações que fazem uso dos controllers, mas que os models deveriam fazer.

Helpers serve para você tirar a lógica de views, models servem para ter a lógica da aplicação, quem ainda não entedeu, lógica de views são apenas lógicas de controle, algo como if - else, switch - case, "div com o login da aplicação", e coisas nas views que se vai repetir em toda a aplicação. Os models servem para fazer autenticação da aplicação, para verificarem se o usuário está cadastrado no banco de dados, para calcularem a quantidade de juros de uma venda a prazo, enfim, tudo aquilo que está ligado a lógica da aplicação.

Se repita menos ([Don't Repeat Yourself](http://en.wikipedia.org/wiki/DRY)), use helpers, construa bons models, limpem seus controllers, seja um melhor profissional e uma pessoa menos estressada no final do dia (coisas simples nos deixam mais felizes que coisas complexas), use da simplicidade.
