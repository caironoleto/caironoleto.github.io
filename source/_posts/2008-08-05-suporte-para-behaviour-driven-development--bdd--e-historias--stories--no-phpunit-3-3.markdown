---
layout: post
title: Suporte para Behaviour Driven Development (BDD) e Histórias (Stories) no PHPUnit 3.3
comments: true
---

Agora o PHPUnit tem suporte para Behaviour Driven Development (BDD) e criação de histórias (Stories). Além de Test-Driven Development (TDD) agora BDD no PHPUnit. <a title="Support for BDD and Stories in PHPUnit 3.3" href="http://sebastian-bergmann.de/archives/738-Support-for-BDD-and-Stories-in-PHPUnit-3.3.html" target="_blank"></a>

Nesse artigo do Sebastian Bergmann, ele demonstra como utilizar BDD no PHPUnit descrevendo uma partida de boliche. O que eu achei legal foi o formato de saída:

```bash
sb@vmware ~ % phpunit --story BowlingGameSpec
PHPUnit @package_version@ by Sebastian Bergmann.
BowlingGameSpec
 - Score for gutter game should be 0 [successful]
   Given New game
    Then Score should be 0
 - Score for all ones is 20 [successful]
   Given New game
    When Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
     and Player rolls 1
    Then Score should be 20
 - Score for one spare is 16 [successful]
   Given New game
    When Player rolls 5
     and Player rolls 5
     and Player rolls 3
    Then Score should be 16
 - Score for one strike is 24 [successful]
   Given New game
    When Player rolls 10
     and Player rolls 3
     and Player rolls 4
    Then Score should be 24
 - Score for perfect game is 300 [successful]
   Given New game
    When Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
     and Player rolls 10
    Then Score should be 300
Scenarios: 5, Failed: 0, Skipped: 0, Incomplete: 0.
```
