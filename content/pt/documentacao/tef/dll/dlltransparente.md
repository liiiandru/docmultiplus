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

```txt{title="ConfigMC.ini"}
  CAMINHO=C:\ClientD
```

O arquivo **CliMC.ini** deve estar presente na pasta em que o executável ClientD.exe foi instalado, nele são adicionadas as configurações relacionadas ao Pinpad, confirmação de transações, venda digitada, etc. A seguir um exemplo de como deve ser o arquivo e os campos básicos que devem ser configurados.

```txt{title="CliMC.ini"}
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

```txt{title="Exemplo de Retorno"}
  [MENU]#MODO#1,1-MAGNETICO/CHIP|2,2-DIGITADO
```

**Observação**: o tipo de retorno é **BSTR**, na linguagem que for implementada a utilização da DLL verifique qual é a melhor forma de realizar uma possível conversão.

#### Tags Retornadas

Tag [MENU]

Solicita a exibição de um menu para o usuário selecionar opções relativas à transação, como tipo de financiamento, opções de reimpressão, seleção de produtos, etc. O formato utilizado é o seguinte:
```txt{title="Formato do Retorno"}
  [MENU]#TÍTULO DO MENU#1,1-OPÇÃO 1|2,2-OPÇÃO 2
```

| Descrição dos Campos   | Descrição dos Campos                                                                                                        |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| Título do Menu         | O título que deve ser exibido na interface                                                                                  |
| 1,1-Opção              | As opções do menu são separadas por “|” (pipe), e cada opção é acompanhada por seu respectivo índice, separados por virgula |

Para selecionar um item deve ser informado o índice (valor antes da virgula e não a posição na lista/array com as opções). Exemplo de utilização:

Retornando para a DLL o valor “3”, será escolhida a opção Crédito parcelado loja.

```txt{title="Exemplo de Retorno"}
  [MENU]#TIPO DE FINCANCIAMENTO#1,1-CREDITO A VISTA|2,2-CREDITO PARCELADO ADM|3,3-CREDITO PARCELADO LOJA
```

- Tag [MSG]

Retorna uma mensagem que deve ser exibida pela automação, durante o processo podem ser retornadas mensagens vazias, e estas devem ser desconsideradas. Ao receber esta tag não é necessário retornar valores para a DLL.

```txt{title="Exemplo de Retorno"}
  [MSG]#AGUARDE A SENHA
  [MSG]#CONECTANDO COM O SERVIDOR
  [MSG]#TRANSACAO APROVADA
```

- Tag [PERGUNTA]

Solicita informações para a realização transação, como número de parcelas, valores, data de vencimento, etc. As perguntas podem ter os seguintes tipos de dado INT (inteiro), STRING, DECIMAL e DATE (data).

Estas são retornadas no seguinte formato:

```txt{title="Formato do Retorno"}
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

```txt{title="Exemplo de Retorno"}
  [PERGUNTA]#INFORME O NSU#INT#0#0#0,00#0,00#0
  [PERGUNTA]#INFORME A DATA DE VALIDADE (MM/AA) #DATE#0#0#0,00#0,00#0
  [PERGUNTA]#BOM PARA (DD/MM/AA)#DATE#0#0#0,00#0,00#0
  [PERGUNTA]#INFORME O VALOR#DECIMAL#0#0#0,00#0,00#2
```

- **Tag [RETORNO]**

Esta tag indica para a automação que a função solicitada foi concluída e os dados retornados podem estar relacionados a transações com cartão, Pix, captura de CPF ou relatório de transações. Assim que recebida deve ser processada para armazenar as informações necessárias para a automação e finalização da transação, além de realizar a impressão do comprovante. Seu formato varia de acordo com a transação realizada, a seguir está um exemplo de retorno de uma transação com cartão.

```txt{title="Exemplo de Retorno"}
  [RETORNO]#CAMPO0160=CUPOM#CAMPO0002=VALOR#CAMPO0132=COD BANDEIRA#CAMPO0131=COD REDE#CAMPO0135=COD AUTORIZACAO#
  CAMPO0133=NSU#CAMPO0505=QTDE DE PARCELAS#CAMPO0504=VALOR TAXA SERVICO#CAMPO0136=PRIMEIROS NUMEROS CARTAO#
  CAMPO1190=ULTIMOS NUMEROS CARTAO#CAMPO0950=CNPJ AUTORIZADORA#CAMPO1003=NOME CLIENTE#CAMPO0134=NSU REDE#
  CAMPO0513=VENCTO CARTAO#CAMPO122=COMPROVANTE PARA IMPRESSAO UTILIZANDO ‘|’ COMO SEPARADOR |CORTAR#
  CAMPO0003=NUMERO DE VIAS DO COMPROVANTE#CAMPO9999=NOME DA BANDEIRA#CAMPO9998=NOME DA REDE#CAMPO4221=CARTAO PRE PAGO#
  CAMPO0101#DESC DO TIPO DE TRANSACAO#CAMPO0121=COMPROV REDUZIDO
```

