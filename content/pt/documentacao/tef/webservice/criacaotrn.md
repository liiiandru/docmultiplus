---
title: "Criação de Transações"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 404
seo:
  title: "Criação de Transações" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para ser criada uma transação é necessário que seja feita uma requisição **POST** no endpoint `SetVendaTef`, com os seguintes headers:

| Header    | Descrição                                                                 |
|-----------|---------------------------------------------------------------------------|
| CNPJ      | Cnpj (somente números) do cliente                                         |
| PDV       | Número do PDV com 3 caracteres. Ex: “001”                                 |
| TOKEN     | String necessária para as requisições, fornecida pela Multiplus Card      |
| CONTEUDO  | Conteúdo da requisição, nas seções seguintes será descrito os campos necessários |
| CALLBACK  | (Opcional) URL que devemos notificar                                      |

## Callback

Caso inclua o header `CALLBACK` no `SetVendaTef`, seu valor deverá conter a URL que será utilizada na notificação, desta forma cada requisição pode ter um endereço de callback diferente da outra.

O conteúdo do body será o mesmo que receberia ao fazer uma requisição no endpoint `GetVendasTef`, após o ClientTEF realizar a execução.

```txt {title="Exemplo de Callback"}
https://api.meusistema.com.br/tef/SetVendaTefCallback
```

```txt {title="Exemplo de Retorno"}
000-000 = CRT¬001-000 = 3¬003-000 = 002¬009-000 = 255¬028-000 = 0¬
030-000 = Cancelado pelo operador.¬800-000 = v1.416.71¬999-999 = 0
```

{{< callout context="note"  icon="outline/info-circle">}}
  Em automações desenvolvidas com linguagens/frameworks que sigam a RFC 7230, que não permite caracteres não-ASCII nos headers (ex.: .NET Core), o caractere ‘**¬**’ deve ser enviado com codificação HTML da seguinte forma: ‘**&amp;#172;**’
{{< /callout >}}


## Retornos da Transação

Ao realizar a requisição o endpoint `SetVendaTef` em caso de sucesso (HTTP status 200), poderá ter algum dos seguintes retornos:

| Retorno                        | Descrição                                                              |
|-------------------------------|-------------------------------------------------------------------------|
| “0000”                        | Hash da transação, é um número inteiro utilizado para as consultas referentes à mesma. |
| [ERRO]: Erro ao incluir a venda | Erro na inclusão da venda                                               |
| [ERRO]: Verificar Parâmetros   | Ocorre na ausência de parâmetros necessários para a consulta.          |
| [ERRO]: “descrição do erro”    | Erro não previsto, retorno com a descrição do mesmo.                   |
