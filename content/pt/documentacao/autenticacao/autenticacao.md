---
title: "Credenciais/Tokens"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 1101
seo:
  title: "Credenciais/Tokens" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Antes de acessar determinados recursos das APIs da Multiplus Card, é necessário realizar o processo de autenticação. Esse processo gera um JWT Token, que deverá ser incluído nas requisições subsequentes para garantir a segurança e a identificação do usuário.

A seguir, apresentamos os métodos de autenticação atualmente disponíveis, juntamente com suas finalidades específicas.

## Credenciais

Os recursos da API são divididos em duas categorias `Cliente` e `Representante`, existem recusos exclusivos para cada categoria, e alguns que são compartilhados por ambas.

### Cliente

A autenticação é feita utilizando os dados do cliente (estabelecimento).

```bash {title="Exemplo de Requisição de Autenticação - Cliente"}
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

### Representante

A autenticação é feita utilizando os dados do representante (parceiro comercial).

```bash {title="Exemplo de Requisição de Autenticação - Representante"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/autenticacao' \
--header 'Content-Type: application/json' \
--data '{
    "usuario": "<USUARIO>",
    "senha": "<SENHA EM BASE64>",
    "tipo": "1"
}'
```

## Inspeção de Cliente

Em determinados cenários, o representante pode necessitar acessar os dados de um cliente. Nesses casos, não é necessário utilizar as credenciais do cliente para autenticação.
O processo é realizado utilizando as credenciais do representante, acompanhadas do usuário do cliente que se deseja consultar.

```bash {title="Exemplo de Requisição de Autenticação - Inspeção de Cliente"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/autenticacao' \
--header 'Content-Type: application/json' \
--data '{
    "usuario": "<USUARIO REPRESENTANTE>",
    "senha": "<SENHA REPRESENTANTE EM BASE64>",
    "tipo": 3,
    "inspecionar": "<USUARIO CLIENTE>"
}'
```

```bash {title="Exemplo de Requisição de Autenticação - Inspeção de Cliente (Token de Longa Duração)"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/autenticacao/token' \
--header 'Authorization: Bearer ••••••'
--header 'Content-Type: application/json' \
--data '{
    "tipo": 3,
    "inspecionar": "<USUARIO CLIENTE>"
}'
```

## Token de Longa Duração

Para conceder o acesso da API à terceiros sem a necessidade de informar as credenciais, existe a opção de criar um token de longa duração, que pode ser bloqueado ou excluído a qualquer momento.

### Geração do Token

O token de longa duração possuí validade máxima de 3 anos, no momento da criação deve ser informada a data de expiração. O campo `descrição` pode ser utilizado para controlar melhor os tokens gerados.

```bash {title="Exemplo de Criação de Token de Longa Duração"}
curl -X POST --location 'https://webapi.relatoriotef.com.br/usuario/token' \
--header 'Authorization: Bearer ••••••'
--header 'Content-Type: application/json' \
--data '{
    "descricao": "DESCRIÇÃO",
    "dataExpiracao": "2025-12-31T00:00:00"
}'
```

{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
O token retornado pela requisição **não é armazenado** pela API da Multiplus Card. Portanto, é responsabilidade do integrador armazená-lo de forma segura após a autenticação.
Caso o token seja perdido, será necessário realizar uma nova autenticação para gerar um novo token.
{{< /callout >}}

### Listagem de Tokens

Lista os tokens gerados com suas respectivas validades.

```bash {title="Exemplo de Listagem de Token"}
curl --location 'https://webapi.relatoriotef.com.br/usuario/token' \
--header 'Authorization: Bearer ••••••'
```

```json {title="Exemplo de Retorno"}
{
    "paginaAtual": 1,
    "itensPorPagina": 100,
    "quantidadeDePaginas": 1,
    "quantidadeTotalDeItens": 1,
    "primeiroRegistro": 1,
    "ultimoRegistro": 1,
    "paginaAnterior": false,
    "paginaProxima": false,
    "data": [
        {
            "id": 123456,
            "descricao": "TESTES PROD",
            "dataExpiracao": "2027-12-31T23:59:59"
        }
    ]
}
```

### Revogação/Exclusão de Token

Para revogar um token é necesasário fazer a seguinte requisição:

```bash {title="Exemplo de Exclusão de Token"}
curl --location --request DELETE 'https://webapi.relatoriotef.com.br/usuario/token' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer ••••••' \
--data '{"id": <ID>}'
```
