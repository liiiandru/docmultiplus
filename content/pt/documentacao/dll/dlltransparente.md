---
title: "DLL TefClientMC - Modo Interativo (DLL Transparente)"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
seo:
  title: "DLL TefCLientMC" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A DLL **TefClientMC** proporciona uma integração flexível, permitindo à automação um alto nível de controle sobre o fluxo da transação e ampla personalização por meio de um processo iterativo. É ideal para automações que buscam preservar sua identidade visual e ter maior controle sobre cada etapa da transação.

## Requisitos para Execução

Para a execução da DLL, é necessário que os seguintes componentes de software estejam instalados:

- Microsoft .Net Framework 4.6.2 [(download)](https://dotnet.microsoft.com/pt-br/download/dotnet-framework/net462)
- Microsoft Visual C++ 2015-2022 Redistributable [(download)](https://learn.microsoft.com/pt-br/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022)

Para garantir o funcionamento adequado da DLL, o ambiente deve atender aos seguintes requisitos mínimos:

- Windows 7 SP1
- Processador Intel Core i3
- 3GB de memória RAM
- 1GB de espaço disponível no HD
- 1 porta USB para a conexão do Pinpad

Para um **melhor desempenho** e establididade recomendamos a seguinte configuração:

- Windows 10 ou superior
- Processador Intel Core i3, AMD Ryzen 3 ou superior
- 4GB de memória RAM
- 1,5GB de espaço disponível no SSD
- 1 porta USB para a conexão do Pinpad

## Fluxo de Utilização

A integração com a DLL TefClientMC ocorre por meio da implementação das funções básicas de comunicação da biblioteca. O processo de transação se inicia com a chamada da função **IniciaFuncaoMCInterativo**.

Ao chamar essa função com os parâmetros adequados (detalhados posteriormente), deve-se verificar o retorno. Se for 0, o processo continua normalmente; caso contrário (retorno maior que 0), a transação deve ser encerrada.

Em seguida, dentro de um laço de repetição, a função **AguardaFuncaoMCInterativo** é chamada. Essa função retorna um **BSTR** contendo informações essenciais para a transação. Se o retorno for **[ERROABORTAR]** ou **[ERRODISPLAY]**, o processo deve ser interrompido imediatamente. Caso contrário, a comunicação prossegue por meio da função **ContinuaFuncaoMCInterativo**. O laço se encerra quando **AguardaFuncaoMCInterativo** retorna a tag **[RETORNO]**.

Após o recebimento dessa tag, deve-se imprimir o comprovante da transação, armazenar suas informações e, por fim, chamar a função **FinalizaFuncaoMCInterativo**, confirmando assim a transação.

Se for necessário cancelar a transação a qualquer momento, a função **CancelarFluxoMCInterativo** pode ser chamada. Essa função retorna 0 em caso de sucesso e interrompe imediatamente o fluxo da transação.

O retorno das funções indica o sucesso de sua execução. No caso específico de **FinalizaFuncaoMCInterativo**, o resultado da confirmação ou do cancelamento da transação pode ser obtido por meio da função AguardaFuncaoMCInterativo ou pela verificação do status da transação.

| Retorno   | Função                     |
|-----------|----------------------------|
| INT       | IniciaFuncaoMCInterativo   |
| *BSTR     | AguardaFuncaoMCInterativo  |
| INT       | ContinuaFuncaoMCInterativo |
| INT       | FinalizaFuncaoMCInterativo |
| INT       | CancelarFluxoMCInterativo  |

*Informações sobre o tipo BSTR [(Clique aqui)](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/automat/bstr)

(diagrama)

### Observações sobre o Processo Iterativo

Durante a execução do laço de repetição, evite a inclusão de *delays*, *sleeps* ou qualquer instrução que possa resultar em atrasos na execução. Isso é crucial, pois pode levar à perda de dados ou ao encerramento inesperado da execução.
As chamadas para a função **AguardaFuncaoMCInterativo** devem ser feitas de maneira contínua, com tratamento adequado para cada tag.
Se uma mensagem for exibida pela automação e você receber uma nova mensagem ou algum outro retorno, substitua a mensagem anterior apenas nesses casos. Durante o processo, é importante estar ciente de que em certos momentos, podem ser retornadas strings vazias.

## Instalação/Configuração

Esta solução foi desenhada para funcionar no modelo *standalone*, ou seja, todos os componentes requeridos devem estar instalados no mesmo dispositivo (disco C:\\). As funções TEF somente podem ser executadas através de um equipamento que possua o Pinpad, conexão de banda larga e a solução instalada.

Para o funcionamento da DLL são necessários que os arquivos da DLL e o **ConfigMC.ini** estejam presentes na mesma pasta em que está o executável do sistema. O Conteúdo do arquivo **ConfigMC.ini**, deve apontar o caminho da pasta onde está o executável **ClientD.exe**.

```{title="ConfigMC.ini"}
  CAMINHO=C:\ClientD
```

O arquivo **CliMC.ini** deve estar presente na pasta em que o executável ClientD.exe foi instalado, nele são adicionadas as configurações relacionadas ao Pinpad, confirmação de transações, venda digitada, etc. A seguir um exemplo de como deve ser o arquivo e os campos básicos que devem ser configurados.

```{title="CliMC.ini"}
  Porta=3
  MensagemPadrao=MUTLIPLUS CARD
  TransacoesAdicionaisHabilitadas=29
  ConfirmacaoAutomatica=SIM
```

| Campo                           | Valor                                                            | Descrição                                                                                                                                                 |
|---------------------------------|------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Porta                           | Número da porta ou AUTO_USB para identificar de forma automática | Configuração de porta do Pinpad                                                                                                                           |
| MensagemPadrao                  | Texto                                                            | Mensagem padrão exibida na tela do Pinpad                                                                                                                 |
| TransacoesAdicionaisHabilitadas | Verificar configurações adicionais                                               | Configurações referente a serviços disponibilizados                                                                                                       |
| ConfirmacaoAutomatica           | SIM/NAO                                                          | Realiza a confirmação automática de todas as transações realizadas (a opção deve ter o valor NAO, quando são realizadas transações com múltiplos cartões) |
| NomeImpressora                  | Nome da Impressora selecionada                                   | Parâmetro gerado automaticamente ao salvar a impressora padrão do QR Code                                                                                 |

### Configurações Adicionais

Valores configurações adicionais
- |   Código | Descrição                                                                                             |
|----------|-------------------------------------------------------------------------------------------------------|
|       29 | Opcional, habilita venda crédito digitada (em alguns cartões não é permitido esse modo de utilização) |
|       42 | Opcional habilita venda débito digitada (em alguns cartões não é permitido esse modo de utilização)   |
- Requisitos de Execução

Para que o funcionamento da DLL ocorra de forma correta é necessário que a automação seja executada em **Modo Administrador**, caso não seja seguido erros poderão ser ocasionados. A aplicação ClientD.exe também deve estar configurada para executar em Modo Administrador.

### Processo de Atualização

A aplicação ClientD.exe é atualizada automaticamente em momentos estratégicos, ou seja, quando não está realizando transações. Se, por algum motivo, estiver em processo de atualização e uma transação for iniciada, você receberá o código de erro 41. Nesse caso, tente realizar a transação novamente até que o erro não seja mais retornado.
No entanto, a DLL TefClientMC requer uma **atualização manual**, pois tem um impacto direto na automação. Em caso de mudanças significativas na versão disponibilizada (*break changes*), haverá um aviso no Portal. Sempre é recomendável testar as novas versões antes de liberar a automação para produção.

## Funções da DLL

### IniciaFuncaoMCInterativo

Esta função dá inicio ao processo de transação, seja com cartão, Pix ou administrativa, somente em algumas exceções que ela não é utilizada. Seus parâmetros são os seguintes:

| Tipo   | Nome             | Descrição                                                                                                                      |
|--------|------------------|--------------------------------------------------------------------------------------------------------------------------------|
| INT    | iComando         | Comando para executar uma operação, por ex.: venda Crédito, Débito, verifique na tabela seguinte todos os comandos disponíveis |
| STRING | sCnpjCliente     | CNPJ do Cliente (apenas números ex.: 12345678000100)                                                                           |
| INT    | iParcela         | Número de Parcelas                                                                                                             |
| STRING | sCupom           | Número do cupom fiscal (Até 6 caracteres, Ex:123456)                                                                           |
| STRING | sValor           | Valor da transação utilizando virgula como separador, ex.: 100,00                                                              |
| STRING | sNSU             | Número do NSU gerado pela frente de caixa (Até 6 caracteres, Ex:123456)                                                        |
| STRING | sData            | Data da transação no seguinte formato yyyyMMdd, ex.: 20211207                                                                  |
| STRING | sNumeroPDV       | Número do PDV que está realizando a transação.                                                                                 |
| STRING | sCodigoLoja      | Código da Loja (Solicitar para Multiplus Card)                                                                                 |
| INT    | iTipoComunicação | Parâmetro de comunicação, preencher com 0                                                                                      |
| STRING | sParametro       | Preencher com uma string vazia “”, string caso não seja utilizado                                                              |

```csharp {title="Protótipo da Função"}
int IniciaFuncaoMCInterativo(int iComando, string sCnpjCliente, int iParcela, string sCupom, string sValor, string sNsu, string sData, string sNumeroPDV, string sCodigoLoja, int iTipoComunicacao, string sParametro)
```

#### Códigos de Erro

Quando executada corretamente a função retorna 0, e o fluxo deve continuar; caso ocorra algum erro serão retornados um dos códigos da tabela a seguir:

|   Código | Descrição                                                            |
|----------|----------------------------------------------------------------------|
|        0 | Retorno OK (função foi executada sem erros)                          |
|        1 | Erro genérico na execução                                            |
|       30 | Não foi encontrado o caminho do ClientD.exe                          |
|       31 | ConfigMC.ini está vazio                                              |
|       32 | ClientD.exe não encontrado                                           |
|       33 | ClientD.exe não está em execução                                     |
|       34 | Erro ao iniciar ClientD.exe                                          |
|       35 | Erro interno do ClientD.exe                                          |
|       36 | Erro interno do ClientD.exe                                          |
|       37 | Erro na leitura do arquivo ConfigMC.ini                              |
|       38 | Valor da transação com formato incorreto                             |
|       39 | Executável de envio de transações não encontrado                     |
|       40 | CNPJ Inválido ou no formato incorreto                                |
|       41 | ClientD.exe está em processo de atualização                          |
|       42 | A automação não está sendo executada no modo administrador           |
|       43 | ClientD.exe está em execução devido a uma transação anterior         |
|       44 | Parâmetros ausentes em operação que utiliza o sParametro             |
|       45 | Parâmetros no formato incorreto em operação que utiliza o sParametro |
|       46 | Erro ao identificar localização da DLL                               |
|       47 | Não Foi encontrada a localização da DLL                              |
|       48 | Não houve confirmação da conclusão da execução do ClientD            |
|       49 | Número de parcelas inválido                                          |
|       52 | Código de operação inválido para a função                            |

#### Códigos de Operação

Para utilizar as funções disponíveis, informe um dos códigos da tabela abaixo no parâmetro **iComando**. A maioria das funções segue o mesmo fluxo; caso haja um fluxo alternativo, ele será especificado.

|   Código | Descrição                        |
|----------|----------------------------------|
|        4 | Venda Débito                     |
|       20 | Venda Débito a Vista             |
|        1 | Venda Crédito                    |
|        0 | Venda Crédito a Vista            |
|        2 | Venda Crédito Parcelado Loja     |
|        3 | Venda Crédito Parcelado Adm      |
|       11 | Venda Frota                      |
|       18 | Venda Voucher                    |
|        9 | Consulta saldo credito           |
|       10 | Consulta saldo debito            |
|       15 | Pré Autorização                  |
|       16 | Confirma Pré Autorização         |
|       17 | Cancela Pré Autorização          |
|        5 | Cancelamento                     |
|        6 | Reimpressão                      |
|       98 | Confirma Transação               |
|       99 | Desfaz Transação                 |
|      110 | Limpeza de Tabelas               |
|      200 | Coleta de CPF                    |
|      300 | Relatório de Transações          |
|      400 | Comprovante de Transação com POS |
|      700 | Status da Transação              |
|      806 | Status do Pin Pad                |
|      807 | Informações do Pin Pad           |

#### Observações Sobre os Códigos de Operação

Os códigos de operação em sua maioria são autoexplicativos, a seguir estão descritos alguns códigos, e nomenclaturas que possam gerar dúvidas.

- **Venda Crédito e Venda Crédito a Vista**: a primeira opção apresenta um menu para que seja escolhida a modalidade a vista ou parcelada; a segunda opção inicia a venda à vista.
- **Parcelado Loja e Parcelado Adm**: A utilização destas opções está vinculada diretamente com a operadora do cliente, pois determina quem irá pagar os juros do parcelamento, dependendo da configuração da operadora pode não estar disponível os dois tipos.
- **Limpeza de Tabelas**: Na primeira inicialização ou venda do dia, a operadora realiza uma atualização das informações no Pinpad. Contudo, ao longo do dia, podem ocorrer erros que impedem a conclusão das transações, frequentemente manifestados através da mensagem "Cartão Inválido". Nesses casos, uma solução possível é realizar uma atualização forçada das tabelas, o que é feito utilizando a função "Limpeza de Tabelas". Após essa ação, ao efetuar uma nova transação, o Pinpad receberá a carga de tabelas atualizada, permitindo o funcionamento correto do TEF.
Ao utilizar esta função os seguintes retornos poderão ser obtidos: SUCESSO ou FALHA, no formato: [RETORNO]#SUCESSO, [RETORNO]#FALHA.
**Obs.**: Para esta operação **não é necessário** chamar a função FinalizaFuncaoMCInterativo.

- **Coleta de CPF**: Esta função tem por objetivo de auxiliar a automação, tornando a coleta mais discreta para o cliente, no seu funcionamento o CPF é inserido, em seguida deve ser confirmado pelo cliente se o mesmo está correto. A validação do mesmo deve ser feita pela automação, caso esteja incorreto deve ser solicitado novamente.

- **Cancelamento e Reimpressão**: o fluxo destas transações pode ter alguma pequena variação dependendo da operadora (algumas não permitem reimpressão), ao utilizar elas para finalizar o fluxo a função FinalizaFuncaoMCInterativo, deve ser chamada com o comando 98, pois nestas funções não é realizado desfazimento.

#### Seleção do Dispositivo para Realizar a Transação

Caso esteja habilitado o uso da função POS TEF será possível definir em qual dispositivo a transação será realizada, no Pinpad ou no SmartPOS, para isso é necessário realizar a chamada da função IniciaFuncaoMCInterativo, preenchendo o parâmetro sParametro da seguinte forma:

| Dispositivo | Descrição |
|------------|-----------|
| Pinpad     | LEITOR=1  |
| SmartPOS   | LEITOR=3  |

Caso não seja informado nenhum valor ou algum diferente dos mencionados acima, será questionado o dispositivo em que a transação será realizada.

### AguardaFuncaoMCInterativo

A função **AguardaFuncaoMCInterativo** não possui parâmetros, ela deve ser chamada continuamente em um laço até que o processo da transação seja finalizado, com base em seus retornos serão exibidas mensagens solicitando a interação do operador e do cliente. Cada retorno tem um tratamento diferente, porém todos iniciam a string com uma tag informando a ação.

```csharp {title="Protótipo da Função"}
  BSTR AguardaFuncaoMCInterativo()
```
A tag sempre é no seguinte formato [Ação] e possui como separador um #, em seguida vem o campo a ser exibido.

```{title="Exemplo de Retorno"}
  [MENU]#MODO#1,1-MAGNETICO/CHIP|2,2-DIGITADO
```

**Observação**: o tipo de retorno é **BSTR**, na linguagem que for implementada a utilização da DLL verifique qual é a melhor forma de realizar uma possível conversão.

#### Tags Retornadas

Tag [MENU]

Solicita a exibição de um menu para o usuário selecionar opções relativas à transação, como tipo de financiamento, opções de reimpressão, seleção de produtos, etc. O formato utilizado é o seguinte:
```{title="Formato do Retorno"}
  [MENU]#TÍTULO DO MENU#1,1-OPÇÃO 1|2,2-OPÇÃO 2
```

| Descrição dos Campos   | Descrição dos Campos                                                                                                        |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| Título do Menu         | O título que deve ser exibido na interface                                                                                  |
| 1,1-Opção              | As opções do menu são separadas por “|” (pipe), e cada opção é acompanhada por seu respectivo índice, separados por virgula |

Para selecionar um item deve ser informado o índice (valor antes da virgula e não a posição na lista/array com as opções). Exemplo de utilização:

Retornando para a DLL o valor “3”, será escolhida a opção Crédito parcelado loja.

```{title="Exemplo de Retorno"}
  [MENU]#TIPO DE FINCANCIAMENTO#1,1-CREDITO A VISTA|2,2-CREDITO PARCELADO ADM|3,3-CREDITO PARCELADO LOJA
```

- Tag [MSG]

Retorna uma mensagem que deve ser exibida pela automação, durante o processo podem ser retornadas mensagens vazias, e estas devem ser desconsideradas. Ao receber esta tag não é necessário retornar valores para a DLL.

```{title="Exemplo de Retorno"}
  [MSG]#AGUARDE A SENHA
  [MSG]#CONECTANDO COM O SERVIDOR
  [MSG]#TRANSACAO APROVADA
```

- Tag [PERGUNTA]

Solicita informações para a realização transação, como número de parcelas, valores, data de vencimento, etc. As perguntas podem ter os seguintes tipos de dado INT (inteiro), STRING, DECIMAL e DATE (data).

Estas são retornadas no seguinte formato:

```{title="Formato do Retorno"}
  [PERGUNTA]#TEXTO DA PERGUNTA#TIPO DE DADO#TAMANHO MINÍMO#TAMANHO MÁXIMO#VALOR MINÍMO#VALOR MÁXIMO#CASAS DECIMAIS
```

| Descrição dos Campos   | Descrição dos Campos                                                              |
|------------------------|-----------------------------------------------------------------------------------|
| Texto da pergunta      | A pergunta que deve ser exibida para o usuário.                                   |
| Tipo de Dado           | Tipo de dado que deve ser utilizado para a validação na resposta.                 |
| Tamanho Mínimo         | O tamanho mínimo do valor informado, se for 0 não há restrição.                   |
| Tamanho Máximo         | O tamanho máximo do valor informado, se for 0 não há restrição.                   |
| Valor Mínimo           | Valor mínimo para ser informado, se for 0 não há restrição.                       |
| Valor Máximo           | Valor máximo para ser informado, se for 0 não há restrição.                       |
| Casas decimais         | Número de casas decimais solicitada, se for 0 significa que não está solicitando. |

Para o tipo DATE no tamanho deve ser considerada a máscara do campo (caso utilize), ex.: (DD/MM/AA) tem o tamanho 8, valores mínimo e máximo não são retornados.

A seguir exemplos de perguntas que podem ser retornadas:

```{title="Exemplo de Retorno"}
  [PERGUNTA]#INFORME O NSU#INT#0#0#0,00#0,00#0
  [PERGUNTA]#INFORME A DATA DE VALIDADE (MM/AA) #DATE#0#0#0,00#0,00#0
  [PERGUNTA]#BOM PARA (DD/MM/AA)#DATE#0#0#0,00#0,00#0
  [PERGUNTA]#INFORME O VALOR#DECIMAL#0#0#0,00#0,00#2
```

- **Tag [RETORNO]**

Esta tag indica para a automação que a função solicitada foi concluída e os dados retornados podem estar relacionados a transações com cartão, Pix, captura de CPF ou relatório de transações. Assim que recebida deve ser processada para armazenar as informações necessárias para a automação e finalização da transação, além de realizar a impressão do comprovante. Seu formato varia de acordo com a transação realizada, a seguir está um exemplo de retorno de uma transação com cartão.

```{title="Exemplo de Retorno"}
  [RETORNO]#CAMPO0160=CUPOM#CAMPO0002=VALOR#CAMPO0132=COD BANDEIRA#CAMPO0131=COD REDE#CAMPO0135=COD AUTORIZACAO#
  CAMPO0133=NSU#CAMPO0505=QTDE DE PARCELAS#CAMPO0504=VALOR TAXA SERVICO#CAMPO0136=PRIMEIROS NUMEROS CARTAO#
  CAMPO1190=ULTIMOS NUMEROS CARTAO#CAMPO0950=CNPJ AUTORIZADORA#CAMPO1003=NOME CLIENTE#CAMPO0134=NSU REDE#
  CAMPO0513=VENCTO CARTAO#CAMPO122=COMPROVANTE PARA IMPRESSAO UTILIZANDO ‘|’ COMO SEPARADOR |CORTAR#
  CAMPO0003=NUMERO DE VIAS DO COMPROVANTE#CAMPO9999=NOME DA BANDEIRA#CAMPO9998=NOME DA REDE#CAMPO4221=CARTAO PRE PAGO#
  CAMPO0101#DESC DO TIPO DE TRANSACAO#CAMPO0121=COMPROV REDUZIDO
```

- **Tag [ERROABORTAR]**

É retornada quando ocorre algum erro durante a execução, ou quando um comando de cancelamento é enviado, seja pela automação ou pelo pin pad. Ao receber esta tag, o fluxo deve ser interrompido imediatamente, acionando a função **CancelarFluxoMCInterativo** A mensagem informa a razão da interrupção do fluxo. Exemplo:

```{title="Exemplo de Retorno"}
  [ERROABORTAR]#CANCELADO PELO OPERADOR
  [ERROABORTAR]#SEM COMUNICACAO COM O TEF
```

- **Tag [ERRODISPLAY]**

Esta tag indica a ocorrência de erros no fluxo de transação, frequentemente relacionados com o pin pad ou problemas com o cartão. A ação a ser tomada após recebe-la é a mesma da tag **[ERROABORTAR]**: o fluxo deve ser interrompido imediatamente, acionando a função **CancelarFluxoMCInterativo**. Exemplos de retorno:

```{title="Exemplo de Retorno"}
  [ERRODISPLAY]#T-14 ERRO PINPAD
  [ERRODISPLAY]#ERRO AO REALIZAR REIMPRESSAO DA TRANSACAO
```

- **Observações**

No **[RETORNO]** o Campo122 contém o comprovante a ser impresso, a palavra **CORTAR**, ela indica que é o fim do comprovante, facilitando para a automação imprimir as vias separadamente, realizar o acionamento da guilhotina da impressora ou imprimir uma linha para corte manual.

Quando a função possuir um retorno com a tag [MSG], não é necessário utilizar a função ContinuaFuncaoMCInterativo, pois a DLL está informando o que está ocorrendo durante o processo, e a exibição destas mensagens fica a critério da implementação na automação, assim decide-se quais serão relevantes para a exibição.

A função para Coleta de CPF retorna para a automação uma string no seguinte formato **[RETORNO]#CPF=12345678912**, sendo necessário que a mesma realize a validação do CPF coletado.


### ContinuaFuncaoMCInterativo

A função ContinuaFuncaoMCInterativo possui como parâmetro uma string. A função deve ser chamada após o retorno da AguardaFuncaoMCInterativo para dar continuidade ao processo da transação, quando este conter uma tag que solicita interação ex.: [PERGUNTA], [MENU]; tags como [MSG], [RETORNO] não necessitam que seja chamada a função.

Esta função também pode ser utilizada para abortar a transação, um cenário de utilização é quando o pinpad está solicitando que seja inserido o cartão e se deseja cancelar o processo, para isso basta fazer a chamada da forma abaixo, em caso de sucesso o retorno será 0, em seguida deve ser aguardado o seguinte retorno:

```{title="Exemplo de Retorno"}
  [ERROABORTAR]#CANCELADO PELO OPERADOR.
```

- Códigos de Erro

Como retorno a função pode ter os códigos da tabela abaixo, quando for 0 o fluxo continua.

|   Código | Descrição                   |
|----------|-----------------------------|
|        0 | Retorno OK                  |
|        1 | Erro Genérico na execução   |
|       35 | Erro interno do ClientD.exe |
|       36 | Erro interno do ClientD.exe |

### FinalizaFuncaoMCInterativo

### CancelarFluxoMCInterativo

## Relatório de Transações

## Integração com QRMultiplus (PIX)

## Integração com o App PinPDV Lite

## Funções de Pinpad

## Códigos de Erro

## Solução de Problemas
