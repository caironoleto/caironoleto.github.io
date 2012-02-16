---
layout: post
title: "Tradução: Rails Database Migrations - Parte III"
---

Esta é a terceira parte da tradução do artigo Rails Database Migrations. Mais uma vez, antes de desfrutarem da leitura, quero dizer-lhes que se encontrar erros de português ou a tradução com sentido diferente, por favor, **comuniquem-me!** Avisem-me por email, twitter (@caironoleto), ou qualquer mensageiro!! :P

[Rails Database Migrations - Parte I](/2008/09/23/traducao--rails-database-migrations---parte-i)
[Rails Database Migrations - Parte II](/2008/10/21/traducao--rails-database-migrations---parte-ii)

### 4. Rodando migrações

Rails fornece um conjunto de tarefas rake para trabalhar com as migrações, que se resume em rodar alguns conjuntos de migrações. A tarefa rake mais relatada que você provavelmente usará é `db:migrate`. Na sua forma mais básica certamente rodará o método `up` para todas as migrações que ainda não foram rodadas. Se não existir migrações ele sai.

Se você especificar uma migração, o Active Record irá rodar as migrações requeridas (up ou down) até que ela tenha chegado nessa versão específica. A versão é o prefixo numérico do nome de uma migração. Por exemplo para migrar até a versão 20080906120000 execute

`rake db:migrate VERSION=20080906120000`

Se a versão for maior do que a versão corrente (ou seja, está migrando para cima) irá rodar o método `up` de em todas as migrações acima e incluindo a versão 20080906120000, se a migração for para baixo, então será executado os métodos `down` de todas as migrações para baixo até, mas não incluindo, 20080906120000.

### 4.1 Reversão

Uma tarefa comum é regressar a última migração, por exemplo, se você cometeu um engano e deseja corrigi-lo. Ao invés de monitorar o método down com a migração anterior, você pode rodar

`rake db:rollback`

Isso irá rodar o método down da migração mais recente. Se você precisa se desfazer de várias migrações, você pode fornecer o parâmetro `STEP`:

`rake db:rollback STEP=3`

irá rodar o método `down` das 3 últimas migrações.

A tarefa `db:migrate:redo` é um atalho para fazer uma reversão e a migração de volta. Assim como na tarefa `db:rollback` você pode usar o parâmetro `STEP` se você precisar voltar em mais de uma versão, por exemplo

`rake db:migrate:redo STEP=3`

Nenhuma dessas tarefas Rake fazem qualquer coisa que você não poderia fazer com `db:migrate`, são simplesmente mais convenientes, desde que você não precise especificar explicitamente de uma migração para outra.

Finalmente, a tarefa `db:reset` irá destruir sua base de dados, recria-la e carregar o schema atual dentro dela.

### 4.2 Especificando uma migração

Se você precisa especificar uma migração para cima ou para baixo, as tarefas `db:migrate:up` e `db:migrate:down` irão fazer isso. Basta especificar a versão apropriada e a migração correspondente e terá seu método `up` ou `down` invocado, por exemplo

`rake db:migrate:up VERSION=20080906120000`

irá rodar o método `up` da migração 20080906120000. Estas tarefas checa se a migração já tenha sido executada, se por exemplo `db:migrate:up VERSION=20080906120000` não irá fazer nada se o Active Record acreditar que 20080906120000 já tenha sido executada.

### 4.3 Sendo comunicativo

Por padrão, as migrações falam exatamente o que elas estão fazendo e o tempo de duração. Uma migração criando uma tabela e adicionando um index produz uma saída como esta

```bash
== 20080906170109 CreateProducts: migrating ===================================

-- create_table(:products)

   -> 0.0021s

-- add_index(:products, :name)

   -> 0.0026s

== 20080906170109 CreateProducts: migrated (0.0059s) ==========================
```

Vários método fornecem para você o controle tudo isto:

 * `suppress_messages` suprime qualquer mensagem gerada pelo bloco
 * `say` saída de texto (o segundo argumento controla se é recortado ou não)
 * `say_with_time` saída de texto com o tempo utilizado pelos blocos. Se o bloco retornar um inteiro, assume-se que este é o número de linhas afetadas.

Por exemplo, esta migração

```ruby
class CreateProducts < ActiveRecord::Migration
  def self.up
    suppress_messages do
      create_table :products do |t|
        t.string :name
        t.text :description
        t.timestamps
      end
    end
    say "Created a table"
    suppress_messages {add_index :products, :name}
    say "and an index!", true
    say_with_time 'Waiting for a while' do
      sleep 10
      250
    end
  end

  def self.down
    drop_table :products
  end
end
```
gera a seguinte saída

```bash
== 20080906170109 CreateProducts: migrating ===================================

-- Created a table

   -> and an index!

-- Waiting for a while

   -> 10.0001s

   -> 250 rows

== 20080906170109 CreateProducts: migrated (10.0097s) =========================
```

Se você quiser que o Active Record mantenha-se em silêncio, então execute `rake db:migrate VERBOSE=false` irá suprimir qualquer saída.

### 5. Usando Models nas suas migrações

Ao criar ou atualizar dados na sua migração, muitas vezes, é tentador utilizar um de seus models. Afinal eles existem para fornecer acesso fácil nos dados subjacentes. Isto pode ser feito mas um certo cuidado devem ser observados.

Considere por exemplo a migração que usa o modelo Product para atualizar a linha na tabela correspondente. Alice depois atualiza o modelo Product, adicionando uma nova coluna e uma validação. Bobs volta do feriado, atualiza o código e roda as migrações pendentes com `rake db:migrate`, incluindo o modelo que é utilizado para o Product. Quando o código é atualizado e só então o modelo Product possui a atualização adicionada pela Alice. O banco de dados entretanto não é atualizado e assim não possui a coluna e então gerará um erro, por que a validação para esta coluna ainda não existe.

Frequentemente eu preciso atualizar as linhas no banco de dados sem escrever SQL pelas minhas mãos. Eu não estou usando qualquer especialidade do modelo. Um padrão para isso é definir uma cópia do modelo dentro da própria migração, por exemplo

```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  class Product < ActiveRecord::Base
  end

  def self.up
    ...
  end

  def self.down
    ...
  end
end
```

A migração possuirá uma própria copia do modelo Product e não mais precisará saber sobre o modelo Product definido na própria aplicação.

### 5.1 Lidando com as mudanças no modelo

Por razões de performance, as informações sobre as colunas de um modelo é cacheada. Por exemplo, se você adicionar a coluna na tabela e tentar usar o modelo correspondente para inserir uma nova linha, ele pode tentar usar as informações antigas. Você pode forçar o Active Record para reler a informação da coluna com o método `reset_column_information`, por exemplo

```ruby
class AddPartNumberToProducts < ActiveRecord::Migration
  class Product < ActiveRecord::Base
  end

  def self.up
    add_column :product, :part_number, :string
    Product.reset_column_information
    ...
  end

  def self.down
    ...
  end
end
```
[Continuação: Rails Database Migrations - Parte IV](2008/11/12/traducao--rails-database-migrations---parte-iv)
