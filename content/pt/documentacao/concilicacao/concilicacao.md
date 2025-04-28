---
title: "Conciliação"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 703
seo:
  title: "Conciliação" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A Conciliação e Pré-Conciliação compartilham alguns enpoints, nestes estarão identificados o praâmetro necessário para obter a inbformação desejada.

## Resumo - Conciliação/Pré-Conciliação

Parametros da requisição:

| Parâmetro    | Valor/Descrição                                              |
|--------------|--------------------------------------------------------------|
| Cnpj         | 05625872000169 *(somente números)*                           |
| DataInicial  | 2024-01-01T00:00:00                                          |
| DataFinal    | 2024-01-01T23:59:59 *(Range máximo entre as datas: 90 dias)* |
| DataTipo     | 0 : Pré-Conciliação - Data Venda                             |
|              | 1 : Pré-Conciliação - Data Previsão                          |
|              | 2 : Conciliação - Data do Agendamento                        |
|              | 3 : Conciliação - Data do Pagamento                          |


```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/total?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59
&DataTipo=3' \
--header 'Authorization: Bearer ••••••'
```

```json {title="Exemplo de Retorno"}
{
    "vendaQtde": 1913,              //Quantidade de venda única

    //Pré-Conciliação
    "preTotalBruto": 0.00,          //Total bruto
    "preTotalLiquido": 0.00,        //Total líquido
    "preTotalDeposito": 0.00,       //Total aprox. depósito
    "preTotalDespesa": 0.00,        //Total despesa

    //Conciliação
    "concTotalBruto": 0.00,         //Total bruto
    "concTotalLiquido": 0.00,       //Total líquido
    "concTotalDeposito": 0.00,      //Total aprox. depósito
    "concTotalConciliacao": 0.00,   //Total depósito bancário
    "concTotalDiferenca": 0.00,     //Diferença
    "concTotalPgtoAvulso": 0.00,    //Total pgto. avulso
    "concTotalDespesa": 0.00        //Total despesa
}
```

## Vendas - Conciliação/Pré-Conciliação

Parâmetros da Requisição:

| Campo             | Descrição / Exemplo                                                   |
|-------------------|------------------------------------------------------------------------|
| Cnpj              | 05625872000169 *(somente números)*                                     |
| DataInicial       | 2024-01-01T00:00:00                                                    |
| DataFinal         | 2024-01-01T23:59:59 *(Intervalo máximo entre datas: 90 dias)*         |
| DataTipo          | Tipo de data utilizada para filtro                                     |
|                   | 0: Pré-Conciliação - Data da Venda                                     |
|                   | 1: Pré-Conciliação - Data de Previsão                                  |
|                   | 2: Conciliação - Data de Agendamento                                   |
|                   | 3: Conciliação - Data de Pagamento                                     |
| QtdRegistro       | Quantidade de registros por página                                     |
| PaginaNumero      | Número da página                                                       |
| OrdenarPor        | Campo pelo qual os resultados serão ordenados                          |
| OrdenarTipo       | Tipo de ordenação: `ASC` (crescente) ou `DESC` (decrescente)          |
| Codigo            | Código da transação ou referência                                      |
| DataVenda         | Data da venda                                                          |
| AdquirenteNome    | Nome da adquirente (ex: Cielo, Rede, Stone)                           |
| BandeiraNome      | Nome da bandeira do cartão (ex: Visa, Mastercard)                      |
| ServicoNome       | Nome do serviço associado (ex: Crédito, Débito, Voucher)              |
| Parcela           | Número da parcela da transação                                         |
| PreTaxaValor      | Valor da taxa na pré-conciliação                                       |
| PreValorBruto     | Valor bruto na pré-conciliação                                         |
| PreValorLiquido   | Valor líquido na pré-conciliação                                       |
| ConcTaxaValor     | Valor da taxa na conciliação                                           |
| ConcValorBruto    | Valor bruto na conciliação                                             |
| ConcValorLiquido  | Valor líquido na conciliação                                           |


```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/venda?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59&
DataTipo=3&QtdRegistro=10&PaginaNumero=1' \
--header 'Authorization: Bearer ••••••'
```

## Despesas - Conciliação/Pré-Conciliação

Parâmetros da Requisição:

| Campo             | Descrição / Exemplo                                                    |
|-------------------|------------------------------------------------------------------------|
| Cnpj              | 05625872000169 *(somente números)*                                     |
| DataInicial       | 2024-01-01T00:00:00                                                    |
| DataFinal         | 2024-01-01T23:59:59 *(Intervalo máximo entre datas: 90 dias)*          |
| QtdRegistro       | Quantidade de registros por página                                     |
| PaginaNumero      | Número da página                                                       |

```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/despesa?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59' \
--header 'Authorization: Bearer ••••••'
```

## Resumo de Vendas - Conciliação/Pré-Conciliação

Parâmetros da Requisição:

