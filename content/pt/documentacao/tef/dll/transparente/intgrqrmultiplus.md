---
title: "Integração com QRMultiplus (PIX)"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 106
seo:
  title: "Integração com QRMultiplus (PIX)" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A realização de transações com o QR Multiplus (PIX) segue um fluxo semelhante ao realizado nas transações com cartão. São utilizadas as mesmas funções, diferenciando os códigos de operação (não são os mesmos da transação com cartão) e um parâmetro que deve ser configurado no arquivo CliMC.ini.

### Configuração

Antes da utilização é necessária a realização da configuração do modo de utilização, ou seja, a forma de exibição ou retorno dos dados da transação juntamente com o QR Code. O modo de utilização é escolhido alterando o parâmetro ExibirQrCode no arquivo CliMC.ini, na tabela a seguir estão os códigos e a descrição do modo de exibição. Uma observação deve ser especificada a porta utilizada no Pinpad, e não utilizado o modo procurar automático.

|   Código | Exibição do QR Code                                                                  |
|----------|--------------------------------------------------------------------------------------|
|        0 | Retorno por string para a automação realizar a exibição em uma janela personalizada. |
|        1 | Exibe no Pinpad e em uma janela padrão.                                              |
|        2 | Exibe apenas na janela padrão.                                                       |
|        3 | Exibe apenas no Pinpad.                                                              |

#### Parâmetro de Timeout para Exibição do QR Code no Pinpad

Se optar pela exibição no Pinpad, é opcional configurar o tempo de refresh do QR Code na tela do mesmo, com o parâmetro TimeoutPinPad=N. A partir da versão 1.2023.12.230 do ClientD.exe default é 8 segundos, sendo desaconselhado tempos inferiores a 3 segundos, pois pode comprometer a leitura do QR Code pelo cliente. A alteração desse parâmetro pode resultar em uma otimização do tempo necessário para a conclusão de uma transação. Caso clientes estejam enfrentando dificuldades na leitura o tempo pode ser aumentado.

#### Parâmetro de Retorno da String do QR Code

Para que a automação obtenha a string contendo o QR Code do PIX nos modos de exibição 1, 2 e 3; deve ser adicionado no arquivo CliMC.ini o seguinte parâmetro:
```txt{title="CliMC.ini"}
  RetornaStrQrCode=SIM.
```

Após iniciar o fluxo de transação, assim que for gerada a cobrança a string será retornada para a automação para a automação no formato abaixo. Em seguida, será exibido o QR Code no local configurado.

```txt{title="Exemplo da string retornada"}
  [MSG]#NSU=900011802|ORIGEM=GERENCIANET|VALOR=20,00|
  QRCODE=00020101021226830014BR.GOV.BCB.PIX2561qrcodespix.sejaefi.com.br/v2/0f90f0af8523422d9f8025908a127777204000053099865802BR5905EFISA6008SAOPAULO62070000***6304FFE6
```


**Observação**: A string será retornada apena uma vez.

### Utilizando as Funções do QR Multiplus

Para realizar uma transação, estorno ou remoção da cobrança deve ser chamada a função IniciaFuncaoMCInterativo, passando como código de operação que serão descritos a seguir. Deve ser mantido o mesmo fluxo de leitura e respostas realizado em uma transação de cartão utilizando as funções AguardaFuncaoMCInterativo e ContinuaFuncaoMCInterativo; ao final deve ser chamada a função FinalizaFuncaoMCInterativo, respeitando os códigos 98 se foi realizada e 99 se foi cancelada. Os códigos de erro são os mesmos descritos em seções anteriores.

#### Códigos das Funções

Para utilizar o QR Multiplus os códigos das operações são diferentes dos utilizadas para uma transação com TEF.