- **Tag [ERROABORTAR]**

É retornada quando ocorre algum erro durante a execução, ou quando um comando de cancelamento é enviado, seja pela automação ou pelo pin pad. Ao receber esta tag, o fluxo deve ser interrompido imediatamente, acionando a função **CancelarFluxoMCInterativo** A mensagem informa a razão da interrupção do fluxo. Exemplo:

```txt{title="Exemplo de Retorno"}
  [ERROABORTAR]#CANCELADO PELO OPERADOR
  [ERROABORTAR]#SEM COMUNICACAO COM O TEF
```

- **Tag [ERRODISPLAY]**

Esta tag indica a ocorrência de erros no fluxo de transação, frequentemente relacionados com o pin pad ou problemas com o cartão. A ação a ser tomada após recebe-la é a mesma da tag **[ERROABORTAR]**: o fluxo deve ser interrompido imediatamente, acionando a função **CancelarFluxoMCInterativo**. Exemplos de retorno:

```txt{title="Exemplo de Retorno"}
  [ERRODISPLAY]#T-14 ERRO PINPAD
  [ERRODISPLAY]#ERRO AO REALIZAR REIMPRESSAO DA TRANSACAO
```

- **Observações**

No **[RETORNO]** o Campo122 contém o comprovante a ser impresso, a palavra **CORTAR**, ela indica que é o fim do comprovante, facilitando para a automação imprimir as vias separadamente, realizar o acionamento da guilhotina da impressora ou imprimir uma linha para corte manual.

Quando a função possuir um retorno com a tag [MSG], não é necessário utilizar a função ContinuaFuncaoMCInterativo, pois a DLL está informando o que está ocorrendo durante o processo, e a exibição destas mensagens fica a critério da implementação na automação, assim decide-se quais serão relevantes para a exibição.

A função para Coleta de CPF retorna para a automação uma string no seguinte formato **[RETORNO]#CPF=12345678912**, sendo necessário que a mesma realize a validação do CPF coletado.


### ContinuaFuncaoMCInterativo

A função ContinuaFuncaoMCInterativo possui como parâmetro uma string. A função deve ser chamada após o retorno da AguardaFuncaoMCInterativo para dar continuidade ao processo da transação, quando este conter uma tag que solicita interação ex.: [PERGUNTA], [MENU]; tags como [MSG], [RETORNO] não necessitam que seja chamada a função.

```csharp {title="Protótipo da Função"}
  int ContinuaFuncaoMCInterativo(string sInformacao)
```

Esta função também pode ser utilizada para abortar a transação, um cenário de utilização é quando o pinpad está solicitando que seja inserido o cartão e se deseja cancelar o processo, para isso basta fazer a chamada da forma abaixo, em caso de sucesso o retorno será 0, em seguida deve ser aguardado o seguinte retorno:

```csharp {title="Exemplo de utilização"}
  Retorno = ContinuaFuncaoMCInterativo(“ABORTAR”);
```

```txt{title="Exemplo de Retorno"}
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
A função FinalizaFuncaoMCInterativo, deve ser chamada após a impressão do comprovante da transação, ela vai confirmar a transação. Seus parâmetros, retornos e códigos de erro são idênticos a IniciaFuncaoMCInterativo, inclusive os parâmetros a serem passados devem ser os mesmos com a diferença que o parâmetro iComando deverá ser 98 para confirmar a transação ou 99 se desejar desfazer a transação, e o parâmetro sNSU é o que está no CAMPO0133 do retorno da transação.

```csharp {title="Protótipo da Função"}
  int FinalizaFuncaoMCInterativo(int iComando, string sCnpjCliente, int iParcela, string sCupom, string sValor, string sNsu, string sData, string sNumeroPDV, string sCodigoLoja, int iTipoComunicacao, string sParametro)
```

Ao utilizar esta função com o parâmetro de confirmação automática configurado com o valor NÃO ou em uma transação com múltiplos cartões após seu retorno é obrigatório chamar a AguardaFuncaoMCInterativo para obter o status da confirmação ou desfazimento. Este retorno possui o seguinte formato:

```txt{title="Exemplo de Retorno"}
  [RETORNO]#TRANSACAO NSU=81949 CONFIRMADA COM SUCESSO
  [RETORNO]#TRANSACAO NSU=81949 HOUVE ERROS AO CONFIRMAR
