---
layout: post
title: "Tradução: Rails database Migrations - Parte I"
---

Olá, este é o primeiro de quatro partes [desse artigo](http://guide.rails.info/migrations.html) sobre as Migrations do Rails. Antes de se desfrutarem da leitura quero dizer-lhes que se encontrar erros de português ou a tradução com sentido diferente, por favor, comuniquem-me! Avisem-me por email, twitter ([caironoleto](http://twitter.com/caironoleto)), ou qualquer mensageiro!! :P

###Migrações em Banco de Dados em Rails

Migrações é a forma conveniente de você alterar seu banco de dados de uma maneira organizada e estruturada. Você poderia editar fragmentos de SQL na mão mas você teria a responsabilidade de comunicar aos outros desenvolvedores que eles precisam ir lá e executá-los. Você também necessita acompanhar as mudanças na máquina de produção na próxima vez que você for fazer deploy. O Active Record marca as migrações que já foram executadas e tudo que você precisa fazer é atualizar seu código e rodar rake db:migrate. O Active Record irá trabalhar para que suas migrações sejam executadas.

Migrações é a forma de você descrever essas transformações usando Ruby. A grande coisa disso tudo (como muito das funcionalidades do Active Record) é a independência do banco de dados: você não precisa se preocupar com mais nenhuma sintaxe para CREATE TABLE, ou que você se preocupe sobre variações de SELECT * (você pode abstrair os requisitos específicos de banco de dados SQL). Por exemplo, você poderia usar SQLite 3 no desenvolvimento, mas MySQL em produção.

Você aprenderá tudo sobre migrações incluindo:

 * Os geradores que você pode usar para criá-los
 * Os métodos que o Active Record provê para manipular seu banco de dados
 * As tarefas Rake que você pode manipular
 * Como eles são relativo ao schema.rb

###1. Anatomia de uma migração

Antes de eu mergulhar nos detalhes de uma migração, aqui estão alguns exemplos curtos de coisas que você pode fazer:

```ruby
class CreateProducts < ActiveRecord::Migration
  def self.up
    create_table :products do |t|
      t.string :name
      t.text :description
      t.timestamps
    end
  end

  def self.down
    drop_table :products
  end
end
```

Essa migração adiciona uma tabela chamada `products` com uma coluna string chamada `name` e uma coluna text chamada `description`. Uma chave primária chamada id também será adicionada, no entanto por padrão, não precisamos pedir isso. As colunas timestamps `created_at` e `updated_at` que o Active Record preenche automaticamente também são adicionadas. Revertendo essa migração simplesmente remove a tabela.

Migrações não tem limite para alterar o esquema. Você pode usar para corrigir dados errados no banco de dados ou popular novos campos:

```ruby
class AddReceiveNewsletterToUsers < ActiveRecord::Migration
  def self.up
    change_table :users do |t|
      t.boolean :receive_newsletter, :default => false
    end
    User.update_all ["receive_newsletter = ?", true]
  end

  def self.down
    remove_column :users, :receive_newsletter
  end
end
```

Esta migração adiciona a coluna `receive_newsletter` para a tabela users. Nós queremos que o padrão seja falso para novos usuários, mas para os usuários existentes nos consideramos que eles já fizeram a sua opção, então nos usamos o modelo User para setar a bandeira para true para usuários existentes.

###1.1 Migrações são classes

A migração é uma subclasse de ActiveRecord::Migration que implementa dois métodos: up (para realizar as transformações exigidas) e down (reverte o que foi feito).

O Active Record prover métodos para executar as tarefas comuns na definição dos dados independente do banco de dados (Você verá mais detalhes mais tarde):

 * `create_table`
 * `change_table`
 * `drop_table`
 * `add_column`
 * `remove_column`
 * `change_column`
 * `rename_column`
 * `add_index`
 * `remove_index`

Se você precisa executar tarefas específicas para seu banco de dados (por exemplo, criar uma chave estrangeira) então a função `execute `permite executar SQL arbitrarias. As migrações é apenas uma classe regular em Ruby, então você não esta limitado a apenas essas funções. Por exemplo, após adicionar uma coluna, você pode escrever código para setar o valor dessa coluna para dados existentes (se necessário usando seus modelos).

###1.2 O que está no nome

Migrações sao armazenadas em arquivos em `db/migrate`, uma para cada classe de migração. O nome dos arquivos é na forma de `YYYYMMDDHHMMSS_create_products.rb`, ou seja, uma hora UTC identificando a migração seguida de um sublinhado e seguido do nome da migração. O Nome da classe de migração deve bater com a ultima parte do arquivo. Por exemplo, `20080906120000_create_products.rb` deveria definir CreateProducts e `20080906120001_add_details_to_products.rb` deveria definer AddDetailsToProducts. Se você acha que necessita mudar o nome do arquivo, então você deve atualizar o nome da classe dentro do arquivo ou então o Rails ira queixar-se que não existe a classe.

Internamente Rails usa apenas o numero da migração (data) para identificá-lo. Antes do Rails 2.1 os números das migrações eram iniciados em 1 e apenas incrementado cada vez que era gerado uma nova migração. Com múltiplos desenvolvedores era mais fácil haver colisões, necessitando você voltar e reordena-los. Você pode reverter para o esquema da velha numeração setando `config.active_record.timestamped_migrations `para `false `no `environment.rb`.

A combinação do tempo e o registro que permite executar as migrações foram a forma como o Rails fez para manipular as situações comuns que ocorrem com múltiplos desenvolvedores.

Por exemplo Alice adicionou a migração `20080906120000`e `20080906123000`e Bob adicionou `20080906124500` e rodou. Alice finaliza suas mudanças e checa nas suas migrações e Bob puxa as mais recentes atualizações. Rails sabe que ele não rodou as duas migrações de Alice assim `rake db:migrate` irá executá-los (Apesar que a migração do Bob seja executada uma hora mais tarde), e similarmente fazendo regressão da migração não teria executado esses dois métodos.

Claro que isso não substitui a comunicação dentro da equipe, por exemplo, na migração da Alice ela removeu uma tabela que Bob assumiu a existência na sua migração, então claro que um problema irá acontecer.

###1.3 Mudando migrações

Ocasionalmente você comete um erro enquanto escrevia a migração. Se você já tiver executado a migração, então você não pode simplesmente editar e executar novamente a migração: Rails acha que a migração já foi executada, então ele não fará nada quando você rodar `rake db:migrate`. Você deve voltar a migração (por exemplo, com `rake db:rollback`), editar sua migração e rodar `rake db:migrate `para a correta versão.

No geral, editar migrações existentes não é uma boa idéia: você criará trabalho extra para você mesmo e para seus parceiros de trabalho e causar um maior problema se a versão da migração for rodada em uma máquina de produção. Em vez disso, você deve escrever uma nova migração para realizar as alterações que você solicita. Editando uma recém migração gerada e que ainda não foi `commitada` para o controle de versão (ou que geralmente não foi propagada além da sua máquina de desenvolvimento) é relativamente inofensivo. Apenas use com bom senso.

[Continuação: Rails Database Migrations - Parte II](2008/10/20/traducao--rails-database-migrations---parte-ii)
[Continuação: Rails Database Migrations - Parte III](2008/11/04/traducao--rails-database-migrations---parte-ii)
[Continuação: Rails Database Migrations - Parte IV](2008/10/20/traducao--rails-database-migrations---parte-iv)

E aqui finaliza a primeira parte deste artigo.

Até a próxima!