| Código | Função                                                      |
|--------|-------------------------------------------------------------|
|     50 | Retorna um Menu com as opções de PSP para realizar a cobrança  |
|     51 | Realiza a cobrança com o PSP padrão configurado para o Cliente |
|     52 | Realiza a Cobrança com Mercado Pago                            |
|     53 | Realiza a Cobrança com PicPay                                  |
|     54 | Cancelar/Estorno Transação                                     |
|     55 | Remover Transação                                              |
|     56 | Status da Transação                                            |
|     59 | Reimpressão de Comprovante                                     |

#### Fluxo de Utilização

{{< figure
  src="images/diagrama_qrmultiplus.png"
  alt="Diagrama do fluxo de utilização do QRMultiplus"
  caption="Diagrama do fluxo de utilização do QRMultiplus"
  max-width="100%"
>}}

#### Criação da Cobrança

Esta função gera uma cobrança com o QR Multiplus, que pode ser o PIX no PSP padrão do cliente ou uma das carteiras digitais disponíveis. Para isso basta escolher uma das operações disponíves para criação: 50, 51, 52 ou 53; em seguida com o uso da função IniciaFuncaoMCInterativo dar início ao processo.

##### Retornos da Função

Durante o fluxo a função AguardaFuncaoMCInterativo retornará o local onde está sendo exibido o qr code para realizar o pagamento.

```txt{title="Exemplos de Retorno"}
  [MSG]#AGUARDANDO PAGAMENTO – PINPAD
  [MSG]#AGUARDANDO PAGAMENTO – FRENTE DE CAIXA
  [MSG]#AGUARDANDO PAGAMENTO – TELA E PINPAD
```

**Observação**: Quando utilizado o Pix da Multiplus Pay, antes de retornar o qr code para o local configurado, será solicitado o telefone do cliente, o mesmo deve ser devolvido para DLL através da função ContinuaFuncaoMCInterativo, com o seguinte formato: **(00)00000-0000**;

```txt{title="Solicitação de telefone"}
  [PERGUNTA]#SOLICITE O TELEFONE DO CLIENTE
```

##### Impressão do QR Code

Na tela de exibição padrão existe a possibilidade de realizar a impressão do QR Code para o cliente; em sua primeira utilização é necessário definir qual será a impressora padrão, para isso é exibida a caixa de seleção da imagem a seguir.

{{< figure
  src="images/janelapadrao.png"
  alt="Janela padrão de exibição do Qr Code"
  caption="Janela padrão de exibição do Qr Code"
  max-width="100px"
>}}

{{< figure
  src="images/impressora.png"
  alt="Janela de selção da impressora"
  caption="Janela de selção da impressora"
>}}

Caso seja necessária a redefinição da impressora o parâmetro NomeImpressora deve ser apagado do arquivo CliMC.ini.

```txt{title="CliMC.ini",hl_lines=5}
  Porta=3
  MensagemPadrao=MUTLIPLUS CARD
  TransacoesAdicionaisHabilitadas=29
  ConfirmacaoAutomatica=SIM
  NomeImpressora=Microsoft Print to PDF
```

#### Cancelamento/Estorno

Esta função realiza o cancelamento/estorno de uma cobrança com o QR Multiplus, por questões de segurança transações com a Multiplus Pay não podem ser canceladas desta maneira.

#### Remoção da Cobrança

Esta função realiza a remoção de uma cobrança com o QR Multiplus, para que no relatório de transações não existam valores pendentes de recebimento. Seu uso se dá no cenário, onde o cliente desistiu de utilizar a forma de pagamento, ou houve um cancelamento do processo de venda. No fluxo de utilização ao realizar um cancelamento na tela padrão ou no Pinpad, esta operação é feita automaticamente; caso esteja sendo utilizada o retorno por string, ou a aplicação ClientD.exe seja encerrada de forma inesperada, essa função deve ser chamada antes de iniciar novamente o ciclo da transação, informando no campo NSU, o código da cobrança retornado inicialmente.

#### Status da Cobrança

Esta função retorna o status de uma cobrança (APROVADA, PENDENTE, CANCELADA, DESFEITA), e também outros detalhes relativos a ela como código, NSU, valor, PSP de origem, Identificação do pagamento e data/hora.

