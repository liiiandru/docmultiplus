---
title: "Introdução"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 201
seo:
  title: "Introdução" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
A DLL **TefClientMC** Modo Client oferece uma integração ágil e eficiente com o Gerenciador de Pagamentos da Multiplus Card, responsável por gerenciar todo o fluxo de pagamento. Ideal para automações que exigem uma implementação simples, rápida e confiável.

## Requisitos para Execução

Para a execução da DLL, é necessário que os seguintes componentes de software estejam instalados:

- Gerenciador de Pagamentos da Multiplus Card (download no Portal do Representante)
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

A integração com a DLL no Modo Client ocorre por meio da implementação das funções de transação disponíveis, seguindo o fluxo descrito pelo fluxograma abaixo.


{{< figure
  src="images/dll_com_tela.png"
  alt="Diagrama do fluxo de utilização da DLL"
  caption="Diagrama do fluxo de utilização da DLL"
>}}
