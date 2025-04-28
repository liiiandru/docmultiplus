---
title: "Transações de POS"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 904
seo:
  title: "Transações de POS" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
As transações realizadas por meio dos dispositivos POS da Multiplus Pay ou dos aplicativos da Multiplus Card podem ser consultadas de forma detalhada. A consulta permite, ainda, a obtenção dos comprovantes associados, possibilitando sua utilização em processos de geração de documentos fiscais.

A seguir, são descritos os processos de consulta e extração de dados de transações provenientes dessas plataformas.

## Consulta de Transações POS

A consulta utiliza os mesmos parâmetros já descritos anteriormente, clique abaixo para verificar.

{{< link-card
  title="Parâmetros da Consulta"
  href="/pt/documentacao/transacao/consulta"
  target="_blank"
>}}

```bash {title="Exemplo de Consuta - Transação POS"}
curl --location 'https://webapi.relatoriotef.com.br/transacao/pos?cnpj=05625872000169&
DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59' \
--header 'Authorization: Bearer ••••••'
```

## Detalhes da Transação

Para consultar os detalhes de uma transação é necessário informar o código da transação desejada `ID`.

```bash {title="Exemplo de Consulta - Detalhes da Transação"}
curl --location 'https://webapi.relatoriotef.com.br/transacao/{id}' \
--header 'Authorization: Bearer ••••••'
```

```json {title="Exemplo de Detalhe"}
{
   "comprovante":"DEMONSTRACAO CONCILIADOR PLUS - MULTIPLUS CARD\n           48.999.979/0001-80           \n                REDECARD                \n                CREDITO                 \nValor R$: 8.76\nStatus: Aprovada (00)\nData/Hora ADM: 16/04/2025 11:06:54\nTerminal: 49051687\nCartao: 376464XXXXX5910\nAUTORIZACAO: 984566\nNSU: 076658371\nNSU TEF: 076658\n\n",
   "id":977877869,
   "cnpj":"48.999.979/0001-80",
   "dataMensagem":"2025-04-16",
   "dataEstabelecimento":"2025-04-16T11:06:54",
   "dataAdministradora":"2025-04-16T11:06:54",
   "dataServer":"2025-04-16T11:06:54",
   "dataComprovante":null,
   "valor":8.76,
   "valorTaxa":0,
   "parcela":1,
   "pdv":"49051687",
   "origem":"CONCILIADOR",
   "cartao":{
      "numero":"376464XXXXX5910",
      "vencimento":""
   },
   "formaPgto":2,
   "status":{
      "id":2,
      "valor":"00"
   },
   "codigoResposta":"00",
   "mensagem":null,
   "nsu":"076658",
   "nsuCompleto":"076658371",
   "nsuExpandido":null,
   "nsuHost":"076658371",
   "pagamentoId":null,
   "autorizacao":"984566",
   "codigoRefId":null,
   "isSplit":false,
   "isSplitPrimario":false,
   "isSplitHeader":null,
   "empresa":{
      "id":27878,
      "nome":"DEMONSTRACAO CONCILIADOR PLUS - MULTIPLUS CARD",
      "razao":"DEMONSTRACAO CONCILIADOR PLUS - MULTIPLUS CARD",
      "cnpj":"48.999.979/0001-80"
   },
   "adquirente":{
      "id":45011,
      "nome":"REDECARD"
   },
   "bandeira":{
      "id":114165,
      "nome":"AMEX"
   },
   "servico":{
      "id":233,
      "nome":"CREDITO"
   },
   "grupoServico":{
      "id":59,
      "nome":"CREDITO"
   },
   "splits":[

   ],
   "acoes":{
      "pendente":false,
      "solicitadoAlteracaoStatus":false
   }
}
```

## Comprovante da Transação

Para obter o comprovante de uma transação é semelhante à consulta de detalhes, sendo necessário informar o código da transação `ÌD`.

O comprovante é retornado em uma string com a formatação necessária para a impressão.

```bash {title="Exemplo de Consulta - Comprovante da Transação"}
curl --location 'https://webapi.relatoriotef.com.br/transacao/{id}/comprovante' \
--header 'Authorization: Bearer ••••••'
```

```txt {title="Exemplo de Retorno do Comprovante"}
DEMONSTRACAO CONCILIADOR PLUS - MULTIPLUS CARD
           48.999.979/0001-80
                REDECARD
                CREDITO
Valor R$: 8.76
Status: Aprovada (00)
Data/Hora ADM: 16/04/2025 11:06:54
Terminal: 49051687
Cartao: 376464XXXXX5910
AUTORIZACAO: 984566
NSU: 076658371
NSU TEF: 076658

```

{{< callout note >}}
Após a realização da requisição para obtenção do comprovante, a resposta retornará os dados do comprovante, incluindo a indicação de que se trata de uma **reimpressão**.
{{< /callout >}}
