---
title: "Obtenção de Transações"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 805
sidebar:
  collapsed: true
seo:
  title: "Obtenção de Transações" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Para obter as transações realizadas com o Link Pago, verificar seu status entre outras informações, é necessário que seja feita uma requisição `GET` na Url a seguir e informar os headers:

```txt {title="URL da API"}
https://api.multipluscard.com.br/api/Servicos/GetTransacoes
```

| **Header**                   | **Tipo**           | **Obrigatório** | **Descrição**                                                                                                                                                                                                                      |
|------------------------------|--------------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| token                    | `string`           | Sim             | Solicite seu token junto à Multiplus Card.                                                                                                                                                                                         |
| DataInicio               | `datetime (string)`| Sim             | Data de início da pesquisa.                                                                                                                                                                                                        |
| DataFinal                | `datetime (string)`| Sim             | Data de fim da pesquisa.                                                                                                                                                                                                           |
| tipodatapesquisa         | `string`           | Sim             | Tipo da pesquisa: `PC`, `ADM` ou `SERVER`.                                                                                                                                                                                         |
| terminal                 | `string`           | Sim             | Exemplo: `"001"` ou `"0"` para todos os terminais.                                                                                                                                                                                 |
| NumeroPagina             | `int`              | Sim             | Número da página para paginação dos resultados.                                                                                                                                                                                    |
| CNPJ                     | `string`           | Sim             | CNPJ da empresa.                                                                                                                                                                                                                   |
| Hash                     | `string`           | Não             | Retorna dados de uma transação específica (obtido no retorno do serviço `SetLinkPagoIniciaCheckout`).                                                                                                                              |
| QtdePorPagina            | `int`              | Não             | Quantidade de registros por página (máximo 1000).                                                                                                                                                                                  |
| HoraInicio               | `Time (string)`    | Não             | Horário inicial da pesquisa, padrão `"00:00:00"`.                                                                                                                                                                                  |
| HoraFim                 | `Time (string)`    | Não             | Horário final da pesquisa, padrão `"23:59:00"`.                                                                                                                                                                                    |
| RetornaJSON              | `string`           | Não             | `"SIM"` para o serviço retornar os dados em JSON.                                                                                                                                                                                  |
| nsu                      | `string`           | Não             | Número de solicitação único.                                                                                                                                                                                                       |
| autorizacao              | `string`           | Não             | Código de autorização.                                                                                                                                                                                                             |
| ValorTransacao           | `string`           | Não             | Valor da transação (ex.: `"1000,00"` para mil reais).                                                                                                                                                                              |
| status                   | `string`           | Não             | Status da transação (`"OK"`, `"PEND"`, `"CANC"`, `"DESF"`, `"NEG"`), separados por `|` (pipe) se múltiplos. Exemplo: `"OK|CANC|DESF"`.                                                                                              |
| RetornarDadosConciliacao | `int`              | Não             | `1` = sim, `0` = não.                                                                                                                                                                                                              |
| Recarga                  | `int`              | Não             | `1` ou vazio = não retornar recargas; `2` = somente recargas; `3` = recargas e outras transações.                                                                                                                                  |
| LinkPago                | `int`              | Não             | `1` ou vazio = não retornar transações LinkPago; `2` = somente LinkPago; `3` = LinkPago e outras.                                                                                                                                  |
| IdentificadorLinkPago    | `string`           | Não             | Retorna transação de uma identificação LinkPago específica.                                                                                                                                                                        |
| LinkPagoSomenteDisponiveis| `string`          | Não             | Retorna somente transações LinkPago não utilizadas. Recomenda-se usar também `LinkPago=2`.                                                                                                                                         |


{{< callout context="caution" title="Atenção" icon="outline/alert-triangle" >}}
Para otimizar o desempenho, recomenda-se realizar esta requisição com um intervalo mínimo de 3 segundos entre cada execução.
{{< /callout >}}


