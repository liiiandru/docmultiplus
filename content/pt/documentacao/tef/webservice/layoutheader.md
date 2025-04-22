---
title: "Layout do Header CONTEUDO"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 406
seo:
  title: "Layout do Header CONTEUDO" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
O Header ‘CONTEUDO’ é o responsável por enviar os parâmetros necessários para iniciar transações, realizar cancelamentos, confirmações, operações administrativas, etc.

As informações são enviadas com uso do caractere ‘¬’ como separador; uma observação a ser feita é a respeito de linguagens/frameworks que sigam a RFC 7230 (ex.: .Net Core), desta forma caracteres não-ASCII, não são permitidos no header de uma requisição, para isso é necessário utilizar sua versão codificada: ‘**&amp;#172;**’.
O formado da string a ser enviada segue o formato genérico demonstrado abaixo:

```txt {title="Layout da string"}
AAA-BBBC=CDDDDDDD...DDDDDDEF
```

| Campo       | Descrição                                                                 |
|-------------|---------------------------------------------------------------------------|
| AAA         | Identificação do tipo de informação ou campo                              |
| BBB         | Nº de sequência complementar ao tipo de informação ou campo               |
| C           | Posição contendo espaço em branco                                          |
| DDD...DDD   | Informação (sempre alinhada à esquerda, sem preenchimento de zeros ou espaços) |
| E           | Carriage Return (CR), caractere 13 da tabela ASCII                        |
| F           | Line Feed (LF), caractere 10 da tabela ASCII                              |


A seguir o exemplo do header **CONTEUDO** de uma requisição com o comando **ATV**, utilizando o separados sem com dificação e com codificação HTML:

```txt {title="Exemplos do comando ATV"}
"000-000 = ATV¬001-000 = 1¬999-999 = 0"
"000-000 = ATV&#172;1001-000 = 1&#172;999-999 = 0"

```

## Operações Disponíveis

| Operação | Descrição                                                                 |
|----------|---------------------------------------------------------------------------|
| ADM      | Aciona a solução TEF para execução de funções administrativas.            |
| ATV      | Verifica se o Gerenciador Padrão está ativo.                              |
| REL      | Aciona a Solução TEF para entrar no Relatório de Vendas.                  |
| CRT      | Pedido de autorização para transação por meio de Cartão                   |
| CPF      | Solicita para digitar o CPF na tela do PinPad                             |
| CEL      | Pedido de autorização para transação de recarga de celular                |
| CNC      | Cancelamento de venda feita por qualquer meio de pagamento                |
| CNF      | Confirmação da venda e impressão do comprovante                           |
| NCN      | Não confirmação de finalização da venda por desistência do cliente ou erro na impressão do cupom.                           |


### Observações

- O Cancelamento de uma venda pode ser realizado de duas formas distintas, através da operação **ADM**, onde o Gerenciador de Pagamentos realizará a captura dos dados da transação a ser cancelada; ou pela operação **CNC**, onde a automação captura as informações da transação e envia para o Gerenciador de Pagamentos.

- Antes de iniciar uma transação é recomendado que seja executada a operação ATV, para verificar se o Gerenciador Padrão está ativo e conseguirá dar prosseguimento ao processo, caso isso não seja realizado, a automação pode ficar aguardando uma resposta que não será retornada.

- Após realizar uma requisição no `SetVendaTef` com a operação **CNF**, não é necessário fazer outra requisição no `GetVendasTef`.
