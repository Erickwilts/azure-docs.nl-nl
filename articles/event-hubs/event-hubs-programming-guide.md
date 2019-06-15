---
title: Programmeerhandleiding - Azure Event Hubs | Microsoft Docs
description: In dit artikel bevat informatie over het schrijven van code voor Azure Event Hubs met behulp van de Azure .NET SDK.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
ms.service: event-hubs
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 29814cb8aef09a8ead30d6daa615554dd55135dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60764365"
---
# <a name="programming-guide-for-azure-event-hubs"></a>Programmeerhandleiding voor Azure Event Hubs
Dit artikel worden enkele algemene scenario's bij het schrijven van code met Azure Event Hubs. Er wordt uitgegaan van een basisbegrip van Event Hubs. Zie het [Overzicht van Event Hubs](event-hubs-what-is-event-hubs.md) voor een conceptueel overzicht van Event Hubs.

## <a name="event-publishers"></a>Gebeurtenisuitgevers

U kunt gebeurtenissen verzenden naar een event hub met behulp van HTTP POST of via een AMQP 1.0-verbinding. Welke van deze te gebruiken en wanneer is afhankelijk van het specifieke scenario worden binnenkort aangepakt. AMQP 1.0-verbindingen zijn brokered verbindingen in Service Bus. Ze zijn met name geschikt voor scenario‘s met vaak voorkomende hogere berichtvolumes en lagere latentievereisten, omdat ze een permanent berichtenkanaal bieden.

Wanneer de beheerde .NET-API‘s worden gebruikt, zijn de klassen [EventHubClient][] en [EventData][] de primaire constructs voor het publiceren van gegevens naar Event Hubs. [EventHubClient][] biedt het AMQP-communicatiekanaal over welke gebeurtenissen worden verzonden naar de event hub. De [EventData][] klasse vertegenwoordigt een gebeurtenis en wordt gebruikt om berichten te publiceren naar een event hub. Deze klasse bevat de hoofdtekst, bepaalde metagegevens en headerinformatie over de gebeurtenis. Andere eigenschappen worden toegevoegd aan de [EventData][] zoals deze wordt doorgegeven via een event hub-object.

