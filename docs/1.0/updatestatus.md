---
layout: page-classic-sidebar-left
title: Update de Status
featimg: MapAntifraud.png
previous: /docs/1.0/linktransaction
next: /docs/1.0/analise
---
---

Serviço para alterar o status de transações em revisão (review) para aceita (accept) ou rejeita (reject), disponível somente para o provedor Cybersource.  

## Hosts

**Test** https://riskhomolog.braspag.com.br  
**Live** https://risk.braspag.com.br

<a name="contract"></a>
  
## Atributos
-----------------------------------

**Status**{:.custom-attrib} `required`{:.custom-tag} `Guid`{:.custom-tag} `Cybersource`{:.custom-provider-cyber}  
Novo status da transação.  
Enum: Accept | Reject  

**Comments**{:.custom-attrib} `optional`{:.custom-tag} `255`{:.custom-tag}.`string`{:.custom-tag} `Cybersource`{:.custom-provider-cyber}  
Comentário associado a mudança de status.  

<a style="float: right;" href="#attributes"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="http_operations"></a>

## Operação HTTP
-----------------------------------

`PATCH`{:.http-patch} [https://riskhomolog.braspag.com.br/Analysis/{Id}](#http_patch){:.custom-attrib}  
Altera o status da transação

<a style="float: right;" href="#http_operations"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="http-patch"></a>

#### `PATCH`{:.http-patch} Altera o status da transação
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
    "Status":"Accept",
    "Comments":"Transação aceita após contato com o cliente."
}
```

**RESPONSE:**  

- Quando a transação for recebida para processamento.
``` http
HTTP/1.1 200 OK
```
```json
{
    "Message": "Change status request successfully received. New status: Accept."
}
```

- Quando a transação não for encontrada na base de dados.
``` http
HTTP/1.1 404 Not found
```
```json
{
    "Message": "The transaction does not exist."
}
```

- Quando a transação não estiver elegivel para alteração de status.
``` http
HTTP/1.1 400 Bad Request
```
```json
{
    "Message": "The transaction is not able to update status. Actual status: Reject."
}
```

- Quando o novo status enviado for diferente de Accept ou Reject.
``` http
HTTP/1.1 400 Bad Request
```
```json
{
    "Message": "The new status is invalid to update transaction. Accepted status are: 'Accept' or 'Reject'."
}
```

- Quando o tipo ou tamanho de algum campo não for enviado conforme especificado no manual.
``` http
HTTP/1.1 400 Bad Request
```
```json
{
    "Message": "The request is invalid.",
    "ModelState": {
        "request.Status": [
            "Error converting value \"Review\" to type 'Antifraude.Domain.Enums.StatusType'. Path 'Status', line 2, position 16."
        ],
        "request.Comments": [
            "The field Comments must be a string or array type with a maximum length of '255'."
        ]
    }
}
```