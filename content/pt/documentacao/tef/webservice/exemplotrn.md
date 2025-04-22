---
title: "Exemplo de Utilização"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 409
seo:
  title: "Exemplo de Utilização" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
A seguir, é apresentado um exemplo de transação via cartão de crédito, com as duas formas possíveis de requisição.

## Requisição Normal
A Automação faz a requisição de forma simplificada, o Gerenciador de Pagamentos é responsável por detalhes como transação parcelada, número de parcelas, etc.

```txt {title="Exemplo de Requisição"}
POST: https://api.multipluscard.com.br/api/Servicos/SetVendaTef
Headers
CNPJ: "123456789000100"
PDV: "001"
TOKEN: "token"
CONTEUDO: "000-000 = CRT¬001-000 = 1¬002-000 = 123456¬003-000 = 150000¬999-999 = 0"
```

## Requisição com Integração Direta
A Automação informa todos os parâmetros necessários para realizar a transação, por ex:  venda parcelada no crédito, número de parcelas, parcelamento da loja ou administradora.

```txt {title="Exemplo de Requisição Direta"}
POST: https://api.multipluscard.com.br/api/Servicos/SetVendaTef
Headers
CNPJ: "123456789000100"
PDV: "001"
TOKEN: "token"
CONTEUDO: "000-000 = CRT¬001-000 = 1¬002-000 = 123456¬003-000 = 150000¬800-001 = 0¬
800-002 = 1¬800-003 = 1¬800-004 = 10¬800-005= 05625872000169¬800-006= 1¬999-999 = 0"
```

## Retorno da Requisição (Criação da Transação)

Obtendo sucesso na requisição HTTP (200) será retornado o Hash da Transação

```txt {title="Retorno da Requisição - HTTP 200"}
123456
```

## Requisição de Consulta

```txt {title="Requisição de Consulta da Transação"}
GET: https://api.multipluscard.com.br/api/Servicos/GetVendasTef
Headers
HASH: "123456"
TOKEN: "token"
```

## Retorno da Requisição (Comprovante da Transação)

```txt {title="Retorno da Requisição - Comprovante da transação"}
000-000 = CRT¬001-000 = 3¬002-000 = 3¬003-000 = 005¬004-000 = 0¬009-000 = 0¬
010-000 = REDE¬010-001 = 01425787000104¬010-002 = 25¬011-000 = 10¬012-000 = 001229¬
013-000 = 620441¬015-000 = 1506160039¬016-000 = 1506160039¬017-000 = 0¬018-000 = 01¬
022-000 = 15062023¬023-000 = 160034¬027-000 = 06502921017160039¬
028-000 = 25¬
029-001 = "VENDA EM AMBIENTE DE HOMOLOGACAO"¬
029-002 = "******NÃO USAR EM CLIENTES******"¬
029-003 = ""¬
029-004 = "                 REDE                 "¬
029-005 = "            ELO CREDITO            C"¬
029-006 = "COMPR:5040035    VALOR      0,05"¬
029-007 = "ESTAB:041006458 MULTIPLUSCARD         "¬
029-008 = "CNPJ/CPF:05.625.872/0001-69           "¬
029-009 = "ENDERECO:R DOZE DE OUTUBRO,623        "¬
029-010 = "COMPL:                                "¬
029-011 = "CIDADE-UF:PRESIDENTE PRU-SP           "¬
029-012 = "15.06.23-16:00:35 TERM:PV674548/018229"¬
029-013 = "CARTAO: XXXXXXXXXXXX8040              "¬
029-014 = "AUTORIZACAO: 620441                   "¬
029-015 = "ARQC:0E048530B11B8250"¬
029-016 = "AID: A0000008947012                   "¬
029-017 = "    TRANSACAO AUTORIZADA MEDIANTE     "¬
029-018 = "    USO DE SENHA PESSOAL.             "¬
029-019 = " "¬
029-020 = " "¬
029-021 = "CONTROLE 03509927017  TEF"¬
029-022 = " "¬
029-023 = "CUPOM FISCAL: 3     PDV: 001"¬
029-024 = "CNPJ EMITENTE: 81.403.179/0001-78"¬
029-025 = "15/06/2023  16:00:39  CREDITO A VISTA"¬
030-000 = Transação Aceita.¬
040-000 = ELO CREDITO¬
170-000 = 507416175¬
200-000 = 000¬200-001 = 000¬740-000 = 655001******8040¬
741-000 = NOME DO CLIENTE¬742-000 = 241231¬743-000 = 0¬
800-001 = 0¬800-002 = 0¬800-007 = 06502921017¬
800-000 = v1.416.71¬
999-999 = 0
```

{{< callout context="note"  title="Comprovante da Transação" icon="outline/info-circle">}}
  O Gerenciador de Pagamentos disponibiliza apenas uma cópia do comprovante da transação. A responsabilidade pela emissão de vias adicionais cabe à Aplicação de Automação Comercial, considerando as características e limitações da impressora utilizada. Recomenda-se a geração de no mínimo, duas vias: uma destinada ao cliente e outra ao estabelecimento.
{{< /callout >}}
