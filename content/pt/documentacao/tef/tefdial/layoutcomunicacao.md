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
