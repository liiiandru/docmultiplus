---
title: "Fluxo de Utilização"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 303
seo:
  title: "Fluxo de Utilização" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A integração é fundamentada na troca de arquivos entre o sistema de Automação e o Gerenciador de Pagamentos (GP). Após a finalização de uma venda, a Automação aciona a solução de TEF, iniciando o processo de pagamento.

O diagrama a seguir apresenta em detalhes o fluxo de comunicação entre a Automação e o GP.


{{< figure
  src="images/fluxo_tefdial.png"
  alt="Interação entre Automação e Gerenciador de Pagamentos"
  caption="Interação entre Automação e Gerenciador de Pagamentos"
  max-width="100%"
>}}

{{< callout note >}}
Para a transação **ATV**, o fluxo é encerrado na **Etapa 2**. Já para transações que **não exigem** a impressão do comprovante TEF, o fluxo se encerra na **Etapa 4**. Caso a impressão seja necessária, o processo deve seguir até a **Etapa 6**.

Ressalta-se que **não é possível determinar previamente**, apenas com base no tipo de transação, se a impressão do comprovante TEF será requerida. O Aplicativo de Automação Comercial deve sempre analisar todas as informações contidas no arquivo recebido na **Etapa 4** para tomar a decisão adequada.
{{< /callout >}}

## Estrutura de Diretórios

Para que ocorra a troca de arquivos são necessárias que existam duas pastas:

```txt
  C:\TEF_DIAL\REQ
  C:\TEF_DIAL\RESP
```

### Diretório de Envio de Dados

Este é utilizado pela Automação para enviar os dados para o Gerenciador de Pagamentos (GP). A automação deve gerar uma arquivo para **cada envio** de mensagem.
Após recebido pelo GP ele realiza o processamento e excluí o mesmo.

O arquivo a ser criado deve ter o seguinte nome: **IntPos.001**

```txt {title="Arquivo de Requisição"}
  C:\TEF_DIAL\REQ\IntPos.001
```

{{< callout context="tip" title="Dica" icon="outline/rocket" >}}
Para a geração do arquivo **IntPos.001**, recomenda-se inicialmente a criação de um arquivo temporário, nomeado como **IntPos.tmp**. Após a inclusão completa de todos os dados necessários para o envio da mensagem, o arquivo deve ser renomeado para **IntPos.001**.

Essa abordagem evita que o Gerenciador de Pagamentos (GP) realize a leitura do arquivo antes de sua finalização, garantindo a integridade da comunicação.
{{< /callout >}}

### Diretório de Retorno de Dados

Este arquivo é utilizado pela Automação para receber os dados enviados pelo Gerenciador de Pagamentos (GP). Durante o fluxo de comunicação, são gerados arquivos que devem ser lidos, processados e excluídos pela Automação. O arquivo de resposta, nomeado como **InPos.sts**, contém a indicação de aceite ou recusa da solicitação previamente enviada.

```txt {title="Arquivo de Retorno"}
  C:\TEF_DIAL\RESP\IntPos.sts
```

## Diagrama de Comunicação

{{< figure
  src="images/fluxo_completo_tefdial.png"
  alt="Fluxo da Transação entre Automação e Gerenciador de Pagamentos"
  caption="Fluxo da Transação entre Automação e Gerenciador de Pagamentos"
  max-width="100%"
>}}

### Observações

- Em caso de queda de energia durante a impressão do cupom, a Automação deverá automáticamente enviar o comando **NCN** apresentando a mensagem:

```
  Cancela a Transação:
  Rede:
  NSU:
  Valor:
```

- Na exibição de mensagens de não confirmação, os campos Doc. No (campo 12) e Rede (campo 10) devem ser apresentados obrigatoriamente. O campo Valor (campo 3) deve ser exibido apenas se estiver presente no arquivo Intpos.001 localizado no diretório Resp, e possuir valor diferente de zero. Quando exibido, o campo Valor deve ser formatado como moeda.

- O número de vias impressas poderá ser parametrizado; Para fins de homologação, os testes serão realizados com a impressão de duas vias.

- A verificação da impressão correta do comprovante é de responsabilidade da automação

- Exibição da Mensagem – Campo 30

A mensagem contida no **Campo 30** deve ser apresentada pela Automação Comercial **somente quando o conteúdo for diferente de vazio**. Caso o campo esteja vazio, **nenhum Message Box deverá ser exibido**.

- **Com impressão de comprovante (linhas presentes)**:
  Se houver linhas a serem impressas, a mensagem do Campo 30 deve ser exibida **simultaneamente à impressão do comprovante TEF**.
  O Message Box **não deve bloquear ou aguardar confirmação do usuário (OK)** para iniciar a impressão.
  A mensagem deve permanecer visível por **no mínimo 5 segundos**, ou até o término da impressão.

- **Sem impressão de comprovante (sem linhas)**:
  Na ausência de linhas para impressão, a mensagem do Campo 30 deve ser exibida e **aguardar a confirmação (OK) do usuário** antes de ser encerrada.
