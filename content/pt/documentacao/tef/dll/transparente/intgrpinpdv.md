---
title: "Integração com App PinPDV Lite"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 107
seo:
  title: "Integração com App PinPDV Lite" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

O app PinPdv Lite da Multiplus Card para SmartPOS está integrado à DLL TefClientMC, permitindo o uso de diversas funcionalidades. Em uma automação já homologada sua implementação exige poucos ajustes – ou até nenhum, dependendo da forma de utilização desejada. A seguir apresentamos os passos para a utilização.

## Configuração

Para utilizar o PinPdv Lite é necessário primeiramente obter o token do cliente, consulte a Multiplus Card para verificar como o mesmo deve ser gerado. Em seguida o mesmo deve ser colocado no arquivo **CliMC.ini** no parâmetro **TokenPinPdvLite**.

```txt{title="CliMC.ini"}
  TokenPinPdvLite=eyJOb3RoaW5nSXMlUmVhZGFibGUiOiJUcnVlIn0.KXpzVlJYZU1qUldXYm5aT3N3V1dLaVJYUnNVZy1QbFRQZ05uSldXbE9UQnlXSFZUY3pFNU1qWnZOV1JIV1ZKSGFXUkpVR2xPVUVGSlFXRlpkRkZW
  PosTef=1
```

Após esta etapa deve ser decidido o modo de funcionamento da integração através do parâmetro PosTef, por default ele é desativado. A seguir os possíveis valores para configurar este parâmetro.

|   Valor | Descrição                              |
|---------|----------------------------------------|
|       0 | Desativada a utilização do PinPdv Lite |
|       1 | Habilitada a utilização                |
|       2 | Modo POS TEF                           |

## Utilização

Para utilizar esta integração existem dois modos possíveis: através das funções já existentes para crédito, débito, voucher e pix ou de forma direta.

No fluxo já existente deve ser habilitado o parâmetro PosTef com o valor 1, assim será retornado um menu para decidir onde a transação será realizada, em um Pin Pad ou em um POS através da função POS TEF.

```txt{title="Exemplo de Retorno"}
  [MENU]#SELECIONE O DISPOSITIVO PARA REALIZAR A TRANSACAO#1,1-PINPAD|2,2-POS TEF
```

Selecionada a opção POS TEF será questionado em qual SmartPos a transação dará continuidade.

```txt{title="Exemplo de Retorno"}
  [MENU]#SELECIONE O POS PARA REALIZAR A TRANSACAO#1,1-MULTIPLUS PAY CAIXA1|2,2-MULTIPLUS PAY CAIXA2|3,3-REDE DELIVERY
```

Após a seleção o fluxo de transação ocorrerá normalmente da mesma forma como nas outras transações com o TEF.

Se desejar realizar a transação diretamente no SmartPos é possível utilizar a operação 401, assim não será exibido o primeiro menu; haverá outro solicitando qual tipo de transação a ser realizada (Crédito a vista/parcelado, Débito e Voucher).

A seguir os códigos de operação para utilizar com a função POS TEF do PinPDV.

|   Código | Descrição                                                                                                                                         |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|      401 | Realiza uma transação com a função POS TEF do PinPdv Lite                                                                                         |
|      402 | Consulta o status de uma transação                                                                                                                |
|      403 | Realiza o cancelamento do processo de transação, somente se a mesma não foi iniciada no POS, se ocorreu essa situação, deve ser cancelada no POS. |

## Modo Exclusivo POS TEF

Com a utilização deste modo é possível utilizar a automação sem a necessidade de um Pin Pad instalado, somente com o uso SmartPos para realizar as transações. Quando ativado as operações a serem utilizadas, serão as mesmas do uso normal (ex: 0 – Crédito à vista, 4 – Débito).

## Pré-venda

A Pré-venda é um recurso disponível para iniciar uma venda a partir da automação e concluir posteriormente; sendo possível informar qual será a forma de pagamento a ser utilizada previamente ou no momento do pagamento. Uma característica é a impossibilidade de alterar o seu valor.

### Criação de uma Pré-venda

Para criar uma deve ser utilizada a operação 404, os parâmetros a serem informados serão os mesmos já utilizados na função IniciaFuncaoMCInterativo; a quantidade de parcelas pode ser informada neste momento, o cupom fiscal não deve se repetir a cada chamada, pois o mesmo compõe o identificador da transação. Em seguida ao iniciar o loop com a função AguardaFluxoMCInterativo, será questionada a forma de pagamento, parcelas (caso não tenha sido informado), descrição da venda, após isso será emitido um comprovante da Pré-venda realizada. Não é necessário realizar a confirmação desta transação.

```txt{title="Exemplo de Retorno"}
  [MENU]#INFORME O TIPO DE PGTO#1,1-CREDITO A VISTA|2,2-CREDITO PARCELADO|3,3-DEBITO|4,4-SELECIONAR NO POS
```

```txt{title="Exemplo de Retorno"}
  [PERGUNTA]#INFORME UMA DESCRICAO PARA A PRE-VENDA##0#0#0,00#0,00#0
```

{{< callout note >}}
Uma observação a ser feita é relativa aos tipos de transação aceitos que são: crédito a vista, crédito parcelado (loja), débito à vista, voucher e seleção no POS.
{{< /callout >}}

### Consulta de Status de uma Pré-venda

Para consultar o status de uma Pré-venda deve ser utilizada a operação 405, os parâmetros a serem informados serão os mesmos utilizados na função IniciaFuncaoMCInterativo no momento da criação da Pré-venda, em seguida deve ser iniciado o loop até retornar o status da transação no seguinte formato:

```txt{title="Exemplo de Retorno"}
  [RETORNO]#IDENTIFICADOR=20250325-60-001|VALOR=50,00|STATUS=AGUARDANDO
```

### Cancelamento de uma Pré-venda

Para cancelar uma Pré-venda deve ser utilizada a operação 406, os parâmetros a serem informados serão os mesmos utilizados na função IniciaFuncaoMCInterativo no momento da criação da Pré-venda, em seguida deve ser iniciado o loop até obter o seguinte retorno:

```txt{title="Exemplo de Retorno"}
  [RETORNO]#IDENTIFICADOR=20250325-60-001|VALOR=50,00|STATUS=REMOVIDA
```

{{< callout note >}}
Somente será possível realizar o cancelamento de uma Pré-venda caso a mesma esteja com o status “AGUARDANDO” ou seja, ainda não paga.
{{< /callout >}}


