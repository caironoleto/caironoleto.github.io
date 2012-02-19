---
layout: post
title: Instalando Apache CouchDB no Linux
comments: true
---

Para instalar o [Apache CouchDB](http://couchdb.apache.org/) no Ubuntu/Debian-likes use

`sudo apt-get install couchdb`

Mas a versão que ele instala (pelo menos no Ubuntu 8.10) é uma versão um pouco mais antiga, então resolvi instalar via source

```bash
wget [http://linorg.usp.br/apache/couchdb/0.9.0/apache-couchdb-0.9.0.tar.gz](http://linorg.usp.br/apache/couchdb/0.9.0/apache-couchdb-0.9.0.tar.gz)
tar zxvf apache-couchdb-0.9.0.tar.gz
cd apache-couchdb-0.9.0
```

Antes de instalar, você deve instalar as dependências

`sudo apt-get install build-essential erlang libicu38 libicu-dev libmozjs-dev libcurl4-openssl-dev`

Após isso a coisa fica fácil :)

```bash
./configure
make
sudo make install
```

Ainda não está finalizado, você deve adicionar um usuário para iniciar o couchdb

`sudo adduser --system --home /usr/local/var/lib/couchdb --no-create-home --shell /bin/bash --group --gecos "CouchDB Administrator" couchdb`

Você deve dar permissões a algumas pastas

```bash
sudo chown -R couchdb /usr/local/etc/couchdb
sudo chown -R couchdb /usr/local/var/lib/couchdb
sudo chown -R couchdb /usr/local/var/log/couchdb
sudo chown -R couchdb /usr/local/var/run/
```

Pronto, agora está instalado, você pode inicializar o Apache CouchDB com

`sudo -i -u couchdb couchdb -b`

"Apache CouchDB has started, time to relax" ;) assim você inicia o CouchDB manualmente, se você quiser deixar ele iniciando automaticamente, você pode fazer

```bash
sudo ln -s /usr/local/etc/init.d/couchdb /etc/init.d/
sudo update-rc.d couchdb defaults
```

Agora você pode digitar `sudo /etc/init.d/couchdb {start|stop|restart|force-reload|status}` e facilmente você inicializará ou finalizará o Apache CouchDB, para você ver seu Apache CouchDB funcionando, [clique aqui](http://127.0.0.1:5984/_utils/index.html).
