---
layout: post
title: Trabalhando com datas naturalmente com Chronic
---

Esta semana passei gastando um tempão implementando um Parser que me retornaria um dia em que um evento ocorre em uma determinada data.

Mas a data seria algo "Quero trazer todos os usuários que se cadastraram na terceira terça feira do mês de janeiro". A forma que eu estava implementando não era assim tão segura, apesar de todos os testes estarem passando.

Então hoje eu encontro o [Chronic](http://chronic.rubyforge.org). É uma gem ruby que faz justamente o que eu precisava:

```ruby
irb(main):005:00> Chronic.parse("3rd tuesday in january", :context => :past)
=> Tue Jan 19 12:00:00 -0300 2010
```

Simples e rápido.
