---
layout: post
title: Instalando Nginx com Passenger
---

Para quem acompanha o mundo [Ruby On Rails](http://www.rubyonrails.com) soube que no dia 16 foi [lançado](http://blog.phusion.nl/2009/04/16/phusions-one-year-anniversary-gift-phusion-passenger-220) o [Passenger](http://www.modrails.com) com suporte total para [Nginx](http://nginx.net) (Lê-se `Engine X`). Assim temos mais uma rápida opção de deployment para Ruby On Rails.

Para instalar, primeiro instala a gem do Passenger

`sudo gem install passenger`

O Passenger gera o instalador fácil para Nginx e para o Apache, no nosso caso só digitarmos

`sudo passenger-install-nginx-module`

Nessa parte eu estava obtendo um erro e não estava instalando o Nginx e [aqui](http://github.com/FooBarWidget/passenger/commit/7a9092a560de4f2096a73fc39375260f827f6153) [nessa](http://code.google.com/p/phusion-passenger/issues/detail?id=249) [thread](http://groups.google.com/group/phusion-passenger/browse_thread/thread/cbf0ed3f143e01fc) os Phusion Guys resolveram o problema. Então eu fiz um clone do [projeto](http://github.com/FooBarWidget/passenger/) e rodei um `rake package` para gerar a gem com a modificação e fiz a instalação da gem e tudo foi resolvido.

Após isso é só configurar o Nginx, que por padrão é instalado em `/opt/nginx`. Para fazer o deploy é simples, adicione o seguinte código em `/opt/nginx/conf/nginx.conf` (altere a pasta conforme onde você instalou o Nginx)

```nginx
server {
   listen 80;
   server_name localhost;
   root /var/www/aplicacao
   passenger_enabled on;
}
```

Para completar, eu fiz um script onde posso iniciar, parar ou reiniciar o servidor

```bash
#! /bin/sh
 
### BEGIN INIT INFO
# Provides:          nginx
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO
 
#Alter to nginx path
NGINXPATH=/opt/nginx

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=$NGINXPATH/sbin/nginx
NAME=nginx
DESC=nginx
 
test -x $DAEMON || exit 0
 
# Include nginx defaults if available
if [ -f /etc/default/nginx ] ; then
        . /etc/default/nginx
fi
 
set -e
 
case "$1" in
  start)
        echo -n "Starting $DESC: "
        start-stop-daemon --start --quiet --pidfile $NGINXPATH/logs/$NAME.pid --exec $DAEMON -- $DAEMON_OPTS
        echo "$NAME."
        ;;
  stop)
        echo -n "Stopping $DESC: "
        start-stop-daemon --stop --quiet --pidfile $NGINXPATH/logs/$NAME.pid --exec $DAEMON
        echo "$NAME."
        ;;
 
  restart|force-reload)
        echo -n "Restarting $DESC: "
        start-stop-daemon --stop --quiet --pidfile $NGINXPATH/logs/$NAME.pid --exec $DAEMON
        sleep 1
        start-stop-daemon --start --quiet --pidfile $NGINXPATH/logs/$NAME.pid --exec $DAEMON -- $DAEMON_OPTS
        echo "$NAME."
        ;;
  reload)
      echo -n "Reloading $DESC configuration: "
      start-stop-daemon --stop --signal HUP --quiet --pidfile $NGINXPATH/logs/$NAME.pid --exec $DAEMON
      echo "$NAME."
      ;;
  *)
        N=/etc/init.d/$NAME
        echo "Usage: $N {start|stop|restart|reload|force-reload}" >&2
        exit 1
        ;;
esac
 
exit 0
```

Você pode colocar a inicialização automaticamente no servidor

`sudo update-rc.d nginx defaults`

Depois é só inicializar o servidor e pronto.
