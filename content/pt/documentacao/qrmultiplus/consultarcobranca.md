---
title: "Consulta de Status"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 604
seo:
  title: "Consulta de Status" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para consultar o status de uma cobrança deve ser realizada uma requisição `POST` no endpoint `GetInformacoesVendaQrCode`, informando o código da cobrança retornado no momento da sua criação.

Os headers que devem ser informados são:

| Header        | Obrigatório | Descrição                                                                                     |
|---------------|-------------|-----------------------------------------------------------------------------------------------|
| `token`       | *           | Solicite seu token junto à Multiplus Card.                                                    |
| `cnpj`        | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                            |
| `codigo`      | *           | Código retornado no momento da criação da cobrança.                                           |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional

```bash {title="Exemplo de Consulta de Status"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/GetInformacoesVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "codigo: 123456" \
```

Em caso de sucesso será retornado o JSON a seguir, o campo **Status** pode conter um dos seguintes valores: `APROVADO`, `NEGADA`, `DESFEITA`, `PENDENTE` ou `CANCELADA`.

```json {title="Exemplo de Retorno - Sucesso"}
{
  "Codigo": 999428888,
  "NSU": "999428888",
  "CodigoAutorizacao": "999428888",
  "DataHora": "2025-04-23 17:57:31",
  "Valor": "0,01",
  "StatusCode": "00",
  "PagamentoId": "E75236320272504232058s700e30e45c",
  "Status": "APROVADO",
  "Origem": "BB",
  "CaixaId": "60177876000130001"
}
```
Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: CNPJ inválido.
```

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
As requisições a este endpoint devem ser feitas com o intervalo de 2 segundos.
{{< /callout >}}


## Multiplus Pay

Ao utilizar a adquirente Multiplus Pay como provedor das transações via Pix, as requisições para cadastro da empresa e do caixa apresentam alterações nos headers: o header `CNPJEC` deve ser utilizado para informar o **CNPJ da empresa**, enquanto o header `CNPJ` passa a ser utilizado para o **CNPJ da Multiplus Pay**.

```bash {title="Exemplo de Consulta de Status - Multiplus Pay"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/GetInformacoesVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: CNPJ da Multiplus Pay" \
  -H "cnpjec: CNPJ da Empresa" \
  -H "codigo: 123456" \
```
