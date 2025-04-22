---
title: "Descrição dos Campos"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 408
seo:
  title: "Descrição dos Campos" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

| Campo | Conteúdo / Descrição |
|-------|----------------------|
| 000-000 | **HEADER** <br><br> **Finalidade:** <br> Indica o início do arquivo e o tipo de operação relacionada ao arquivo. <br><br> **Formato:** <br> Alfanumérico de 3 bytes. <br><br> **Conteúdos válidos:** <br> ATV - Verifica se o Gerenciador Padrão está ativo <br> ADM - Administrativa <br> REL - Relatório <br> CRT - Cartão <br> CPF - Solicita o CPF na tela do pinpad <br> CEL - Recarga de Celular <br> CNC - Cancelamento <br> CNF - Confirmação de finalização da venda e impressão do cupom <br> NCN - Não confirmação de finalização da venda por desistência do cliente ou erro na impressão do cupom. |
| 001-000 | **IDENTIFICAÇÃO** <br><br> **Finalidade:** <br> Indica o número de controle da solicitação que está sendo feita. Este número é gerado pelo aplicativo de automação comercial, o qual deverá colocar um conteúdo diferente a cada nova solicitação. Este mesmo conteúdo é devolvido nos arquivos de resposta. <br><br> **Formato:** <br> Numérico de até 10 bytes. |
| 002-000 | **DOCUMENTO FISCAL VINCULADO** <br><br> **Finalidade:** <br> Número do documento fiscal vinculado à forma de pagamento ou finalização. <br><br> **Formato:** <br> Numérico de até 12 bytes. |
| 003-000 | **VALOR TOTAL** <br><br> **Finalidade:** <br> Valor total desta forma de pagamento. <br><br> **Formato:** <br> Numérico de até 12 bytes, sendo duas casas decimais sem a virgula separadora |
| 004-000 | **MOEDA** <br><br> **Finalidade:** <br> Indica a moeda utilizada na operação <br><br> **Formato:** <br> Numérico de 1 byte <br><br> **Conteúdos Válidos:** <br> 0 -- para Real <br> 1 -- para Dólar |
| 009-000 | **STATUS DA TRANSAÇÃO** <br><br> **Finalidade:** <br> Indica se a transação foi aprovada ou recusada e qual o motivo da recusa. <br><br> **Formato:** <br> Alfanumérico de até 3 bytes. <br><br> **Conteúdos Válidos:** <br> 0 -- para transação aprovada <br> Outro valor -- transação negada |
| 010-0X0 | **NOME DA REDE (OPERADORA/CREDENCIADORA)** <br><br> **Finalidade:** <br> Nome da rede que tratou a transação <br><br> **Formato:** <br> Alfanumérico de até 16 bytes <br><br> **Conteúdos Válidos:** <br> CIELO <br> REDE <br> GETNET <br> BIN <br> STONE <br> etc. |
| 010-0X1 | **CNPJ DA REDE (OPERADORA/CREDENCIADORA)** <br><br> **Finalidade:** <br> Informar o CNPJ da rede que tratou a transação <br><br> **Formato:** <br> Alfanumérico de até 14 bytes |
| 010-0X2 | **CÓDIGO SAT DA REDE** <br><br> **Finalidade:** <br> Informar o código SAT da rede que tratou a transação <br><br> **Formato:** <br> Alfanumérico de até 3 bytes |
| 011-00X | **TIPO DA TRANSAÇÃO** <br><br> **Finalidade:** <br> Código identificando o tipo da transação executada <br><br> **Formato:** <br> Numérico de até 2 bytes. <br><br> **Conteúdos Válidos:** <br> 00- Administrativas -- Outras (Reimpressão, Iniciação de Terminal etc.) <br> 01- Administrativa -- Fechamento/Transmissão de Lote <br> 10- Cartão de Crédito à Vista <br> 11- Cartão de Crédito Parcelado pelo Estabelecimento <br> 12- Cartão de Crédito Parcelado pela Administradora <br> 13- Pré-Autorização com Cartão de Crédito <br> 20- Cartão de Débito à Vista <br> 21- Cartão de Débito Pré-Datado <br> 22- Cartão de Débito Parcelada <br> 23- Cartão de Débito à Vista Forçada <br> 24- Cartão de Débito Pré-Datado Forçada <br> 25- Cartão de Débito Pré-Datado sem Garantia <br> 26- Cartão de Débito Pré-Pago <br> 27- Cartão de Crédito Pré-Pago <br> 30- Outros Cartões <br> 40- CDC <br> 41- Consulta CDC <br> 50- Convênio <br> 60- Voucher <br> 99- Outras <br><br> **Obs.:** No cancelamento de venda feito através da operação ADM (Administrativa) este campo conterá o tipo da transação de venda que foi cancelada. <br><br> Exemplo-1: 000-000 = ADM <br> 011-000 = 00 (Administrativas: Reimpressão, Iniciação de Terminal etc.) <br><br> Exemplo-2: 000-000 = ADM <br> 011-000 = 01 (Administrativa: Fechamento/Transmissão de Lote) <br><br> Exemplo-3: 000-000 = ADM <br> 011-000 = 03 (Cancelamento Genérico (Crédito, Débito, Private Label, qualquer que tenha sido a modalidade: à Vista, Parcelado pelo Estabelecimento ou pela Administradora)) <br><br> Exemplo-4: 000-000 = ADM <br> 011-000 = 10 (Cancelamento de Cartão de Crédito, qualquer que tenha sido a modalidade: à Vista, Parcelado pelo Estabelecimento ou pela Administradora) <br><br> Exemplo-5: 000-000 = ADM <br> 011-000 = 20 (Cancelamento de Cartão de Débito, qualquer que tenha sido a modalidade: à Vista, Pré-Datado, Parcelado etc.) |
| 012-00X | **NÚMERO DA TRANSAÇÃO - NSU** <br><br> **Finalidade:** <br> Indica o número de sequência (NSU -- Número Sequencial Único) da transação atribuído pelo Host (Sistema das Redes de Cartão que recebe e trata as solicitações das transações TEF). <br><br> Quando este campo é enviado do Gerenciador Padrão para o Aplicativo de Automação Comercial, ele representa o NSU do Host estabelecido para a transação. <br><br> Quando este campo é enviado do Aplicativo de Automação Comercial para o Gerenciador Padrão, ele representa o NSU da transação a ser tratada (cancelada, confirmada etc.) <br><br> **Formato:** <br> Alfanumérico de até 12 bytes. |
| 013-00X | **CÓDIGO DE AUTORIZAÇÃO DA TRANSAÇÃO** <br><br> **Finalidade:** <br> Indica o número de autorização da transação atribuída pelo Host. <br><br> Cada transação TEF possui um número de autorização. <br><br> **Formato:** <br> Alfanumérico de até 6 bytes. |
| 014-000 | **NÚMERO DO LOTE DA TRANSAÇÃO** <br><br> **Finalidade:** <br> Indica o número de lote da transação <br><br> **Formato:** <br> Alfanumérico de até 10 bytes. |
| 015-000 | **TIMESTAMP DA TRANSAÇÃO HOST** <br><br> **Finalidade:** <br> Indica a data e hora da transação no Host <br><br> **Formato:** <br> Numérico -- DDMMHHMMSS |
| 016-000 | **TIMESTAMP DA TRANSAÇÃO LOCAL** <br><br> **Finalidade:** <br> Indica a data e hora da transação no ponto de venda. <br><br> **Formato:** <br> Numérico -- DDMMHHMMSS |
| 017-00X | **TIPO PARCELAMENTO** <br><br> **Finalidade:** <br> Indica o tipo de parcelamento aplicado à operação <br><br> **Formato:** <br> Numérico de 1 byte <br><br> **Conteúdos Válidos:** <br> 0 -- Parcelado estabelecimento <br> 1 -- Parcelado administradora |
| 018-00X | **QUANTIDADE DE PARCELAS** <br><br> **Finalidade:** <br> Indica o número de parcelas no caso de transações Parceladas (Crédito ou Débito). <br><br> **Formato:** <br> Numérico de até 2 bytes. |
| 019-yyy | **DATA VENCIMENTO DA PARCELA** <br><br> **Finalidade:** <br> Indica a data em que foi agendada a parcela (yyy). A parcela é indicada pelos três caracteres (yyy) do campo correspondente, onde yyy será igual a 001, 002, ... respectivamente para cada uma das parcelas definidas no campo QUANTIDADE DE PARCELAS. <br><br> **Formato:** <br> Numérico -- DDMMAAAA |
| 020-yyy | **VALOR DA PARCELA** <br><br> **Finalidade:** <br> Indica o valor da parcela (yyy). A parcela é indicada pelos três caracteres (yyy) do campo correspondente, onde yyy será igual a 001, 002, ... respectivamente para cada uma das parcelas definidas no campo QUANTIDADE DE PARCELAS. <br><br> **Formato:** <br> Numérico de até 12 bytes, sendo duas casas decimais sem a vírgula separadora. |
| 021-yyy | **NÚMERO DA TRANSAÇÃO -- NSU DA PARCELA** <br><br> **Finalidade:** <br> Indica o NSU da parcela (yyy). A parcela é indicada pelos três caracteres (yyy) do campo correspondente, onde yyy será igual a 001, 002, ... respectivamente para cada uma das parcelas definidas no campo QUANTIDADE DE PARCELAS. <br><br> **Formato:** <br> Numérico de até 12 bytes. |
| 022-000 | **DATA DA TRANSAÇÃO - COMPROVANTE** <br><br> **Finalidade:** <br> Indica a data da transação <br><br> **Formato:** <br> Numérico -- DDMMAAAA |
| 023-000 | **HORA DA TRANSAÇÃO (LOCAL) - COMPROVANTE** <br><br> **Finalidade:** <br> Indica a hora da transação <br><br> **Formato:** <br> Numérico -- HHMMSS |
| 024-000 | **DATA PRÉ-DATADO** <br><br> **Finalidade:** <br> Contém data de agendamento para pré-datado. <br><br> **Formato:** <br> Numérico -- DDMMAAAA |
| 025-000 | **NÚMERO DA TRANSAÇÃO CANCELADA - NSU** <br><br> **Finalidade:** <br> Número de sequência (NSU) da transação cancelada. <br><br> **Formato:** <br> Numérico de até 12 bytes. |
| 026-000 | **TIMESTAMP DA TRANSAÇÃO CANCELADA - Host** <br><br> **Finalidade:** <br> Contém a data e hora da transação cancelada no Host. <br><br> **Formato:** <br> Numérico -- DDMMHHMMSS |
| 027-000 | **FINALIZAÇÃO** <br><br> **Finalidade:** <br> Dados recebidos do Módulo TEF que executou a transação e que devem ser devolvidos no comando de finalização de uma venda <br><br> **Formato:** <br> Alfanumérico de até 32 bytes |
| 028-000 | **QUANTIDADE DE LINHAS DO COMPROVANTE** <br><br> **Finalidade:** <br> Indica a quantidade de linhas do comprovante. Quando não há comprovante o campo deve conter zero. <br><br> **Formato:** <br> Numérico de até 3 bytes |
| 028-001 | **QUANTIDADE DE LINHAS DO 1º COMPROVANTE** <br><br> **Finalidade:** <br> Indica a quantidade de linhas do 1º comprovante (Caso possuir 2 vias, o corte será realizado após o número indicado nesse campo). Para retornar esse campo, precisa alterar o Layout de Integração para "TEF_DIAL (c/ linha 028-001)" nas configurações do Client TEF, na aba "Parâmetros". <br><br> **Formato:** <br> Numérico de até 3 bytes |
| 029-yyy | **IMAGEM DE CADA LINHA DO COMPROVANTE** <br><br> **Finalidade:** <br> Apresenta a imagem a ser impressa de cada uma das linhas do comprovante. A linha de impressão é indicada pelos três caracteres (yyy) do campo correspondente, onde yyy será igual a 001, 002, ... respectivamente para cada uma das linhas definidas no campo QUANTIDADE DE LINHAS DO COMPROVANTE. <br><br> **Formato:** <br> Alfanumérico de até 40 bytes. |
| 030-000 | **TEXTO ESPECIAL OPERADOR** <br><br> **Finalidade:** <br> Texto que, quando presente, deve ser apresentado pelo aplicativo de Automação Comercial ao operador. <br><br> **Formato:** <br> Alfanumérico de até 40 bytes. |
| 031-000 | **TEXTO ESPECIAL CLIENTE** <br><br> **Finalidade:** <br> Texto que, quando presente, deve ser apresentado pelo aplicativo de Automação Comercial ao cliente. <br><br> **Formato:** <br> Alfanumérico de até 40 bytes. |
| 040-00X | **NOME DA ADMINISTRADORA (BANDEIRA)** <br><br> **Finalidade:** <br> Identifica a Administradora do Cartão utilizado. <br><br> **Formato:** <br> Alfanumérico de até 12 bytes. <br><br> **Conteúdos Válidos:** <br> ELECTRON <br> MAESTRO <br> MASTERCARD <br> VISA <br> ELO <br> GOODCARD <br> VALECARD <br> etc. |
| 740-00X | **NÚMEROS DO CARTÃO** <br><br> **Finalidade:** <br> Exibe o número do cartão que realizou a transação. <br><br> **Formato:** <br> Numérico de 16 bytes. <br><br> **Conteúdos Válidos:** <br> 0000000000000000 a 9999990000009999 |
| 741-00X | **NOME DO PORTADOR DO CARTÃO** <br><br> **Finalidade:** <br> Exibe o nome do portador do cartão que realizou a transação <br><br> **Formato:** <br> Alfanumérico de até 20 bytes. |
| 742-00X | **VALIDADE DO CARTÃO DO PORTADOR** <br><br> **Finalidade:** <br> Exibe o número de validade do cartão que realizou a transação <br><br> **Formato:** <br> Numérico de 4 bytes. |
| 743-00X | **VALOR RESTANTE** <br><br> **Finalidade:** <br> Exibe o valor restante à ser pago quando utilizada a função de múltiplos cartões. <br><br> **Formato:** <br> Numérico de até 12 bytes, sendo duas casas decimais sem a virgula separadora. |
| 999-999 | **TRAILER - REGISTRO FINAL** <br><br> **Finalidade:** <br> Indica fim do arquivo. <br><br> **Formato:** <br> Numérico de 1 byte -- Constante zero |

