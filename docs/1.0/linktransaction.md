---
layout: page-classic-sidebar-left
title: Associar transação Pagador e Antifraude
featimg: MapAntifraud.png
previous: /docs/1.0/retrochargeback
next: /docs/1.0/updatestatus
---
---

Serviço que associa uma transação do Pagador Braspag à uma transação do Antifraude Gateway Braspag.

* O cliente deverá realizar esta chamada quando o mesmo estiver utilizando o fluxo abaixo:

    - 1º Realiza a análise de fraude
    - 2º Realiza a autorização
    
    - O 3º passo deverá ser a chamada ao a este serviço para associar a transação do Pagador à transação do Antifraude Gateway.

## Hosts

**Test** https://riskhomolog.braspag.com.br  
**Live** https://risk.braspag.com.br

<a name="contract"></a>
  
## Atributos
-----------------------------------

**BraspagTransactionId**{:.custom-attrib}  `required`{:.custom-tag} `Guid`{:.custom-tag}  
Id da transação no Pagador da Braspag.  
Ex.: a3e08eb2-2144-4e41-85d4-61f1befc7a3b

<a style="float: right;" href="#attributes"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="http_operations"></a>

## Operação HTTP
-----------------------------------

`PATCH`{:.http-patch} [https://riskhomolog.braspag.com.br/Transaction/{Id}](#http_patch){:.custom-attrib}  
Associa a transação do Pagador à transação do Antifraude Gateway

<a style="float: right;" href="#http_operations"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="http-patch"></a>

#### `PATCH`{:.http-patch} Associa a transação do Pagador à transação do Antifraude Gateway 
-------------------------------------------------

**PARÂMETROS:**  

``` csharp
Id: Guid  // Id da Transação no Antifraude Gateway
```

**REQUEST:**  

``` http
GET https://riskhomolog.braspag.com.br/Transaction/{Id} HTTP/1.1
Host: riskhomolog.braspag.com.br
Authorization: Bearer {access_token}
Content-Type: application/json
```

``` json
{
    "BraspagTransactionId": "a3e08eb2-2144-4e41-85d4-61f1befc7a3b"
}
```

**RESPONSE:**  

- Quando a transação do Pagador for associada corretamente com a transação do Antifraude Gateway
``` http
HTTP/1.1 200 Ok
```
- Quando a transação do Pagador não for informada na requisição
``` http
HTTP/1.1 400 Bad Request
```
- Quando a transação do Antifraude Gateway não for encontrada na base de dados
``` http
HTTP/1.1 404 Not found
```
- Quando a transação do Pagador já estiver associada a outra transação do Antifraude Gateway
``` http
HTTP/1.1 409 Conflict
```