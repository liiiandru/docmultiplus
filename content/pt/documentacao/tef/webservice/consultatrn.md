---
title: "Consulta de Transações"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 405
seo:
  title: "Consulta de Transações" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para realizar a consulta em uma transação criada é necessário que seja feita uam requisição **GET** no endpoint `GetVendasTef`, com os seguintes headers:

| Header    | Descrição                                                                 |
|-----------|---------------------------------------------------------------------------|
| HASH      | Código retornado ao realizar a requisição de criação da transação         |
| TOKEN     | String necessária para realizar aas requisições (fornecida pela Multiplus Card)          |

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
É necessário o inttervalo mínimo de **500ms** antes de realizar uma nova requisição GET no endpoint `GetVendasTef`.
{{< /callout >}}

## Retorno da Consulta

Em caso de sucesso da requisição (HTTP status 200), os possíveis retornos estão a seguir:

| Retorno                        | Descrição                                                              |
|-------------------------------|-------------------------------------------------------------------------|
| Pendente                      | O Gerenciador Padrão ainda não iniciou o processo da transação.         |
| Processando                   | O Gerenciador Padrão iniciou a transação e a mesma está sendo executada.|
| Cancelado                     | Venda Cancelada por parte do PDV                                        |
| [ERRO]: Hash invalido         | O Hash informado não existe                                             |
| [ERRO]: Verificar Parâmetros  | Ocorre na ausência de parâmetros necessários para a consulta.           |
| [ERRO]: “descrição do erro”   | Erro não previsto, retorno com a descrição do mesmo.                    |
| XXXX¬yyyyy¬zzzz......         | Retorno da transação realizada                                          |
