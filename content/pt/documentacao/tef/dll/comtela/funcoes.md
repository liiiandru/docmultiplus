---
title: "Funções da DLL"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 203
seo:
  title: "Funções da DLL" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
Para realizar transações e operações administrativas, é necessário implementar a função correspondente ao recurso desejado. A seguir, são descritas todas as funções disponíveis, juntamente com seus parâmetros e tipos de retorno.

## Parâmetros Utilizados

A maioria das funções utiliza um conjunto comum de parâmetros. Abaixo, apresentamos esses parâmetros com seus respectivos tamanhos e uma breve descrição de cada um.

| Parâmetro     | Tamanho       | Descrição                                                                     |
|---------------|---------------|-------------------------------------------------------------------------------|
| sCNPJCliente  | 14 caracteres | CNPJ do Cliente que está utilizando o ClientTEF                               |
| sCnpjParceiro | 14 caracteres | CNPJ do Representante/Desenvolvedor da Automação                              |
| iCupom        |               | Número do Cupom Fiscal, um número inteiro                                     |
| dValor        |               | Valor da Transação, com duas casas decimais e virgula como separador.         |
| iLeitor       |               | Informa se a venda será finalizada pelo **Pinpad=0** ou **SmartPOS=3**        |

{{< callout note >}} Caso não utilize integração com o App PinPDV, o parâmetro **iLeitor** deve ter o valor **0**. {{< /callout >}}

## Funções de Venda

### Venda Crédito
Realiza uma Transação com Cartão de Crédito, sendo necessário selecionar a modalidade e parcelas posteriormente.

