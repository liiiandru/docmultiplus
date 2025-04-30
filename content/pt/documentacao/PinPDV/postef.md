---
title: "POS TEF"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 505
draft: false
seo:
  title: "POS TEF" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A execução de transações no modo POS TEF permite que o SmartPOS opere de forma similar a um Pinpad tradicional, proporcionando um fluxo de pagamento alinhado à experiência oferecida por uma TEF House.

## Cadastro da Venda

Para iniciar uma transação deve-se primeiramente definir qual dispositivo irá receber a transação (verificar informações comuns), em seguida realizar o cadastro no endpoint `/pos-venda`.

{{< callout note >}}
É obrigatório informar o valor da transação, o tipo de pagamento é opcional.

Informado o tipo de pagamento a quantidade de parcelas é obrigatória.
{{< /callout >}}

Deve ser observado que cabe ao sistema o critério de escolha do dispositivo que será utilizado, quando retornado pela consulta cada SmartPOS é listado com Nome, Código, data e hora da última atividade.

{{< callout note >}}  O SmartPOS é considerado ativo e a data/hora atualizada sempre que entrar no fluxo POS-TEF ou for para a página inicial do APP PinPDV. {{< /callout >}}

```bash {title="Cadastro de uma Venda"}
curl --request POST \
  --url 'https://webapi.pinpdv.com.br/pos-venda \
  --header 'Authorization: Bearer xyz' \
  --header 'Content-Type: application/json' \
  --data '{
    "PinPdvId": {pinpdvId},
    "Identificador": "vendaCf981239a",
    "Valor": 100.11,
    "Descricao": "mensagem",
    "TipoPagamento": 2,
    "Parcelas": 1
}'
```

```txt {title="Exemplo de Resposta"}
200 - OK

{
	"id": 61239
}
```

## Abortar uma venda

Uma venda pode ser abortada enquanto o PinPDV não iniciar a transação. Para realizar o cancealmento deve ser utilizado o endpoint `pos-venda` e o identificador fornecido pelo sistema na criação da venda.

```bash {title="Abortar uma Venda"}
curl --request DELETE \
  --url 'https://webapi.pinpdv.com.br/pos-venda/{Identificador}' \
  --header 'Authorization: Bearer xyz' \
```

```txt {title="Exemplo de Resposta"}
202 - OK
```

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
Uma vez iniciada a venda no App PinPDV, somente será possível cancelar a transação através dele.
{{< /callout >}}

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
Não sendo possível realizar o cancelamento da venda através do App (ex: Ocorreu um travamento e o App fechou), é possível realizar o cancelamento de forma forçada, utilizando o parâmetro `forca=true`.

```bash {title="Abortar uma Venda Forçadamente"}
curl --request DELETE \
  --url 'https://webapi.pinpdv.com.br/pos-venda/{Identificador}?forca=true' \
  --header 'Authorization: Bearer xyz' \
```
{{< /callout >}}

## Consulta de uma Venda

Para consultar o status de uma venda é utilizado o endpoint `/pos-venda` e o identificador fornecido pelo sistema.

{{< callout note >}}
Verifique o campo venda para obter os dados do pagamento.
{{< /callout >}}

```bash {title="Consulta de uma Venda"}
curl --request GET \
  --url 'https://webapi.pinpdv.com.br/pos-venda/{Identificador}' \
  --header 'Authorization: Bearer xyz' \
```

```txt {title="Exemplo de Resposta"}
200 - OK
{
	"id": 10,
	"identificador": "Id010101",
	"descricao": "Exemplo",
	"valor": 789.0000,
	"parcelas": 1,
	"tipoPagamento": 3,
	"status": {
		"key": 2,
		"value": "Concluido"
	},
	"vendas": [
		{
			"identificador": "90672461828311ef8e6ca79b7a6aaeae",
			"posVendaIdentificador": "Id010101",
			"valor": 789.0000,
			"comprovante": null,
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"valor": 789.0000,
					"parcelas": 0,
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 3,
					"dados": {
						"dataHora": "2024-10-04T16:05:04.679023",
						"nsu": "000000153",
						"autorizacao": "164550",
						"bandeira": "MASTERCARD",
						"adquirente": "GETNET"
					}
				}
			],
			"pinPdv": {
				"codigo": "526989",
				"nome": "CAIXA01"
			}
		}
	]
}
```
