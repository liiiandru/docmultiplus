---
title: "Fluxo de Comunicação"
description: ""
lead: "Congrats on setting up a new Doks project!"
date: 2023-09-07T16:33:54+02:00
lastmod: 2023-09-07T16:33:54+02:00
draft: false
weight: 403
seo:
  title: "Fluxo de Comunicação" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---

A comunicação entre a Automação e a Web Api é realizada utilizando dois endpoints: `SetVendaTef` responsável por criar a requisiçãoda transação e `GetVendasTef`, responsável por realizar as consultas das transações.

```txt {title="Urls"}
GET: https://api.multipluscard.com.br/api/Servicos/SetVendaTef
POST: https://api.multipluscard.com.br/api/Servicos/GetVendasTef
```

{{< figure
  src="images/diagrama_webservice.png"
  process="fill 571x433"
  alt="Fluxo de Comunicação entre Automação, Gerenciador de Pagamentos e Web API"
  caption="Fluxo de Comunicação entre Automação, Gerenciador de Pagamentos e Web API"
  max-width="100%"
>}}


## Diagrama do Fluxo de Transação

{{< figure
  src="images/diagrama_webservice_fluxo.png"
  alt="Fluxo da Transação"
  caption="Fluxo da Transação"
  max-width="100%"
>}}