## <a name="get-started"></a>Aan de slag
De .NET-klassen die ondersteuning bieden voor Event Hubs vindt u in de [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet-pakket. U kunt installeren met behulp van Visual Studio Solution explorer of de [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) in Visual Studio. Hiervoor voert u de volgende opdracht uit in het venster voor de [Pakketbeheerconsole](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):

```shell
Install-Package Microsoft.Azure.EventHubs
```

## <a name="create-an-event-hub"></a>Een Event Hub maken

U kunt de Azure portal, Azure PowerShell of Azure CLI gebruiken om Event Hubs te maken. Zie voor meer informatie, [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](event-hubs-create.md).

## <a name="create-an-event-hubs-client"></a>Een Event Hubs-client maken

De primaire klasse voor interactie met Event Hubs is [Microsoft.Azure.EventHubs.EventHubClient][EventHubClient]. U kunt ook het gebruik van deze klasse instantiëren de [CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createfromconnectionstring) methode, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
private const string EventHubConnectionString = "Event Hubs namespace connection string";
private const string EventHubName = "event hub name";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
{
    EntityPath = EventHubName

};
eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

## <a name="send-events-to-an-event-hub"></a>Gebeurtenissen verzenden naar een event hub

Verzenden van gebeurtenissen naar een event hub door het maken van een [EventHubClient][] exemplaar en het verzenden van asynchrone wijze via de [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) methode. Deze methode heeft één [EventData][] instantieparameter en verzendt het asynchroon naar een event hub.

## <a name="event-serialization"></a>Gebeurtenisserialisatie

De [EventData][] klasse heeft [twee overbelaste constructors](/dotnet/api/microsoft.azure.eventhubs.eventdata.-ctor) die verschillende parameters, bytes of een matrix van bytes, die staan voor de nettolading van de gegevens uitvoeren. Als u JSON gebruikt met [EventData][], kunt u **Encoding.UTF8.GetBytes()** gebruiken om de bytematrix op te halen voor een met JSON gecodeerde tekenreeks. Bijvoorbeeld:

```csharp
for (var i = 0; i < numMessagesToSend; i++)
{
    var message = $"Message {i}";
    Console.WriteLine($"Sending message: {message}");
    await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
}
```

## <a name="partition-key"></a>Partitiesleutel

Bij het verzenden van gebeurtenisgegevens, kunt u een waarde die wordt gehasht om een partitietoewijzing te produceren. U geeft u de partitie met de [PartitionSender.PartitionID](/dotnet/api/microsoft.azure.eventhubs.partitionsender.partitionid) eigenschap. De beslissing om te gebruiken van partities impliceert echter een keuze tussen beschikbaarheid en consistentie. 

### <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Met behulp van een partitiesleutel is optioneel en Overweeg zorgvuldig of u wel of niet een te gebruiken. Als u bij het publiceren van een gebeurtenis geen partitiesleutel opgeeft, wordt er gebruikgemaakt van round robin-toewijzing. In veel gevallen is met behulp van een partitiesleutel een goede keuze als Gebeurtenisvolgorde belangrijk is. Wanneer u een partitiesleutel, beschikbaarheid op een enkel knooppunt in deze partities nodig hebt en storingen kunnen optreden na verloop van tijd; bijvoorbeeld, wanneer compute-knooppunten opnieuw opstarten en patch. Als zodanig, als u een partitie-ID ingesteld en die partitie niet meer beschikbaar is voor een of andere reden, mislukken een poging tot toegang tot de gegevens in de betreffende partitie. Als u maximale beschikbaarheid is belangrijk, niet een partitiesleutel; opgeven in dat geval worden gebeurtenissen verzonden naar partities die gebruikmaken van de eerder beschreven round robin-model. In dit scenario maakt u een expliciete keuze tussen beschikbaarheid (geen partitie-ID) en consistentie (vastmaken gebeurtenissen naar een partitie-ID).

Een andere overweging is vertragingen bij de verwerking van gebeurtenissen verwerken. In sommige gevallen is het mogelijk beter om te verwijderen van gegevens en probeer het opnieuw dan opnieuw te verwerken, waardoor mogelijk meer downstream-verwerkingen vertragingen bijbenen. Met een aandelenkoersen is het bijvoorbeeld beter moet worden gewacht voor volledige actuele gegevens, maar in een live chat of VOIP-scenario in plaats daarvan zijn er gegevens snel, zelfs als deze is niet voltooid.

Deze beschikbaarheidsoverwegingen die in deze scenario's kunt u een van de volgende fout verwerking strategieën:

- Stoppen (stop lezen uit Event Hubs totdat dingen zijn opgelost)
- Verwijderen (berichten niet belangrijk is, zijn ze neerzetten)
- Opnieuw proberen (opnieuw proberen die de berichten die naar eigen inzicht)

Zie voor meer informatie en een discussie over de wisselwerking tussen beschikbaarheid en consistentie [beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Batchbewerkingen voor het verzenden van gebeurtenissen

Verzenden van gebeurtenissen in batches kan helpen betere doorvoer. U kunt de [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API voor het maken van een batch tot welke gegevens kunnen objecten later worden toegevoegd voor een [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) aanroepen.

Één batch mag niet groter zijn dan de limiet van 1 MB van een gebeurtenis. Daarnaast maakt elk bericht in de batch gebruik van dezelfde uitgever-id. Het is de verantwoordelijkheid van de afzender om ervoor te zorgen dat de batch de maximale gebeurtenisgrootte niet overschrijdt. Als dit wel gebeurt, wordt aan clientzijde een **Verzendfout** gegenereerd. U kunt de Help-methode [EventHubClient.CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) om ervoor te zorgen dat de batch niet groter zijn dan 1 MB. U krijgt een lege [EventDataBatch](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch) uit de [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API en gebruik vervolgens [TryAdd](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch.tryadd) om toe te voegen gebeurtenissen om te maken van de batch. 

## <a name="send-asynchronously-and-send-at-scale"></a>Asynchroon verzenden en op schaal verzenden

U verzenden gebeurtenissen asynchroon naar een event hub. Asynchroon verzenden verhoogt de snelheid waarmee een client is het verzenden van gebeurtenissen. [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) retourneert een [taak](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) object. U kunt de [RetryPolicy](/dotnet/api/microsoft.servicebus.retrypolicy) opties voor nieuw pogingen klasse op de client naar de client.

## <a name="event-consumers"></a>Gebeurtenisconsumers
Met de klasse [EventProcessorHost][] worden gegevens uit Event Hubs verwerkt. Gebruik deze implementatie bij het bouwen van gebeurtenislezers op het .NET-platform. [EventProcessorHost][] biedt een thread-veilige, beveiligde runtimeomgeving met meerdere processen voor implementaties van gebeurtenisprocessors die ook beheer biedt van controlepunten en partitielease.

Als u de klasse [EventProcessorHost][] wilt gebruiken, kunt u [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) implementeren. Deze interface bevat vier methoden:

* [OpenAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.openasync)
* [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.closeasync)
* [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)
* [ProcessErrorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processerrorasync)

Voor het starten van de verwerking van gebeurtenissen instantiëren [EventProcessorHost][], bieden de juiste parameters voor uw event hub. Bijvoorbeeld:

> [!NOTE]
> EventProcessorHost en de gerelateerde klassen zijn opgegeven in de **Microsoft.Azure.EventHubs.Processor** pakket. Het pakket toevoegen aan uw Visual Studio-project met de instructies in [in dit artikel](event-hubs-dotnet-framework-getstarted-send.md#add-the-event-hubs-nuget-package) of door de volgende opdracht in de [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) venster:`Install-Package Microsoft.Azure.EventHubs.Processor`.

```csharp
var eventProcessorHost = new EventProcessorHost(
        EventHubName,
        PartitionReceiver.DefaultConsumerGroupName,
        EventHubConnectionString,
        StorageConnectionString,
        StorageContainerName);
```

Roep vervolgens [RegisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync) registreren uw [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) -implementatie met de runtime:

```csharp
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();
```

Op dit punt probeert de host een lease op elke partitie in de event hub met behulp van een 'greedy' algoritme te verkrijgen. Deze leases gedurende een bepaald tijdsbestek geldig en moeten daarna worden vernieuwd. Zodra nieuwe knooppunten, in dit geval werkrolexemplaren, online komen, worden er reserveringen voor leases geplaatst. Later verschuift de werklast tussen de knooppunten wanneer elk knooppunt probeert om meer leases te verkrijgen.

![Gebeurtenisprocessorhost](./media/event-hubs-programming-guide/IC759863.png)

In de loop van de tijd komt er een evenwicht tot stand. Dankzij deze dynamische mogelijkheid kan er op consumers automatisch schalen op basis van CPU worden toegepast, zowel voor omhoog als voor omlaag schalen. Omdat Event Hubs heeft geen een direct concept voor berichtentelling, is Gemiddeld CPU-gebruik vaak de beste manier om te meten back end- of consumerschaling schaal. Als uitgevers meer gebeurtenissen publiceren dan consumers kunnen verwerken, kan de toename van het CPU-gebruik voor consumers worden gebruikt om het aantal werkrolexemplaren automatisch te schalen.

Met de klasse [EventProcessorHost][] wordt ook een op Azure Storage gebaseerd mechanisme van controlepunten geïmplementeerd. Hiermee wordt de offset opgeslagen op basis van partitie, zodat elke consumer kan bepalen wat het laatste controlepunt van de vorige consumer was. Terwijl partities via leases schakelen tussen knooppunten, maakt dit synchronisatiemechanisme de verschuiving van werklasten mogelijk.

## <a name="publisher-revocation"></a>Uitgever intrekken

Naast de geavanceerde functies van de runtime van [EventProcessorHost][], Event Hubs maakt intrekken van uitgevers als u wilt dat specifieke uitgevers geen gebeurtenis kunnen verzenden naar een event hub. Deze functies zijn handig als token van een uitgever is aangetast, of een software-update wordt veroorzaakt door ze naar behoren functioneert vanwege. In deze gevallen kan de uitgever-id, die onderdeel is van het bijbehorende SAS-token, worden geblokkeerd voor het uitgeven van gebeurtenissen.

Zie het voorbeeld [Op grote schaal veilig publiceren met Event Hubs](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) voor meer informatie over het intrekken van uitgevers en over het als uitgever verzenden naar Event Hubs.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Event Hubs-scenario‘s:

* [Event Hubs-API-overzicht](event-hubs-api-overview.md)
* [Wat is Event Hubs](event-hubs-what-is-event-hubs.md)
* [Beschikbaarheid en consistentie in Event Hubs](event-hubs-availability-and-consistency.md)
* [API-verwijzing voor de gebeurtenisprocessorhost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.azure.eventhubs.eventhubclient
[EventData]: /dotnet/api/microsoft.azure.eventhubs.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.azure.eventhubs.processor
