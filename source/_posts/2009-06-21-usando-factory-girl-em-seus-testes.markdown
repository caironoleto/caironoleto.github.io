---
layout: post
title: Usando Factory Girl em seus testes
comments: true
---

Hoje quem desenvolve em Ruby On Rails tem nas mãos as poderosas ferramentas de testes, sejam eles para desenvolvimento orientado a testes ou orientado a comportamentos. E no início nós usávamos fixtures, que são arquivos que contém um amontoado de dados que serão usados em nossos testes.

Junto com a evolução dessas ferramentas nasceram os [fixtures replacements](http://ruby-toolbox.com/categories/rails_fixture_replacement.html). Nasceram para suprir a necessidade de organização e praticidade na utilização de fixtures. Atualmente temos [Machinist](http://github.com/technoweenie/machinist/tree/master), [Factory Girl](http://github.com/thoughtbot/factory_girl), [Object Daddy](http://github.com/flogic/object_daddy), [Dataset](http://github.com/aiwilliams/dataset), [Fixjour](http://github.com/nakajima/fixjour) e [FixtureReplacement](http://github.com/smtlaissezfaire/fixturereplacement). Certamente existem outros, mas esses são os mais famosos e usados.

Hoje vou mostrar como você pode usar o Factory Girl em seus testes/especificações.

Para instalar o Factory Girl:

`gem install thoughtbot-factory_girl`

Depois você tem que configurar no seu environment o uso da gem:

`config.gem :thoughtbot-factory_girl, :lib => :factory_girl, :source => "http://gems.github.com"`

Depois de configurado, vamos a criação de suas factories. Por padrão, o Factory Girl carrega automaticamente em seus testes as factories que se encontram em `test/factories/*.rb`, `spec/factories/*.rb` ou você pode criar todos os seus factories em um único arquivo e colocar em `test/factories.rb` ou `spec/factories.rb`. Você pode criar todos os seus factories em um único arquivo ou separar por model (O que eu acho mais organizado).

Antes de usar o Factory Girl nos seus testes você deve definir como cada model deve funcionar:

```ruby
Factory.define :user do |factory|
  factory.name => "Cairo"
  factory.surname => "Noleto"
end
```

Você pode ter outras definições, por exemplo:

```ruby
Factory.define :admin, :class => User do |factory|
  factory.name => "Administrador"
  factory.surname => "Cairo Noleto"
  factory.admin => true
end
```

Você pode fazer vários tipos de factories, um para cada situação que você precisar, é mais fácil para fazer a manutenção.

Depois que você define suas factories é hora de realmente brincar com elas ;)

```ruby
user = Factory.build(:user)
user_admin = Factory.build(:admin)
```

Quando você usa build, ele constroi um objeto não salvo.

```ruby
user = Factory.create(:user)
user_admin = Factory.create(:admin)
```

Ao utilizar create, você está criando um objeto salvo.

Você pode criar uma hash com os atributos:

```ruby
admin_attributes = Factory.attributes_for(:admin)
```

Você pode definir associações:

```ruby
Factory.define :picture do |factory|
  factory.name "Random name"
  factory.association :user, :factory => :user
end
```

Basicamente o Factory Girl é isso, dessa forma você já pode abandonar as fixtures. Existe mais opções de uso do Factory Girl que você pode olhar a [documentação oficial](http://rdoc.info/projects/thoughtbot/factory_girl).