Em caso de sucesso serão retornadas as transações no formato selecionado no header `RetornaJSON`.

```csv {title="Exemplo de Retorno em CSV"}
C;CNPJ_ESTABELECIMENTO;REDE;BANDEIRA;DATA_TRANSACAO;NSU;CODIGO_AUTORIZACAO;VALOR_TAXA_SER
VICO;VALOR;QTDE_PARCELAS;DATA_HORA_ADM;DATA_HORA_SERVER;DATA_HORA_ESTABELECIMENTO;CODIG
O_RESPOSTA;COD_SERVICO;SERVICO;COD_GRUPO_SERVICO;GRUPO_SERVICO;TERMINAL;ORIGEM;IDENTIFICAD
OR;HASH_LINKPAGO
D;05625872000169;CIELO;MAESTRO;11/01/2017;005180;972216;0,00;49,99;1;11/01/2017 20:50:34;11/01/2017
20:50:29;11/01/2017 20:50:35;055;6;Débito à Vista;3;Cartão de Débito;005;WEB; MOTIVO DO LINK ¬
CELULAR(SMS) ¬ CLIENTE QUE PAGOU;XXXXXXXXXXXXXXXXX
D;05625872000169;CIELO;MAESTRO;11/01/2017;005182;501117;0,00;49,99;1;11/01/2017 20:51:36;11/01/2017
20:51:32;11/01/2017 20:51:38;000;6;Débito à Vista;3;Cartão de Débito;005;WEB; MOTIVO DO LINK ¬
CELULAR(SMS) ¬ CLIENTE QUE PAGOU;XXXXXXXXXXXXX
D;05625872000169;CIELO;MAESTRO;11/01/2017;003641;111816;0,00;116,01;1;11/01/2017
11:32:25;11/01/2017 11:32:23;11/01/2017 11:32:27;000;6;Débito à Vista;3;Cartão de Débito;003;WEB; MOTIVO
DO LINK ¬ CELULAR(SMS) ¬ CLIENTE QUE PAGOU;XXXXXXXXXXXXXXXXX
F;1-1;3
```

{{< callout note >}}
A primeira letra indica o tipo de linha no retorno: "C" para Cabeçalho, "D" para Detalhe e "F" para Final.

Na última linha, após o "F", serão exibidos a página atual, o total de páginas e o total de registros.
{{< /callout >}}

