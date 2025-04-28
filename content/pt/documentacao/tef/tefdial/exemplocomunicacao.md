---
title: "Exemplo de Comunicação"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 306
seo:
  title: "Exemplo de Comunicação" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A seguir, é apresentado um exemplo de transação via cartão de crédito, destacando os formatos possíveis dos arquivos de requisição a serem gerados, bem como os respectivos arquivos de retorno esperados.

## Arquivo de Requisição de Transação

```txt {title="C:\TEF_DIAL\REQ\IntPos.001"}
000-000 = CRT
001-000 = 1
002-000 = 123456
003-000 = 150000
999-999 = 0
```

| Campo             | Descrição                                     |
|-------------------|-----------------------------------------------|
| 000-000 = CRT     | Header p/venda p/cartão                       |
| 001-000 = 1       | Identificação da solicitação                  |
| 002-000 = 123456  | Número do documento fiscal vinculado          |
| 003-000 = 150000  | Valor total da transação (R$1.500,00)         |
| 999-999 = 0       | Registro final do arquivo                     |


## Exemplo de requisição com integração de forma direta

```txt {title="C:\TEF_DIAL\REQ\IntPos.001"}
000-000 = CRT
001-000 = 1
002-000 = 123456
003-000 = 150000
800-001 = 0
800-002 = 1
800-003 = 1
800-004 = 10
800-005 = 05625872000169
800-006 = 1
999-999 = 0
```

| Campo               | Descrição                                     |
|---------------------|-----------------------------------------------|
| 000-000 = CRT       | Header p/venda p/cartão                       |
| 001-000 = 1         | Identificação da solicitação                  |
| 002-000 = 123456    | Número do documento fiscal vinculado          |
| 003-000 = 150000    | Valor total da transação (R$1.500,00)         |
| 800-001 = 0         | Venda função crédito                          |
| 800-002 = 1         | Venda Parcelada                               |
| 800-003 = 1         | Parcelamento pela Loja                        |
| 800-004 = 10        | Quantidade de Parcelas                        |
| 800-005 = 05625872000169 | CNPJ Empresa                          |
| 800-006 = 1         | Finalização no equipamento pinpad             |
| 999-999 = 0         | Registro final do arquivo                     |


## Arquivo que indica que a função está em execução

```txt {title="C:\TEF_DIAL\RESP\IntPos.Sts"}
000-000 = CRT
001-000 = 1
999-999 = 0
```

| Campo           | Descrição                          |
|-----------------|-------------------------------------|
| 000-000 = CRT   | Header p/venda p/cartão            |
| 001-000 = 1     | Identificação da solicitação       |
| 999-999 = 0     | Registro final do arquivo          |


## Arquivo de Resposta

```txt {title="C:\TEF_DIAL\RESP\IntPos.Sts"}
000-000 = CRT
001-000 = 1
002-000 = 123456
003-000 = 150000
004-000 = 1
009-000 = 0
010-000 = “Rede X”
010-001 = 12345678900000
010-002 = 25
011-000 = 10
012-000 = 123456789000
013-000 = 123456
014-000 = 1234567890
015-000 = DDMMHHMMSS
016-000 = DDMMHHMMSS
022-000 = DDMMAAAA
023-000 = HHMMSS
027-000 = 123XXYTZAAABC
028-000 = 20
029-001 = “Rede X”
029-002 = “Loja X”
029-003 = “Rua X”
029-004 = “TERM.666666”
029-005 = “ADM. X”
...
029-017 = “”
029-018 = “”
029-019 = “ASSINATURA”
029-020 = “CLIENTE X”
040-000 = “ADM. X”
999-999 = 0
```

| Campo             | Descrição                                      |
|-------------------|------------------------------------------------|
| 000-000 = CRT     | Header p/de venda c/cartão                     |
| 001-000 = 1       | Identificação da solicitação                   |
| 002-000 = 123456  | Número do documento fiscal vinculado           |
| 003-000 = 150000  | Valor total da transação (R$1.500,00)          |
| 004-000 = 1       | Moeda = Real                                   |
| 009-000 = 0       | Status da transação = OK                       |
| 010-000 = “Rede X”| Nome da Rede                                   |
| 010-001 = 12345678900000 | CNPJ da Rede                           |
| 010-002 = 25      | Código SAT da Rede                             |
| 011-000 = 10      | Tipo da Transação = Cartão de Crédito à Vista  |
| 012-000 = 123456789000 | Número da NSU                            |
| 013-000 = 123456  | Código de Autorização da Transação             |
| 014-000 = 1234567890 | Número do Lote da Transação                |
| 015-000 = DDMMHHMMSS | Data e a Hora da Transação (Host)          |
| 016-000 = DDMMHHMMSS | Data e Hora da Transação local (ponto de venda) |
| 022-000 = DDMMAAAA| Data da Transação - Comprovante                |
| 023-000 = HHMMSS  | Hora da Transação - Comprovante                |
| 027-000 = 123XXYTZAAABC | Finalização                            |
| 028-000 = 20      | Quant. de linhas do comprovante = 20           |
| 029-001 = “Rede X”| Imagem do comprovante – linha 01               |
| 029-002 = “Loja X”| Imagem do comprovante – linha 02               |
| 029-003 = “Rua X” | Imagem do comprovante – linha 03               |
| 029-004 = “TERM.666666” | Imagem do comprovante – linha 04       |
| 029-005 = “ADM. X”| Imagem do comprovante – linha 05               |
| ...               | Imagem do comprovante – linha...               |
| 029-017 = “”      | Imagem do comprovante – linha 17               |
| 029-018 = “”      | Imagem do comprovante – linha 18               |
| 029-019 = “ASSINATURA” | Imagem do comprovante – linha 19        |
| 029-020 = “CLIENTE X” | Imagem do comprovante – linha 20         |
| 040-000 = “ADM. X”| Administradora do Cartão                       |
| 999-999 = 0       | Registro final do arquivo                      |


## Arquivo de Confirmação

```txt {title="C:\TEF_DIAL\REQ\IntPos.001"}
000-000 = CNF
001-000 = 1
002-000 = 123456
010-000 = Rede X
012-000 = 123456789000
027-000 = 123XXYTZAAABC
999-999 = 0
```

| Campo               | Descrição                                                              |
|---------------------|------------------------------------------------------------------------|
| 000-000 = CNF       | Header p/confirmação de finalização da venda e impressão do cupom     |
| 001-000 = 1         | Identificação da solicitação                                           |
| 002-000 = 123456    | Número do documento fiscal vinculado                                   |
| 010-000 = Rede X    | Rede responsável pela autorização                                      |
| 012-000 = 123456789000 | Número da Transação NSU                                             |
| 027-000 = 123XXYTZAAABC | Finalização                                                       |
| 999-999 = 0         | Registro final do arquivo                                              |


{{< callout context="note"  title="Comprovante da Transação" icon="outline/info-circle">}}
  O Gerenciador de Pagamentos emite apenas uma via do comprovante da transação. A responsabilidade pela geração de vias adicionais é da Aplicação de Automação Comercial, que deve implementar esse recurso conforme as capacidades e limitações do equipamento de impressão utilizado. Recomenda-se a emissão de pelo menos duas vias: uma para o cliente e outra para o estabelecimento.
{{< /callout >}}
