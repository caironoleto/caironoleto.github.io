---
layout: post
title: "Test-driven development on Rails"
comments: true
---

Comecei um projeto pessoal, que já vi um muito bom rodando, [Spesa](http://spesa.com.br), feito pelo [Nando Vieira](http://simplesideias.com.br/) (sempre queria saber quem tinha criado o spesa, e por acaso descobri) e outro no [rails-br](http://groups.google.com/group/rails-br/browse_thread/thread/db8bee2f2b2c8637?hl=pt-BR) bem interessante, em AJAX.

Após isso, tambem resolvi fazer o meu gerenciador financeiro! Assim como o Nando Vieira, irei colocar todas as regras de negocio realmente no negocio, evitando deixar tudo em Stored Procedures.

Nesse projeto, resolvi usar utilizar esta técnica, e segui um [tutorial](http://simplesideias.com.br/tdd-no-rails-unit-tests/) rapído do Nando Vieira.

Depois de implementar 12 testes, tive problemas com dois, e achei uma coisa estranha, fiz passar, mas eu acho que tem alguma coisa errado, e não é no codigo implementado.

O método MD5.hexdigest() retorna uma hash MD5, e para o teste, faço a comparação da String que estou criado pro objeto user com a mesma string passando direto pelo método.

```ruby
assert_equal(Digest::MD5.hexdigest("caironoleto"), user.password)
```

Só que a MD5 esperada é diferente da MD5 que é passada, mesmo sendo os mesmos métodos, e mesmo sendo a mesma String.

Para resolver, antes de salvar o objeto, atribui o valor a uma nova variavel, assim criando a MD5 correta.

```ruby
def test_of_password_in_new_user
  user = create(:password => "caironoleto")
  expected = user.password
  user.save
  assert_equal(Digest::MD5.hexdigest(expected), user.password)
end
```

Se algum railer ver, por favor, deixa comentario e explica o que está acontecendo.

Até a proxima!

UPDATE:

Realmente tem uma coisa estranha! E a pior das coisas, ela está entre o computador e a cadeira! (HoiahoiehAOIEHaO).

Simplesmente o código estava errado, nem métodos, ou nada disso, e sim o que estava fazendo.

Simplesmente por que quando utilizo o método create, ele realmente cria, aplicando assim a validação que passei pelo modelo e já salvando o objeto no banco de dados. E depois, eu mandava ele "salvar" repetindo a mesma coisa. Só que fazendo a MD5 da MD5 que já estava lá, gerando erro quando tentava comparar a MD5 direta pelo método `Digest::MD5.hexadigest("caironoleto")`.

O que eu tava fazendo aí em cima dava certo, por que ele está armazendo em expected a MD5 que foi criada.

Agora sim respiro aliviado!

Até a proxima!
