---
title: "Layout do Retorno"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 204
seo:
  title: "Layout do Retorno" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
O layout de retorno é composto por três partes:
- 1ª iniciando pelo caractere ‘S’ indica o status da transação;
- 2ª iniciada pelo caractere ‘C’ os dados do comprovante (valor, bandeira, nsu, etc.);
- 3ª iniciada por ‘I’ indica as linhas do comprovante a serem impressas. Uma observação será disponibilizada apenas uma via, a automação será responsável por imprimir quantas vias forem necessárias, e pelo menos duas devem ser impressas.

Na 1ª linha ‘S’, o segundo campo (S;1;Transacao Aceita.) pode ser retornado com os seguintes valores:

|   Valor | Descrição                        |
|---------|----------------------------------|
|       0 | Ocorreu um erro.                 |
|       1 | Possui cupom/comprovante gerado. |
|       2 | Não retornou cupom/comprovante.  |

Uma observação alguns campos como “Nome do Portador” podem ser retornados com uma string vazia em algumas situações.  Na utilização do cartão via *Contactless* existe outro possível retorno, o nome da tecnologia utilizada, em cartões Visa por exemplo é retornado PAYWAVE/VISA.


```txt{title="Exemplo de Retorno"}
  S;1;Transacao Aceita.
  C;321;100;VISA;REDE;906277;010836;10;1;010836;271705105;01425787000104;
  4589100000004700;ARAGAO/GUALTHER H;0125;;1234567
  I;"               REDECARD               "
  I;"     CREDITO OU PRE-AUT "
  I;"COMPROV: 003304581 "
  I;" VALOR RS:1.00 "
  I;"03.10.23-10:47:38"
  I;"ESTAB:011867493 XXXXXX       "
  I;"06.04.09-11:42:07 TERM:PV703148/000870"
  I;"CARTAO: XXXXXXXXXXXXXXXX              "
  I;"AUTORIZACAO: 326669                   "
  I;"ARQC:8F34589B6F23F4EF"
  I;"    TRANSACAO AUTORIZADA MEDIANTE     "
  I;"    USO DE SENHA PESSOAL.             "
  I;""
  I;"                                      "
  I;"                                  1234"
  I;"5678901234567890FIM"
  I;"        (NSU TEF   : 010836)"
  I;""
  I;""
  I;"CUPOM FISCAL: 321     PDV: 001"
  I;"CNPJ EMITENTE: 60.177.876/0001-30"
  I;"03/10/2023  10:47:46"
```

A segunda parte, que corresponde aos dados da transação possui os seguintes campos:

```txt {title="Campos da 2ª parte do Retorno"}
C;
[CUPOM];
[VALOR];
[NOME DA BANDEIRA];
[NOME DA REDE];
[AUTORIZACAO];
[NSU];
[TIPO TRANSAÇÃO];
[QUANTIDADE DE PARCELAS];
[CONTROLE];
[NSU HOST];
[CNPJ DA OPERADORA];
[NÚMERO CARTÃO];
[NOME DO PORTADOR];
[VALIDADE DO CARTÃO];
[TEXTO ESPECIAL DO CLIENTE];
[FINALIZAÇÃO/ID PGTO (Utilizado para PIX)]
```


