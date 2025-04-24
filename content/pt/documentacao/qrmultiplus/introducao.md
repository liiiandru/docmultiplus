---
title: "Introdução"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 601
seo:
  title: "Introdução" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
O QRMultiplus é uma solução da Multiplus Card que viabiliza a integração com diferentes PSPs (Provedores de Serviços de Pagamento) para a realização de pagamentos via Pix e carteiras digitais. Com uma única integração, é possível centralizar o acesso a múltiplos PSPs, simplificando o processo de integração e manutenção do sistema.


## Fluxo de Transação

A seguir é apresentado o fluxo básico de transação com o QRMultiplus.

{{< figure
  src="images/qr_multiplus_api.png"
  alt="Diagrama do fluxo de Transação do QRMultiplus"
  caption="Diagrama do fluxo de Transação do QRMultiplus"
>}}

## Endpoints da API

A URL base para acesso à API é:

```txt
https://api.multipluscard.com.br/api/VendasQrCode/
```


### Endpoints disponíveis

| Endpoint                   | Descrição                                                                 |
|----------------------------|---------------------------------------------------------------------------|
| `SetEmpresaQrCode`         | Registra um novo ponto de venda (loja) na plataforma.                     |
| `SetCaixaQrCode`           | Registra os caixas disponíveis no estabelecimento.                        |
| `SetVendaQrCode`           | Inicia uma transação de venda na plataforma.                              |
| `GetInformacoesVendaQrCode`| Consulta o status e o andamento de uma transação existente.               |
| `SetRemoveVendaQrCode`     | Remove o pagamento de uma transação em andamento.                         |
| `SetEstornaVendaQrCode`    | Estorna (cancela) o pagamento de uma transação já aprovada.               |

