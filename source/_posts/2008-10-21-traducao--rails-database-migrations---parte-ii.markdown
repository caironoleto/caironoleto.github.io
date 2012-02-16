---
layout: post
title: "Tradução: Rails database Migrations - Parte II"
---

Esta é a segunda parte da tradução do [artigo Rails database Migrations](http://guide.rails.info/migrations.html). Mais uma vez, antes de desfrutarem da leitura, quero dizer-lhes que se encontrar erros de português ou a tradução com sentido diferente, por favor, comuniquem-me! Avisem-me por email, twitter ([caironoleto](http://twitter.com/caironoleto)), ou qualquer mensageiro!! :P

[Rails Database Migrations - Parte I](/2008/09/23/traducao--rails-database-migrations---parte-i)

### 2.0 Criando migrações

### 2.1 Criando um Modelo

Um Modelo e os geradores de scaffold criará migrações apropriadas para criação de um novo Modelo. Esta migração contém instruções prontas para criação de uma tabela relevante. Se você mostrar pro Rails as colunas que você precisa então as declarações já estarão criadas. Por exemplo, rodando `ruby script/generate model Product name:string description:text` gerará uma migração como esta

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

Você pode adicionar quantos nomes/tipo de colunas como você deseja. Por padrão `t.timestamps` (Que criam as colunas `updated_at` e `created_at` que serão populadas automáticamentes pelo Rails) podem ser adicionadas para você.

###2.2 Criando uma migração autônoma

Se você criar uma migração para outros propósitos (Por exemplo, adicionar uma coluna numa tabela já existente) então você pode usar o gerador de migrações:

`ruby script/generate migration AddPartNumberToProducts`

Criará uma migração vazia mas já apropriada com o nome da migração:

```ruby
class AddPartNumberToProducts < ActiveRecord::Migration

  def self.up
  end

  def self.down
  end
end
```

Se o nome da migração é na forma AddXXXtoYYY ou RemoveXXXtoYYY e for seguida de uma lista de nomes de colunas e seus tipos então a migração será criada contendo declarações apropriadas para adicionar e remover colunas.

`ruby script/generate migration AddPartNumberToProducts part_number:string`

Gerará:

```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  def self.up
    add_column :products, :part_number, :string
  end

  def self.down
    remove_column :products, :part_number
  end
end
```

Similarmente:

`ruby script/generate migration RemovePartNumberFromProducts part_number:string`

gerará

```ruby
class RemovePartNumberFromProducts < ActiveRecord::Migration
  def self.up
    remove_column :products, :part_number
  end

  def self.down
    add_column :products, :part_number, :string
  end
end
```

E você não está limitado a gerar magicamente apenas uma coluna, por exemplo

`ruby script/generate migration AddDetailsToProducts part_number:string price:decimal`

gerará

```ruby
class AddDetailsToProducts < ActiveRecord::Migration
  def self.up
    add_column :products, :part_number, :string
    add_column :products, :price, :decimal
  end

  def self.down
    remove_column :products, :price
    remove_column :products, :part_number
  end
end
```

E sempre o que foi gerado é apenas um ponto de partida, você pode adicionar ou remover a partir dele o que você quiser, como você achar melhor.

### 3.0 Escrevendo uma migração

Uma vez que você criou uma migração usando um dos geradores, é a hora de trabalhar!

### 3.1 Criando uma tabela

`create_table` será um dos métodos mais usados. Tipicamente usada assim

```ruby
create_table :products do |t|
  t.string :name
end
```
criará uma tabela `products` com uma coluna chamada `name` (e como discutido anteriormente, implicitamente criará uma coluna id).

O objeto "renderizado" (yielded) no bloco permite você criar colunas na tabela. Existe duas formas de se fazer isso. A primeira é algo assim

```ruby
create_table :products do |t|
  t.column :name, :string, :null => false
end
```

a segunda forma, que é chamada de migração "sexy", que elimina redundância dos métodos. Em vez de `string`, `integer`, etc os métodos são criados a partir do tipo da coluna, onde os parâmetros subseqüentes são idênticos.

Por padrão `create_table` criará uma chave primária chamada `id`. Você pode alterar o nome da chave primária com a opção `:primary_key` (Não esqueça de atualizar o modelo correspondente) ou se você não precisar de uma chave primária (por exemplo HABTM ([has and belongs to many](http://wiki.rubyonrails.org/rails/pages/has_and_belongs_to_many)) join table) você pode passar `:id => false`. Se você precisa passar alguma informação específica, você pode passar um fragmento SQL na opção `:options`. Por exemplo:

```ruby
create_table :products, :options => "ENGINE=BLACKHOLE" do |t|
  t.string :name, :null => false
end
```

Irá anexar `ENGINE=BLACKHOLE` na sql usada para criar a tabela (Quando se usa MySQL por padrão é usado "ENGINE=InnoDB").

Os tipos que o Active Record suporta são `:primary_key`, `:string`, `:text`, `:integer`, `:float`, `:decimal`, `:datetime`, `:timestamp`, `:time`, `:date`, `:binary`, `:boolean`.

Eles vão ser mapeados apropriadamente para cada banco de dados, por exemplo com MySQL `:string` é mapeada para `VARCHAR(255)`. Você pode criar colunas e tipos não suportados pelo Active Record usando uma sintaxe não sexy, por exemplo:

```ruby
create_table :products do |t|
  t.column :name, 'polygon', :null => false
end
```

Esta forma, no entanto, dificulta a portabilidade para outros banco de dados.

### 3.2 Mudando tabelas

O primo mais próximo de `create_table` é `change_table`. Usado para alterar tabelas existentes, é similarmente usada como o `create_table` mas o objeto "rendenrizado" (yielded) para o bloco conhece mais truques. Por exemplo

```ruby
change_table :products do |t|
  t.remove :description, :name
  t.string :part_number
  t.index :part_number
  t.rename :upccode, :upc_code
end
```
remove a coluna `description` e `name`, adiciona a coluna `part_number` e adiciona um index nesta mesma coluna. E por ultimo altera o nome da coluna `upccode`. É o mesmo que fazer

```ruby
remove_column :products, :description
remove_column :products, :name
add_column :products, :part_number, :string
add_index :products, :part_number
rename_column :products, :upccode, :upc_code
```

Você não deve manter repetindo o nome da tabela e de todos os grupos de declarações relatados para modificar uma tabela em particular. Em uma transformação individual os nomes são curtos, por exemplo `remove_column` torna-se `remove` e `add_index` torna-se `index`.

### 3.3 Helpers especiais

O Active Record provê alguns atalhos para as funcionalidades mais comuns. Por exemplo, é muito comum adicionar as colunas `created_at `e `updated_at` e o método que faz exatamente isso é:

```ruby
create_table :products do |t|
  t.timestamps
end
```

Criará uma tabela products com essas duas colunas

```ruby
change_table :products do |t|
  t.timestamps
end
```

Adiciona essas duas colunas a uma tabela existente.

Outro helper é chamado de `references` (Também disponível como `belongs_to` ). Na sua forma mais simples adiciona alguma habilidade de reutilização.

```ruby
create_table :products do |t|
  t.references :category
end
```

então criará a coluna `category_id` com o tipo apropriado. Note que você deve passar o nome do modelo e não da coluna. O Active Record adicionará o sufixo _id para você. Se você tiver uma associação `belongs_to` polimórfica então `references` irá adicionar as colunas referidas

```ruby
create_table :products do |t|
  t.references :attachment, :polymorphic => { :default => 'Photo' }
end
```

irá adicionar a coluna `attachment_id` e a coluna `attachment_type` com o valor padrão Photo.

Se os helpers fornecidos pelo Active Record não for suficiente, você pode utilizar a função `execute` para executar SQL arbitrárias.

Para mais detalhes e exemplos de métodos individuais dê uma olhada na documentação da API, em particular a documentação para [ActiveRecord::ConnectionAdapters::SchemaStatements](http://api.rubyonrails.com/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html) (na qual provê os métodos disponíveis nos métodos `up` e `down`), [ActiveRecord::ConnectionAdapters::TableDefinition](http://api.rubyonrails.com/classes/ActiveRecord/ConnectionAdapters/TableDefinition.html) (na qual provê os métodos disponíveis para o objeto "rendenrizado" (yielded) por `create_table`) e o [ActiveRecord::ConnectionAdapters::Table](http://api.rubyonrails.com/classes/ActiveRecord/ConnectionAdapters/Table.html) (na qual fornece os métodos disponíveis para o objeto "rendenrizado" (yielded) por `change_table`).

### 3.4 Escrevendo seu método down

O método `down` da sua migração deve reverter as transformações concluídas pelo método `up`. Em outras palavras, o banco de dados deve se manter inalterado se você fizer um `up` seguido de um `down`. Por exemplo, se você criar uma tabela no método `up` você deverá excluí-la no método `down`. Deve ser inteligente e precisamente inverso ao método `up`. Por exemplo

```ruby
class ExampleMigration < ActiveRecord::Migration
  def self.up
    create_table :products do |t|
      t.references :category
    end
    #add a foreign key
    execute "ALTER TABLE products ADD CONSTRAINT fk_products_categories FOREIGN KEY (category_id) REFERENCES categories(id)"
    add_column :users, :home_page_url, :string
    rename_column :users, :email, :email_address
  end

  def self.down
    rename_column :users, :email_address, :email
    remove_column :users, :home_page_url
    execute "ALTER TABLE products DROP FOREIGN KEY fk_products_categories"
    drop_table :products
  end
end
```

as vezes a sua migração pode fazer algumas coisas irreversíveis, por exemplo quando você destrói alguns dados. Em casos como esse onde você não pode reverter uma migração, você pode lançar IrreversibleMigration para o seu método `down`. Se alguém tentar reverter a sua migração uma mensagem será mostrada falando que ela não será completa.

E aqui finaliza a segunda parte do artigo.

[Continuação: Rails Database Migrations - Parte III](2008/11/04/traducao--rails-database-migrations---parte-ii)
[Continuação: Rails Database Migrations - Parte IV](2008/10/20/traducao--rails-database-migrations---parte-iv)

Até a próxima!
