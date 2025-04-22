---
title: "Cadastro de Produtos"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 503
draft: false
seo:
  title: "Cadastro de Produtos" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Os produtos exibidos no PinPDV devem ser previamente cadastrados na API do PinPDV e os mesmos devem estar ativos.

## Cadastro

O cadastro de um produto é realizado no endpoint `/produto`, informando o ID da empresa; Quando informada a imagem deve estar codificada em Base64.

```bash{title="Cadastro de Produto"}
curl --request POST \
  --url 'https://webapi.pinpdv.com.br/produto' \
  --header 'Authorization: Bearer XYZ' \
  --header 'Content-Type: application/json' \
  --data '[
    {
        "Identificador": "cod1",
        "Nome": "Coca Cola 350",
        "Preco": 2.90,
        "Imagem": "{ImagemEmBase64}"
    }
]'
```

```txt{title="Exemplo de Resposta"}
200 - OK

[
	{
		"id": 15,
		"nome": "Coca Cola 350",
		"descricao": null,
		"categoria": null,
		"marca": null,
		"modelo": null,
		"codigoBarras": null
	}
]
```

## Atualização

A atualização de um produto é realizada no endpoint `/produto`, informando o ID da empresa.

```bash{title="Atualização de Produto"}
curl --request PUT \
  --url 'https://webapi.pinpdv.com.br/produto' \
  --header 'Authorization: Bearer XYZ' \
  --header 'Content-Type: application/json' \
  --data '{
	"Identificador": "cod1",
	"Nome": "Coca Cola 350",
	"Preco": 2.90,
	"Imagem": "{ImagemEmBase64}"
}'
```

```txt{title="Exemplo de Resposta"}
200 - OK

[
	{
		"id": 15,
		"nome": "Coca Cola 350",
		"descricao": null,
		"categoria": null,
		"marca": null,
		"modelo": null,
		"codigoBarras": null
	}
]
```

## Desativar Produto

Caso deseje que um produto não seja exibido no PinPDV, não é necessário que seja realizada a sua exclusão, basta desativá-lo.

```bash{title="Desativar Produto"}
curl --request PUT \
  --url 'https://webapi.pinpdv.com.br/produto/{Identificador}/desativar' \
  --header 'Authorization: Bearer xyz' \
```

```txt{title="Exemplo de Resposta"}
202 - OK
```

## Ativar Produto

Para retornar a exibir um produto desativado, é necessário que seja realizada sua ativação informando o identificador.

```bash{title="Ativar Produto"}
curl --request PUT \
  --url 'https://webapi.pinpdv.com.br/produto/{Identificador}/ativar' \
  --header 'Authorization: Bearer xyz' \
```

```txt{title="Exemplo de Resposta"}
202 - OK
```

## Excluir Produto

Para excluir um produto é necessário informar seu identificador.

```bash{title="Excluir Produto"}
curl --request DELETE \
  --url 'https://webapi.pinpdv.com.br/produto/{Identificador}' \
  --header 'Authorization: Bearer XYZ' \
  --header 'Content-Type: application/json'
```

```txt{title="Exemplo de Resposta"}
202 - OK
```