## Descrição dos Campos
- | Campo                                    | Conteúdo / Descrição                                                                                                                                                                                                                                                                                           |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CUPOM                                    | Finalidade:  Indica o número de controle da solicitação que está sendo feita. Este número é gerado pelo aplicativo de automação comercial, o qual deverá colocar um conteúdo diferente a cada nova solicitação. Este mesmo conteúdo é devolvido nos arquivos de resposta.  Formato:  **Numérico de até 10 bytes**. |
| VALOR                                    | Finalidade: Valor total desta forma de pagamento. Formato: **Numérico de até 12 bytes**, sendo duas casas decimais sem a vírgula separadora.                                                                                                                                                                       |
| NOME DA BANDEIRA                         | Finalidade: Identifica a Administradora do Cartão utilizado. Formato: **Alfanumérico de até 12 bytes**. Conteúdos Válidos: ELECTRON MAESTRO MASTERCARD VISA ELO GOODCARD VALECARD Etc.                                                                                                                             |
| NOME DA REDE (OPERADORA/CREDENCIADORA)   | Finalidade: Nome da rede que tratou a transação. Formato: **Alfanumérico de até 16 bytes** Conteúdos Válidos: CIELO REDE GETNET BIN STONE etc.                                                                                                                                                                     |
| AUTORIZACAO                              | Finalidade: Indica o número de autorização da transação atribuída pelo Host. Cada transação TEF possui um número de autorização. Formato: **Alfanumérico de até 6 bytes**.                                                                                                                                         |
| NSU                                      | Finalidade: Indica o número de sequência da transação atribuído pelo TEF estabelecido para a transação, ele representa o NSU da transação a ser tratada (cancelada, confirmada etc.) Formato: **Alfanumérico de até 12 bytes**.                                                                                    |
| TIPO TRANSAÇÃO                           | Finalidade: Código identificando o tipo da transação executada. Formato: **Numérico de até 2 bytes**. Conteúdos Válidos: Consultar o item 7.2 Tipos de Transação                                                                                                                                                   |
| QUANTIDADE DE PARCELAS                   | Finalidade: Indica o número de parcelas no caso de transações Parceladas (Crédito ou Débito). Formato: **Numérico de até 2 bytes**.                                                                                                                                                                                |
| CONTROLE                                 | Finalidade: Código de controle de transação. Formato: **Alfanumérico de até 12 bytes**.                                                                                                                                                                                                                            |
| NSU HOST                                 | Finalidade: Indica o número de sequência (NSU – Número Sequencial Único) da transação atribuído pelo Host (Sistema das Redes de Cartão que recebe e trata as solicitações das transações TEF). Formato: **Alfanumérico de até 16 bytes**.                                                                          |
| CNPJ DA OPERADORA                        | Finalidade: Informar o CNPJ da rede que tratou a transação Formato: **Alfanumérico de 14 bytes**                                                                                                                                                                                                               |
| NÚMERO CARTÃO                            | Finalidade: Exibe o número do cartão que realizou a transação. Formato: **Numérico de 16 bytes**. Conteúdos Válidos: 0000000000000000 a 9999990000009999                                                                                                                                                           |
| NOME DO PORTADOR                         | Finalidade: Exibe o nome do portador do cartão que realizou a transação Formato: **Alfanumérico de até 20 bytes**.                                                                                                                                                                                                 |
| VALIDADE DO CARTÃO                       | Finalidade: Exibe o número de validade do cartão que realizou a transação Formato: **Numérico de 4 bytes**.                                                                                                                                                                                                        |
| TEXTO ESPECIAL DO CLIENTE                | Finalidade: Texto que, quando presente, deve ser apresentado pelo aplicativo de Automação Comercial ao cliente. Formato: **Alfanumérico de até 40 bytes**.                                                                                                                                                         |
| FINALIZAÇÃO/ID PGTO (Utilizado para PIX) | Finalidade: Dados recebidos do Módulo TEF que executou a transação.  Obs.: O campo ID PGTO **(E2E)** é retornado em transações com PIX. Essa informação é empregada em algumas UFs para a geração da NFCe (Nota Fiscal de Consumidor eletrônica).  Formato: **Alfanumérico de até 32 bytes**                                 |

## Tipos de Transação

Os tipos de transação realizados são retornados por meio de códigos na tabela a seguir estão os possíveis retornos.

|   Código | Descrição                                                        |
|----------|------------------------------------------------------------------|
|       00 | Administrativas – Outras (Reimpressão, inicialização de terminal |
|       01 | Administrativa – Fechamento/Transmissão de lote                  |
|       10 | Cartão de Crédito à Vista                                        |
|       11 | Cartão de Crédito Parcelado pelo Estabelecimento                 |
|       12 | Cartão de Crédito Parcelado pela Administradora                  |
|       13 | Pré-Autorização com Cartão de Crédito                            |
|       20 | Cartão de Débito à Vista                                         |
|       21 | Cartão de Débito Pré-Datado                                      |
|       22 | Cartão de Débito Parcelado                                       |
|       23 | Cartão de Débito à Vista Forçada                                 |
|       24 | Cartão de Débito Pré-Datado Forçada                              |
|       25 | Cartão de Débito Pré-Datado sem Garantia                         |
|       26 | Cartão de Débito Pré-Pago                                        |
|       27 | Cartão de Crédito Pré-Pago                                       |
|       30 | Outros cartões                                                   |
|       40 | CDC                                                              |
|       41 | Consulta CDC                                                     |
|       50 | Convênio                                                         |
|       60 | Voucher                                                          |
|       70 | Consulta Cheque                                                  |
|       71 | Garantia Cheque                                                  |
|       99 | Outras                                                           |
