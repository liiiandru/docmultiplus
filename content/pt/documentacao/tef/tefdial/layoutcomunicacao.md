---
title: "Layout das Mensagens"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 304
seo:
  title: "Layout das Mensagens" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
As mensagens de comunicação entre a Automação e o Gerenciador de Pagamentos (GP) possuem um layout definido, onde cada linha de um arquivo texto contém um tipo de informação.

As linhas possuem formato padrão e tamanho variável, utilizando o conceito de palavra chave.

```txt{title="Layout da linha"}
  AAA-BBBC = CDDDDDDD...DDDDDEF
```

| Posição             | Significado                                                                 |
|---------------------|------------------------------------------------------------------------------|
| `AAA`               | **Tipo de informação ou campo** (ex: "001" pode ser cabeçalho, "010" valor, etc.) |
| `-`                 | Separador fixo                                                               |
| `BBB`               | **Número de sequência complementar**, geralmente para repetir o mesmo tipo (ex: múltiplos itens). |
| `C`                 | **Espaço ou branco** (sempre deve existir, pode até ser um caractere `' '` ) |
| `DD...D`            | **Informação propriamente dita**, alinhada à esquerda, **sem preenchimento de zeros ou espaços**. |
| `E`                 | Fim da linha, composto por CR (Carriage Return = 13) e LF (Line Feed = 10)   |


## Tipos de Operações

Na tabela a seguir são descritas as operações possíveis de serem utilizadas para acionar o GP.

| Código | Descrição                                                                        |
|--------|----------------------------------------------------------------------------------|
| ATV    | Verifica se o Gerenciador de Pagamentos está ativo                               |
| ADM    | Permite o acionamento da Solução TEF para execução das funções administrativas   |
| CRT    | Iniciar transação por meio de cartão                                             |
| CPF    | Solicita para digitar o CPF na tela do PinPad                                    |
| CEL    | Iniciar transação de recarga de celular                                          |
| REL    | Permite o acionamento da Solução TEF para entrar no Relatório de Vendas          |
| CNC    | Cancelamento de venda efetuada por qualquer meio de pagamento                    |
| CNF    | Confirmação da venda e impressão de cupom                                        |
| NCN    | Não confirmação da venda e/ou da impressão (desfazimento)                        |


### Observações

#### GP Ativo

- Para verificar se o GP está ativo a Automação envia a mensagem contendo o comando **ATV** e deve aguardar até 7 segundos para receber o arquivo `C:\TEF_DIAL\Resp|IntPos.sts` indicando que o GP está ativo.

- Sempre que o GP abrir irá informar à Automação que está ativo através do arquivo `C:\TEF_DIAL\RespAtivo.001`, se o mesmo não existir significa que o Gerenciador de Pagamentos não está ativo.

```txt {title="Conteúdo do arquivo Ativo.001"}
  000-000 = TEF
  016-000 = DDMMHHMMSS (Timestamp)
  999-999 = 0
```

#### Cancelamento de Vendas

O cancelamento de uma venda pode ser realizado de duas formas:

- Pela operação **ADM**, onde o GP exibe uma tela para capturar os dados da transação a ser cancelada;
- Pela operação **CNC**, onde a Automação informa o GP os dados da transação a ser cancelada.


## Especificação dos Campos das Mensagens

A seguir serão apresentados os campos que devem estar presentes nos arquivos criados ou recebidos pela Automação. As informações variam de acordo com o tipo de operação e possuem a seguinte indicação **M** quando é mandatório, **O** quando é opcional e **-** quando ausente.


{{< callout context="note"  icon="outline/info-circle">}}
  É importante que o aplicativo de automação esteja preparado para reconhecer e ignorar campos desconhecidos. Essa abordagem garante maior robustez e compatibilidade com versões futuras, permitindo a evolução do sistema sem comprometer integrações já existentes.
{{< /callout >}}

{{< callout context="caution" icon="outline/alert-triangle" >}}
  Em caso de discrepância entre o valor do campo enviado pela Automação Comercial e o valor processado pelo Gerenciador de Pagamentos, **prevalecerá o conteúdo definido pelo Gerenciador de Pagamentos**.
{{< /callout >}}

### Mensagens de Solicitações da Automação

```txt {title="Arquivo de Solicitação"}
  C:\TEF_DIAL\REQ\IntPos.001
```