```

{{< callout context="danger" title="Atenção" icon="outline/alert-octagon" >}}
Essa função deve ser chamada obrigatoriamente. Caso contrário, a transação poderá ser estornada, pois não terá sido devidamente confirmada.
{{< /callout >}}


### CancelarFluxoMCInterativo

A função CancelarFluxoMCInterativo, interrompe o processo de transação ela pode ser chamada a qualquer momento, mas é obrigatório a sua chamada sempre que retornar as TAG "[ERROABORTAR]" ou "[ERRODISPLAY]". Em caso de sucesso retorna 0, se ocorrer algum erro durante sua execução os códigos serão os mesmos das funções anteriores. Ao utilizar esta função o fluxo de utilização da DLL (laço de repetição) deve ser interrompido, pois a função AguardaFuncaoMCInterativo deixará de retornar valores. Para continuar a transacionar se deve reiniciar o processo.

```csharp {title="Protótipo da Função"}
  int CancelarFluxoMCInterativo()
```

### Verificar Status da Transação

Esta função tem por finalidade verificar o status de uma transação realizada; seu uso é possível a partir do momento que é retornado o NSU da transação; na tabela a seguir estão os possíveis status.

Uma recomendação de uso é realizar esta verificação antes da impressão do comprovante para o cliente, criando uma camada extra de segurança para a correta finalização da transação.

| Status       |
|--------------|
| 00-APROVADA  |
| 77-PENDENTE  |
| 78-CANCELADA |
| 86-DESFEITA  |

Para utilizar a função o fluxo é basicamente o mesmo de uma venda, deve ser realizada a chamada da função IniciaFuncaoMCInterativo, com o código 700, o NSU retornado, os demais parâmetros (data, valor, etc.) são os mesmos utilizados para iniciar a transação; em seguida entrar no loop com a função **AguardaFuncaoMCInterativo**, até obter o retorno que possui o seguinte formato:

```csharp {title="Exemplo de chamada"}
  IniciaFuncaoMCInterativo(700, "12345678000100", 1, "100", "50,00", "110500", "20250331", "001", "123", 0, "")
```

Obs.: Não é necessário utilizar a função FinalizaFuncaoMCInterativo para concluir o processo de verificação do status.

### Comprovante de Transação POS

Esta função tem por finalidade recuperar comprovantes de transações efetuadas em terminais POS da Multiplus Pay ou SmartPOS que utilizam apps da Multiplus Card, para a geração posterior de NFe/NFCe.

Para utilizar esta função deve ser realizada a chamada da função IniciaFuncaoMCInterativo com o código de operação 400, e o parâmetro sParametro com pelo menos a indicação de data e hora da transação que deseja obter o comprovante. Na tabela a seguir estão os parâmetros disponíveis para utilização.

| Parâmetro   | Valores    | Descrição                                | Tipo   |
|-------------|------------|------------------------------------------|--------|
| DATAINICIO  | dd/MM/yyyy | Data inicial do período a ser consultado | M      |
| DATAFIM     | dd/MM/yyyy | Data final do período a ser consultado   | M      |
| HORAINICIO  | HH:mm:ss   | Hora inicial do período a ser consultado | M      |
| HORAFIM     | HH:mm:ss   | Hora final do período a ser consultado   | M      |
| AUTORIZACAO | STRING     | Código de autorização da transação       | O      |

(M: Mandatório, O: Opcional)

Caso não seja informado o Código de Autorização da transação, será retornada uma pergunta (tag [PERGUNTA]) para que seja informado, a resposta não é obrigatória, pois pode ocorrer uma situação em que esse código não esteja disponível; contudo haverá uma filtragem das transações com base no parâmetro sValor informado. O retorno das transações é feito através da tag [MENU], e sua utilização é igual aos demais menus.

```csharp {title="Protótipo da Função"}
  IniciaFuncaoMCInterativo(400, “12348650000170”, 1, “1”, “1,00”, “1”, “20230706”, “1”, “123”, 0,
  “DATAINICIO=10/09/2024;DATAFIM=10/09/2024;HORAINICIO=00:00:00;HORAFIM=23:59:59;AUTORIZACAO=123”);
