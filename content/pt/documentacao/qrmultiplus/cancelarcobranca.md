---
title: "Cancelamento de Cobrança"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 606

seo:
  title: "Cancelamento de Cobrança" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para cancelar uma transação (estorno) deve ser feita uma requisição `POST` no endpoint `SetEstornaVendaQrCode`, informando os seguintes headers:

| Header        | Obrigatório | Descrição                                                                                     |
|---------------|-------------|-----------------------------------------------------------------------------------------------|
| `token`       | *           | Solicite seu token junto à Multiplus Card.                                                    |
| `cnpj`        | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                            |
| `codigo`      | *           | Código retornado no momento da criação da cobrança.                                           |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional

{{< callout context="caution" title="Transações com Multiplus Pay" icon="outline/alert-triangle" >}}
Transações processadas via Multiplus Pay não possuem funcionalidade de cancelamento.

Para esclarecimentos ou suporte durante a operação, contate o suporte técnico.
{{< /callout >}}

```bash {title="Exemplo de Remoção de Cobrança"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetEstornaVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "codigo: 123456" \
```

Em caso de sucesso retornará o JSON a seguir, com o status da transação com o valor `CANCELADA`.

```json {title="Exemplo de Retorno - Sucesso"}
{
  "Codigo": 970468008,
  "NSU": "970468008",
  "CodigoAutorizacao": "f7af500a-0313-4cf9-a29d-bb6fd9f9589e",
  "DataHora": "2025-04-23 18:01:15",
  "Valor": "0,01",
  "StatusCode": "78",
  "Status": "CANCELADA",
  "Origem": "BB",
  "CaixaId": "60177876000130001"
}
```

Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: Transação não se encontra APROVADA.
```
