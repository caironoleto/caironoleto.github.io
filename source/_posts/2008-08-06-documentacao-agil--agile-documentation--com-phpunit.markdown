---
layout: post
title: Documentação Ágil (Agile documentation) com PHPUnit
---

Para quem acha que times Ágeis não criam documentação, **vocês estão enganados!** O que acontece é que a documentação está sempre em mudanças, que levaria bem mais tempo documentar as coisas do que programar ou entender do negócio.

Mas nem por isso times Ágeis não documentam, times Ágeis usam as próprias ferramentas para criar documentação, e com [PHPUnit](http://www.phpunit.de) não é diferente. No PHPUnit existe uma forma de criar documentação a partir dos testes utilizando o comando phpunit `--testdox ClasseDeTeste`.

Esse simples comando gera a partir de seus testes uma breve documentação sobre o que são os testes que você está escrevendo:

```bash
phpunit --testdox BankAccountTest
PHPUnit 3.2.10 by Sebastian Bergmann.
BankAccount
 - Balance is initially zero
 - Balance cannot become negative
```

Exemplo retirado da [documentação oficial](http://www.phpunit.de/pocket_guide/3.2/en/other-uses-for-tests.html). Daí você pode exportar para html ou qualquer outro formato de saída que o próprio PHPUnit oferece.

Até a próxima!
