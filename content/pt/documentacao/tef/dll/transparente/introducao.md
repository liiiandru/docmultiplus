---
title: "Introdução"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 102
seo:
  title: "Introdução" # custom title (optional)
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
- Pinpad

Para um **melhor desempenho** e establididade recomendamos a seguinte configuração:

- Windows 10 ou superior
- Processador Intel Core i3, AMD Ryzen 3 ou superior
- 4GB de memória RAM
- 1,5GB de espaço disponível no SSD
- 1 porta USB para a conexão do Pinpad
- Pinpad

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

{{< figure
  src="images/diagrama_dll.png"
  alt="Diagrama do fluxo de utilização da DLL"
  caption="Diagrama do fluxo de utilização da DLL"
  max-width="100%"
>}}


## Observações sobre o Processo Iterativo

Durante a execução do laço de repetição, evite a inclusão de *delays*, *sleeps* ou qualquer instrução que possa resultar em atrasos na execução. Isso é crucial, pois pode levar à perda de dados ou ao encerramento inesperado da execução.
As chamadas para a função **AguardaFuncaoMCInterativo** devem ser feitas de maneira contínua, com tratamento adequado para cada tag.
Se uma mensagem for exibida pela automação e você receber uma nova mensagem ou algum outro retorno, substitua a mensagem anterior apenas nesses casos. Durante o processo, é importante estar ciente de que em certos momentos, podem ser retornadas strings vazias.