#### Reimpressão do Comprovante

Esta função reimprime o comprovante da transação com o NSU informado, além do comprovante também são retornados os mesmos campos com os dados da transação.

#### Retorno da Transação

As informações de retorno de uma transação efetuada por meio do QR Multiplus seguem o padrão das transações com cartão. Nem todos os campos são utilizados, uma vez que o tipo de transação difere. Além disso, há um campo adicional no retorno, denominado **CAMPO2620**. Esse campo corresponde à identificação do pagamento **(E2E)** e contém uma sequência alfanumérica de 32 caracteres. Essa informação é empregada em determinadas Unidades Federativas (UFs) para a geração da NFCe (Nota Fiscal de Consumidor eletrônica).

O CAMPO0121 também é retornado contendo o comprovante reduzido da transação.

```txt{title="Exemplo de Retorno"}
  [RETORNO]#CAMPO0160=1000#CAMPO0002=0,02#CAMPO0132=000#CAMPO0131=000#CAMPO0133=955801668#
  CAMPO0135=955801668#CAMPO0505=1#CAMPO0504=0.00#CAMPO0136=000000XXXXXX0000#CAMPO1190=0000#
  CAMPO0950=#CAMPO1003=SEM NOME#CAMPO0134=955801668#CAMPO0513=00/00#
  CAMPO122=           VENDA PIX|            |      TEF EXPRESS|  |DATA: 2023-08-29 12:22:19|
  AUT: 955801668|NSU: 955801668|ID PGTO:  E18236170601304201626s0305512762 |
  VALOR TOTAL: R$ 10,00|  |
  CUPOM FISCAL: 1000        PDV: 001|CNPJ EMITENTE: 60.177.876/0001-30|29/08/2023 09:22:22|CORTAR#
  CAMPO2620=E18236170601304201626s0305512762#CAMPO0121= VENDA   PIX	29/08/2023 09:22:22|
  NSU: 955801668	AUT: 955801668|VALOR: R$ 10,00|ID PGTO: E18236170601304201626s0305512762|
  CUPOM FISCAL: 1000|	PDV: 001|CNPJ EMITENTE:   60.177.876/0001-30
```

#### Restrições de Utilização

Atualmente a exibição do QR Code no Pinpad, está disponível somente para o modelo **Gertec PPC 930**, os demais não são suportados. Se não estiver disponível este Pinpad, não utilize as opções de exibição 1 e 3, pois ocorrerá falha na geração do QR Code.

#### Observações Sobre o Fluxo de Transação

Durante o processo de realizar uma transação com PIX, pode ocorrer o seguinte cenário: o cliente efetua o pagamento e recebe o comprovante de que o mesmo foi enviado ao PSP do estabelecimento. No entanto, o QR Code ainda permanece visível. Caso não haja resposta após um período de espera de aproximadamente 2 minutos, o operador do caixa opta por cancelar a transação. Nesse momento, podem ocorrer duas situações distintas:

**Primeira situação**: Mesmo após o cancelamento, o comprovante de pagamento é recebido.

**Segunda situação**: O processo é encerrado sem o retorno do comprovante.

Se a segunda situação ocorrer e o sistema não tiver armazenado o código da transação, é necessário acessar o Portal do Cliente para visualizar a transação e obter o código, possibilitando o estorno. Em seguida, o processo de pagamento deve ser repetido para finalizar o fluxo de venda corretamente. Outra opção é implementar a função "Relatório de Transações" no PDV, o que permitirá obter diretamente todas as transações realizadas e automatizar o processo de identificação da transação PIX realizada de forma incompleta.

A explicação para situações como esta ocorrer pode ocorrer devido ao tempo de resposta dos PSPs ou momentos de alta demanda de transações. Garantindo o acesso ao Portal do Cliente ou a implementação do Relatório de Transações é possível lidar com essa inconveniência de maneira eficiente e garantir a conclusão adequada da transação PIX.