| Campo         | Descrição / Valor                                                        |
|---------------|--------------------------------------------------------------------------|
| Cnpj          | 05625872000169 *(somente números)*                                       |
| DataInicial   | 2024-01-01T00:00:00                                                      |
| DataFinal     | 2024-01-01T23:59:59 *(Intervalo máximo entre datas: 90 dias)*            |
| DataTipo      | Tipo de data utilizada para o filtro                                     |
|               | 0 : Pré-Conciliação - Data da Venda                                      |
|               | 1 : Pré-Conciliação - Data de Previsão                                   |
|               | 2 : Conciliação - Data do Agendamento                                    |
|               | 3 : Conciliação - Data do Pagamento                                      |
| QtdRegistro   | Quantidade de registros por página (paginado)                            |
| PaginaNumero  | Número da página para retorno dos dados                                  |

```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/resumo?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59' \
--header 'Authorization: Bearer ••••••'
```

## Pagamentos Avulsos - Conciliação

Parâmetros da Requisição

| Campo           | Descrição / Valor                                                        |
|------------------|-------------------------------------------------------------------------|
| Cnpj             | 05625872000169 *(somente números)*                                      |
| DataInicial      | 2024-01-01T00:00:00                                                     |
| DataFinal        | 2024-01-01T23:59:59 *(Intervalo máximo entre datas: 90 dias)*           |
| DataTipo         | Tipo de data utilizada no filtro:                                       |
|                  | 0 : Pré-Conciliação - Data da Venda                                     |
|                  | 1 : Pré-Conciliação - Data de Previsão                                  |
|                  | 2 : Conciliação - Data do Agendamento                                   |
|                  | 3 : Conciliação - Data do Pagamento                                     |
| QtdRegistro      | Quantidade de registros por página                                      |
| PaginaNumero     | Número da página (para paginação dos resultados)                        |
| OrdenarPor       | Campo utilizado para ordenação dos resultados                           |
| OrdenarTipo      | Tipo de ordenação: `ASC` (crescente) ou `DESC` (decrescente)            |
| DataVenda        | Data da venda da transação                                              |
| AdquirenteNome   | Nome da adquirente (ex: Cielo, Rede, Stone)                             |
| BandeiraNome     | Nome da bandeira do cartão (ex: Visa, Mastercard)                       |
| Taxa             | Percentual da taxa aplicada                                             |
| TaxaValor        | Valor monetário da taxa                                                 |
| ValorBruto       | Valor bruto da transação                                                |
| ValorLiquido     | Valor líquido após aplicação da taxa                                    |
| DataPgto         | Data de pagamento repassado pelo provedor de pagamento                  |

```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/pagamento/
avulso?Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59&
DataTipo=3&QtdRegistro=10&PaginaNumero=1&OrdenarPor=DataVenda&OrdenarTipo=ASC' \
--header 'Authorization: Bearer ••••••'
```

## Conciliação Bancária - Conciliação

Parâmetros da Requisição:

| Campo         | Descrição                                                                 |
|---------------|---------------------------------------------------------------------------|
| Cnpj          | 05625872000169 *(somente números)*                                        |
| DataInicial   | 2024-01-01T00:00:00 – Data/hora de início do período de consulta          |
| DataFinal     | 2024-01-01T23:59:59 – Data/hora de fim do período *(máximo de 90 dias)*   |
| DataTipo      | Tipo de data usada como base para o filtro:                               |
|               | 0 : Pré-Conciliação - Data da Venda                                       |
|               | 1 : Pré-Conciliação - Data de Previsão                                    |
|               | 2 : Conciliação - Data do Agendamento                                     |
|               | 3 : Conciliação - Data do Pagamento                                       |
| QtdRegistro   | Quantidade de registros retornados por página                             |
| PaginaNumero  | Número da página atual para paginação dos resultados                      |

```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/deposito?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59&
DataTipo=3&QtdRegistro=10&PaginaNumero=1' \
--header 'Authorization: Bearer ••••••'
```

## Conciliação Bancária/Resumo de Taxas - Conciliação

Parâmetros da Requisição

| Campo         | Descrição                                                                 |
|---------------|---------------------------------------------------------------------------|
| Cnpj          | 05625872000169 *(somente números)*                                        |
| DataInicial   | 2024-01-01T00:00:00 – Data/hora de início do período de consulta          |
| DataFinal     | 2024-01-01T23:59:59 – Data/hora de fim do período *(máximo de 90 dias)*   |
| DataTipo      | Tipo de data usada como base para o filtro:                               |
|               | 0 : Pré-Conciliação - Data da Venda                                       |
|               | 1 : Pré-Conciliação - Data de Previsão                                    |
|               | 2 : Conciliação - Data do Agendamento                                     |
|               | 3 : Conciliação - Data do Pagamento                                       |
| QtdRegistro   | Quantidade de registros retornados por página                             |
| PaginaNumero  | Número da página atual para paginação dos resultados                      |
| Agrupar       | Tipo de agrupamento dos dados no resultado:                               |
|               | 1 : Serviço                                                               |
|               | 2 : Bandeira                                                              |
|               | 3 : Adquirente                                                            |

```bash {title="Exemplo de Requisição"}
curl -X GET --location 'https://webapi.relatoriotef.com.br/conciliacao/taxa?
Cnpj=05625872000169&DataInicial=2024-01-01T00:00:00&DataFinal=2024-01-01T23:59:59&
DataTipo=3&QtdRegistro=10&PaginaNumero=1&Agrupar=1' \
--header 'Authorization: Bearer ••••••'
```
