---
layout: post
title: "Tradução: Rails Database Migrations - Parte IV"
comments: true
---

Esta é a última parte da tradução [desse artigo](http://guide.rails.info/migrations.html) sobre as Migrations do Rails. Antes de se desfrutarem da leitura quero dizer-lhes que se encontrar erros de português ou a tradução com sentido diferente, por favor, comuniquem-me! Avisem-me por email, twitter ([caironoleto](http://twitter.com/caironoleto)), ou qualquer mensageiro!! :P

 * [Continuação: Rails Database Migrations - Parte I](/2008/09/23/traducao--rails-database-migrations---parte-i)
 * [Continuação: Rails Database Migrations - Parte II](/2008/10/21/traducao--rails-database-migrations---parte-ii)
 * [Continuação: Rails Database Migrations - Parte III](/2008/11/04/traducao--rails-database-migrations---parte-iii)

### 6. Armazenando esquemas e você

### 6.1 Quais são os arquivos do esquema?

Migrações, poderosas como são, não são fontes autorizada para o esquema do seu banco de dados. Esse papel cabe ao `schema.rb` ou ao um arquivo SQL gerado pelo Active Record através da análise do banco de dados. Este arquivo não foi projetado para ser editado, ele é uma representação do estado atual do banco de dados.

Não é necessário (e isto é um erro propenso) para implantar uma nova instancia de uma aplicação repetindo o histórico inteiro da migração. É mais simples e rápido apenas carregar dentro do banco de dados a descrição do esquema atual.

Por exemplo, esta é a forma de como o banco de dados de testes é criado: o banco de dados atual de desenvolvimento é excluído (tanto para o `schema.rb` ou para `development.sql`) e então carregado dentro do banco de dados de teste.

Arquivos de esquema também são uteis se deseja olhar rapidamente quais atributos o objeto do Active Record possui. Estas informações não está no código do modelo e frequentemente está espalhado pelas várias migrações mas está resumido no arquivo de esquema. O plugin [annotate_models](http://agilewebdevelopment.com/plugins/annotate_models), adiciona automaticamente cada (e atualiza) um dos comentários no início de cada modelo resumindo o esquema, que pode ser de seu interesse.

### 6.2 Formas de armazenar o esquema

Existe duas formas de armazenar o esquema. Uma é setar em `config/environment.rb` atribuindo o `config.active_record.schema_format`, que pode ser `:sql` ou `:ruby`.

Se `:ruby` é selecionada então o esquema será armazenado em `db/schema.rb`. Se você olhar este arquivo você verá que não encontrará um pouco mais do que uma grande migração:

```ruby
ActiveRecord::Schema.define(:version => 20080906171750) do
  create_table "authors", :force => true do |t|
    t.string   "name"
    t.datetime "created_at"
    t.datetime "updated_at"
  end

  create_table "products", :force => true do |t|
    t.string   "name"
    t.text     "description"
    t.datetime "created_at"
    t.datetime "updated_at"
    t.string   "part_number"
  end
end
```

De muitas formas é exatamente isso. Este arquivo é criado pela examinação do banco de dados e expressado em estruturas usando `create_table`, `add_index` e assim por diante. Por causa da independência do banco de dados ele deve carregar dentro de qualquer banco de dados que o Active Record suporta. Isso poderia ser muito útil se você quiser distribuir a aplicação para rodar em vários banco de dados.

Existe porém uma desvantagem: `schema.rb` não expressa itens específicos de banco de dados como constraints de chaves estrangeiras, triggers ou stored procedures. Enquanto na migração você pode executar SQL customizadas, o esquema armazenado não pode reconstituir essas atribuições do banco de dados. Se você usar recursos como este, então você deve atribuir o esquema para `:sql`.

Em vez de usar o esquema de armazenamento do Active Record a estrutura será armazenada usando uma ferramenta específica do banco de dados (pela tarefa rake `db:structure:dump`) dentro de `db/#\{RAILS_ENV\}_structure.sql`. Por exemplo para o PostgreSQL a utilidade `pg_dump` é usada e para o MySQL este arquivo irá conter a saída de SHOW CREATE TABLE para as várias tabelas. Carregando este esquema é uma simples questão de executar as declarações SQL contida dentro.

Por definição irá fazer uma copia perfeita da estrutura do banco de dados mas isso vai impedir o carregamento do esquema dentro de outros banco de dados que não seja um dos utilizados para criá-lo.

### 6.3 Armazenamento de esquema e controle de código

Por causa da autoridade do código do seu esquema de banco de dados, é altamente recomendado que você verifique dentro de seu controle de código.

### 7. Active Record e Integridade Referencial

O Active Record é a maneira inteligente de permanecer em seus modelos, não no banco de dados. Alguns recursos como triggers ou constraints de chaves estrangeiras, que empurra alguma inteligência de volta para os banco de dados não são muito usados.

Validações como `validates_uniqueness_of` é uma forma de seus modelos podem valer a integridade dos dados. A opção `:dependent` em associações adiciona aos modelos automaticamente destruir os objetos filhos que seus pais são destruídos. Como tudo que funciona a nível de aplicação, estes não podem garantir integridade referencial e algumas pessoas aumentam com constraints de chaves estrangeiras.

Apesar de o Active Record não fornecer todas as ferramentas para trabalhar diretamente com todos os recursos, o método `execute` pode ser usado para executar SQL arbitrárias. Há também uma série de plugins como [redhillonrails](http://agilewebdevelopment.com/plugins/search?search=redhillonrails) que adicionam suporte a chaves estrangeiras para o Active Record (incluindo suporte para destruir chaves estrangeiras no `schema.rb`)
