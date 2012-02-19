---
layout: post
title: Extraindo informações usando Ruby
comments: true
---

Hoje lancei [uma aplicação](http://hackmarkoapp.heroku.com) que faz a análise dos palpites de um bolão ([O bolão da Marko Informática](http://www.bolaodamarko.com.br)) e mostra um gráfico com a quantidade de palpites por placa em um determinado jogo.

Para extrair essas informações eu não precisei acessar o banco de dados. Apenas consumi o que todos já podem ver, como por exemplo este [link](http://www.bolaodamarko.com.br/palpitejogo.php?jogo=84).

Essa brincadeira de extrair essas informações do bolão da Marko Informática começou com uma dúvida da minha esposa se tinha condição de ver no site os palpites dos outros participantes. Em menos de cinco minutos depois eu vi que tinha a possibilidade de ver essa informação, mas que infelizmente para mim é irrelevante.

Essa informação é desorganizada e não vai me ajudar a tomar uma decisão quanto aos meus palpites. Para mim é mais importante ter esses dados sumarizados de forma que eu não perca tempo analisando todos os palpites, um por um. Então, organizar a informação para que eu saiba quantos apostaram em 2x1 é mais interessante do que ter que contabilizar isso manualmente.

Para fazer isso em ruby eu utilizo a `gem nokogiri`, que faz justamente a análise e busca em html/xml. Com ela eu posso facilmente consumir o conteúdo de uma página e extrair os dados que realmente importa.

Nada muito complexo de se aprender, como você pode ver [aqui](http://gist.github.com/457000).

Eu pensei em fazer usando o Rails 3, mas depois de fazer o primeiro protótipo, vi que não seria necessária algo tão grande, então tirei o Rails e coloquei o Sinatra.

Para montar o gráfico eu perguntei no Twitter quem indicaria bibliotecas js que fizessem Chart. Depois de olhar todas, a do [Google Charts](http://code.google.com/apis/charttools/index.html) foi a que eu achei mais simples e sem burocracia para colocar no site.

E por fim, para fazer deploy da aplicação eu usei o Heroku, que é um serviço Cloud para aplicações em Ruby.
