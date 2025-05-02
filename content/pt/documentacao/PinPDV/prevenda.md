---
title: "Pré-venda"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 504
draft: false
seo:
  title: "Pré-venda" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
A funcionalidade de Pré-Venda permite que operações de venda sejam iniciadas e registradas previamente, antes de sua finalização no App PinPDV. Essa funcionalidade é especialmente útil para cenários onde o atendimento começa no sistema e é finalizado fora do caixa (ex: Delivery, abastecimentos, etc.).

No PinPDV, serão exibidas todas as pré-vendas previamente cadastradas na API PinPDV e que ainda não tenham sido concluídas. Assim, o operador pode visualizar, selecionar e finalizar a venda de maneira rápida e eficiente.

## Cadastro

O cadastro de uma pré-venda é realizado no endpoint `/pre-venda` informando o ID da empresa. É opcional informar tipo de pagamento, número de parcelas e produtos.

```bash{title="Cadastro de Pré-venda"}
curl --request POST \
  --url 'https://webapi.pinpdv.com.br/pre-venda' \
  --header 'Authorization: Bearer xyz' \
  --header 'Content-Type: application/json' \
  --data '{
    "Identificador": "{identificador}",
    "Valor": 34.83,
    "Descricao": "teste 1",
    "Parcelas": 1,
    "TipoPagamento": 3,
    "PinPdvId": {pinpdvId},
    "Produtos": [
        {
            "Identificador": "{identificadorProduto}",
            "Quantidade": 1
        }
    ]
}'
```

```txt{title="Resposta"}
200 - OK

{
	"id": 8240
}
```

## Consulta de Pré-venda

Para realizar a consulta do status de uma pré-venda é necessário informar o identificador utilizado pelo sistema no cadastro da mesma.

```bash{title="Consulta de Pré-venda"}
curl --request GET \
  --url 'https://webapi.pinpdv.com.br/pre-venda/{identificadorSistema}' \
  --header 'Authorization: Bearer xyz' \
```

```txt{title="Exemplo de Resposta - Status Aguardando"}
200 - OK

{
	"id": 1,
	"identificador": "d858ee21dac64f71a10cd4a22fa1d80a",
	"descricao": "Mesa 01",
	"valor": 34.8300,
	"parcelas": 1,
	"tipoPagamento": 3,
	"status": {
		"key": 0,
		"value": "Aguardando"
	},
	"produtos": [
		{
			"id": 0,
			"identificador": "Coca01",
			"nome": "Coca Cola Lata",
			"descricao": null,
			"categoria": null,
			"marca": null,
			"modelo": null,
			"codigoBarras": null,
			"quantidade": 2,
			"precoUnitario": 10.0000,
			"precoTotal": 10.0000,
			"preco": 0,
			"isAtivo": false,
			"imagem": null
		}
	],
	"vendas": []
}
```

```txt{title="Exemplo de Resposta - Status Concluída"}
200 - OK

{
	"id": 1,
	"identificador": "d858ee21dac64f71a10cd4a22fa1d80a",
	"descricao": "Mesa 01",
	"valor": 34.8300,
	"parcelas": 1,
	"tipoPagamento": 3,
	"status": {
		"key": 0,
		"value": "Aguardando"
	},
	"produtos": [
		{
			"id": 0,
			"identificador": "Coca01",
			"nome": "Coca Cola Lata",
			"descricao": null,
			"categoria": null,
			"marca": null,
			"modelo": null,
			"codigoBarras": null,
			"quantidade": 2,
			"precoUnitario": 10.0000,
			"precoTotal": 10.0000,
			"preco": 0,
			"isAtivo": false,
			"imagem": null
		}
	],
	"vendas": [
		{
			"identificador": "44142e20828111ef8e6ca79b7a6aaeae",
			"preVendaIdentificador": "f99f8d8ce3354caca3c0cae660d14821",
			"valor": 34.83,
			"comprovante": null,
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"valor": 34.83,
					"parcelas": 1,
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 3,
					"dados": {
						"dataHora": "2024-10-04T15:48:37.635649",
						"nsu": "000000151",
						"autorizacao": "164550",
						"bandeira": "MASTERCARD",
						"adquirente": "GETNET"
					}
				}
			],
			"pinPdv": {
				"codigo": "526989",
				"nome": "CAIXA 01"
			}
		}
	]
}
```

## Impressão

Quando finalizada a venda será disponibilizado o documento para a impressão; verifique a seção sobre impressão de documentos.


{{< link-card
  title="Impressão de Documentos"
  description="Mais detalhes"
  href="/pt/documentacao/pinpdv/infocomuns/#impressão-de-documentos"
  target="_blank"
>}}