```json {title="Exemplo de Retorno em JSON"}
{
  "PaginaAtual": 1,
  "UltimaPagina": 1,
  "QtdeRegistrosPorPagina": 1,
  "Transacoes": [
    {
      "Origem": "POS",
      "CodigoEmpresa": 0,
      "RazaoSocial": "CLIENTE RAZAO",
      "NomeFantasia": "CLIENTE FANTASIA",
      "CNPJ": "00000000000000",
      "RedeCodigo": 0,
      "RedeNome": "OPERADORA",
      "RedeDescricao": null,
      "RedeNomeDicionario": null,
      "RedeConciliadorCodigo": 0,
      "BandeiraCodigo": 0,
      "BandeiraNome": "BANDEIRA",
      "BandeiraNomeDicionario": null,
      "BandeiraConciliadorCodigo": 0,
      "GrupoServicoCodigoOrigem": "0",
      "GrupoServicoDescricao": "VENDA CREDITO",
      "ServicoCodigo": 0,
      "ServicoDescricao": "CARTAO DE CREDITO",
      "TransacaoCodigo": 123456,
      "TransacaoDataBruta": 0,
      "TransacaoDataMensagem": "/Date(1589511600000)/",
      "TransacaoNSU": "",
      "TransacaoCodigoEstabelecimento": "",
      "TransacaoDataHoraEstabelecimento": "/Date(1589563703410)/",
      "TransacaoDataHoraAdministradora": "/Date(1589563703410)/",
      "TransacaoValor": 10,
      "TransacaoValorTaxaServico": 0,
      "TransacaoNumeroConta": "",
      "TransacaoVencmentoCartao": null,
      "TransacaoCodigoAutorizacao": "",
      "TransacaoNsuHost": null,
      "TransacaoStatusMensagem": null,
      "TransacaoSituacaoMensagem": null,
      "TransacaoCodResposta": "00",
      "TransacaoTipoCartao": null,
      "TransacaoQtdParcelas": 1,
      "TransacaoDataHoraGMT": "/Date(1589511600000)/",
      "TransacaoTipoMensagem": null,
      "TransacaoCodProcessamento": null,
      "TransacaoDataHoraServer": "/Date(1589563703410)/",
      "TransacaoVersaoGrav": 0,
      "TransacaoCodigoEmpresa": null,
      "TransacaoCodigoFilial": null,
      "TransacaoCodigoRede": null,
      "TransacaoCodigoBandeira": null,
      "TransacaoCodigoServico": 0,
      "TransacaoDigSenha": null,
      "TransacaoReservado": null,
      "TransacaoReservado2": null,
      "TransacaoMensagemDisplay": null,
      "TransacaoReferencia": null,
      "TransacaoScope": null,
      "TransacaoTefPos": "POS",
      "TransacaoTerminal": "001",
      "TransacaoEnviadaConciliador": false,
      "TransacaoSistemaIntegrador": "TEF",
      "TransacaoCodSituacaoSitef": 0,
      "TransacaoHomologacao": false,
      "TransacaoNsuSitef": null,
      "TransacaoNsuFePas": null,
      "TransacaoCodRespostaSITEF": null,
      "ConciliacaoCodigo": 0,
      "ConciliacaoValorTaxa": 0,
      "ConciliacaoValorLiquido": 0,
      "ConciliacaoDataVencimento": "/Date(-62135586000000)/",
      "ConciliacaoDataDeposito": "/Date(-62135586000000)/",
      "ConciliacaoParcela": 0,
      "ConciliacaoTotalParcelas": 0,
      "ConciliacaoValorTaxaServico": 0,
      "ConciliacaoTipoDeposito": null,
      "ConciliacaoClienteCodigo": null,
      "ConciliacaoTipoServico": null,
      "ConciliacaoFinalValorBruto": null,
      "ConciliacaoFinalValorLiquido": null,
      "ConciliacaoFinalTaxa": null,
      "ConciliacaoFinalValorTaxa": null,
      "ConciliacaoFinalStatus": null,
      "ConciliacaoFinalRejeitada": false,
      "ConciliacaoFinalCancelada": false,
      "ConciliacaoFinalPrevisaoPagamento": null,
      "ConciliacaoFinalPagamento": null,
      "ConciliacaoFinalBancoDeposito": null,
      "ConciliacaoFinalAgenciaDeposito": null,
      "ConciliacaoFinalContaDeposito": null,
      "ConciliacaoFinalAntecipacao": null,
      "ConciliacaoFinalResumoVenda": null,
      "ConciliacaoFinalDataProcessamentoPagamento": null,
      "ConciliacaoObservacao": null,
      "ConciliacaoStatus": null,
      "ConciliacaoTaxaDiferente": false,
      "TransacaoUniqueId": 0,
      "LinkPagoConteudo": "",
      "LinkPagoIdentificador": "MOTIVO DO LINK ¬ CELULAR(SMS) ¬ CLIENTE QUE PAGOU",
      "LoteCodigo": 0,
      "LoteStatus": null
    }
  ]
}
```

Caso ocorra erro na requisição será retornada uma string contendo o motivo:

```txt {title="Exemplo de Retorno com Erro"}
[ERRO] CNPJ INVÁLIDO
```

{{< callout note >}}
Caso não existam transações com os parâmetros fornecidos, será retornada a string `Não houve registros`.

Se o header `RetornarDadosConciliacao` for igual a **1**, serão retornadas informações referentes aos pagamentos das transações.
{{< /callout >}}
