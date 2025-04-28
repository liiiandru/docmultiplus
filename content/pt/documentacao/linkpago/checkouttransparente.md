---
title: "Checkout Transparente"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 803
sidebar:
  collapsed: true
seo:
  title: "Checkout Transparente" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A integração do Link Pago por meio do Checkout Transparente proporciona uma solução para integrar sistemas e e-commerces ao gateway de pagamentos, permitindo uma implementação flexível que se adapta ao sistema em questão. Essa abordagem elimina a necessidade de redirecionamentos para páginas externas durante o processo de pagamento, garantindo uma experiência contínua para o usuário.

## Realização do Pagamento

Para realizar um pagamento é necessário que seja feita uma requisição `POST` para a Url a seguir, informando os headers necessários para a transação.

```txt {title="URL da API"}
https://api.multipluscard.com.br/api/Servicos/SetLinkpagoCheckoutTransparente”
```

| Campo                  | Tipo       | Obrigatório | Descrição                                                                                           |
|------------------------|------------|-------------|-----------------------------------------------------------------------------------------------------|
| valor              | string     | Sim         | Valor total do link a ser gerado, deve ser maior que zero (ex: "1000,00", separador de decimal vírgula). |
| cnpj               | string     | Sim         | CNPJ do utilizador do serviço.                                                                      |
| identificacao      | string     | Não         | Texto para identificar o que se refere este pagamento.                                              |
| produtos           | string     | Não         | Produtos para demonstrar a compra, com separadores “;” entre campos e “|” entre produtos. Ex: "1;Produto 1;1;5,00\|2;Produto 2;1;2,50"           |
| metodo             | string     | Sim         | Tipo do método de pagamento (ex: "Visa Credito", "Mastercard Credito", etc.).                       |
| parcelas           | inteiro    | Sim         | Número de parcelas (1 a 12).                                                                         |
| identificacaofatura  | string     | Não         | Nome a aparecer na fatura, só enviar se a operadora permitir a troca da identificação da fatura, Caso contrário **NÃO** envie este campo.   |
| nome_cartao        | string     | Sim         | Nome do responsável pelo cartão.                                                                   |
| num_cartao         | string     | Sim         | Número do cartão (somente os números, 16 caracteres).                                               |
| cvv_cartao         | string     | Sim         | Código verificador do cartão (3 caracteres).                                                       |
| mes_cartao         | string     | Sim         | Mês de vencimento do cartão (formato numérico, 2 caracteres).                                       |
| ano_cartao         | string     | Sim         | Ano de vencimento do cartão (formato numérico, 2 caracteres).                                      |
| ClienteNome        | string     | Sim         | Nome do pagador.                                                                                    |
| ClienteDocto       | string     | Não         | CPF/CNPJ do pagador.                                                                                |
| ClienteEmail       | string     | Não         | E-mail do pagador.                                                                                 |
| ClienteFone        | string     | Não         | Telefone ou celular do pagador.                                                                     |
| ClienteAniversario | string     | Não         | Data de nascimento do cliente (dd/mm/aaaa).                                                        |
| retornarJson       | string     | Não         | Informar “S” caso queira o retorno em JSON.                                                         |

Em caso de sucesso seré retornado da seguinte forma:

```csv{title="Exemplos de Retorno em CSV"}
C;STATUS;MENSAGEM;NSU;AUTORIZACAO;CLIENTE;IDENTIFICACAO;HASH
D; 00 ; Aprovado ; 0000 ; 00000 ; cpf | nome | email | fone ; identificação ; xxxxxxxxxxx

C;STATUS;MENSAGEM;NSU;AUTORIZACAO;CLIENTE;IDENTIFICACAO;HASH
D ; 78 ; Cancelada ; ; ; ; identificação ; xxxxxxxxxxx

C;STATUS;MENSAGEM;NSU;AUTORIZACAO;CLIENTE;IDENTIFICACAO;HASH
D ; 57 ; Recusada / Negada ; ; ; ; identificação ; xxxxxxxxxxx
````

```json{title="Exemplo de Retorno em JSON"}
{
  "STATUS": "00",
  "MENSAGEM": "Aprovado e Capturado",
  "NSU": "0000000-00000000000-00000000-0000000",
  "AUTORIZACAO": "000000",
  "CLIENTE": "cpf/cnpj | nome | e-mail | fone",
  "IDENTIFICACAO": "xxxxxxxxxxxx",
  "HASH": "xxxxxxxxxxxxxx"
}
```

Caso ocorra erro na requisição será retornada uma string contendo o motivo:

```txt {title="Exemplo de Retorno com Erro"}
[ERRO] CNPJ INVÁLIDO
```

{{< callout context="note" icon="outline/info-circle">}}
- O campo `STATUS` com o valor `00` indica que a venda foi aprovada com sucesso e está em conformidade.

- O campo `STATUS` com o valor `57` indica que a venda não foi realizada. Nesse caso, o campo `MENSAGEM` irá fornecer a causa do problema. Além disso, os campos `NSU`, `Autorização` e `Cliente` não estarão preenchidos.

- O campo `STATUS` com o valor `78` indica que a venda foi inicialmente aprovada, mas posteriormente foi cancelada.

{{< /callout >}}
