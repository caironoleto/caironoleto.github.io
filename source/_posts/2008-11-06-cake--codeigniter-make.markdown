---
layout: post
title: "Cake: Codeigniter mAKE!"
---

Olá, hoje eu lancei o [cake](http://github.com/caironoleto/cake/tree/master) e ele tem esse nome por que é um acrônimo de **C**odeIgniter m**AKE**.

### Mas o que ele faz?

Ele aumenta a velocidade para você trabalhar com CodeIgniter. Ele tem a mesma funcionalidade dos scripts/generate do Ruby On Rails, apenas criando uma estrutura para seu projeto CodeIgniter.

Ele cria magicamente controllers, views e models.

### E o que ele gera?

Bom, no momento ele está gerando controllers e models, e com um aperitivo a mais, ele está gerando também as views passada como parâmetro para gerar os controllers.

Outra coisa ele está o seguindo padrão `nomeController.php` e o nome da classe como `NomeController`, para os models está criando as classes como `nome.php` e `Nome`.

As views estão sendo geradas em `nome_controller/` com o sufixo `_view.php`, algo como `nome_controller/index_view.php`.

Ele é verboso, então ele vai dizendo o que está sendo feito a cada passo.

### Como faço pra usar?!

Eu escrevi um Read Me bem simples, assim mesmo para quem não está muito bem no inglês possa entender.

### O que eu espero para o futuro?

Atualmente só tem suporte ao linux e ao macOS, então quem se interessar em portar para funcionar no windows, fique a vontade ;) (Os scripts que geram os arquivos estão em php então independe de OS para funcionar, o que falta é o script funciona no windows, que se não me engano é necessário fazer os bats e configurar a PATH, isso é quase a solução :).

Bom, primeiramente vou ajustar para também gerar rotas padrões para cada controller.

Depois vou adicionar opções como nomenclaturas diferentes.

Vou adicionar também um suporte para gerar os testes para controllers e models.

E gostaria de incluir a opção de baixar a versão mais nova do CodeIgniter e criar a estrutura do projeto, algo como `cake app nomeDaApp` e `cake update`.

O projeto está sobre licença [Creative Commons 3.0](http://creativecommons.org/licenses/by/3.0/legalcode), você pode usar comercialmente, modificar e distribuir, apenas mantendo meus créditos!

e o mais importante: **VOCÊ PODE CONTRIBUIR**, só ir lá no github, fazer um git clone e depois me mandar um pull request ou um patch :P

Até a próxima!
