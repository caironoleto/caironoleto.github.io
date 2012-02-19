---
layout: post
title: Behavior Driven Development em PHP com PHPSpec
comments: true
---

Comecei a ler muito sobre [TDD](http://en.wikipedia.org/wiki/Test_driven_development), depois na comunidade [rails-br](http://groups.google.com/group/rails-br) descobri o [BDD](http://en.wikipedia.org/wiki/Behavior_driven_development) li e me pareceu mais simples do que TDD.

Para fazer TDD no rails é bem mais simples do que parece, é só executar os scripts de criação de models, controllers e views que o framework já cria os arquivos básicos para fazer TDD, bastando apenas escrever os casos de testes e executar o comando rake para tudo funcionar.

Para fazer BDD em rails é necessário a instalação de um plugin chamado RSPec, esse plugin é responsável pela criação das especificações em BDD.

Em rails é muito simples fazer TDD e BDD, já que o próprio ambiente te prepara para desenvolver se utilizando dessas técnicas, diferente dos frameworks PHP que poucos deles possuem facilidades como rails (Na sua última versão, o [CodeIgniter](http://www.codeigniter.com) introduziu TDD em seu core, ainda não tive tempo para dar uma olhada de como funciona. No [cakePHP](http://cakephp.org/), pelo que eu vi na comunidade, é capaz de se fazer TDD.

Para se fazer TDD e BDD no PHP é necessário a instalação de frameworks para o mesmo, no meu caso eu escolhi o [PHPUnit](http://www.phpunit.de/) para testes unitários e para BDD eu escolhi o [PHPSpec](http://www.phpspec.org). Só que essa escolha foi por livre e espontânea pressão, é que somente existe esse framework para se trabalhar com BDD em PHP.

Ele foi escrito após o desenvolvedor ([Pádraic Brady](http://blog.astrumfutura.com/)) ter se aventurado por Ruby e Rails e conhecido RSpec, ele resolveu desenvolver o "RSpec" para PHP e o batizou de PHPSpec.

A documentação é bastante simples e fácil de ler, usei algumas horas e li toda a documentação, e agora estou planejando para utiliza-los nos projetos que faço parte.

Agora vou explicar um pouco de como funciona com um exemplo.

Primeiro, antes da gente começar, devemos especificar o problema, no caso, como um usuário irá fazer um comentário.

    Usuário deverá colocar seu nome, depois ele deverá colocar seu email.
    Se o usuário tiver um site, poderá coloca-lo.
    Então depois o usuário coloca o conteúdo do comentário e clica em enviar.

Agora que temos a especificação, vamos criar nosso primeiro exemplo:

```php

require_once "Post.php";

class DescribeNewPostInBlog extends PHPSpec_Context {
  private $post = null;
  public function before() {
    $this->post = new Post;
  }
  
  public function itShouldSetName() {
    $this->post->setName("Cairo Noleto");
    $this->spec($this->post->getName())->should->beEqualTo("Cairo Noleto");
  }
  
  public function itShouldSetEmail() {
    $this->post->setEmail("caironoleto@gmail.com");
    $this->spec($this->post->getEmail())->should->beEqualTo("caironoleto@gmail.com");
  }
  
  public function itShouldSetWebSite() {
    if ("user set as website") {
      $this->post->setWebSite("http://www.caironoleto.com/");
    }
    $this->spec($this->post->getWebSite())->should->beEmpty();
  }
  
  public function itShouldSetContent() {
    $this->post->setContent("Aqui vai o comentário do post :P");
    $this->spec($this->post->getContent())->should->be("Aqui vai o comentário do post :P");
  }
}

```

Se rodarmos no terminal, teremos o resultado:

```bash

cairo@angus:~/BDDonPHP$ phpspec DescribeNewPostInBlog

E

Fatal error: Call to undefined method Post::setName() in /home/cairo/BDDonPHP/DescribeNewPostInBlog.php on line 15

```

Da erro por que nossa classe Post não contém o método setName, e vai da erro a cada um dos métodos que não funciona, então irei cria-los:

```php

class Post {

  private $name;
  private $email;
  private $website;
  private $content;
  
  public function setName($name) {
    $this->name = $name;
  }
  
  public function setEmail ($email) {
    $this->email = $email;
  }
  
  public function setWebSite($website) {
    $this->website = $website;
  }
  
  public function setContent($content) {
    $this->content = $content;
  }
  
  public function getName() {
    return $this->name;
  }

  public function getEmail() {
    return $this->email;
  }
  
  public function getWebSite() {
    return $this->website;
  }

  public function getContent() {
    return $this->content;
  }
}

```

Agora rode mais uma o código e veja tudo rodando perfeitinho :D

*Algumas considerações:*

A primeira e mais importante é que o framework está na versão 0.2.3 e ainda não saiu nenhuma versão release, todas as versões são betas.

Por ser beta, falta alguns métodos (Como os "Matchers" beAnInstaceOf, beString, beOfType). Por assinar a lista, foi resolvido hoje e inserido o método beString, ainda não sei como faço pra pegar a versão do SVN que seria bem mais interessante, vou colocar um email na lista perguntando como faço e blogo aqui.

O Framework como está funcionando já da pra escrever muita especificação, então é bola pra frente.

Até a próxima!</p>
