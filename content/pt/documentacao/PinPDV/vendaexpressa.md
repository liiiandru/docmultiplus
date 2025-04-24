---
title: "Venda Expressa"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 506
draft: false
seo:
  title: "Venda Expressa" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

As vendas realizadas no SmartPOS através da função **Venda Expressa**, são possíveis de serem consultadas através da API do PinPDV.

## Consulta de Venda Expressa

Para consultar as vendas realizadas no PinPDV deve ser filtrado somente pelo tipo **Venda Expressa**.

```txt {title="Filtros Disponíveis"}
OrdenarPor = (Id, Valor, Identificador, Comprovante, Status, PinPdv, TipoVenda,
CadastradoEm, AtualizadoEm)
OrdenarTipo = (ASC, DESC)
DataInicial=YYYY-MM-DDTHH:MM:SS
DataFinal=YYYY-MM-DDTHH:MM:SS
PaginaNumero=1
QtdRegistro=100
Filtros (
  FiltroComprovante={true,false},
  FiltroPinPdv={Codigo}
  FiltroStatus={Realizada, Cancelada, Error}
  FiltroTipoPagamento={Credito, Debito, Pix, Crediario}
)
```

```bash {title="Consulta de Transação"}
curl --request GET \
  --url 'https://webapi.pinpdv.com.br/venda?DataInicial=2024-12-12T00:00:00&DataFinal=2024-12-12T23:59:59&FiltroTipoVenda=Expressa' \
  --header 'Authorization: Bearer xyz' \
```

```json {title="Exemplo de Resposta"}
200 - OK
{
	"paginaAtual": 1,
	"itensPorPagina": 100,
	"quantidadeDePaginas": 1,
	"quantidadeTotalDeItens": 3,
	"primeiroRegistro": 1,
	"ultimoRegistro": 3,
	"paginaAnterior": false,
	"paginaProxima": false,
	"data": [
		{
			"id": 1004,
			"identificador": "035c6370b8e611ef9d282f9fae714dd9",
			"tipoVenda": {
				"key": 1,
				"value": "Expressa"
			},
			"valor": 15.0000,
			"comprovante": "****************************************\n*                                      *\n*             HOMOLOGAÇÃO              *\n*                                      *\n****************************************\n*                                      *\n*         ESSE DOCUMENTO NÃO           *\n*          TEM VALOR FISCAL            *\n*                                      *\n****************************************\n\nRECEBEMOS SEU PAGAMENTO:\n\n--> 035c6370b8e611ef9d282f9fae714dd9\"\n\n### VALOR: 15\n### ITENS: 0\n\n\n     RETIRE SEU DOCUMENTO FISCAL        \n      NO BALCÃO DE ATENDIMENTO          \n\n\nhttp://www.sefazexemplo.gov.br/nfce/qrcode?p=28170800156225000131650110000151341562040824|2|1|1|DC6AE2C2B9A992BE59679AC365E29922DE6B7511\n\n\n\nOBRIGADO\nVOLTE SEMPRE!\n\n",
			"cadastradoEm": "2024-12-12T21:05:56.113478",
			"atualizadoEm": null,
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"id": 1039,
					"valor": 15.0000,
					"parcelas": 1,
					"cadastradoEm": "2024-12-12T21:05:51.154713",
					"atualizadoEm": "2024-12-12T21:05:56.115344",
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 3,
					"dados": {
						"dataHora": "2024-12-12T21:05:51.154713",
						"nsu": "2024121200003394",
						"autorizacao": "001425",
						"bandeira": "VISA",
						"adquirente": "VERO"
					}
				}
			],
			"produtos": [],
			"preVenda": null,
			"posVenda": null,
			"pinPdv": {
				"codigo": "421730",
				"nome": "NOME"
			}
		},
		{
			"id": 1005,
			"identificador": "f40f5890b8e611efbb406b5cca06cb38",
			"tipoVenda": {
				"key": 1,
				"value": "Expressa"
			},
			"valor": 3.0000,
			"comprovante": "****************************************\n*                                      *\n*             HOMOLOGAÇÃO              *\n*                                      *\n****************************************\n*                                      *\n*         ESSE DOCUMENTO NÃO           *\n*          TEM VALOR FISCAL            *\n*                                      *\n****************************************\n\nRECEBEMOS SEU PAGAMENTO:\n\n--> f40f5890b8e611efbb406b5cca06cb38\"\n\n### VALOR: 3\n### ITENS: 0\n\n\n     RETIRE SEU DOCUMENTO FISCAL        \n      NO BALCÃO DE ATENDIMENTO          \n\n\nhttp://www.sefazexemplo.gov.br/nfce/qrcode?p=28170800156225000131650110000151341562040824|2|1|1|DC6AE2C2B9A992BE59679AC365E29922DE6B7511\n\n\n\nOBRIGADO\nVOLTE SEMPRE!\n\n",
			"cadastradoEm": "2024-12-12T21:12:39.893618",
			"atualizadoEm": null,
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"id": 1040,
					"valor": 3.0000,
					"parcelas": 1,
					"cadastradoEm": "2024-12-12T21:12:34.969081",
					"atualizadoEm": "2024-12-12T21:12:39.896144",
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 2,
					"dados": {
						"dataHora": "2024-12-12T21:12:34.969081",
						"nsu": "2024121200003413",
						"autorizacao": "002751",
						"bandeira": "VISA",
						"adquirente": "VERO"
					}
				}
			],
			"produtos": [],
			"preVenda": null,
			"posVenda": null,
			"pinPdv": {
				"codigo": "421730",
				"nome": "TESTE"
			}
		},
		{
			"id": 1006,
			"identificador": "8cf77b00b8e711efabfa4d65d86230a8",
			"tipoVenda": {
				"key": 1,
				"value": "Expressa"
			},
			"valor": 8.0000,
			"comprovante": "****************************************\n*                                      *\n*             HOMOLOGAÇÃO              *\n*                                      *\n****************************************\n*                                      *\n*         ESSE DOCUMENTO NÃO           *\n*          TEM VALOR FISCAL            *\n*                                      *\n****************************************\n\nRECEBEMOS SEU PAGAMENTO:\n\n--> 8cf77b00b8e711efabfa4d65d86230a8\"\n\n### VALOR: 8\n### ITENS: 0\n\n\n     RETIRE SEU DOCUMENTO FISCAL        \n      NO BALCÃO DE ATENDIMENTO          \n\n\nhttp://www.sefazexemplo.gov.br/nfce/qrcode?p=28170800156225000131650110000151341562040824|2|1|1|DC6AE2C2B9A992BE59679AC365E29922DE6B7511\n\n\n\nOBRIGADO\nVOLTE SEMPRE!\n\n",
			"cadastradoEm": "2024-12-12T21:16:56.511982",
			"atualizadoEm": null,
			"status": {
				"key": 0,
				"value": "Realizada"
			},
			"transacoes": [
				{
					"id": 1041,
					"valor": 8.0000,
					"parcelas": 1,
					"cadastradoEm": "2024-12-12T21:16:51.511809",
					"atualizadoEm": "2024-12-12T21:16:56.514429",
					"status": {
						"key": 0,
						"value": "Aprovada"
					},
					"tipoPagamento": 3,
					"dados": {
						"dataHora": "2024-12-12T21:16:51.511809",
						"nsu": "2024121200003420",
						"autorizacao": "003590",
						"bandeira": "VISA",
						"adquirente": "VERO"
					}
				}
			],
			"produtos": [],
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
