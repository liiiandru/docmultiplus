---
title: "Criação da Cobrança"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 603
seo:
  title: "Introdução" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para criar uma cobrança deve ser feita uma requisição `POST` para o endpoint `SetVendaQrCode`, informando os seguintes headers:

| Header        | Obrigatório | Descrição                                                                                     |
|---------------|-------------|-----------------------------------------------------------------------------------------------|
| `token`       | *           | Solicite seu token junto à Multiplus Card.                                                    |
| `cnpj`        | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                            |
| `caixaid`     | *           | ID para identificação do caixa em nossa plataforma.                                           |
| `integrador`  | *           | Envie `"PIX"` para utilizar o PSP padrão, caso haja apenas um configurado.                    |
| `valor`       | *           | Valor da transação, o separador de casa decimal é a virgula. Ex: `10,99`                      |
| `mensagempix` | -           | Mensagem que pode exibir na hora de gerar o QrCode (Utilizável apenas no Efi Bank)            |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional

{{< callout context="note" icon="outline/info-circle" >}}
Ao criar uma cobrança, caso o cliente possua múltiplos PSPs configurados, é possível especificar o provedor desejado. Para isso, utilize o header `INTEGRADOR`, informando o identificador do PSP a ser utilizado. Exemplo: `INTEGRADOR = BB`.

Consulte a lista de PSPs disponíveis para verificar os valores válidos que podem ser utilizados neste header.

{{< /callout >}}

```bash {title="Exemplo de Criação de Cobrança"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "caixaid: CX01" \
  -H "integrador: PIX" \
  -H "valor: 10,90" \
```

Em caso de sucesso será retornado um JSON com os seguintes campos: `Codigo`, `Controle`, `Valor`, `DataExpiracaoTransacao` e `qr_code`. O campo `qr_code` contem a string que deve ser convertida na imagem do QR Code.

```json {title="Exemplo de Retorno - Sucesso"}
{
  "codigo": "1",
  "controle": "12345",
  "valor": "10,99",
  "dataexpiracaotransacao": "01/01/2026 09:05:57",
  "qr_code": "00020126400014br.gov.bcb.pix0114+5511999999995204000053039865405100.
  005802BR5913Fulano de Tal6009SAO PAULO62100506ABC1236304B14F"
}
```

Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: CNPJ inválido.
```

## Multiplus Pay

Ao utilizar a adquirente Multiplus Pay como provedor das transações via Pix, as requisições para cadastro da empresa e do caixa apresentam alterações nos headers: o header `CNPJEC` deve ser utilizado para informar o **CNPJ da empresa**, enquanto o header `CNPJ` passa a ser utilizado para o **CNPJ da Multiplus Pay**.

```bash {title="Exemplo de Criação de Cobrança - Multiplus Pay"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetVendaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: CNPJ da Multiplus Pay" \
  -H "cnpjec: CNPJ da Empresa" \
  -H "caixaid: 09912345678901234" \
  -H "integrador: PIX" \
  -H "valor: 10,90" \
```
