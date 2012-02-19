---
layout: post
title: Rails Initializers!
comments: true
---

No rails existem 4 lugares onde você pode configurar sua aplicação, no config/environment.rb, config/environments/production.rb, config/environments/development.rb, config/environments/test.rb.

Nesses 4 lugares você pode adicionar configurações para um do environments do rails (production, test e development) ou em todos os environments (config/environment.rb).

Dentro desses arquivos, você pode configurar gems específicas para cada environment, ou para toda a aplicação. Mas uma coisa que muitos fazem é centralizar nesses locais configurações de inicialização de constantes, de conexões com serviços externos, monkey patches entre outros.

Recentemente estou desenvolvendo um projeto e brincando com [Apache CouchDB](http://couchdb.apache.org) e [CouchRest](http://github.com/mattetti/couchrest/tree/master). No CouchRest eu resolvi fazer um monkey patch e na hora de colocar eu não sabia a melhor forma. Depois de um pouco de buscas no google lembrei do `config/initializers/`.

Nesse lugar é onde você deve colocar as coisas que vão ser inicializadas junto com o Rails. Todos os arquivos serão executados após o carregamento dos plugins.

Por exemplo, você pode inicializar o MemCached, fazer com que ele conecte nos 4 servidores de cache que você tem, limpar a cache e colocar na cache toda a sua aplicação.

Ou você pode fazer um monkey patch.

Assim você mantem seus environments limpos.