```

Observação: Para esta função não é necessária a realização da chamada da função FinalizaFuncaoMCInterativo, pois a mesma não é passível de confirmação ou desfazimento.

- Retorno da Transação

Ao realizar a consulta se for encontrada apenas 1 (uma) transação e o código de autorização foi informado, não haverá a exibição de menu, será retornada a tag [RETORNO] contendo os dados da transação e o comprovante, no mesmo formato de uma transação realizada com TEF. Caso existam 1 ou mais será retornado o menu contendo as seguintes informações: Bandeira, Valor, Data e Hora, Parcelas e Status da impressão.

```txt{title="Exemplo de Retorno"}
  [MENU]#INFORME A TRANSACAO DESEJADA PARA A IMPRESSAO DO COMPROVANTE#
  1,1-VISA;0,1;09/09/2024 12:21:46;1;IMPRESSO|
  2,2-ELO;0,1;09/09/2024 12:26:17;1;NAO IMPRESSO|
  3,3-MASTERCARD;0,1;09/09/2024 12:43:52;1;NAO IMPRESSO|
  4,4-ELO;0,1;09/09/2024 12:46:19;1;NAO IMPRESSO
```

Uma vez impresso esse status será alterado, e todas as impressões seguintes da mesma transação virá com a indicação de que se trata de uma Reimpressão.

## Relatório de Transações

O Relatório de Transações permite obter uma lista das transações realizadas durante um período específico. Sua utilização segue um fluxo semelhante ao de uma venda, com a diferença de que se utiliza o parâmetro sParametro na função IniciaFuncaoMCInterativo para fornecer os parâmetros da consulta. Após a inicialização deve ser chamada em loop a função AguardaFuncaoMCInterativo até que seja retornada a string contendo a tag [RETORNO] e os dados do relatório; não é necessária a utilização da função FinalizaFuncaoMCInterativo.  A seguir está um exemplo de utilização:

```csharp {title="Exemplo de utilização"}
  IniciaFuncaoMCInterativo(300, “12348650000170”, 1, “1”, “1,00”, “1”, “20230706”, “1”, “123”, 0,
  “DATAINICIO=05/07/2023;DATAFIM=05/07/2023;HORAINICIO=00:00:00;HORAFIM=23:59:59;PDV=1;JSON=SIM;PAGINA=1”);
