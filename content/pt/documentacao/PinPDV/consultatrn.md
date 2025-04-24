---
title: "Consulta de Transações"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 507
draft: false
seo:
  title: "Consulta de Transações" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Todas as transações realizadas por meio do PinPDV podem ser consultadas por meio da API do PinPDV. Existem diversos parâmetros que podem ser utilizados para ordenar, filtrar e controlar a paginação das consultas, a seguir estão os parâmetros disponíveis atualmente:

| Parâmetro             | Tipo / Valores Possíveis                                             | Descrição                                                                 |
|-----------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------|
| `OrdenarPor`          | `Id`, `Valor`, `Identificador`, `Comprovante`, `Status`, `PinPdv`, `TipoVenda`, `CadastradoEm`, `AtualizadoEm` | Campo pelo qual os resultados serão ordenados.                           |
| `OrdenarTipo`         | `ASC`, `DESC`                                                        | Define a direção da ordenação: ascendente (`ASC`) ou descendente (`DESC`).|
| `DataInicial`         | `YYYY-MM-DDTHH:MM:SS`                                                | Data e hora inicial para o filtro de período.                            |
| `DataFinal`           | `YYYY-MM-DDTHH:MM:SS`                                                | Data e hora final para o filtro de período.                              |
| `PaginaNumero`        | Número inteiro (ex: `1`)                                             | Número da página de resultados a ser retornada.                          |
| `QtdRegistro`         | Número inteiro (ex: `100`)                                           | Quantidade de registros por página.                                      |
| `FiltroComprovante`   | `true`, `false`                                                      | Filtra se a transação possui comprovante.                                |
| `FiltroTipoVenda`     | `Avulsa`, `Expressa`, `PreVenda`, `PosVenda`                         | Filtra pelo tipo da venda realizada.                                     |
| `FiltroPinPdv`        | `Código`                                                             | Filtra pelas transações associadas a um PinPDV específico.               |
| `FiltroStatus`        | `Realizada`, `Cancelada`, `Error`                                    | Filtra pelo status da transação.                                         |
| `FiltroTipoPagamento` | `Dinheiro`, `Credito`, `Debito`, `Pix`, `Crediario`                 | Filtra pelo meio de pagamento utilizado.                                 |


```bash {title="Consulta de Transações"}
curl --request GET \
  --url 'https://webapi.pinpdv.com.br/venda?DataInicial=2024-12-12T00:00:00&DataFinal=2024-12-12T23:59:59' \
  --header 'Authorization: Bearer xyz' \
```

```txt {title="Exemplo de Retorno"}
200 - OK
{
	"paginaAtual": 1,
	"itensPorPagina": 100,
	"quantidadeDePaginas": 1,
	"quantidadeTotalDeItens": 2,
	"primeiroRegistro": 1,
	"ultimoRegistro": 2,
	"paginaAnterior": false,
	"paginaProxima": false,
	"data": [
		{
			"id": 1002,
			"identificador": "68164940b8e411efb1804576f07d30fa",
			"tipoVenda": {
				"key": 0,
				"value": "Avulsa"
			},
			"valor": 23.0000,
			"comprovante": "****************************************\n*                                      *\n*             HOMOLOGAÇÃO              *\n*                                      *\n****************************************\n*                                      *\n*         ESSE DOCUMENTO NÃO           *\n*          TEM VALOR FISCAL            *\n*                                      *\n****************************************\n\nRECEBEMOS SEU PAGAMENTO:\n\n--> 68164940b8e411efb1804576f07d30fa\"\n\n### VALOR: 23\n### ITENS: 2\n\n\n     RETIRE SEU DOCUMENTO FISCAL        \n      NO BALCÃO DE ATENDIMENTO          \n\n\nhttp://www.sefazexemplo.gov.br/nfce/qrcode?p=28170800156225000131650110000151341562040824|2|1|1|DC6AE2C2B9A992BE59679AC365E29922DE6B7511\n\n\n\nOBRIGADO\nVOLTE SEMPRE!\n\n",
			"cadastradoEm": "2024-12-12T20:54:23.449928",
			"atualizadoEm": "2024-12-12T20:54:33.783144",
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"id": 1037,
					"valor": 23.0000,
					"parcelas": 1,
					"cadastradoEm": "2024-12-12T20:54:23.451749",
					"atualizadoEm": null,
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 1,
					"dados": null
				}
			],
			"produtos": [
				{
					"id": 16,
					"identificador": "bd11f408cc714c8eba069e8bb69a88de",
					"nome": "Hot Dog ??",
					"descricao": null,
					"quantidade": 1,
					"precoUnitario": 10.5000,
					"precoTotal": 10.5000
				},
				{
					"id": 17,
					"identificador": "1cd99ebf09864438adbb943ed8fbdc39",
					"nome": "Pastel",
					"descricao": null,
					"quantidade": 1,
					"precoUnitario": 12.5000,
					"precoTotal": 12.5000
				}
			],
			"preVenda": null,
			"posVenda": null,
			"pinPdv": {
				"codigo": "421730",
				"nome": "TESTE"
			}
		},
		{
			"id": 1003,
			"identificador": "32d312d0b8e511efb0c5abe3013abda6",
			"tipoVenda": {
				"key": 0,
				"value": "Avulsa"
			},
			"valor": 21.2500,
			"comprovante": "****************************************\n*                                      *\n*             HOMOLOGAÇÃO              *\n*                                      *\n****************************************\n*                                      *\n*         ESSE DOCUMENTO NÃO           *\n*          TEM VALOR FISCAL            *\n*                                      *\n****************************************\n\nRECEBEMOS SEU PAGAMENTO:\n\n--> 32d312d0b8e511efb0c5abe3013abda6\"\n\n### VALOR: 21.25\n### ITENS: 2\n\n\n     RETIRE SEU DOCUMENTO FISCAL        \n      NO BALCÃO DE ATENDIMENTO          \n\n\nhttp://www.sefazexemplo.gov.br/nfce/qrcode?p=28170800156225000131650110000151341562040824|2|1|1|DC6AE2C2B9A992BE59679AC365E29922DE6B7511\n\n\n\nOBRIGADO\nVOLTE SEMPRE!\n\n",
			"cadastradoEm": "2024-12-12T21:00:02.583575",
			"atualizadoEm": "2024-12-12T21:00:07.908576",
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"id": 1038,
					"valor": 21.2500,
					"parcelas": 1,
					"cadastradoEm": "2024-12-12T21:00:01.151783",
					"atualizadoEm": "2024-12-12T21:00:02.585883",
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 2,
					"dados": {
						"dataHora": "2024-12-12T21:00:01.151783",
						"nsu": "2024121200003377",
						"autorizacao": "000279",
						"bandeira": "VISA",
						"adquirente": "VERO"
					}
				}
			],
			"produtos": [
				{
					"id": 17,
					"identificador": "1cd99ebf09864438adbb943ed8fbdc39",
					"nome": "Pastel",
					"descricao": null,
					"quantidade": 1,
					"precoUnitario": 12.5000,
					"precoTotal": 12.5000
				},
				{
					"id": 41,
					"identificador": "4c6b53be5cff45b0b3a17b654a7e76c0",
					"nome": "Coca Cola Lata 350ml",
					"descricao": null,
					"quantidade": 1,
					"precoUnitario": 8.7500,
					"precoTotal": 8.7500
				}
			],
			"preVenda": null,
			"posVenda": null,
			"pinPdv": {
				"codigo": "421730",
				"nome": "TESTE"
			}
		}
	]
}
```
