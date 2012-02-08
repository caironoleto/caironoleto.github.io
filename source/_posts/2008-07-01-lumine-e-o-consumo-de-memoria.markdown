---
layout: post
title: Lumine e o consumo de memória
---

Olá, alguns dias atrás estava fazendo o mapeamento de um projeto que estou desenvolvendo na Add4 Comunicação no Lumine Framework.

Totalmente corriqueiro, até que resolvi fazer alguns testes, bem simples, para verificar a integração entre os modelos e se tudo estava correto. Só que resolvi fazer de uma forma diferente, ao invés de apenas criar um registro e verificar a compatibilidade entre eles, resolvi fazer vários registros, então criei um loop de inserção:

```php

$crianca = new Crianca;

for($i = 1; $i <= 10; $i++) {
  $brinquedo[] = Brinquedos::staticGet(rand(1,1000));
}

$crianca->brinquedos = $brinquedos;
$crianca->save();

```

Até aí nada demais, digo que a criança tem 10 brinquedos. Mas então resolvi adicionar 10000 crianças no banco:

```php

for($i = 1; $i <= 10000; $i++) {
  $crianca = new Crianca;
  
  for($i = 1; $i <= 10; $i++) {
    $brinquedos[] = Brinquedos::staticGet(rand(1,10000));
  }
  
  $crianca->brinquedos = $brinquedos;
  $crianca->save();
  unset($brinquedos);
}

</pre>

Bom, aí agora adiciono as 10000 crianças, cada um com 10 brinquedos. O consumo de memória continuou normal, então resolvi pegar esses registros:

```php

$criancas = new Crianca;
$criancas->find();

while($criancas->fetch()) {
  $criancas->_getLink('brinquedos');
  print_r($criancas->toArray());
}

```

**BUM!**, então tive o problema, o retorno de uns 1500 registros, o PHP para a execução e lança o erro de estouro de memória.

A cada interação, o consumo de memória aumentava substancialmente, quando comecei a debugar o lumine, descobri que ele não tem um controle de memória, e assim vai criando objetos conforme a requisição, como no caso são várias de uma unica vez, então o consumo aumenta substancialmente até ocorrer o estouro de memória e não se preocupa em destruir esses objetos criados.

No momento estou trabalhando pra "enxugar" esse consumo e enviar um patch para o Lumine.

Então, até não ser resolvido, cuidado ao tratar com vários registros ao mesmo tempo, o consumo pode aumentar conforme o numero de `Get Links` forem sendo utilizados.

Até a próxima!
