---
layout: post
title: "is_paranoid destrua seus registros sem destruí-los!"
comments: true
---

[is_paranoid](http://github.com/jchupp/is_paranoid/) é uma gem que faz uma coisa mágica na sua aplicação. Ele faz com que seus registros não sejam excluídos do banco de dados.

Para quem é um railer experiente vai dizer: "Ahh, então eu prefiro usar o [acts_as_paranoid](http://github.com/technoweenie/acts_as_paranoid/) ele faz a mesma coisa". Só que tem um pequeno detalhe, é que o is_paranoid já funciona se usufruindo de uma nova funcionalidade incluída no Rails 2.3 o [default_scope](http://ryandaigle.com/articles/2008/11/18/what-s-new-in-edge-rails-default-scoping).

É simples usar o is_paranoid, instale a gem

`sudo gem install jchupp-is_paranoid`

e no seu model adicione

```ruby
class User < ActiveRecord::Base
  is_paranoid
end
```

assim seu model já funciona com is_paranoid.

Mais detalhes, visite o projeto no github.
