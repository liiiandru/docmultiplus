---
title: "Checkout"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 802
sidebar:
  collapsed: true
seo:
  title: "Checkout" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
A integração do Link Pago por meio do Checkout é uma forma simples de gerir a criação e manutenção de links de pagamento, utilizando a interface já disponibilizada pela Multiplus Card para a interação do cliente na realização do Pagamento.

A seguir serão descritos os processos necessários para a criação, manutenção e obtenção dasa transações realizadas no link Pago.

## Criação do Link de Pagamento

A criação de um link de pagamento é realizada com uma requisição `POST` na seguinte Url:

```txt {title="URL da API"}
https://api.multipluscard.com.br/api/Servicos/SetLinkPagoIniciaCheckout
```

As informações relativas à transação são enviadas nos headers descritos a seguir:

| Campo                       | Tipo    | Obrigatório | Descrição |
|------------------------------|---------|-------------|-----------|
| valor                        | `string`  | Sim     | Valor total do link, deve ser maior que zero. Exemplo: "1000,00" (separador de decimal vírgula). |
| cnpj                         | `string`  | Sim     | CNPJ do utilizador do serviço. |
| celular                      | `string`  | Não     | Celular no formato DDD + Número, ex: "18999998888". Se informado, será enviado SMS com o link. |
| identificacao                | `string`  | Não     | Texto para identificar o pagamento. |
| produtos                     | `string`  | Não     | Produtos da compra, separados conforme exemplo: "1;Produto 1;1;5,00\|2;Produto 2;1;2,50". |
| DiasValidade                 | `string`  | Não     | Número de dias de validade do link (padrão 7 dias, se não informado). |
| ClienteNome                  | `string`  | Sim     | Nome do pagador. |
| ClienteDocto                 | `string`  | Sim     | CPF do pagador. |
| ClienteEmail                 | `string`  | Não     | E-mail do pagador. |
| ClienteFone                  | `string`  | Não     | Telefone ou celular do pagador (se não informado, usará o header "celular"). |
| ClienteCep                   | `string`  | Não     | CEP do pagador. |
| ClienteLogradouro            | `string`  | Não     | Logradouro do pagador. |
| ClienteLogradouroNumero      | `string`  | Não     | Número do logradouro do pagador. |
| ClienteComplemento           | `string`  | Não     | Complemento do pagador. |
| ClienteEstado                | `string`  | Não     | Estado do pagador (ex: "SP", 2 posições). |
| ClienteCidade                | `string`  | Não     | Cidade do pagador. |
| QtdeMaxParcelas              | `string`  | Sim     | Quantidade máxima de parcelas permitidas. |
| ValorMinimoParcelas          | `string`  | Sim     | Valor mínimo de cada parcela. |
| QtdeParcelasSemJuros         | `string`  | Sim     | Quantidade de parcelas sem juros. |
| JurosParcela                 | `string`  | Não     | Juros aplicados no valor total conforme a quantidade de parcelas. |

Em caso de sucesso a API retornará uma string contendo o link para a realização do pagamento.

```txt {title="Exemplo de Retorno"}
http://api-linkpago.ddns.net/portalcliente/LinkPago/Checkout/XXXXXXXXXXXXXXXXX
```

{{< callout context="note" icon="outline/info-circle">}}
  Caso o header `CELULAR` seja informado durante a criação do link, um SMS contendo o link de pagamento será enviado automaticamente ao pagador.
{{< /callout >}}


Caso ocorra erro na requisiçaõ será retornada uma string contendo o motivo:

```txt {title="Exemplo de Retorno com Erro"}
[ERRO] CNPJ INVÁLIDO
```

{{< callout context="note" icon="outline/info-circle">}}
- **Headers `QtdeMaxParcelas` e `ValorMinimoParcelas`**
  Esses headers possuem uma validação em que o valor mínimo prevalece.
  Por exemplo, em uma venda de **R$ 10,00**, se for informado `ValorMinimoParcelas = 5,00`, a `QtdeMaxParcelas` será automaticamente limitada a **2**.

- **Header `QtdeParcelasSemJuros`**
  Define até qual parcela não haverá cobrança de juros.

{{< /callout >}}
