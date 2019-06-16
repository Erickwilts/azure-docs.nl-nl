---
title: Service Bus-wachtrijen voor onbestelbare berichten | Microsoft Docs
description: Overzicht van Azure Service Bus-wachtrijen voor onbestelbare
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2019
ms.author: aschhab
ms.openlocfilehash: af67b27dacf3bb86c2dd5c878a2751e027a53acb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66003120"
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Overzicht van Service Bus-wachtrijen voor onbestelbare berichten

Azure Service Bus-wachtrijen en onderwerpabonnementen bieden u een secundaire onderliggende wachtrij, met de naam een *dead-letter-wachtrij* (DLQ). De dead-letter-wachtrij hoeft niet te expliciet worden gemaakt en kan niet worden verwijderd of anderszins beheerd onafhankelijk van de belangrijkste entiteit.

Dit artikel wordt beschreven dead-letter uitvoeren voor wachtrijen in Service Bus. Veel van de discussie wordt aangegeven door de [Dead-Letter wachtrijen voorbeeld](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) op GitHub.
 
## <a name="the-dead-letter-queue"></a>De dead-letter-wachtrij

Het doel van de dead-letter-wachtrij is voor het opslaan van berichten die naar een ontvanger kunnen niet worden bezorgd, of berichten die niet kunnen worden verwerkt. Berichten kunnen vervolgens worden verwijderd uit de DLQ en gecontroleerd. Een toepassing mogelijk, met behulp van de operator oplossen van problemen en verzend opnieuw het bericht, aanmelden van het feit dat er een fout opgetreden is en neemt u corrigerende maatregelen. 

Vanuit een perspectief API en het protocol is meestal de DLQ die vergelijkbaar is met een andere wachtrij, behalve dat berichten kunnen alleen worden verzonden via de dead-letter uitvoeren voor bewerking van de bovenliggende entiteit. Bovendien time-to-live niet nageleefd, en is niet mogelijk een bericht van een DLQ dead-letter. De dead-letter-wachtrij biedt volledige ondersteuning peek-lock levering en transactionele bewerkingen.

Houd er rekening mee dat er geen automatisch opschonen van de DLQ is. Berichten in de DLQ blijven totdat u expliciet deze worden opgehaald van de DLQ en roep [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) op de onbestelbare berichten.

## <a name="moving-messages-to-the-dlq"></a>Berichten verplaatsen naar de DLQ

Er zijn verschillende activiteiten in Service Bus die ertoe leiden dat berichten naar de DLQ uit binnen de messaging-engine zelf ophalen gepusht. Een toepassing kan ook expliciet berichten verplaatsen naar de DLQ. 

Als u het bericht wordt verplaatst van de broker, twee eigenschappen worden toegevoegd aan het bericht als de broker-zijn interne versie van aanroepen de [onbestelbare berichten](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync) methode voor het bericht: `DeadLetterReason` en `DeadLetterErrorDescription`.

Toepassingen kunnen hun eigen codes definiëren voor de `DeadLetterReason` stelt de volgende waarden in de eigenschap, maar het systeem.

| Voorwaarde | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Altijd |HeaderSizeExceeded |Het quotum voor de grootte voor deze stream is overschreden. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing en SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |exception.GetType().Name |exception.Message |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |Het bericht is verlopen en naar de wachtrij voor onbestelbare berichten verplaatst. |
| SubscriptionDescription.RequiresSession |Sessie-id is null. |Door sessie ingeschakelde entiteit staat geen berichten toe waarvan de sessie-id null is. |
| ! dead-letter-wachtrij |MaxTransferHopCountExceeded |Null |
| Expliciete dode lettering toepassing |Opgegeven door toepassing |Opgegeven door toepassing |

## <a name="exceeding-maxdeliverycount"></a>Exceeding MaxDeliveryCount