## Campos Opcionais - Integração de forma direta -- Requisição com definições da transação

| Campo | Conteúdo / Descrição |
|-------|----------------------|
| 250-000 | **Conteúdos Válidos:** <br> KM (Venda Frota) |
| 250-001 | **Conteúdos Válidos:** <br> Placa (Venda Frota) |
| 250-002 | **Conteúdos Válidos:** <br> Matrícula (Venda Frota) |
| 261-000 | **Conteúdos Válidos:** <br> Quantidade de Itens |
| 261-011 | **Conteúdos Válidos (ITEM 01):** <br> Código do Produto |
| 261-012 | **Conteúdos Válidos (ITEM 01):** <br> Descrição do Produto |
| 261-013 | **Conteúdos Válidos (ITEM 01):** <br> Quantidade (3 Casas decimais) |
| 261-014 | **Conteúdos Válidos (ITEM 01):** <br> Valor Unitário (3 Casas decimais) |
| 261-015 | **Conteúdos Válidos (ITEM 01):** <br> Desconto (2 Casas decimais) (Venda Frota) |
| 261-016 | **Conteúdos Válidos (ITEM 01):** <br> Valor Total (2 Casas decimais) |
| 261-021 | **Conteúdos Válidos (ITEM 02):** <br> Código do Produto |
| 261-022 | **Conteúdos Válidos (ITEM 02):** <br> Descrição do Produto |
| 261-023 | **Conteúdos Válidos (ITEM 02):** <br> Quantidade (3 Casas decimais) |
| 261-024 | **Conteúdos Válidos (ITEM 02):** <br> Valor Unitário (3 Casas decimais) |
| 261-025 | **Conteúdos Válidos (ITEM 02):** <br> Desconto (2 Casas decimais) (Venda Frota) |
| 261-026 | **Conteúdos Válidos (ITEM 02):** <br> Valor Total (2 Casas decimais) |
| 261-XX1 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Código do Produto |
| 261-XX2 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Descrição do Produto |
| 261-XX3 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Quantidade (3 Casas decimais) |
| 261-XX4 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Valor Unitário (3 Casas decimais) |
| 261-XX5 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Desconto (2 Casas decimais) (Venda Frota) |
| 261-XX6 | **Conteúdos Válidos (ITEM XX -- Alterar o 'XX' pelo número do item de 01 até 99):** <br> Valor Total (2 Casas decimais) |
| 800-001 | **Conteúdos Válidos:** <br> 0 = Crédito <br> 1 = Débito <br> 2 = Private Label <br> 3 = Débito CDC <br> 4 = Link de Pagamento <br> 5 = Pix <br> 6 = Lista de Link de Pagamento <br> 7 = Parcele Mais |
| 800-002 | **Conteúdos Válidos:** <br> 0 = A vista <br> 1 = Parcelado |
| 800-003 | **Conteúdos Válidos:** <br> 0 = Parcelado ADM (Juros da transação cobrado do dono do cartão) <br> 1 = Parcelado Loja (Juros da transação cobrado da loja) |
| 800-004 | **Conteúdos Válidos:** <br> (Qtde. de parcelas) |
| 800-005 | **Conteúdos Válidos:** <br> (CNPJ da empresa) |
| 800-006 | **Conteúdos Válidos:** <br> 1 = PINPAD (Finalização de vendas através do equipamento pinpad) <br> 2 = CIELO LIO (Finalização de vendas através do equipamento Cielo Lio) <br> 3 = PINPDV (Finalização de vendas através do aplicativo PINPDV no SmartPOS) |
| 800-007 | **Conteúdos Válidos:** <br> (Código de controle de transação) |
| 800-008 | **Conteúdos Válidos:** <br> (0-Exibe tela para consultar CPF / 1-Solicita CPF somente no pinpad) |
| 800-009 | **Conteúdos Válidos:** <br> Texto Livre (Link de Pagamento) |
| 800-010 | **Conteúdos Válidos:** <br> Quantidade das parcelas (Link de Pagamento) |
| 800-011 | **Conteúdos Válidos:** <br> Telefone com DDD (Link de Pagamento) |
| 800-012 | **Conteúdos Válidos:** <br> Texto Livre (Link de Pagamento) |
| 800-013 | **Conteúdos Válidos:** <br> Parâmetros Split, válido somente para Multiplus Pay |
| 800-014 | **Conteúdos Válidos:** <br> Campo Livre Split, válido somente para Multiplus Pay |