| Código do Campo | Tipo de Informação                                      | ATV | ADM | CRT | CNC | CNF | NCN | REL | CPF |
|-----------------|---------------------------------------------------------|-----|-----|-----|-----|-----|-----|-----|-----|
| 000-000         | HEADER                                                  | M   | M   | M   | M   | M   | M   | M   | M   |
| 001-000         | IDENTIFICAÇÃO                                           | M   | M   | M   | M   | M   | M   | M   | -   |
| 002-000         | DOCUMENTO FISCAL VINCULADO                              | -   | -   | O   | O   | O   | O   | -   | -   |
| 003-000         | VALOR TOTAL                                             | -   | -   | M   | M   | -   | -   | -   | -   |
| 004-000         | MOEDA                                                   | -   | -   | O   | -   | -   | -   | -   | -   |
| 010-000         | NOME DA REDE                                            | -   | -   | -   | M   | M   | M   | -   | -   |
| 012-000         | NÚMERO DA TRANSAÇÃO - NSU                               | -   | -   | -   | M   | M   | M   | -   | -   |
| 022-000         | DATA DA TRANSAÇÃO - COMPROVANTE                         | -   | -   | -   | M   | -   | -   | -   | -   |
| 023-000         | HORA DA TRANSAÇÃO - COMPROVANTE                         | -   | -   | -   | M   | -   | -   | -   | -   |
| 027-000         | FINALIZAÇÃO                                             | -   | -   | -   | -   | M   | M   | -   | -   |
| 250-000         | KM (VENDA FROTA)                                        | -   | -   | O   | -   | -   | -   | -   | -   |
| 250-001         | PLACA (VENDA FROTA)                                     | -   | -   | O   | -   | -   | -   | -   | -   |
| 250-002         | MATRICULA (VENDA FROTA)                                 | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-000         | QUANTIDADE DE ITENS                                     | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX1         | CÓDIGO DO PRODUTO                                       | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX2         | DESCRIÇÃO DO PRODUTO                                    | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX3         | QUANTIDADE                                              | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX4         | VALOR UNITARIO                                          | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX5         | DESCONTO                                                | -   | -   | O   | -   | -   | -   | -   | -   |
| 261-XX6         | VALOR TOTAL                                             | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-001         | TIPO DE TRANSAÇÃO                                       | -   | -   | O   | O   | -   | -   | -   | -   |
| 800-002         | FORMA DE PAGAMENTO                                      | -   | -   | O   | O   | -   | -   | -   | -   |
| 800-003         | JUROS DA TRANSAÇÃO                                      | -   | -   | O   | O   | -   | -   | -   | -   |
| 800-004         | QUANTIDADE DE PARCELAS                                  | -   | -   | O   | O   | -   | -   | -   | -   |
| 800-005         | CNPJ DO ESTABELECIMENTO                                 | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-006         | EQUIPAMENTO P/ RECEBIMENTO (PINPAD OU SmartPOS)        | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-007         | CÓDIGO DE CONTROLE (CANCELAMENTO DE VENDAS)            | -   | -   | O   | O   | -   | -   | -   | -   |
| 800-008         | EXIBE CONSULTA DE CPF                                  | -   | -   | O   | O   | -   | -   | -   | M   |
| 800-009         | NOME DA OPERADORA A SELECIONAR VIA (MULTWAY)           | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-010         | QUANTIDADE DE PARCELAS (LINK DE PAGAMENTO)             | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-011         | TELEFONE COM DD (LINK DE PAGAMENTO)                    | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-012         | TEXTO LIVRE (LINK DE PAGAMENTO)                        | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-013         | PARAMETRO SPLIT (MULTIPLUS PAY)                         | -   | -   | O   | -   | -   | -   | -   | -   |
| 800-014         | CAMPO LIVRE SPLIT (MULTIPLUS PAY)                       | -   | -   | O   | -   | -   | -   | -   | -   |
| 999-999         | TRAILER - REGISTRO FINAL                                | M   | M   | M   | M   | M   | M   | M   | M   |

**Legenda:**
- `M` = Mandatório
- `O` = Opcional
- `-` = Ausente

## Mensagens Geradas pelo Gerenciador de Pagamentos - Status

```txt {title="Arquivo de Status"}
  C:\TEF_DIAL\RESP\IntPos.Sts
```

| Código do Campo | Tipo de Informação        | ATV | ADM | CRT | CNC | CNF | NCN | REL |
|-----------------|---------------------------|-----|-----|-----|-----|-----|-----|-----|
| 000-000         | HEADER                    | M   | M   | M   | M   | M   | M   | M   |
| 001-000         | IDENTIFICAÇÃO             | M   | M   | M   | M   | M   | M   | M   |
| 999-999         | TRAILER – REGISTRO FINAL  | M   | M   | M   | M   | M   | M   | M   |

**Legenda:**
- `M` = Mandatório
- `O` = Opcional
- `-` = Ausente

## Mensagens Geradas pelo Gerenciador de Pagamentos - Respostas

```txt {title="Arquivo de Respostas"}
  C:\TEF_DIAL\RESP\IntPos.Sts
```

