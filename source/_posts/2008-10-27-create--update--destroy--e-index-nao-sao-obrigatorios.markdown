---
layout: post
title: Create, update, destroy e index não são obrigatórios!
comments: true
---

Olá, mas que titulo é esse? O que quer dizer isso!?

Eu acompanho e muito a comunidade RubyOnRails brasileira, e tenho notado um comportamento que vem tomando conta principalmente nos novatos, que é o de querer a obrigatoriedade dos métodos index, create, update e destroy.

Como todos sabemos, temos que adicionar, remover e atualizar nossos dados, em alguns casos específicos listar todos os dados, mas isso não é obrigatório, bem como o nome desses métodos não são obrigatórios!

Ao invés de utilizar o método index para listar, posso utilizar o método show_all para mostrar todos algo como users/show_all.

Eu acho que esse comportamento está derivando do mal uso do scaffold. Para quem ainda não entendeu, scaffold é para gerar uma estrutura INICIAL para seus clientes alimentarem os dados de seus sistemas. Não é para ser usado como a roda que gira o mundo ou como "The golden bullet". O scaffold deve ser usado em casos especificos, geralmente usados no inicio do desenvolvimento em apoio para os desenvolvedores lidarem com dados reais da aplicação ou quando a demanda de dados do cliente não pode parar.

Não usem scaffold para se gabar a seus amigos:

**"Duas linhas de código e já estava pronto"** ou **"Que ver eu fazer um blog em 15 minutos?!"**

Usem com consciência:

**"Maria, aqui está o formulário inicial para entrada dos dados relativo a nova categoria de imóveis, até o final do dia de amanhã eu irei te mostrar como a tela realmente irá funcionar, enquanto não fica finalizado, você já pode começar a trabalhar"**.
