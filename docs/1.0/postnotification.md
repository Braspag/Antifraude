---
layout: page-classic-sidebar-left
title: Notificação de Mudança de Status
featimg: MapAntifraud.png
previous: /docs/1.0/analise
next: /docs/1.0/autenticacao
---
---

Serviço que envia um post de notificação ao cliente caso haja alguma alteração de status

* É necessário solicitar ao Time de Implementação ([implantacao.operacoes@braspag.com.br](mailto:implantacao.operacoes@braspag.com.br)) o cadastramento da URL de mudança de status.
Quando estimulada pelo servidor da Braspag, enviando um POST, a URL cadastrada para receber a notificação da mudança de status da transação, deverá retornar o código HTTP 200 (OK), indicando que a mensagem foi recebida e processada com sucesso pelo servidor da loja.  

* Se a URL de mudança de status da loja for acessada pelo servidor da Braspag e não retornar o código de confirmação HTTP 200 (OK) ou ocorrer uma falha na conexão, serão realizadas mais 3 tentativas de envio.  

* A URL de mudança de status somente pode utilizar a porta 80 (padrão para http) ou a porta 443 (padrão para https). Recomendamos que a loja trabalhe sempre com SSL para esta URL, ou seja, sempre HTTPS.  

* Após a loja receber a notificação de mudança de status, deverá realizar um GET através da URL https://riskhomolog.braspag.com.br/Analysis/{Id}, enviando Id da transação que foi recebido na notficação da mudança de status.  
Para maior detalhes de como realizar o GET, consultar em Análise a sessão [Obtenção dos Detalhes da Análise]({{ site.baseurl }}{% link docs/1.0/analise.md %}).

![Notificação de Mudança de Status]({{ site.url }}/img/PostNotification.png){: .centerimg }{:title="Notificação de Mudança de Status "}

## Hosts

**Test** https://riskhomolog.braspag.com.br  
**Live** https://risk.braspag.com.br

#### `POST`{:.http-post} Notificação de Mudança de Status 
----------------------------------------------
Abaixo exemplo de mensagem que o servidor da Braspag enviará à URL cadastrada, e como deve ser a resposta enviada em caso de sucesso.

**REQUEST:**  

``` http
POST https://urlcadastrada.loja.com.br/Notification/ HTTP/1.1
Host: urlcadastrada.loja.com.br
Content-Type: application/json
```

``` json
{  
   "Id":"9004ba26-f1f1-e611-9400-005056970d6f"
}​​​
```

**RESPONSE:**  

``` http
HTTP/1.1 200 Ok
```