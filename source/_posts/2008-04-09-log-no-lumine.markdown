---
layout: post
title: "Log no Lumine"
comments: true
---

Ontem (08/04/2008) na aplicação que estou desenvolvendo na W7 Solutions tive o seguinte problema:

Foi atualizado algumas coisas no CVS, e quando fiz o update, "puff", a aplicação não funcionava mais.

E olha só que legal, não foi a primeira vez que aconteceu isso. Geralmente quando a aplicação para de funcionar é por que tem algum mapeamento que não funciona no Lumine, ou algum controlador está criando um objeto do Lumine, que não está encontrando o mapeamento.

Como a versão do Lumine que usamos nesse projeto é a 0.73, que ainda utiliza bastante de XML pra fazer mapeamentos e validações, então temos que sair catando um por um até achar o danado do problema.

Eu já tinha visto anteriormente dentro do Lumine, uma classe chamada LumineLog. Só que não sabia como coloca-la pra funcionar.

Bati o pé, e disse que só avançaria quando, de forma computacional, resolver este problema. Pra resolver isso, seria necessário ativar o log do lumine. Tentei de varias formas, não consegui mandei um email pro [Hugo](http://www.hufersil.com.br), criador do framework Lumine. Que prontamente me respondeu.

É o seguinte, na versão 0.73, para o log funcionar, é necessário usar o seguinte método:

```php
LumineLog::setLevel($level, $formaDeSaida);
```

Onde $level é o nível de debulhamento, que dentro de lumineLog está definido como 1 para debug, 2 para warnings e 3 para all (debug, warning, error), e a $formaDeSaida pode ser `output` ou `browser`, onde output cria um arquivo contendo o log e browser é a saída direto no browser.

Não consegui funcionar o output, mas o browser funciona que é uma beleza.

Outra coisa, esse método, eu o chamo dentro do arquivo que cria o objeto do Lumine passando as suas configurações.

E agora, quando está com algum problema do tipo "a aplicação não abre" eu só descomento o Log, e vejo o que está dando errado.

E para quem quer saber como funciona no Lumine 1.05, é só acessar o site oficial e conferir na sua documentação.

Espero que com isso muita gente consiga resolver problemas.

Até a proxima!
