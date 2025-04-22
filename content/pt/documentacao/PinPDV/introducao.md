---
title: "Introdução"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 501
draft: false
seo:
  title: "Introdução" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
A integração entre o sistema parceiro e o **PINPDV** é realizada por meio de uma **API Web no padrão REST**.

Este documento tem como objetivo **servir como guia técnico de integração**, descrevendo os fluxos e processos necessários para o correto funcionamento da comunicação entre os sistemas.

## Funcionalidades Principais

O **PINPDV Lite** oferece quatro funcionalidades principais:

### Produtos
Venda iniciada no PINPDV, utilizando produtos previamente cadastrados na base da API.
O usuário pode realizar a venda e emitir o documento correspondente (fiscal ou não fiscal), desde que autorizado pelo sistema parceiro.

### Pré-venda
Venda iniciada pelo sistema parceiro.
A partir da criação de uma pré-venda, o usuário pode finalizar a transação no PINPDV com a forma de pagamento desejada e gerar o documento referente.

### POS TEF
Captura de transações TEF iniciadas pelo sistema parceiro, mediante envio de parâmetros específicos.
As informações de pagamento são retornadas durante e ao final da operação.

### Venda Expressa
Transações iniciadas diretamente no PINPDV.
Posteriormente, o sistema parceiro pode acessar os dados dessas vendas de forma integrada.

## Informações Importantes

### Pré-requisitos

Para utilizar o aplicativo ou a API, é necessário que o cliente tenha contratado o produto **PINPDV** com a **Multiplus Card** e esteja com o serviço devidamente ativo, sem restrições de uso.

### Padrão de Integração

Todas as integrações são realizadas exclusivamente via **API REST**, seguindo boas práticas previamente acordadas entre as partes (como controle de requisições, _polling_, _rate limit_, etc.).

### Autenticação

Toda requisição à API deve conter um **token de autenticação** no formato: `Bearer <token>`
Esse token pode ser obtido de duas formas:

- Autenticação via **login e senha**
- Utilização do **token de longa duração** fornecido ao cliente

### Formato de Resposta

- As respostas da API são, preferencialmente, em **formato JSON**.
- Os códigos de resposta HTTP devem ser interpretados da seguinte forma:
  - `2xx`: Sucesso
  - `4xx`: Erro na requisição

### Uso de Chaves com `{}`

Campos descritos no formato `{CHAVE}` devem ser substituídos pelo valor real no contexto de uso.
**Exemplo**: `{SENHA}` deve ser substituído pela senha do usuário que está utilizando a API.

### Dados de Exemplo

Alguns nomes de produtos e outras informações contidas nos exemplos deste manual são meramente ilustrativos e têm fins exclusivamente didáticos.

## Próximos Passos

Nas seções seguintes, você encontrará o **fluxo de cada funcionalidade**, acompanhado de exemplos de chamadas via `curl` para facilitar a implementação.


