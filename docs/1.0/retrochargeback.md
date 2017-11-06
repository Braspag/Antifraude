---
layout: page-classic-sidebar-left
title: Retroalimentação de Chargeback
featimg: MapAntifraud.png
previous: /docs/1.0/fingerprint
next: /docs/1.0/linktransaction
---
---

Serviço para enviar as transações para a Braspag que já foram analisadas e sofreram chargeback pelos clientes. Essas informações são usadas para rastrear fraudes e a
ACI / ReD Shield poder recomendar regras para evitar ataques de fraude subsequentes.

Obs.: O serviço aceita requisição POST com no máximo 100 itens na coleção.

## Hosts

**Test** https://riskhomolog.braspag.com.br  
**Live** https://risk.braspag.com.br

<a name="contract"></a>
  
## Atributos
-----------------------------------

**Id**{:.custom-attrib}  `required`{:.custom-tag} `Guid`{:.custom-tag}  
Identificador da transação no Antifraude Gateway.

**BraspagTransactionId**{:.custom-attrib}  `optional`{:.custom-tag} `Guid`{:.custom-tag}  
Identificador da transação no Pagador.

**ChargebackAmount**{:.custom-attrib} `required`{:.custom-tag} `long`{:.custom-tag}  
Valor do chargeback.  
Ex.: 150000 (Valor equivalente a R$1.500,00)

**ChargebackDate**{:.custom-attrib} `required`{:.custom-tag} `date`{:.custom-tag}  
Data da confirmação do chargeback.  
Ex.: 2017-12-02

**ChargebackReasonCode**{:.custom-attrib} `required`{:.custom-tag} `5`{:.custom-tag} `string`{:.custom-tag}  
Código do motivo do chargeback.

**IsFraud**{:.custom-attrib} `required`{:.custom-tag} `bool`{:.custom-tag}  
Flag para identificar se o chargeback foi motivado por fraude ou não

<a style="float: right;" href="#attributes"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="http_operations"></a>

## Operações HTTP
-----------------------------------

`POST`{:.http-post} [https://riskhomolog.braspag.com.br/Chargeback/](#post_retrocbk){:.custom-attrib}  
Retroalimentação de chargeback  

<a style="float: right;" href="#http_operations"><i class="fa fa-angle-double-up fa-fw"></i></a>

<a name="post_retrocbk"></a>

#### `POST`{:.http-post} Retroalimentação de chargeback
-------------------------------------------

**REQUEST:**  

``` http
POST https://riskhomolog.braspag.com.br/Chargeback/ HTTP/1.1
Host: {antifraude endpoint}
Authorization: Bearer {access_token}
Content-Type: application/json
```

``` json
{
    "Chargebacks":
    [
        {
            "Id" : "fb647240-824f-e711-93ff-000d3ac03bed",
            "BraspagTransactionId": "a3e08eb2-2144-4e41-85d4-61f1befc7a3b",
            "ChargebackAmount" : "1000",
            "ChargebackDate" : "2017-12-02",
            "ChargebackReasonCode" : "1",
            "IsFraud" : "false"
        },
        {
            "Id": "9004ba26-f1f1-e611-9400-005056970d6f",
            "ChargebackAmount": "27580",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true"
        },
        {
            "Id": "4493d42c-8732-4b13-aadc-b07e89732c26",
            "ChargebackAmount": "59960",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true"
        },
        {
            "Id": "22b5e829-edf1-e611-9414-0050569318a7",
            "ChargebackAmount": "150000",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true"
        }
    ]
}
```

**RESPONSE:**  

- Quando todas as transações de chargeback enviadas forem processadas com sucesso
``` http
HTTP/1.1 200 Ok
```

- Quando não ocorrer o processamento de todas as transações de chargeback enviadas, será retornado o identificador das transações com motivo através do campo "ChargebackProcessingStatus".  

    * Motivo igual a "Success", a transação de chargeback foi processada com sucesso.  
    * Motivo igual a "AlreadyExist", a transação de chargeback já está associada a transação original de análise de fraude.  
    * Motivo igual a "Remand", a transação de chargeback deverá ser reenviada.  
    * Motivo igual a "NotFound", a transação de chargeback não deverá ser reenviada, pois a origem desta vinculada a análise de fraude não foi encontrada na base de dados.  

``` http
HTTP/1.1 300 Multiple Choices
```
``` json
{
    "Chargebacks":
    [
         {
            "Id" : "fb647240-824f-e711-93ff-000d3ac03bed",
            "BraspagTransactionId": "a3e08eb2-2144-4e41-85d4-61f1befc7a3b",
            "ChargebackAmount" : "1000",
            "ChargebackDate" : "2017-12-02",
            "ChargebackReasonCode" : "1",
            "IsFraud" : "false",
            "ChargebackProcessingStatus": "Success"
        },
        {
            "Id": "9004ba26-f1f1-e611-9400-005056970d6f",
            "ChargebackAmount": "27580",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true",
            "ChargebackProcessingStatus": "AlreadyExist"
        },
        {
            "Id": "4493d42c-8732-4b13-aadc-b07e89732c26",
            "ChargebackAmount": "59960",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true",
            "ChargebackProcessingStatus": "Remand"
        },
        {
            "Id": "22b5e829-edf1-e611-9414-0050569318a7",
            "ChargebackAmount": "150000",
            "ChargebackDate": "2017-12-02",
            "ChargebackReasonCode": "54",
            "IsFraud": "true",
            "ChargebackProcessingStatus": "NotFound"
        }
    ]
}
```
