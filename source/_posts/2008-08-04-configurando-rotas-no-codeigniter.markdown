---
layout: post
title: Configurando rotas no CodeIgniter
---

Olá, para quem já conhece o [CodeIgniter](http://www.codeigniter.com) pode dar uma olhada em outros posts aqui no meu blog. Para quem não conhece resumindo: CodeIgniter é um framework MVC para PHP.

Para quem não sabe, o CodeIgniter trabalha com URI's e por padrão ele utiliza `/controller/method/param1/param2/param3`. Mas o mais legal, é que você pode configurar suas próprias rotas.

Para configurar suas próprias rotas você deve ir no `system/application/config/routes.php`. Para configurar as rotas, você pode utilizar Expressões regulares. Numa pequena aplicação que estou fazendo aqui, configurei um pseudo **[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)** criando rotas como:

```php
$route['tasks/index.json'] = 'tasks/index/json';
$route['tasks/index.html'] = 'tasks/index/html';
$route['tasks/edit/([a-zA-Z0-9 ]+).json/'] = 'tasks/edit/json/$1';
$route['tasks/edit/([a-zA-Z0-9 ]+).html/'] = 'tasks/edit/html/$1';
$route['tasks/save.json'] = 'tasks/save/json';
$route['tasks/save.html'] = 'tasks/save/html';
$route['tasks/destroy/([a-zA-Z0-9 ]+).json'] = 'tasks/destroy/json/$1';
$route['tasks/destroy/([a-zA-Z0-9 ]+).html'] = 'tasks/destroy/html/$1';
```

E controlo as respostas no próprio controlador, no Rails isso é bem mais simples é só apenas utilizar o respond_to, sendo que o Rails consegue identificar o tipo de acesso que ele está requisitando, já aqui no PHP utilizo switch e tenho que passar "no braço" o que realmente eu quero.

Criando essas rotas, identifico a resposta para minha requisição fazendo funcionar o pseudo REST.

Você poder criar qualquer tipo de regra para suas rotas e dizendo para onde elas devem redirecionar. É algo simples de se fazer, especialmente pelo fato de poder utilizar Expressões Regulares.

Até a próxima!
