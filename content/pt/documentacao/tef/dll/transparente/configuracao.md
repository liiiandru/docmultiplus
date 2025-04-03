---
title: "Instalação/Configuração"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 103
seo:
  title: "Instalação/Configuração" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

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

## Configurações Adicionais

Valores configurações adicionais
|   Código | Descrição                                                                                             |
|----------|-------------------------------------------------------------------------------------------------------|
|       29 | Opcional, habilita venda crédito digitada (em alguns cartões não é permitido esse modo de utilização) |
|       42 | Opcional habilita venda débito digitada (em alguns cartões não é permitido esse modo de utilização)   |

{{< callout note >}} Se sua automação não realiza vendas com cartão digitado, mantenha o parâmetro **TransacoesAdicionaisHabilitadas** sem valor. {{< /callout >}}

## Requisitos de Execução

Para que o funcionamento da DLL ocorra de forma correta é necessário que a automação seja executada em **Modo Administrador**, caso não seja seguido erros poderão ser ocasionados. A aplicação ClientD.exe também deve estar configurada para executar em Modo Administrador.

## Processo de Atualização

A aplicação ClientD.exe é atualizada automaticamente em momentos estratégicos, ou seja, quando não está realizando transações. Se, por algum motivo, estiver em processo de atualização e uma transação for iniciada, você receberá o código de erro 41. Nesse caso, tente realizar a transação novamente até que o erro não seja mais retornado.
No entanto, a DLL TefClientMC requer uma **atualização manual**, pois tem um impacto direto na automação. Em caso de mudanças significativas na versão disponibilizada (*break changes*), haverá um aviso no Portal. Sempre é recomendável testar as novas versões antes de liberar a automação para produção.
