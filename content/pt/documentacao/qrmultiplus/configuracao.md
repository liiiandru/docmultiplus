---
title: "Configuração"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 602
seo:
  title: "Configuração" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Ao utilizar pela primeira vez o QRMultiplus em um PDV é necessário que seja realizado o processo de configuração, este é composto por duas etapas:

- Registro da Empresa
- Registro do Caixa (PDV)

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
A aplicação deve gerenciar o estado das etapas de configuração, garantindo que o registro da empresa e do caixa (PDV) tenham sido concluídos com sucesso antes de prosseguir com as operações.
{{< /callout >}}

## Registrando uma Empresa

Esse processo é necessário que seja executado apenas uma vez, para realizar deve ser feita uma requisição `POST` para o endpoint `SetEmpresaQrCode`, com os headers a seguir:


| Header       | Obrigatório | Descrição                                                                                      |
|--------------|-------------|------------------------------------------------------------------------------------------------|
| `token`      | *           | Solicite seu token junto à Multiplus Card.                                                     |
| `cnpj`       | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                             |
| `lojaid`     | *           | ID para identificação da loja. Sugestão: usar os 6 últimos dígitos do CNPJ do estabelecimento. |
| `integrador` | *           | Envie `"PIX"` para utilizar o PSP padrão (caso haja apenas um configurado).                    |
| `latitude`   | *           | Coordenada de latitude do estabelecimento.                                                     |
| `longitude`  | *           | Coordenada de longitude do estabelecimento.                                                    |
| `horarios`   | -           | Horários de funcionamento no formato: `dia\|hora inicio\|hora fim¬`.                           |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional


```bash {title="Exemplo de Registro de Empresa"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetEmpresaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "lojaid: 000195" \
  -H "integrador: PIX" \
  -H "latitude: -23.550520" \
  -H "longitude: -46.633308" \
  -H "horarios: segunda|08:00|18:00¬terca|08:00|18:00¬quarta|08:00|12:00|14:00:1800|quinta|08:00|18:00" \
```

Em caso de sucesso será retornado um JSON com os campos `ID` e `LOJAID`.

```json {title="Exemplo de Retorno - Sucesso"}
{
  "id": "1",
  "lojaid": "000169"
}
```
Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: CNPJ inválido.
```

## Registrando um Caixa

Esse processo é necessário que seja realizado no primeiro uso do QRMultiplus no PDV, para isso deve ser feita uma requisiação `POST` no endpoint `SetcaixaQrCode`, com os headers a seguir:

| Header        | Obrigatório | Descrição                                                                                     |
|---------------|-------------|-----------------------------------------------------------------------------------------------|
| `token`       | *           | Solicite seu token junto à Multiplus Card.                                                    |
| `cnpj`        | *           | CNPJ do estabelecimento sem ponto, traço ou barra.                                            |
| `nomecaixa`   | *           | Nome para identificação do caixa. Exemplo: `CaixaPrincipal`, `CaixaConveniencia`.             |
| `caixaid`     | *           | ID para identificação do caixa em nossa plataforma.                                           |
| `integrador`  | *           | Envie `"PIX"` para utilizar o PSP padrão, caso haja apenas um configurado.                    |

**Legenda:**
- `*` = Obrigatório
- `-` = Opcional

```bash {title="Exemplo de Registro de Caixa"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetCaixaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: 12345678000195" \
  -H "nomecaixa: CaixaPrincipal" \
  -H "caixaid: CX01" \
  -H "integrador: PIX" \
```

Em caso de sucesso será retornado um JSON com os campos `ID`, `LOJAID`, `NOMECAIXA` e `CAIXAID`.

```json {title="Exemplo de Retorno - Sucesso"}
{
  "id": "1",
  "lojaid": "000169",
  "nomecaixa": "CAIXAPRINCIPAL",
  "caixaid": "001"
}

```
Se ocorrer um erro será retornada uma string contendo quais foram os erros.

```txt {title="Exemplo de Retorno - Erro"}
[ERRO]: CNPJ inválido.
```

## Multiplus Pay

Ao utilizar a adquirente Multiplus Pay como provedor das transações via Pix, as requisições para cadastro da empresa e do caixa apresentam alterações nos headers: o header `CNPJEC` deve ser utilizado para informar o **CNPJ da empresa**, enquanto o header `CNPJ` passa a ser utilizado para o **CNPJ da Multiplus Pay**.

A seguir um exemplo de como deve ser feita a requisição para registrar uma empresa, utilizando a Multiplus Pay.

```bash {title="Exemplo de Registro de Empresa - Multiplus Pay"}
curl -X POST https://api.multipluscard.com.br/api/VendasQrCode/SetEmpresaQrCode \
  -H "token: SEU_TOKEN_AQUI" \
  -H "cnpj: CNPJ da Multiplus Pay" \
  -H "cnpjec: CNPJ da Empresa" \
  -H "lojaid: 901234" \
  -H "integrador: PIX" \
  -H "latitude: -23.550520" \
  -H "longitude: -46.633308" \
  -H "horarios: segunda|08:00|18:00¬terca|08:00|18:00¬quarta|08:00|12:00|14:00:1800|quinta|08:00|18:00" \
```

{{< callout context="note" title="Importante" icon="outline/info-circle" >}}
Para o header `LOJAID` sugerimos utilizar os 6 últimos dígitos do CNPJ da Empresa (`CNPJEC`).

O Nome do caixa (`NOMECAIXA`) deve seguir o padrão: **CAIXA PDV|CNPJEC**, ex: `CAIXA 099|12345678901234`.

O Id do caixa (`CAIXAID`) deve seguir o padrão: **PDV(3 digitos)+CNPJEC**, ex: `09912345678901234`
{{< /callout >}}
