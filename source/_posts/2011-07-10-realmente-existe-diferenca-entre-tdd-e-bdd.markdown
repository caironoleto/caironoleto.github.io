---
layout: post
title: Realmente existe diferença entre TDD e BDD?
---

Recebi um comentário do Cândido Sales que me fez reler um texto meu antigo sobre [a diferença entre TDD e BDD](/2008/06/11/diferenca-entre-tdd-e-bdd). Achei legal, interessante, mas vale a pena escrever mais um "dedinho" de palavras e incluir agora minha experiência de se usar TDD/BDD por algum tempo (Uns 2 anos, mais ou menos)!

Depois de toda a experiência que tenho hoje com desenvolvimento orientado a testes/comportamento vejo que não existe muita diferença entre as técnicas. Acredito, como já acreditava, que BDD faz parte da evolução de quem começa a praticar TDD.

Mas a parte mais importante, é que no final, essas duas técnicas não são apenas para testar a sua aplicação. Hoje considero que mais importante do que testar a aplicação é:

 * Fazer um design consciente da sua aplicação;
 * Melhorar a manutenção com a facilidade de se fazer refactoring! (Refactoring sem testes não é refactoring).

### Design consciente:

Quando vamos desenvolver algum código, não temos ainda uma opinião forte de como deve ser esse novo código. Então praticamente não sabemos ainda nomes de classe, métodos, rotinas…

Então TDD/BDD entra para nos ajudar a modelar no inicio do desenvolvimento de nossa API. Se você considerar o desenvolvimento em baby steps, então teremos uma API que evolua naturalmente conforme nossa aplicação vai crescendo. E a base dessa evolução são os testes que criamos antes e que nos dá segurança no desenvolvimento.

### Refactoring:

Refactoring é rescrever nossa API de forma mais simplificada e que os testes continuem passando e devemos evoluir nossos testes para acompanhar essa simplificação. Sem os testes não temos como garantir que essas alterações que visam simplificar o código continuem funcionando, por isso refactoring sem testes não é refactoring.

### Conclusão

Hoje considero TDD e BDD como duas formas diferentes de design da aplicação e que você realmente deve estuda-lás (Ou escolher uma :P) e melhorar o design do seu código.

Não vejo muitas diferenças em se utilizar TDD ou BDD e que vai mais da escolha pessoal do programador ou da sua equipe. E que o mais importante, é que se você está usando alguma dessas técnicas, então isso demonstra que você já tem uma preocupação com a qualidade do código que você escreve.
