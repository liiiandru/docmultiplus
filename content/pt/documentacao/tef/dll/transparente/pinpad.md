---
title: "Funções de Pinpad"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 105
seo:
  title: "Funções de Pinpad" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Estas funções são destinadas para que a automação obtenha dados sobre o dispositivo que está sendo utilizado, se o mesmo está conectado e funcionando corretamente.

## Verificar Status do Pinpad

Esta função retorna 0 se o pinpad estiver conectado e funcional e retorna 1 se ocorrer algum erro no processo de comunicação ou exista algum defeito. Um efeito adicional desta função é a limpeza da tela, após sua execução será exibida a mensagem configurada no CliMC.ini.

Uma forma alternativa para verificar o status é através da função IniciaFuncaoMCInterativo utilizando o código de operação 806; o comportamento será semelhante ao fluxo normal de transação, e não sendo necessário chamar a função FinalizaFuncaoMCInterativo. Os parâmetros cupom, e nsu podem ser fictícios.

Este modo retorna uma string para indicar o status do Pinpad, a seguir estão os possíveis retornos:

Observação: Ao utilizar a função direta VerificarPinpad é necessário que o PDV tenha sido inicializado/configurado, utilizando a partir da operação 806, a função realizará este processo de configuração. Existe diferença no tempo de execução das duas formas (alguns segundos), verifique qual melhor será a mais adequada para sua automação.

## Obter Informações do Pinpad

Esta função retorna as informações sobre o pinpad que está em utilização. O retorno ocorre no seguinte formato:

```txt{title="Exemplo de Retorno"}
  [RETORNO]#FABRICANTE=GERTEC|VHARDWARE=PPC-930;192MB|
  VFIRMWARE=1400.11808.VR1015@|VESPEC=2.12|VAPLIC=002.14 00921|NUMSERIE=7200000000001234
```


| Campo      | Descrição               |
|------------|-------------------------|
| FABRICANTE | Fabricante do pinpad    |
| VHARDWARE  | Versão do Hardware      |
| VFIRMWARE  | Versão do Firmware      |
| VESPEC     | Versão da Especificação |
| VAPLIC     | Versão da Aplicação     |
| NUMSERIE   | Número de Série         |

Caso ocorra algum erro durante a execução, ou falha de comunicação será retornada uma string contendo a tag [ERROABORTAR] junto com o erro ocorrido.

Uma forma alternativa para obter as informações do Pin Pad é através da função IniciaFuncaoMCInterativo utilizando o código de operação 807; o comportamento será semelhante ao fluxo normal de transação, e não sendo necessário chamar a função FinalizaFuncaoMCInterativo. Os parâmetros cupom, e nsu podem ser fictícios. O retorno desta forma é o mesmo da função direta.

Observação: Ao utilizar a função direta **InformacoesPinPad** é necessário que o PDV tenha sido inicializado/configurado, utilizando a partir da operação **807**, a função realizará este processo de configuração. Existe diferença no tempo de execução das duas formas (alguns segundos), verifique qual melhor será a mais adequada para sua automação.

## Atualizar Tabelas do Pinpad

Durante o uso diário do TEF, a atualização das tabelas do Pinpad ocorre automaticamente na primeira transação do dia, sendo um processo essencial para o funcionamento adequado do sistema. A quantidade de tabelas a serem atualizadas depende do número de bandeiras aceitas pelo estabelecimento, e o tempo necessário para essa atualização no Pinpad está diretamente relacionado a essa quantidade.

Para realizar a atualização das tabelas fora do fluxo de transação, existe a função "Atualizar Tabelas". Os possíveis retornos desta função são:
[RETORNO]#SUCESSO ou [RETORNO]#FALHA.

Esta função possui duas opções de execução que são escolhidas através do parâmetro iLimpaTb. Com o valor 0, é realizada a limpeza de tabelas e em seguida a atualização; com o valor 1 é realizada apenas a atualização das tabelas. No caso de utilizar outros valores será retornado o erro código 51.

{{< callout note >}}
Recomenda-se que a função "Atualizar Tabelas" seja executada em uma thread separada ou recurso equivalente, para evitar possíveis travamentos na automação até a conclusão da atualização.
{{< /callout >}}

## Limpar Tabelas do Pinpad

Uma alternativa para realizar a limpeza de tabelas fora do processo iterativo, é através função direta “LimparTabelas”. Os retornos são os mesmos da operação existente [RETORNO]#SUCESSO ou [RETORNO]#FALHA.