| Código do Campo | Tipo de Informação                                | ATV | ADM | CRT   | CNC   | REL | CPF |
|-----------------|---------------------------------------------------|-----|-----|-------|-------|-----|-----|
| 000-000         | HEADER                                            | -   | M   | M     | M     | M   | M   |
| 001-000         | IDENTIFICAÇÃO                                     | -   | M   | M     | M     | M   | -   |
| 002-000         | DOCUMENTO FISCAL VINCULADO                        | -   | O   | O(2)  | O(2)  | -   | -   |
| 003-000         | VALOR TOTAL                                       | -   | O   | M     | M     | -   | -   |
| 004-000         | MOEDA                                             | -   | O   | M(2)  | M(2)  | -   | -   |
| 009-000         | STATUS DA TRANSAÇÃO                               | -   | M   | M     | M     | M   | M   |
| 010-0X0         | NOME DA REDE (OPERADORA/CREDENCIADORA)            | -   | O   | M     | M     | -   | -   |
| 010-0X1         | CNPJ DA REDE (OPERADORA/CREDENCIADORA)            | -   | O   | O     | O     | -   | -   |
| 010-0X2         | CÓDIGO SAT DA REDE                                | -   | O   | O     | O     | -   | -   |
| 011-00X         | TIPO DA TRANSAÇÃO                                 | -   | O   | M     | M     | -   | -   |
| 012-00X         | NÚMERO DA TRANSAÇÃO - NSU                         | -   | O   | M(1)  | M(1)  | -   | -   |
| 013-00X         | CÓDIGO DE AUTORIZAÇÃO DA TRANSAÇÃO                | -   | O   | O     | O     | -   | -   |
| 014-000         | NÚMERO DO LOTE DA TRANSAÇÃO                       | -   | O   | O     | O     | -   | -   |
| 015-000         | TIMESTAMP DA TRANSAÇÃO - HOST                     | -   | O   | M(1)  | M(1)  | -   | -   |
| 016-000         | TIMESTAMP DA TRANSAÇÃO - LOCAL                    | -   | O   | O     | O     | -   | -   |
| 017-00X         | TIPO PARCELAMENTO                                 | -   | -   | O     | -     | -   | -   |
| 018-00X         | QUANTIDADE DE PARCELAS                            | -   | -   | O     | -     | -   | -   |
| 019-yyy         | DATA VENCIMENTO DA PARCELA                        | -   | -   | O     | -     | -   | -   |
| 020-yyy         | VALOR DA PARCELA                                  | -   | -   | O     | -     | -   | -   |
| 021-yyy         | NÚMERO DA TRANSAÇÃO – NSU DA PARCELA             | -   | -   | O     | -     | -   | -   |
| 022-000         | DATA DA TRANSAÇÃO - COMPROVANTE                   | -   | O   | M(1)  | M(1)  | -   | -   |
| 023-000         | HORA DA TRANSAÇÃO - COMPROVANTE                   | -   | O   | M(1)  | M(1)  | -   | -   |
| 024-000         | DATA PRÉ-DATADO                                   | -   | -   | O     | -     | -   | -   |
| 025-000         | NÚMERO DA TRANSAÇÃO CANCELADA - NSU              | -   | O   | -     | O     | -   | -   |
| 026-000         | TIMESTAMP DA TRANSAÇÃO CANCELADA                  | -   | O   | -     | M     | -   | -   |
| 027-000         | FINALIZAÇÃO                                       | -   | O   | M(1)  | M(1)  | -   | -   |
| 028-000         | QUANTIDADE TOTAL DE LINHAS DO COMPROVANTE (*)    | -   | M   | M     | M     | M   | M   |
| 028-001         | RETORNA A ÚLTIMA LINHA DO 1º COMPROVANTE          | -   | O   | O     | O     | O   | -   |
| 029-yyy         | IMAGEM DE CADA LINHA DO COMPROVANTE              | -   | O   | O     | O     | -   | -   |
| 030-000         | TEXTO ESPECIAL OPERADOR                           | -   | O   | O     | O     | M   | O   |
| 031-000         | TEXTO ESPECIAL CLIENTE                            | -   | O   | O     | O     | -   | -   |
| 040-00X         | NOME DA ADMINISTRADORA (BANDEIRA)                 | -   | O   | O     | O     | -   | -   |
| 170-000         | NÚMERO DA TRANSAÇÃO – NSU HOST                    | -   | O   | O     | O     | -   | -   |
| 740-00X         | NÚMEROS DO CARTÃO UTILIZADO NA TRANSAÇÃO         | -   | -   | M     | -     | -   | -   |
| 741-00X         | NOME DO PORTADOR DO CARTÃO                        |     |     |       |       | M   | -   |
| 742-00X         | VALIDADE DO CARTÃO DO PORTADOR                   |     |     |       |       | M   | -   |
| 743-000         | RESTANTE A SER PAGO (USO DE MULTI CARTÕES)        | -   | -   | O     | -     | -   | -   |
| 999-999         | TRAILER - REGISTRO FINAL                          | -   | M   | M     | M     | M   | M   |

**Legenda:**
- `M` = Mandatório
- `O` = Opcional
- `-` = Ausente
- `(1)`, `(2)` = Dependências ou condições específicas (ver a seguir)


{{< callout context="note" title="Observações" icon="outline/info-circle">}}
(1) Estes campos somente existem em transações aprovadas.

(2) Somente estarão preenchidos se a informação for fornecida no arquivo de envio correspondente.

(*) O campo 028-000 QUANTIDADE DE LINHAS DO COMPROVANTE está como obrigatório para todas as operações, por uma medida de segurança para o Aplicativo de Automação Comercial. O Gerenciador Padrão preencherá este campo com zero (0) quando não houver comprovante.

(X) Quando utilizado, o **X** refere-se à possibilidade de variação de número de 0 a 9, que muda de acordo com a quantidade de cartões na transação.
{{< /callout >}}
