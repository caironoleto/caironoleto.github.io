---
layout: post
title: "PHPUnit Series - Instalação"
comments: true
---

Olá, é com prazer que venho aqui iniciar uma série sobre desenvolvimento ágil em PHP.

E vamos utilizar um Framework de testes unitarios chamado de PHPUnit.

PHPUnit é um desses framework pra se trabalhar orientado a testes. Atualmente ele está na versão 3.2.15, e já está se desenvolvendo a versão 4.0 para ser integrada ao PHP 5.3.

Mas vamos ao que interessa

INSTALAÇÃO

Antes de mais nada, é necessário de PHP 5, você pode fazer a instalação seguindo vários blog, foruns e sites espalhados pela internet.

Após isso, é necessário fazer o download do PHPUnit, [neste link](http://pear.phpunit.de/get/) é possivel fazer o download.

Algumas considerações, as versões releases vem com um arquivo em lotes necessário para rodar os testes, deve conter na PATH do windows o caminho para esse arquivo.

Extraia o conteudo em uma pasta do Windows, renomei o arquivo `pear-phpunit.bat` para `phpunit.bat`.

Importa para seu projeto a pasta PHPunit, crie um novo arquivo PHP e digite nele:

```php
require_once 'PHPUnit/Framework.php';

class Test1 extends PHPUnit_Framework_TestCase {
  public function testNewArrayIsEmpty() {
    $fixture = array();
    $this->assertEquals(0, sizeof($fixture));
  }

  public function testArrayContainsAnElement() {
    $fixture = array();
    $fixture[] = 'Element';
    $this->assertEquals(1, sizeof($fixture));
  }
}
```

Abra o prompt de comando do DOS, navegue a pasta do arquivo que você acabou de criar, e digite `phpunit arquivo`.

No proximo, explicarei o codigo acima e demonstrarei novos casos de usos.

Até a proxima!
