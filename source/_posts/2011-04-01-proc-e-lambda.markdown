---
layout: post
title: Proc e lambda
comments: true
---

Você já deve ter ouvido que tudo em Ruby é objeto. Sim, sim, tudo é objeto. Menos blocos.

Blocos não são objetos. Mas existe duas formas de transformar blocos em objetos. Uma é usar Proc e a outra é usa lamda.

Para se criar um Proc existem algumas formas, a mais simples e mais utilizada é:

```ruby
proc = Proc.new do |object|
  puts object.inspect
end
```

Depois que você criar o seu Proc, para chamar o bloco que você acabou de criar, você deve utilizar o método call:

`proc.call Object`

O método call é chamado para executar o bloco. Em blocos, vocês podem fazer o que quiser. Um caso interessante é para processamento sequencial. No rails é bastante utilizado na definição de named scopes.

Outra forma de fazer blocos virarem objetos é utilizar o lambda:

```ruby
lambda = lambda do |object|
  puts object.inspect
end
```

Basicamente, Proc e lambda funcionam da mesma forma. Mas existe uma diferença, que é no caso de se utilizar controles de fluxo, no caso return, break, redo, retry e vários outros.

O problema nesse caso é que esses controles de fluxo quebram quando você está utilizando Proc.