Wachtrijen en abonnementen hebben een [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) en [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount) eigenschap respectievelijk; de standaardwaarde is 10. Wanneer een bericht is afgeleverd bij een vergrendeling ([ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)), maar heeft een expliciet is afgebroken of de vergrendeling is verlopen, wordt het bericht [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) is verhoogd. Wanneer [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) overschrijdt [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount), het bericht wordt verplaatst naar de DLQ op te geven de `MaxDeliveryCountExceeded` redencode.

Dit gedrag kan niet worden uitgeschakeld, maar u kunt instellen [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) naar een groot getal.

## <a name="exceeding-timetolive"></a>Exceeding TimeToLive

Wanneer de [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription) of [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) eigenschap is ingesteld op **waar** (de standaardwaarde is **false**), alle verlopende berichten worden verplaatst naar de DLQ op te geven de `TTLExpiredException` redencode.

Houd er rekening mee dat verlopen berichten alleen worden opgeschoond en naar de DLQ verplaatst wanneer er ten minste één actieve ontvanger binnenhalen van de belangrijkste wachtrij of abonnement. Dit gedrag is standaard.

## <a name="errors-while-processing-subscription-rules"></a>Fouten tijdens het verwerken van abonnementsregels voor

Wanneer de [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) eigenschap voor een abonnement is ingeschakeld, worden eventuele fouten die zich voordoen tijdens de SQL-filterregel van een abonnement wordt uitgevoerd, worden vastgelegd in de DLQ langs met de strijdige bericht.

## <a name="application-level-dead-lettering"></a>Onbestelbare op toepassingsniveau

Toepassingen kunnen de DLQ gebruiken om expliciet weigeren onaanvaardbaar berichten naast het systeem geleverde onbestelbare-functies. Dit kunnen bijvoorbeeld berichten die naar behoren kunnen niet worden verwerkt vanwege een of andere vorm van systeemprobleem, berichten die onjuist gevormde nettolading bevatten of berichten die niet door de verificatie bij sommige bericht beveiligingsniveau-schema wordt gebruikt.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Onbestelbare in scenario's doorsturen naar of verzenden via

Berichten worden verzonden naar de wachtrij voor overdracht dead-letter uitvoeren voor de volgende voorwaarden:

- Een bericht wordt doorgegeven via meer dan 4 wachtrijen of onderwerpen die zijn [samen in een keten](service-bus-auto-forwarding.md).
- De bestemmingswachtrij of onderwerp is uitgeschakeld of verwijderd.
- De bestemmingswachtrij of onderwerp groter is dan de maximale entiteit.

Als u wilt deze berichten dead lettered ophalen, kunt u een ontvanger met behulp van de [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath) hulpprogramma methode.

## <a name="example"></a>Voorbeeld

Het volgende codefragment wordt een bericht ontvanger gemaakt. Op de hoogte van de ontvangen voor de hoofdwachtrij, haalt de code het bericht met [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver), waarin wordt gevraagd de broker om terug te keren direct bericht direct beschikbaar, of om te retourneren met geen resultaat. Als de code wordt een bericht ontvangt, het onmiddellijk verwijdert deze, welke stappen van de `DeliveryCount`. Nadat het systeem wordt het bericht naar de DLQ verplaatst, de belangrijkste wachtrij leeg is en de lus wordt afgesloten, als [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) retourneert **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="path-to-the-dead-letter-queue"></a>Pad naar de dead-letter-wachtrij
U kunt toegang tot de dead-letter-wachtrij met behulp van de volgende syntaxis:

```
<queue path>/$deadletterqueue
<topic path>/Subscription/<subscription path>/$deadletterqueue
```

Als u de .NET SDK gebruikt, kunt u het pad naar de dead-letter-wachtrij ophalen met behulp van de methode SubscriptionClient.FormatDeadLetterPath(). Deze methode wordt de naam van het onderwerp de naam/abonnement en achtervoegsels met **/$DeadLetterQueue**.


## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over Service Bus-wachtrijen:

* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Azure-wachtrijen en Service Bus-wachtrijen in vergelijking met](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