```csharp{title="Protótipo da Função"}
  BSRT VendeCredito(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Crédito À Vista
Realiza uma Transação com Cartão de Crédito na modalidade À Vista, não há questionamento sobre parcelamento.

```csharp{title="Protótipo da Função"}
  BSRT VendeCreditoVista(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Crédito Parcelada Loja
Realiza uma Transação com Cartão de Crédito na modalidade Parcelada Loja, as parcelas já devem ser informadas na chamada da função.

```csharp{title="Protótipo da Função"}
  BSRT VendeCreditoParcLoja(string sCNPJCliente, string sCnpjParceiro, int iParcelas,
  double dValor, int iCupom, int iLeitor)
```

### Venda Crédito Parcelada Adm
Realiza uma Transação com Cartão de Crédito na modalidade Parcelada Adm, as parcelas já devem ser informadas na chamada da função.

```csharp{title="Protótipo da Função"}
  BSRT VendeCreditoparcAdm(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Débito
Realiza uma Transação com Cartão de Débito, sendo necessário selecionar a modalidade posteriormente (Depende das configurações da Operadora).

```csharp{title="Protótipo da Função"}
  BSRT VendeDebito(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Débito À Vista
Realiza uma Transação com Cartão de Débito, na modalidade Débito à Vista.

```csharp{title="Protótipo da Função"}
  BSRT VendeDebitoAVista(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Genérica
Inicia uma transação sem definir a modalidade de pagamento (por ex: Crédito ou Débito) e parcelamento, estas informações serão solicitadas posteriormente pelo Gerenciador Padrão.

```csharp{title="Protótipo da Função"}
  BSRT VendaGenerica(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda Frota Completo
Realiza uma Transação com Cartão Frota (Abastecimento/Combustível), sendo necessário enviar todos os parâmetros da função.

O parâmetro sItens possuí um layout específico para ser utilizado, as informações sobre o item são separadas por hífen “**-**“ e os itens separados por pipe “**|**”.

```txt{title="Layout do parâmetro"}
  [Código]-[Descrição]-[Quantidade]-[Vl Unitário]-[Desconto]-[Vl Total Item]
```

```txt{title="Exemplo"}
  01-ETANOL-1,000-,3,699-0,00-3,70|02-GASOLINACOMUM-2,000-5,699-0,00-11,40
```

```csharp{title="Protótipo da Função"}
  BSRT VendeFrotaCompleto(string sCNPJCliente, string sCnpjParceiro, double dValor,
  string sKM, string sPlaca, string sMatricula, int QtdeItens, string sItens,
  int iCupom, int iLeitor)
```

### Venda Menu Private Label
Realiza uma Transação com Cartão do tipo Private Label.

```csharp{title="Protótipo da Função"}
  BSRT MenuPrivate(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Venda com Carteira Digital - PIX
Realiza uma Transação com PIX e Carteiras Digitais, já previamente configuradas para o cliente.

```csharp{title="Protótipo da Função"}
  BSRT VendeCarteiraDigitalPix(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Recarga de Celular
Realiza uma transação de Recarga de Celular.

```csharp{title="Protótipo da Função"}
  BSRT ReacargaCelular(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Parcele Mais
Realiza o acionamento da função Parcele mais.

```csharp{title="Protótipo da Função"}
  BSRT ParceleMais(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

## Finalização da Venda

### Confirmação
Realiza a Confirmação da última transação realizada.

```csharp{title="Protótipo da Função"}
  BSRT Confirmar(string sCNPJCliente, string sCnpjParceiro, int iCupom)
```

### Desfazimento
Realiza o Desfazimento de uma transação com o status Pendente.

```csharp{title="Protótipo da Função"}
  BSRT Desfazimento(string sCNPJCliente, string sCnpjParceiro, int iCupom, int iLeitor)
```

## Link de Pagamento

### Realizar Venda

Em uma Transação com Link de Pagamento, para que seja exibida ao cliente no momento do pagamento informações relacionadas a compra (produtos, valores) ou alguma observação, devem ser preenchidos os parâmetros: QtdeItens, sItens e sTexto; o parâmetro sTelefone é utilizado para enviar o link por SMS ao cliente.
O parâmetro sItens possui layout onde as informações do item são separadas pelo caractere negação lógica “**¬**” e os itens são separados por pipe “**|**”:

```txt{title="Layout do parâmetro"}
  [Código]¬[Descrição]¬[Quantidade]¬[Vl Unitário]¬[Desconto]¬[Vl Total Item]
```

```txt{title="Exemplo"}
  1¬Mouse¬1¬10,00¬0,00¬10,00|2¬Teclado¬1¬35,00¬0,00¬35,00
```

O parâmetro sTexto se refere a uma observação que pode ser incluída na transação.

```csharp{title="Protótipo da Função"}
  BSRT LinkPagamento(string sCNPJCliente, string sCnpjParceiro, int iParcelas
  double dValor, int iCupom, int QtdeItens, string sItens, string sTelefone,
  string sTexto, int iLeitor)
```

### Listar Link de Pagamento

Esta função retorna os links de pagamento gerados e que possuem pagamento realizado.

```csharp{title="Protótipo da Função"}
  BSRT ListarLinkPagamentoPago(string sCNPJCliente, string sCnpjParceiro,
  double dValor, int iCupom, int iLeitor)
```

### Manutenção de Link de Pagamento

Esta função exibe uma tela onde é possível listar os links gerados, realizar cancelamentos e verificar seu status.

```csharp{title="Protótipo da Função"}
  BSRT ManutencaoLinkPagamento(string sCNPJCliente, string sCnpjParceiro,
  double dValor, int iLeitor)
```

## Funções Administrativas

### Cancelamento
Realiza o Cancelamento da transação informada, o parâmetro sControle não é obrigatório, pode ser enviado vazio.

```csharp{title="Protótipo da Função"}
  BSRT Cancelar(string sCNPJCliente, string sCnpjParceiro, double dValor, int iCupom,
  string sControle, int iLeitor)
```

### Reimpressão
Exibe uma tela listando todas as transações realizadas no PDV, onde deve ser selecionada a desejada para reimprimir o cupom.

```csharp{title="Protótipo da Função"}
  BSRT Reimpressao(string sCNPJCliente, string sCnpjParceiro, int iLeitor)
```

### Reimpressão Direta
Realiza a reimpressão do comprovante de uma transação sem a exibição de telas.

```csharp{title="Protótipo da Função"}
  BSRT ReimpressaoDireto(string sCNPJCliente, string sCnpjParceiro, string sNSU,
  string Data, int iCupom, int iLeitor)
```

### Relatório
Exibe uma tela para a geração e impressão de relatórios com base nos filtros selecionados.

```csharp{title="Protótipo da Função"}
  BSRT Relatorio(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Função Adm
Realiza a chamada da tela de funções administrativas do TEF (configurações, cancelamento, reimpressão e relatórios).

```csharp{title="Protótipo da Função"}
  BSRT Adm(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```

### Função Atv
Verifica se o ClientTEF está em execução.

```csharp{title="Protótipo da Função"}
  BSRT Atv(string sCNPJCliente, string sCnpjParceiro, double dValor,
  int iCupom, int iLeitor)
```