```

### Parâmetros da Consulta

Os parâmetros necessários para realizar a consulta estão descritos na tabela a seguir:

| Parâmetro   | Valores    | Descrição                                                                           |
|-------------|------------|-------------------------------------------------------------------------------------|
| DATAINICIO  | dd/MM/yyyy | Data inicial do período a ser consultado                                            |
| DATAFIM     | dd/MM/yyyy | Data final do período a ser consultado                                              |
| HORAINICIO  | HH:mm:ss   | Hora inicial do período a ser consultado                                            |
| HORAFIM     | HH:mm:ss   | Hora final do período a ser consultado                                              |
| PDV         | INT        | Número do PDV                                                                       |
| JSON        | SIM/NAO    | Formato de Retorno, SIM retorna em Json, NÃO retorna em CSV                         |
| PAGINA      | INT        | Número da Página caso haja muitas transações, utilizado somente para o formato Json |

Obs.: Todos os parâmetros são necessários para realizar a consulta.

### Retorno da Consulta

O Relatório retornado possui os mesmos campos tanto em Json quanto em CSV, com a diferença que em Json existe a informação da paginação dos resultados. É recomendado a consulta de períodos não muito longos (superior a 2 dias), assim o processo será concluído de forma mais rápida.

- Campos Retornados
| Campo                            | Tipo de Dado   | Descrição                    |
|----------------------------------|----------------|------------------------------|
| RedeNome                         | String         |                              |
| ServicoDescricao                 | String         | Ex: Crédito a vista          |
| TransacaoNSU                     | String         |                              |
| TransacaoDataHoraEstabelecimento | DateTime       |                              |
| TransacaoDataHoraAdministradora  | DateTime       |                              |
| TransacaoValor                   | Decimal        |                              |
| TransacaoValorTaxaServico        | Decimal        |                              |
| TransacaoNumeroCartao            | String         |                              |
| TransacaoCodigoAutorizacao       | String         |                              |
| TransacaoNsuHost                 | String         | NSU da operadora             |
| TransacaoStatus                  | String         |                              |
| TransacaoQtdParcelas             | INT            |                              |
| TransacaoTerminal                | String         |                              |
| PaginaAtual                      | INT            | Informações da paginação     |
| UltimaPagina                     | INT            | Informações da paginação     |
| QtdeRegistrosPorPagina           | INT            | Informações da paginação     |
| Transacoes                       | List           | Lista contendo as transações |


```csv {title="Retorno em CSV"}
REDENOME;BANDEIRANOME;SERVICODESCRICAO;TRANSACAONSU;TRANSACAODATAHORAESTABELECIMENTO;TRANSACAODATAHORAADMINISTRADORA;TRANSACAOVALOR;TRANSACAOVALORTAXASERVICO;TRANSACAONUMEROCARTAO;TRANSACAOCODIGOAUTORIZACAO;TRANSACAONSUHOST;TRANSACAOSTATUS;TRANSACAOQTDPARCELAS;TRANSACAOTERMINAL
NAO INFORMADO;NAO INFORMADO;NAO INFORMADO;000000;12/07/2023 04:01:57;12/07/2023 04:01:57;8,90;0;;010157;000000;58-ERRO AO REALIZAR TRANSACAO;1;001
REDE;MAESTRO;NAO INFORMADO;111018;12/07/2023 04:03:03;12/07/2023 04:03:03;8,90;0;;741052;407003963;00-APROVADA;1;001
REDE;ELECTRON;NAO INFORMADO;111022;12/07/2023 04:17:20;12/07/2023 04:17:20;28,90;0;;312269;164193262;78-CANCELADA;1;001
NAO INFORMADO;NAO INFORMADO;NAO INFORMADO;111022;12/07/2023 04:20:49;12/07/2023 04:20:49;28,90;0;;012049;111022;58-ERRO AO REALIZAR TRANSACAO;1;001
REDE;ELECTRON;NAO INFORMADO;111029;12/07/2023 04:22:54;12/07/2023 04:22:54;28,90;0;;028719;866758376;00-APROVADA;0;001
BB;PIX;PIX;;12/07/2023 11:27:13;12/07/2023 11:27:13;0,02;0;;;;86-DESFEITA;1;160
REDE;MASTERCARD;NAO INFORMADO;111049;12/07/2023 11:36:09;12/07/2023 11:36:09;3,42;0;;083300;044521791;78-CANCELADA;1;001
REDE;MASTERCARD;NAO INFORMADO;111055;12/07/2023 11:38:00;12/07/2023 11:38:00;3,42;0;;503483;756225302;00-APROVADA;0;001
```

```json {title="Retorno em JSON"}
  {
    "PaginaAtual": 1,
    "UltimaPagina": 1,
    "QtdeRegistrosPorPagina": 3,
    "Transacoes": [
      {
        "RedeNome": "NAO INFORMADO",
        "BandeiraNome": "NAO INFORMADO",
        "ServicoDescricao": "Nao Informado",
        "TransacaoNSU": "000000",
        "TransacaoDataHoraEstabelecimento": "2023-07-12T04:01:57Z",
        "TransacaoDataHoraAdministradora": "2023-07-12T04:01:57Z",
        "TransacaoValor": 8.9,
        "TransacaoValorTaxaServico": 0,
        "TransacaoNumeroCartao": null,
        "TransacaoCodigoAutorizacao": "010157",
        "TransacaoNsuHost": "000000",
        "TransacaoStatus": "58-ERRO AO REALIZAR TRANSACAO",
        "TransacaoQtdParcelas": 1,
        "TransacaoTerminal": "001"
      },
      {
        "RedeNome": "NAO INFORMADO",
        "BandeiraNome": "NAO INFORMADO",
        "ServicoDescricao": "Nao Informado",
        "TransacaoNSU": "000000",
        "TransacaoDataHoraEstabelecimento": "2023-07-12T04:02:27Z",
        "TransacaoDataHoraAdministradora": "2023-07-12T04:02:27Z",
        "TransacaoValor": 8.9,
        "TransacaoValorTaxaServico": 0,
        "TransacaoNumeroCartao": null,
        "TransacaoCodigoAutorizacao": "010227",
        "TransacaoNsuHost": "000000",
        "TransacaoStatus": "58-ERRO AO REALIZAR TRANSACAO",
        "TransacaoQtdParcelas": 1,
        "TransacaoTerminal": "001"
      },
      {
        "RedeNome": "REDE",
        "BandeiraNome": "MAESTRO",
        "ServicoDescricao": "Nao Informado",
        "TransacaoNSU": "111018",
        "TransacaoDataHoraEstabelecimento": "2023-07-12T04:03:03Z",
        "TransacaoDataHoraAdministradora": "2023-07-12T04:03:03Z",
        "TransacaoValor": 8.9,
        "TransacaoValorTaxaServico": 0,
        "TransacaoNumeroCartao": null,
        "TransacaoCodigoAutorizacao": "990052",
        "TransacaoNsuHost": "407623963",
        "TransacaoStatus": "00-APROVADA",
        "TransacaoQtdParcelas": 1,
        "TransacaoTerminal": "001"
      },
    ]
  }

```

## Integração com QRMultiplus (PIX)

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

## Integração com o App PinPDV Lite

## Funções de Pinpad

## Códigos de Erro

## Solução de Problemas
