---
title: "Manutenção de Link de Pagamento"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 804
sidebar:
  collapsed: true
seo:
  title: "Manutenção de Link de Pagamento" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A realização de manutenções em um link de pagamento consiste em realizar o cancelamento da venda, excluir link, adicionar númeor de celular, etc. Para realizar deve ser feita uma requisição `POST` para a URL a seguir:

```txt {title="URL da API"}
https://api.multipluscard.com.br/api/Servicos/SetLinkPagoManutencao
```

As informações sobre a transação sã oenviadas nos headers descritos a seguir:

| Header           | Tipo    | Obrigatório | Descrição |
|------------------|---------|-------------|-----------|
| hash             | `string`  | Sim         | O hash da transação, obtido pelo serviço `getTransacoes` no campo `HASH_LINKPAGO` ou no JSON como `LinkPagoUrl`. |
| retornarlink     | `string`  | Não         | Se informado como "S", o serviço retornará o link de pagamento; caso contrário, retornará "OK" em caso de sucesso. |
| enviasms         | `string`  | Não         | Se informado como "S", enviará novamente o SMS para o celular cadastrado na criação do link. Também pode ser utilizado em conjunto com o header `celular` para envio a um novo número. |
| celular          | `string`  | Não         | Número de celular no formato DDD + número (ex.: `18999998888`). Se informado junto com `enviasms=S`, o link será enviado para este número. |
| vendadisponivel  | `string`  | Não         | Se enviado como "N", a venda será marcada como utilizada e não será retornada pelo serviço `getTransacoes` quando utilizado o header `LinkPagoSomenteDisponiveis`. Enviando "S" torna a venda disponível novamente. |
| excluirLink      | `string`  | Não         | Se enviado como "S", excluirá o link associado ao hash informado. |
| cancelarVenda    | `string`  | Não         | Se enviado como "S", tentará cancelar a venda finalizada. Retornará "OK" em caso de sucesso ou a mensagem de erro se falhar. |


Em caso de sucesso a API retornará uma string contendo o link para a realização do pagamento.

```txt {title="Exemplo de Retorno"}
http://api-linkpago.ddns.net/portalcliente/LinkPago/Checkout/XXXXXXXXXXXXXXXXX
```

Caso ocorra erro na requisição será retornada uma string contendo o motivo:

```txt {title="Exemplo de Retorno com Erro"}
[ERRO] CNPJ INVÁLIDO
```
