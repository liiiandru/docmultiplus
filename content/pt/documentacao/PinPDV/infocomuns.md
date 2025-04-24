---
title: "Informações Comuns"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
weight: 502
draft: false
seo:
  title: "Informações Comuns" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

Em diversos cenários de integração com a API do **PinPDV Lite**, são requeridas informações específicas e/ou procedimentos operacionais.
A seguir, são descritos os elementos mais recorrentes e essenciais ao processo de integração.

## Autenticação

Será disponibilizado um token de longa duração para cada cliente (estabelecimentoId). Ideal para incluir na aplicação que fica do lado do cliente.

## Status Utilizados
Nesta seção estão definidos os status padronizados utilizados nos processos de venda, pagamento e tipo de pagamento dentro da integração com a API do PINPDV Lite.

### Venda

```txt
Realizada = 0,
Cancelada = 8,
Error = 9
```

### Pagamento

```txt
Aguardando = 0,
Processando = 1,
Concluido = 2
```

### Tipo de Pagamento

```txt
None = 0,
Dinheiro = 1,
Credito = 2,
Debito = 3,
Pix = 4,
ValeRefeicao = 6,
ValeAlimentacao = 7
```

## Listar Empresas

Nas requisições onde é necessário o envio do código de empresa, previamente deve ser feita esta consulta, ela possui os seguintes parâmetros de ordenação:

```txt {title="Parâmetros de Ordenação"}
OrdenarPor = (Id, Nome, Cnpj, CadastradoEm, AtualizadoEm)
OrdenarTipo = (ASC, DESC)
```

```bash {title="Requisição p/ Listar empresas"}
curl --request GET \
  --url https://webapi.pinpdv.com.br/empresa?OrdenarPor=Id&OrdenarTipo=DESC \
  --header 'Authorization: Bearer xyz' \
```

```json {title="Exemplo de Resposta"}
200 - OK

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
      "id": 1,
      "nome": "61492096000147",
      "cnpj": "61.492.096/0001-47",
      "representante": {
        "id": 2,
        "nome": "61492096000147",
        "usuario": "61492096000147",
        "webhook": null
      }
    }
  ]
}
```

## Listar Dispositivos

Em algumas funções é necessário que seja informado o dispositivo em que será realizada a transação, previamente deve ser realizada uma consulta para verificar quais estão disponíveis através do endpoint `/pinpdv`.

```bash {title="Requisição p/ Listar Dispositivos Disponíveis"}
curl --request GET \
  --url 'https://webapi.pinpdv.com.br/pinpdv' \
  --header 'Authorization: Bearer xyz' \
```

```json {title="Exemplo de Resposta"}
200 - OK

{
	"paginaAtual": 1,
	"itensPorPagina": 100,
	"quantidadeDePaginas": 1,
	"quantidadeTotalDeItens": 2,
	"primeiroRegistro": 1,
	"ultimoRegistro": 2,
	"paginaAnterior": false,
	"paginaProxima": false,
	"data": [
		{
			"id": 1,
			"codigo": "526989",
			"nome": "CAIXA 01",
			"isAtivo": true,
			"heartbeat": "2024-10-04T15:49:28.894765"
		},
		{
			"id": 2,
			"codigo": "687836",
			"nome": "CAIXA 02",
			"isAtivo": true,
			"heartbeat": "2024-10-01T15:04:15.204673"
		}
	]
}
```

## Impressão de Documentos

Ao término dos processos que oferecem suporte à impressão de documentos fiscais ou não fiscais, o sistema, caso a impressão seja executada, deverá aplicar uma das abordagens de finalização descritas a seguir.

- URL de Callback

É a melhor experiência de integração; no final de cada processo que permite impressão de documento é realizada chamada para URL de call-back informando que houve uma venda.

A resposta pode ser o documento a ser impresso, ou então nenhum conteúdo. Se houver resposta com conteúdo para impressão no call-back, esse conteúdo será impresso no APP PINPDV. Se não houver conteúdo, o sistema pode enviar o documento assim que estiver pronto, contudo, uma vez que o usuário saia da tela que executa a impressão não há alternativa de reimpressão.

 - Configurar mensagem padrão de resposta (Em breve)

Caso o sistema não disponha de endereço de call-back, é uma boa alternativa.

A API PINPDV se encarrega de sempre devolver um comprovante padrão de resposta mediante configuração do sistema parceiro. Essa resposta padrão pode por exemplo, ser uma lista dos produtos e a forma de pagamento.

- Pooling na API PINPDV

Apesar de disponível, não recomendados principalmente devido ao “timming” da informação. O rate-limit entre as chamadas deve ser respeitado. O sistema parceiro se encarrega de periodicamente monitorar as vendas realizadas no app PINPDV que não possuem documento gerado e providenciar o conteúdo.

### Atualizando o Documento (Comprovante)

Dependendo da escolha do sistema quanto a geração de documento, o endpoint para atualizar o conteúdo deve ser usado, considerar sempre 40 caracteres. O texto será posicionado ao centro. Não há opção de formatação. O codigo de venda é retornado na resposta da requisição anterior. Exemplo, na resposta de status da pré-venda vai constar um objeto venda e seu ID.

```bash {title="Atualização de Documento"}
curl --request PUT \
  --url 'https://webapi.pinpdv.com.br/venda/{vendaIdentificador}/comprovante' \
  --header 'Authorization: Bearer xyz' \
  --header 'Content-Type: text/plain' \
  --data '         Aviso de comprovante          '
LINHA 1
                                 LINHA 2
LINHA 3'
```

```txt {title="Resposta da Requisição"}
202 - OK
```
