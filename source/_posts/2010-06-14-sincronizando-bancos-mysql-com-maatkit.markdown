---
layout: post
title: Sincronizando bancos MySQL com Maatkit
---

Maatkit é uma ferramenta criada para DBAs, programadores e usuários que lidam com bancos de dados opensource em replicação master-master e master-slave.

A maioria das ferramentas foi feita para o MySQL, mas você pode utilizar em seu banco de dados preferido (Não sei quais seriam esses outros bancos :P).

Uma das ferramentas do Maatkit é a sincronização entre bancos de dados. Ela é extremamente útil quando você tem bancos de dados MySQL replicados, sendo ele na configuração master-master ou master-slave.

Após instalado (debian like é apt-get install maatkit) você pode fazer o seguinte comando:

`mk-table-sync --execute --synctomaster --verbose`

`mk-table-sync` é um dos executáveis que ele instala junto com vários outros.

Para sincronizar dois bancos sem ser master-slave você pode fazer da seguinte forma:

`mk-table-sync --execute u=user,p=pass,h=host1,D=db,t=tbl host2`

Se você que mais detalhes e ver os outros executáveis, visite o [site do projeto](http://www.maatkit.org).
