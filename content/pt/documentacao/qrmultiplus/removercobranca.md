---
title: "Remoção de Cobrança"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 605
seo:
  title: "Remoção de Cobrança" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A remoção de uma cobrança deve ser executada quando a transação atingir o tempo de expiração definido no campo `DataExpiracaoTransacao`, ou em caso de cancelamento por parte do cliente — seja da forma de pagamento selecionada ou da compra em si. Essa operação é fundamental para assegurar que o relatório de vendas reflita apenas transações válidas e concluídas, evitando o acúmulo de registros pendentes de pagamento.

Para realizar a remoção deve ser feita uma requisição `POST` no endpoint `SetRemoveVendaQrCode`, informando os seguintes headers:

| Header        | Obrigatório | Descrição                                                                                     |
|---------------|-------------|-----------------------------------------------------------------------------------------------|
| `token`       | *           | Solicite seu token junto à Multiplus Card.                                                    |
| `cnpj`        | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                            |
| `codigo`      | *           | Código retornado no momento da criação da cobrança.                                           |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional

```bash {title="Exemplo de Remoção de Cobrança"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetRemoveVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "codigo: 123456" \
```

Em caso de sucesso será retornada uma string contendo o código da transação e o seu status, que no caso deve ser `86`.

```txt {title="Exemplo de Retorno - Sucesso"}
transacao: 123456789, status: 86
```

Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: CNPJ inválido.
```

{{< callout note >}}
Códigos de status da transação:
- 00 - Aprovada
- 77 - Pendente
- 78 - Cancelada
- 86 - Desfeita
{{< /callout >}}


## Multiplus Pay

Ao utilizar a adquirente Multiplus Pay como provedor das transações via Pix, as requisições para cadastro da empresa e do caixa apresentam alterações nos headers: o header `CNPJEC` deve ser utilizado para informar o **CNPJ da empresa**, enquanto o header `CNPJ` passa a ser utilizado para o **CNPJ da Multiplus Pay**.

```bash {title="Exemplo de Remoção de Cobrança - Multiplus Pay"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetRemoveVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: CNPJ da Multiplus Pay" \
  -H "cnpjec: CNPJ da Empresa" \
  -H "codigo: 123456" \
```
