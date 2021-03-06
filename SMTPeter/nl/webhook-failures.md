# WebHooks voor failures

Het is mogelijk om live notificaties te ontvangen over *failed deliveries*.
Je kunt dit doen door een *feedack loop* op te zetten voor deze vorm van
meldingen. Je krijgt dan meldingen over fouten die ontstaan bij de
*SMTP handshake* en meldingen over e-mails die vooraf werden goedgekeurd 
en later alsnog een failure rapport ontvingen.

## Synchronous versus asynchronous

Het SMTP protocol stelt ontvangende servers in staat om een e-mail af te 
wijzen of te accepteren. SMTPeter maakt een *call*, naar de door jou 
aangegeven URL als een e-mail wordt afgewezen. Het kan ook voorkomen dat
een e-mail wordt geaccepteerd om vervolgens later alsnog afgewezen te worden.
Deze *asynchronous errors* worden ook door SMTPeter opgepikt en vervolgens
meegegeven aan de webhook.

De meeste *e-mail servers* gebruiken vaak de officiele *Delivery Status Notification*
om *bounce* meldingen te versturen. Dit formaat stelt SMTPeter in staat om automatisch
bounces te herkennen, te loggen en te rapporteren via webhooks. Echter,
deze officiele standaard wordt niet door iedere e-mail server gebruikt en
sommige grote spelers sturen zelfs eigen bedachte notificaties. We doen uiteraard
ons uiterste best om alle type bounces te registreren en aan de webhook 
mee te geven, maar dit lukt niet altijd vanwege de vele verschillende formaten
waarin bounces worden gemeld. 

Je kunt alsnog alle bounces ontvangen als je dat wilt. Zelfs degene die we niet
hebben getraceerd met de failures webhook. 
In dat geval kun je additioneel een [webhook opzetten voor bounces](webhook-bounces "WebHooks voor bounces").


## Formaat

SMTPeter gebruikt HTTP POST calls om data naar je toe te sturen. Dit kan gedaan
worden over HTTP of over HTTPS. De volgende variabelen worden gebruikt in POST 
calls:

<table>
    <tr>
        <td>id</td>
        <td>unieke <em>identifier</em> van de e-mail waarvoor het failure report geldt</td>
    </tr>
    <tr>
        <td><em>recipient</em></td>
        <td>e-mailadres van de gebruiker waar de failure voor geldt</td>
    </tr>
    <tr>
        <td>state</td>
        <td><em>state</em> in het smtp protocol waar de <em>failure</em> plaatsvond</td>
    </tr>
    <tr>
        <td>code</td>
        <td>optionele smtp error code</td>
    </tr>
    <tr>
        <td>extended</td>
        <td>optionele extended smtp status code</td>
    </tr>
    <tr>
        <td>description</td>
        <td>optionele omschrijving van de error</td>
    </tr>
</table>

## Meer informatie

* [WebHooks](./webhooks)
* [WebHooks instellen](./webhook-setup)
