---
title: "Consulta de Transações"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 903
seo:
  title: "Consulta de Transações" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Após realizar a autenticação, as transações podem ser consultadas com os parâmetros descritos a seguir:

| Parâmetro          | Obrigatório | Descrição                                                                                         |
|--------------------|-------------|---------------------------------------------------------------------------------------------------|
| Cnpj               | Sim         | CNPJ do solicitante. Exemplo: `05625872000169`.                                                    |
| DataInicial        | Sim         | Data e hora de início do período de consulta. Formato: `YYYY-MM-DDTHH:MM:SS`.                      |
| DataFinal          | Sim         | Data e hora final do período de consulta (máximo 30 dias entre DataInicial e DataFinal).           |
| PaginaNumero       | Não         | Número da página para paginação dos resultados. Exemplo: `1`.                                      |
| QtdRegistro        | Não         | Quantidade de registros por página (máximo 100 registros).                                         |
| PDV                | Não         | Filtro por PDV. Pode enviar até 5 registros. Exemplo: `PDV=1&PDV=2`.                              |
| Adquirente         | Não         | Filtro por adquirente. Pode enviar até 5 registros. Exemplo: `Adquirente=1&Adquirente=2`.          |
| Bandeira           | Não         | Filtro por bandeira. Pode enviar até 5 registros. Exemplo: `Bandeira=1&Bandeira=2`.                |
| GrupoServico       | Não         | Filtro por grupo de serviço. Pode enviar até 5 registros. Exemplo: `GrupoServico=1&GrupoServico=2`.|
| Servico            | Não         | Filtro por serviço. Pode enviar até 5 registros. Exemplo: `Servico=1&Servico=2`.                   |
| Status             | Não         | Filtro por status. Pode enviar até 5 registros. Exemplo: `Status=1&Status=2`.                      |
| Autorizacao        | Não         | Filtro por autorização. Pode enviar até 5 registros. Exemplo: `Autorizacao=1&Autorizacao=2`.       |
| NSU                | Não         | Filtro por NSU. Pode enviar até 5 registros. Exemplo: `NSU=1&NSU=2`.                               |
| RetornaSplit       | Não         | Define se o retorno deve incluir os dados de divisão (`True` para ativar).                         |
| RetornaComprovante | Não         | Define se o retorno deve incluir o comprovante no modelo padrão (`True` para ativar).              |

```bash {title="Exemplo de Consulta de Transações"}
curl --location 'https://webapi.relatoriotef.com.br/transacao?cnpj=05625872000169&
DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59' \
--header 'Authorization: Bearer ••••••'
```

Em caso de sucesso os dados serão retornados da seguinte forma:

```json {title="Exemplo de Retorno"}
{
  "resumo": {
    "todas": {
      "quantidade": 4,
      "valorTotal": 320.50
    },
    "aprovadas": {
      "quantidade": 4,
      "valorTotal": 320.50
    },
    "pendentes": {
      "quantidade": 0,
      "valorTotal": 0
    },
    "negadas": {
      "quantidade": 0,
      "valorTotal": 0
    },
    "canceladas": {
      "quantidade": 0,
      "valorTotal": 0
    },
    "desfeitas": {
      "quantidade": 0,
      "valorTotal": 0
    }
  },
  "transacoes": [
    {
      "id": 488877999,
      "cnpj": "12.345.678/0001-90",
      "dataMensagem": "2025-04-27",
      "dataEstabelecimento": "2025-04-27T14:30:00",
      "dataAdministradora": "2025-04-27T14:30:00",
      "dataServer": "2025-04-27T14:30:00",
      "dataComprovante": null,
      "valor": 80.12,
      "valorTaxa": 1.50,
      "parcela": 1,
      "pdv": "12345678",
      "origem": "CONCILIADOR",
      "cartao": {
        "numero": "512345XXXXX6789",
        "vencimento": "12/26"
      },
      "formaPgto": 1,
      "status": {
        "id": 2,
        "valor": "00"
      },
      "codigoResposta": "00",
      "mensagem": null,
      "nsu": "123456",
      "nsuCompleto": "123456789",
      "nsuExpandido": null,
      "nsuHost": "123456789",
      "pagamentoId": null,
      "autorizacao": "654321",
      "codigoRefId": null,
      "isSplit": false,
      "isSplitPrimario": false,
      "isSplitHeader": null,
      "empresa": {
        "id": 98765,
        "nome": "EMPRESA DEMO LTDA",
        "razao": "EMPRESA DEMO LTDA",
        "cnpj": "12.345.678/0001-90"
      },
      "adquirente": {
        "id": 55022,
        "nome": "CIELO"
      },
      "bandeira": {
        "id": 114200,
        "nome": "VISA"
      },
      "servico": {
        "id": 300,
        "nome": "CREDITO"
      },
      "grupoServico": {
        "id": 60,
        "nome": "CREDITO"
      },
      "splits": null,
      "acoes": {
        "pendente": false,
        "solicitadoAlteracaoStatus": false
      }
    }
  ]
}
```

Em caso de erro será retornada nessa estrutura:

```json{title="Exemplo de Retorno com Erro"}
{
  "Title": "Ocorreram uma ou mais falhas de validação.",
  "Status": 422,
  "Detail": "'Data Final' não atende a condição definida.",
  "Errors": {
    "DataFinal": [
      "'Data Final' não atende a condição definida."
    ]
  }
}
```
