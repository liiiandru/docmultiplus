---
title: "Autenticação"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 902
seo:
  title: "Autenticação" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para acessar os recursos da API é necessário que seja realizada primeiramente a autenticação para gerar o JWT Token utilizado nas requisições.

A autenticação pode ser feita de duas formas: com o uso dos dados do Cliente ou os dados do Representante.

## Dados do Cliente (estabelecimento).

```bash {title="Exemplo de Requisição de Autenticação - Dados Cliente"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/autenticacao' \
--header 'Content-Type: application/json' \
--data '{
    "usuario": "<USUARIO>",
    "senha": "<SENHA EM BASE64>",
    "portal": "<SUFIXO>"
}'
```

{{< callout note >}}
A senha deve estar codificada em **Base64**.
{{< /callout >}}

{{< callout note >}}
A URL do portal deve ser informada no campo SUFIXO, identificando o portal que deseja acessar.
{{< /callout >}}

## Dados do Representante

Durante o processo de autenticação via Representante, o usuário do cliente deve ser informado juntamente com as credenciais de acesso na requisição de login.


```bash {title="Exemplo de Requisição de Autenticação - Dados Representante"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/autenticacao' \
--header 'Content-Type: application/json' \
--data '{
    "usuario": "<USUARIO REPRESENTANTE>",
    "senha": "<SENHA REPRESENTANTE EM BASE64>",
    "tipo": 3,
    "inspecionar": "<USUARIO CLIENTE>"
}'
```

{{< callout note >}}
A senha deve estar codificada em **Base64**.
{{< /callout >}}
