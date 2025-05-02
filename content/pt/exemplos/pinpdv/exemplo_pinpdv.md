---
title: "Exemplos PinPDV"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 201
seo:
  title: "Exemplos PinPDV" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

### Autenticação por requisição

**POST** `https://webapi.pinpdv.com.br/usuario/autenticar`

**Headers:**
```http
Content-Type: application/json
```

**Body:**
```json
{
    "Usuario": "USUARIO",
    "Senha": "SENHA"
}
```

---

### Listar empresas

**GET** `https://webapi.pinpdv.com.br/empresa?OrdenarPor=Id&OrdenarTipo=DESC`

**Auth:** Bearer Token

---

### Autenticação - token de longa duração

**POST** `https://webapi.pinpdv.com.br/usuario/token`

**Auth:** Bearer Token

**Body:**
```json
{
    "EmpresaId": "00",
    "Descricao": "RAZAO SOCIAL",
    "DataExpiracao":"2100-01-01"
}
```

---

### Impressão de documento

**POST** `https://webapi.pinpdv.com.br/venda/{vendaID}/comprovante?empresaId={empresaId}`

**Auth:** Bearer Token

**Headers:**
```http
Content-Type: text/plain
```

**Body:**
```text
Aviso de comprovante
```

---

### PRODUTOS - Cadastro

**GET** `https://webapi.pinpdv.com.br/produto?empresaId={empresaID}&Identificador=cod1&Nome=Refrigerante 350&Preco=2.90&Imagem={ImagemEmBase64}`

---

### PRODUTOS - Atualizar

**GET** `https://webapi.pinpdv.com.br/produto?empresaId={empresaID}&Identificador=cod1&Nome=Refrigerante 350&Preco=3.90&Imagem={ImagemEmBase64}`

---

### Consultar PINPDV Ativo

**GET** `https://webapi.pinpdv.com.br/pinpdv`

**Auth:** Bearer Token

---

### POS TEF - Gerar venda

**POST** `https://webapi.pinpdv.com.br/pos-venda`

**Auth:** Bearer Token

**Body:**
```json
{
    "PinPdvId": "000",
    "Identificador": "Teste-1",
    "Valor": 1.10,
    "Descricao": "teste",
    "TipoPagamento": 3,
    "Parcelas": 1
}
```

---

### POS TEF - Abortar solicitacao

**DELETE** `https://webapi.pinpdv.com.br/pos-venda/{Identificador}`

**Auth:** Bearer Token

---

### POS TEF - Consultar solicitacao

**GET** `https://webapi.pinpdv.com.br/pos-venda/Teste-1234`

**Auth:** Bearer Token

---

### POS TEF - Consultar solicitacao

**GET** `https://webapi.pinpdv.com.br/pos-venda/20250325-000-000`

**Auth:** Bearer Token

---

### Consultar Vendas

**GET** `https://webapi.pinpdv.com.br/venda?empresaId=0&paginaNumero=1&qtdRegistro=10&ordenarPor=id&ordenarTipo=desc&FiltroTipoVenda=Expressa`

**Auth:** Bearer Token

---

### Consultar venda Transação.md

**GET** `https://webapi.relatoriotef.com.br/transacao/pos?cnpj=12345678900000&DataInicial=2024-11-14T00:00:00&DataFinal=2024-11-14T23:59:59`

**Auth:** Bearer Token

**Headers:**
```http
Authorization: Bearer ••••••
```

---

### Consultar venda Transação.md

**GET** `https://webapi.relatoriotef.com.br/transacao/pos?cnpj=12345678900000&DataInicial=2024-11-14T00:00:00&DataFinal=2024-11-14T23:59:59`

**Auth:** Bearer Token

**Headers:**
```http
Authorization: Bearer ••••••
```

---

### Atualizando documento comprovante

**PUT** `https://webapi.pinpdv.com.br/venda/c723aa80b30yy1efb703e922760eb2x7/comprovante`

**Auth:** Bearer Token

**Headers:**
```http
Content-Type: text/plain
```

**Body:**
```text
Aviso de comprovante
```

---

### Consulta de pré-venda

**GET** `https://webapi.pinpdv.com.br/pre-venda/000`

**Auth:** Bearer Token

---

### PRE VENDA - Abortar solicitacao Pre Venda

**DELETE** `https://webapi.pinpdv.com.br/pre-venda/20250321-0000-001`

**Auth:** Bearer Token

---

### Consulta de Vendas - Geral

**GET** `https://webapi.pinpdv.com.br/venda?DataInicial=2024-12-08T00:00:00&DataFinal=2024-12-08T23:59:59`

**Auth:** Bearer Token

---

### https://webapi.pinpdv.com.br/produto

**POST** `https://webapi.pinpdv.com.br/produto`

**Auth:** Bearer Token

**Body:**
```json
[]
```

---
