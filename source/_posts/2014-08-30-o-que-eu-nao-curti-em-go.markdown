---
layout: post
title: "O que eu não curti em go"
date: 2014-08-30 07:12
comments: true
categories: 
---
O título desse artigo poderia ser "Minhas impressões sobre go". Mas não, esse tipo de artigo é fácil encontrar por aí :)

Enfim, das coisas que eu não curti em go, uma delas é a forma como você lida com atribuições de um resultado do banco de dados.

Por exemplo:

```
package main

import (
	"database/sql"
	"errors"
	"fmt"
	_ "github.com/lib/pq"
	"os"
)

type Category struct {
	Id       int64
	Name     string
	ParentId int64
}

const (
	FIND_SQL = `SELECT id, name, parent_id FROM categories WHERE id = $1`
)

func connection() *sql.DB {
	db, err := sql.Open("postgres", os.Getenv("DATABASE_URL"))
	if err != nil {
		fmt.Println("Error: ", err)
	}
	return db
}

func FindCategory(id int64) (Category, error) {
	db := connection()
	var parentId sql.NullInt64
	category := Category{}

	err := db.QueryRow(FIND_SQL, id).Scan(&category.Id, &category.Name, &parentId)

	if err != nil {
		fmt.Println("Error: ", err)
	}

	if parentId.Valid {
		category.ParentId = parentId.Int64
	}

	switch {
	case err == sql.ErrNoRows:
		return category, errors.New("Record Not Found")
	case err != nil:
		return category, errors.New(err.Error())
	}

	return category, nil
}

func main() {
	category, err := FindCategory(1)

	if err == nil {
		fmt.Println(category)
	}
}
```

No código acima, no seguinte trecho:

```
	if parentId.Valid {
		category.ParentId = parentId.Int64
	}
```

O que acontece é que após a consulta, eu preciso verificar se o `parentId` que é retornado na consulta é válido, se ele for, só aí eu atribuo o `category.ParentId`.

Esse tipo de tratamento, eu não curto.

Infelizmente go tem dessas, várias e várias vezes você precisa fazer esse tipo de verificação para poder continuar.

Uma outra coisa que eu não curto é a organização de arquivos do código em go, saca só:

![example](http://monosnap.com/image/TyjibjnO3s2rNQHxvsWhAGW5eL1J90.png)

Fica tudo dentro da raiz do projeto, não existe organização por diretórios. Para quem é acostumado com a estrutura de projetos em Rails é um martírio!

Que eu me lembre são esses pontos. Depois eu faço um artigo contando o que eu curto em go que sinto falta em outras linguagens.
