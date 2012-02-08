---
layout: post
title: Lumine no GitHub
---

Olá, na semana passada o Hugo Ferreira, criador do [Lumine Framework](http://www.hufersil.com.br/lumine), fez um release de uma versão beta onde ele criou destrutores e um método invocando esses destrutores. Eu até comentei aqui no blog sobre esse release, e gostei muito porquê realmente fez diminuir o consumo de memória, mas uma coisa ainda me deixava com a pulga atrás da orelha, porquê o framework ainda "inchava" no consumo de memória.

Aí no sábado fiquei pensando o que eu devia fazer e no domingo comecei a fazer. A primeira coisa que fiz, foi colocar o [Lumine Framework no GitHub](http://github.com/caironoleto/lumine-framework/tree/master), assim fica mais fácil as pessoas verem o que está sendo feito e pode comentar cada linha do que está lá, bem simples mesmo, apenas clicando na linha e comentando. E fica fácil para quem quiser desenvolver, é só fazer um fork e botar a mãos na massa.

Depois eu investiguei como funciona a criação dos objetos e retirei o armazenamento de um objeto de configuração dentro do objeto que estamos lidando. Removi porquê não vejo a necessidade de se manter esse objeto lá, porquê não tem como eu mudar essa configuração em tempo de execução, já que ele necessita de um objeto de configuração.

Nesse ponto, o Hugo me disse que possivelmente não funcionaria multiplas conexões com bancos distintos. Como eu ainda tive a oportunidade de testar se realmente funciona multiplas conexões, então não posso afirmar nada, mas investigando os métodos de criação de configuração, eu acho que irá conectar normalmente.

Depois, eu alterei o destrutor para realmente cumprir o que ele faz destruir o objeto, antes ele estava apenas limpando algumas propriedades do objeto, destruindo outras, agora ele realmente destrói. Se alguem tiver a necessidade de apenas reiniciar o objeto, é só usar o método reset() que ele se propõe a fazer isso.

E por último varri todo o `Lumine_Base` e procurei por onde ele estava criando objeto e comecei a destruir todos após a sua execução.

Com essas alterações, agora o framework não está inchando e mantém a consumo de memória constante.

No momento estou trabalhando na perfomance dele. Ontem eu fiz o mapeamento do método save e fiz um benchmark, ele faz uma média de 32,2 linhas salvas no banco de dados por segundo. Quero aumentar a quantidade de linhas salvas por segundo e estou estudando como ele faz isso, logo após, irei fazer o inverso, verificar a quantidade de linhas navegadas por segundo.

Quem quiser contribuir com o que eu estou fazendo pode fazer um fork no GitHub e fazer os commits para lá ou para apenas quem quer acompanhar, é só assinar os feeds de commits e do projeto.

Até a próxima!
