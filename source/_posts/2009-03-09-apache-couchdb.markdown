---
layout: post
title: Apache CouchDB
---

Para quem é e participa do mundo Ruby On Rails não é nenhuma novidade, para quem já trabalha com banco de dados não relacionais também não é muita novidade, mas, para quem nunca teve contato ou nunca ouviu o que é: Apache Couch DB é um banco de dados distribuído, orientado a documentos e acessível por uma API RESTful HTTP/JSON.

Ele foi criado justamente para suprir a necessidade que a grande maioria dos banco de dados tem, que é a escalabidade. Uma palavra que faz toda a diferença entre ser um sucesso ou não. Escalabilidade o twitter não escala, Ruby On Rails não escala é um jargão que foi bastante usado no ínicio do Twitter, quando ele enfretou problemas de performance e outras coisas, principalmente por conta do banco de dados, que é o seu maior gargalo.

Mas não estou aqui para falar dos problemas enfretados pelo twitter e nem como eles foram resolvidos, por quê não sei se eles estão usando ou não o Apache CouchDB :P.

O grande lance do Apache CouchDB é que ele já foi criado para enfrentar esses problemas e foi pensado como um WebService, e isso faz toda uma diferença. Não existe colunas e campos definidos, você pode armazenar o que quiser em cada documento como se fosse uma linha de uma tabela não há uma definição para os campos dos documentos. Cada documento possui uma identidade única e os dados armazenados devem ser no formato JSON.

Como funciona como um WebService, você pode usar qualquer linguagem de programação para acessar o Apache CouchDB. É extremamente simples para você acessar e fazer o que quiser. Existem alguns projetos para tornar isso mais fácil, para Ruby existe o Stuffing, RelaxDB, CouchRest (escrito pelo criador do Apache CouchDB) e Couch Potato todos estão no GitHub.

Aqui no Brasil já existem empresas que estão usando o Apache CouchDB em produção. Um exemplo é a Boo-box, startup localizada em São Paulo, que usa juntamente com Merb.

A facilidade de uso e de escalabilidade faz do Apache CouchDB uma boa opção para o seu próximo projeto.
