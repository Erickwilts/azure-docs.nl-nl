---
title: Aanbevolen procedures voor het verbeteren van de prestaties met behulp van Azure Service Bus
description: Hierin wordt beschreven hoe u Service Bus kunt gebruiken om de prestaties te optimaliseren bij het uitwisselen van brokered berichten.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: a81e6fa1c6097f46bbfa3016beb1b7780ad3c351
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88065298"
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Best Practices voor prestatieverbeteringen met Service Bus Messaging

In dit artikel wordt beschreven hoe u Azure Service Bus kunt gebruiken om de prestaties te optimaliseren bij het uitwisselen van brokered berichten. In het eerste deel van dit artikel worden de verschillende mechanismen beschreven die worden geboden om de prestaties te verbeteren. Het tweede deel bevat richt lijnen voor het gebruik van Service Bus op een manier die de beste prestaties kan bieden in een bepaald scenario.

In dit artikel verwijst de term ' client ' naar een wille keurige entiteit die toegang heeft tot Service Bus. Een client kan de rol van een afzender of ontvanger hebben. De term ' Sender ' wordt gebruikt voor een Service Bus wachtrij of een onderwerp-client die berichten verzendt naar een Service Bus wachtrij of een onderwerp-abonnement. De term ' Receive ' verwijst naar een Service Bus wachtrij of abonnements-client die berichten ontvangt van een Service Bus wachtrij of een abonnement.

In deze secties worden verschillende concepten geïntroduceerd die Service Bus gebruiken om de prestaties te verbeteren.

## <a name="protocols"></a>Protocollen

Met Service Bus kunnen clients berichten verzenden en ontvangen via een van de drie protocollen:

1. Advanced Message Queuing Protocol (AMQP)
2. Service Bus Messa ging Protocol (SBMP)
3. Hypertext Transfer Protocol (HTTP)

AMQP is de meest efficiënte, omdat de verbinding met Service Bus wordt onderhouden. Het implementeert ook batch verwerking en vooraf ophalen. Tenzij expliciet vermeld, gaat alle inhoud in dit artikel uit van het gebruik van AMQP of SBMP.

> [!IMPORTANT]
> De SBMP is alleen beschikbaar voor .NET Framework. AMQP is de standaard waarde voor .NET Standard.

## <a name="choosing-the-appropriate-service-bus-net-sdk"></a>De juiste Service Bus .NET SDK kiezen

Er worden twee Azure Service Bus .NET-Sdk's ondersteund. De Api's zijn vergelijkbaar en kunnen verwarrend zijn. Raadpleeg de volgende tabel om u te helpen bij het bepalen van uw beslissing. We raden u aan de SDK van micro soft. Azure. ServiceBus te maken, omdat het meer moderne, uitvoerende en platformoverschrijdende compatibel is. Daarnaast ondersteunt het AMQP over websockets en maakt het deel uit van de Azure .NET SDK-verzameling van open-source projecten.

| NuGet-pakket | Primaire naam ruimte (n) | Minimum platform (en) | Protocol(len) |
|---------------|----------------------|---------------------|-------------|
| <a href="https://www.nuget.org/packages/Microsoft.Azure.ServiceBus" target="_blank">Micro soft. Azure. ServiceBus<span class="docon docon-navigate-external x-hidden-focus"></span></a> | `Microsoft.Azure.ServiceBus`<br>`Microsoft.Azure.ServiceBus.Management` | .NET Core 2.0<br>.NET Framework 4.6.1<br>Mono 5,4<br>Xamarin. iOS 10,14<br>Xamarin. Mac 3,8<br>Xamarin. Android 8,0<br>Universeel Windows-platform 10.0.16299 | AMQP<br>HTTP |
| <a href="https://www.nuget.org/packages/WindowsAzure.ServiceBus" target="_blank">WindowsAzure. ServiceBus<span class="docon docon-navigate-external x-hidden-focus"></span></a> | `Microsoft.ServiceBus`<br>`Microsoft.ServiceBus.Messaging` | .NET Framework 4.6.1 | AMQP<br>SBMP<br>HTTP |

Zie [.net-implementatie ondersteuning](/dotnet/standard/net-standard#net-implementation-support)voor meer informatie over minimale .NET Standard-platform ondersteuning.

## <a name="reusing-factories-and-clients"></a>Fabrieken en clients opnieuw gebruiken

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

Service Bus-client objecten, zoals implementaties van [`IQueueClient`][QueueClient] of [`IMessageSender`][MessageSender] , moeten worden geregistreerd voor afhankelijkheids injectie als Singleton (of één keer en gedeeld). U wordt aangeraden geen bericht fabrieken of wacht woord-, onderwerp-en abonnements-clients te sluiten nadat u een-berichten hebt verzonden en ze vervolgens opnieuw te maken wanneer u het volgende bericht verzendt. Als u een Messa ging-Factory sluit, wordt de verbinding met de Service Bus-service verwijderd en wordt er een nieuwe verbinding tot stand gebracht wanneer de fabriek opnieuw wordt gemaakt. Het tot stand brengen van een verbinding is een dure bewerking die u kunt vermijden door dezelfde fabrieks-en client objecten voor meerdere bewerkingen te gebruiken. U kunt deze client objecten veilig gebruiken voor gelijktijdige asynchrone bewerkingen en uit meerdere threads.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

Service Bus-client objecten, zoals `QueueClient` of `MessageSender` , worden gemaakt via een [MessagingFactory][MessagingFactory] -object, dat ook intern beheer van verbindingen biedt. U wordt aangeraden geen bericht fabrieken of wacht woord-, onderwerp-en abonnements-clients te sluiten nadat u een-berichten hebt verzonden en ze vervolgens opnieuw te maken wanneer u het volgende bericht verzendt. Als u een Messa ging-Factory sluit, wordt de verbinding met de Service Bus-service verwijderd en wordt er een nieuwe verbinding tot stand gebracht wanneer de fabriek opnieuw wordt gemaakt. Het tot stand brengen van een verbinding is een dure bewerking die u kunt vermijden door dezelfde fabrieks-en client objecten voor meerdere bewerkingen te gebruiken. U kunt deze client objecten veilig gebruiken voor gelijktijdige asynchrone bewerkingen en uit meerdere threads.

---

## <a name="concurrent-operations"></a>Gelijktijdige bewerkingen

Het uitvoeren van een bewerking (verzenden, ontvangen, verwijderen, enz.) neemt enige tijd in beslag. Deze tijd omvat de verwerking van de bewerking door de Service Bus-service naast de latentie van de aanvraag en het antwoord. Om het aantal bewerkingen per tijd te verhogen, moeten bewerkingen gelijktijdig worden uitgevoerd.

De client plant gelijktijdige bewerkingen door asynchrone bewerkingen uit te voeren. De volgende aanvraag wordt gestart voordat de vorige aanvraag is voltooid. Het volgende code fragment is een voor beeld van een asynchrone verzend bewerking:

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

```csharp
var messageOne = new Message(body);
var messageTwo = new Message(body);

var sendFirstMessageTask =
    queueClient.SendAsync(messageOne).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #1");
    });
var sendSecondMessageTask =
    queueClient.SendAsync(messageTwo).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #2");
    });

await Task.WhenAll(sendFirstMessageTask, sendSecondMessageTask);
Console.WriteLine("All messages sent");
```

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

```csharp
var messageOne = new BrokeredMessage(body);
var messageTwo = new BrokeredMessage(body);

var sendFirstMessageTask =
    queueClient.SendAsync(messageOne).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #1");
    });
var sendSecondMessageTask =
    queueClient.SendAsync(messageTwo).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #2");
    });

await Task.WhenAll(sendFirstMessageTask, sendSecondMessageTask);
Console.WriteLine("All messages sent");
```

---

De volgende code is een voor beeld van een asynchrone ontvangst bewerking.

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

Zie de GitHub-opslag plaats voor de volledige <a href="https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.Azure.ServiceBus/SendersReceiversWithQueues" target="_blank">bron code voor beelden <span class="docon docon-navigate-external x-hidden-focus"></span> </a>:

```csharp
var receiver = new MessageReceiver(connectionString, queueName, ReceiveMode.PeekLock);

static Task LogErrorAsync(Exception exception)
{
    Console.WriteLine(exception);
    return Task.CompletedTask;
};

receiver.RegisterMessageHandler(
    async (message, cancellationToken) =>
    {
        Console.WriteLine("Handle message");
        await receiver.CompleteAsync(message.SystemProperties.LockToken);
    },
    new MessageHandlerOptions(e => LogErrorAsync(e.Exception))
    {
        AutoComplete = false,
        MaxConcurrentCalls = 20
    });
```

Het `MessageReceiver` object wordt geïnstantieerd met de Connection String, de naam van de wachtrij en de modus voor het bekijken van een weer gave. Vervolgens wordt het `receiver` exemplaar gebruikt voor het registreren van de bericht afhandeling.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

Zie de GitHub-opslag plaats voor de volledige <a href="https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/SendersReceiversWithQueues" target="_blank">bron code voor beelden <span class="docon docon-navigate-external x-hidden-focus"></span> </a>:

```csharp
var factory = MessagingFactory.CreateFromConnectionString(connectionString);
var receiver = await factory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);

// Register the handler to receive messages asynchronously
receiver.OnMessageAsync(
    async message =>
    {
        Console.WriteLine("Handle message");
        await message.CompleteAsync();
    },
    new OnMessageOptions
    {
        AutoComplete = false,
        MaxConcurrentCalls = 20
    });
```

`MessagingFactory`Hiermee maakt u een `factory` object van de Connection String. Met het `factory` exemplaar wordt a `MessageReceiver` geïnstantieerd. Vervolgens wordt de `receiver` instantie gebruikt om de handler voor berichten te registreren.

---

## <a name="receive-mode"></a>Ontvangst modus

Wanneer u een wachtrij of een abonnements-client maakt, kunt u een ontvangst modus opgeven: *bekijken/vergren delen* of *ontvangen en verwijderen*. De standaard modus voor ontvangen is `PeekLock` . In de standaard modus verzendt de client een aanvraag om een bericht van Service Bus te ontvangen. Nadat de client het bericht heeft ontvangen, verzendt het een aanvraag om het bericht te volt ooien.

Wanneer de ontvangst modus wordt ingesteld op `ReceiveAndDelete` , worden beide stappen gecombineerd in één aanvraag. Met deze stappen wordt het totale aantal bewerkingen verminderd en kunnen de algehele bericht doorvoer worden verbeterd. Deze prestatie verbetering is het risico dat berichten verloren gaan.

Service Bus biedt geen ondersteuning voor trans acties voor het ontvangen en verwijderen van bewerkingen. Daarnaast zijn de semantiek van kortere vergren delingen vereist voor scenario's waarin de client een bericht wil uitstellen of [onbestelt](service-bus-dead-letter-queues.md) .

## <a name="client-side-batching"></a>Batch verwerking aan client zijde

Met batch verwerking aan de client zijde kan een wachtrij of een onderwerp-client het verzenden van een bericht gedurende een bepaalde periode vertragen. Als de client aanvullende berichten verstuurt tijdens deze periode, worden de berichten in één batch verstuurd. Batch verwerking aan client zijde zorgt er ook voor dat een wachtrij of abonnements client meerdere **voltooide** aanvragen batcheert in één aanvraag. Batch verwerking is alleen beschikbaar voor asynchrone bewerkingen voor **verzenden** en **volt ooien** . Synchrone bewerkingen worden direct naar de Service Bus-service verzonden. Er vindt geen batch verwerking plaats voor Peek-en ontvangst bewerkingen en er worden geen batches op clients uitgevoerd.

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

De functionaliteit van een eigenschap voor het bewerken van de .NET Standard-SDK is nog niet beschikbaar.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

Een client gebruikt standaard een batch-interval van 20 MS. U kunt de batch-interval wijzigen door de eigenschap [BatchFlushInterval][BatchFlushInterval] in te stellen voordat u de Messa ging-Factory maakt. Deze instelling is van invloed op alle clients die zijn gemaakt door deze Factory.

Als u batch verwerking wilt uitschakelen, stelt u de eigenschap [BatchFlushInterval][BatchFlushInterval] in op **time span. Zero**. Bijvoorbeeld:

```csharp
var settings = new MessagingFactorySettings
{
    NetMessagingTransportSettings =
    {
        BatchFlushInterval = TimeSpan.Zero
    }
};
var factory = MessagingFactory.Create(namespaceUri, settings);
```

Batch verwerking heeft geen invloed op het aantal factureer bare berichten bewerkingen en is alleen beschikbaar voor het Service Bus-client protocol met behulp van de [micro soft. ServiceBus. Messa ging](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) -bibliotheek. Het HTTP-protocol biedt geen ondersteuning voor batch verwerking.

> [!NOTE]
> Instelling `BatchFlushInterval` zorgt ervoor dat batch verwerking impliciet is vanuit het perspectief van de toepassing. d.w.z. de toepassing maakt `SendAsync` en `CompleteAsync` aanroepen en maakt geen specifieke batch-aanroepen.
>
> Expliciete batch verwerking aan de client zijde kan worden geïmplementeerd door gebruik te maken van de onderstaande methode aanroep:
> ```csharp
> Task SendBatchAsync(IEnumerable<BrokeredMessage> messages);
> ```
> Hier moet de gecombineerde grootte van de berichten kleiner zijn dan de maximale grootte die wordt ondersteund door de prijs categorie.

---

## <a name="batching-store-access"></a>Toegang tot batch-archief

Voor het verg Roten van de door Voer van een wachtrij, onderwerp of abonnement Service Bus batches meerdere berichten wanneer deze naar de interne Store worden geschreven. Als deze functie is ingeschakeld voor een wachtrij of onderwerp, worden berichten in de Store naar batch geschreven. Als deze functie is ingeschakeld voor een wachtrij of abonnement, worden de berichten uit de Store in batches verwijderd. Als de toegang tot een batch-archief is ingeschakeld voor een entiteit, wordt door Service Bus een Store-schrijf bewerking voor die entiteit met Maxi maal 20 MS vertraagd.

> [!NOTE]
> Er is geen risico dat berichten met batch verwerking verloren gaan, zelfs als er een Service Bus fout aan het einde van een batch-interval voor 20ms.

Aanvullende archief bewerkingen die worden uitgevoerd tijdens dit interval, worden toegevoegd aan de batch. De toegang tot een batch-archief is alleen van invloed op **Verzend** -en **volledige** bewerkingen. ontvangst bewerkingen worden niet beïnvloed. De toegang tot een batch-archief is een eigenschap van een entiteit. Batch verwerking vindt plaats in alle entiteiten die de toegang tot een batch-archief inschakelen.

Bij het maken van een nieuwe wachtrij, een onderwerp of een abonnement, wordt de toegang tot een batch-archief standaard ingeschakeld.

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

Als u de toegang tot de batch-Store wilt uitschakelen, hebt u een exemplaar van een nodig `ManagementClient` . Een wachtrij maken op basis van een wachtrij beschrijving waarmee de eigenschap wordt ingesteld `EnableBatchedOperations` op `false` .

```csharp
var queueDescription = new QueueDescription(path)
{
    EnableBatchedOperations = false
};
var queue = await managementClient.CreateQueueAsync(queueDescription);
```

Raadpleeg de volgende artikelen voor meer informatie:
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.queuedescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.QueueDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.subscriptiondescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.SubscriptionDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.topicdescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.TopicDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

Als u de toegang tot de batch-Store wilt uitschakelen, hebt u een exemplaar van een nodig `NamespaceManager` . Een wachtrij maken op basis van een wachtrij beschrijving waarmee de eigenschap wordt ingesteld `EnableBatchedOperations` op `false` .

```csharp
var queueDescription = new QueueDescription(path)
{
    EnableBatchedOperations = false
};
var queue = namespaceManager.CreateQueue(queueDescription);
```

Raadpleeg de volgende artikelen voor meer informatie:
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.QueueDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.SubscriptionDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicdescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.TopicDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

---

De toegang tot de batch-Store heeft geen invloed op het aantal factureer bare berichten bewerkingen en is een eigenschap van een wachtrij, onderwerp of abonnement. Het is onafhankelijk van de ontvangst modus en het protocol dat wordt gebruikt tussen een client en de Service Bus service.

## <a name="prefetching"></a>Vooraf ophalen

Als u [vooraf ophaalt](service-bus-prefetch.md) , kan de wachtrij of abonnements-client extra berichten van de service laden wanneer er een receive-bewerking wordt uitgevoerd. De client slaat deze berichten op in een lokale cache. De grootte van de cache wordt bepaald door de- `QueueClient.PrefetchCount` of- `SubscriptionClient.PrefetchCount` Eigenschappen. Elke client die het vooraf ophalen van een eigen cache beheert. Een cache wordt niet gedeeld tussen clients. Als de client een receive-bewerking initieert en de cache leeg is, verzendt de service een batch berichten. De grootte van de batch is gelijk aan de grootte van de cache of 256 KB, afhankelijk van wat kleiner is. Als de client een receive-bewerking initieert en de cache een bericht bevat, wordt het bericht opgehaald uit de cache.

Wanneer een bericht vooraf is opgehaald, wordt het vooraf opgehaalde bericht door de service vergrendeld. Met de vergren deling kan het vooraf opgehaalde bericht niet worden ontvangen door een andere ontvanger. Als de ontvanger het bericht niet kan volt ooien voordat de vergren deling verloopt, wordt het bericht beschikbaar voor andere ontvangers. De vooraf opgehaalde kopie van het bericht blijft in de cache. De ontvanger die de verlopen kopie in de cache gebruikt, ontvangt een uitzonde ring wanneer deze probeert dat bericht te volt ooien. De bericht vergrendeling verloopt standaard na 60 seconden. Deze waarde kan worden verlengd tot 5 minuten. Om het verbruik van verlopen berichten te voor komen, moet de cache grootte altijd kleiner zijn dan het aantal berichten dat door een client kan worden gebruikt binnen het time-outinterval van de vergren deling.

Wanneer u de standaard verval datum van 60 seconden gebruikt, is een goede waarde voor `PrefetchCount` 20 keer de maximale verwerkings snelheid van alle ontvangers van de fabriek. Een Factory maakt bijvoorbeeld drie ontvangers en elke ontvanger kan Maxi maal 10 berichten per seconde verwerken. Het aantal prefetch mag niet hoger zijn dan 20 X 3 X 10 = 600. Standaard `PrefetchCount` is ingesteld op 0, wat betekent dat er geen extra berichten van de service worden opgehaald.

Het vooraf ophalen van berichten verhoogt de algehele door Voer voor een wachtrij of abonnement, omdat hiermee het totale aantal bericht bewerkingen of afrondingen wordt verminderd. Als het eerste bericht wordt opgehaald, duurt dit echter langer (vanwege de verhoogde bericht grootte). Het ontvangen van vooraf opgehaalde berichten gaat sneller omdat deze berichten al zijn gedownload door de client.

De TTL-eigenschap (time-to-Live) van een bericht wordt gecontroleerd door de server op het moment dat de server het bericht naar de client verzendt. De client controleert de TTL-eigenschap van het bericht niet wanneer het bericht wordt ontvangen. In plaats daarvan kan het bericht worden ontvangen, zelfs als de TTL van het bericht is verstreken terwijl het bericht door de client in de cache is opgeslagen.

Het vooraf ophalen heeft geen invloed op het aantal factureer bare berichten bewerkingen en is alleen beschikbaar voor het Service Bus-client protocol. Het HTTP-protocol biedt geen ondersteuning voor het vooraf ophalen van. Het vooraf ophalen is beschikbaar voor synchrone en asynchrone ontvangst bewerkingen.

# <a name="microsoftazureservicebus-sdk"></a>[Micro soft. Azure. ServiceBus SDK](#tab/net-standard-sdk)

Zie de volgende eigenschappen voor meer informatie `PrefetchCount` :

* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.QueueClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.SubscriptionClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure. ServiceBus SDK](#tab/net-framework-sdk)

Zie de volgende eigenschappen voor meer informatie `PrefetchCount` :

* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.queueclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.QueueClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.SubscriptionClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

---

## <a name="prefetching-and-receivebatch"></a>Vooraf ophalen en ReceiveBatch

> [!NOTE]
> Deze sectie is alleen van toepassing op de SDK WindowsAzure. ServiceBus, omdat de SDK van micro soft. Azure. ServiceBus geen batch-functies beschikbaar maakt.

Hoewel de concepten van het vooraf ophalen van meerdere berichten samen een vergelijk bare semantiek hebben voor het verwerken van berichten in een batch ( `ReceiveBatch` ), zijn er enkele kleine verschillen die in aanmerking moeten worden genomen wanneer ze met elkaar samen werken.

Prefetch is een configuratie (of modus) op de client ( `QueueClient` en `SubscriptionClient` ) en `ReceiveBatch` is een bewerking (die een reactie op aanvraag-antwoord heeft).

Houd rekening met de volgende gevallen wanneer u deze samen gebruikt:

* Prefetch moet groter zijn dan of gelijk zijn aan het aantal berichten waarvan u verwacht dat ze worden ontvangen `ReceiveBatch` .
* Prefetch kan Maxi maal n/3 keer het aantal berichten dat per seconde wordt verwerkt, waarbij n de standaard vergrendelings duur is.

Er zijn enkele uitdagingen met een Greedy-benadering (waarbij het aantal prefetch zeer hoog blijft), omdat het impliceert dat het bericht is vergrendeld op een bepaalde ontvanger. Het is aan te raden om prefetch-waarden uit te proberen tussen de hierboven genoemde drempels en te identificeren wat past.

## <a name="multiple-queues"></a>Meerdere wacht rijen

Als de verwachte belasting niet kan worden verwerkt door één wachtrij of onderwerp, moet u meerdere Messa ging-entiteiten gebruiken. Wanneer u meerdere entiteiten gebruikt, maakt u een speciale client voor elke entiteit, in plaats van dezelfde client voor alle entiteiten te gebruiken.

## <a name="development-and-testing-features"></a>Functies voor ontwikkelen en testen

> [!NOTE]
> Deze sectie is alleen van toepassing op de SDK WindowsAzure. ServiceBus, omdat de SDK van micro soft. Azure. ServiceBus deze functionaliteit niet beschikbaar maakt.

Service Bus heeft één functie, die specifiek wordt gebruikt voor ontwikkeling, die **nooit moet worden gebruikt in productie configuraties**: [`TopicDescription.EnableFilteringMessagesBeforePublishing`][TopicDescription.EnableFiltering] .

Wanneer er nieuwe regels of filters aan het onderwerp worden toegevoegd, kunt u gebruiken [`TopicDescription.EnableFilteringMessagesBeforePublishing`][TopicDescription.EnableFiltering] om te controleren of de nieuwe filter expressie werkt zoals verwacht.

## <a name="scenarios"></a>Scenario's

In de volgende secties worden typische bericht scenario's beschreven en worden de voorkeurs instellingen voor Service Bus geschetst. Doorvoer tarieven worden geclassificeerd als klein (minder dan 1 bericht/seconde), gemiddeld (1 bericht/seconde of meer, maar minder dan 100 berichten/seconde) en hoog (100 berichten/seconde of hoger). Het aantal clients wordt geclassificeerd als klein (5 of minder), gemiddeld (meer dan 5, kleiner dan of gelijk aan 20), en groot (meer dan 20).

### <a name="high-throughput-queue"></a>Wachtrij voor hoge door Voer

Doel: Maximaliseer de door Voer van één wachtrij. Het aantal afzenders en ontvangers is klein.

* Als u de totale verzend snelheid naar de wachtrij wilt verhogen, gebruikt u meerdere bericht fabrieken om afzenders te maken. Gebruik voor elke afzender asynchrone bewerkingen of meerdere threads.
* Als u de totale ontvangst snelheid van de wachtrij wilt verhogen, gebruikt u meerdere bericht fabrieken om ontvangers te maken.
* Gebruik asynchrone bewerkingen om te profiteren van batch verwerking aan de client zijde.
* Stel het interval voor batch verwerking in op 50 MS om het aantal Service Bus-client protocol-verzen dingen te verminderen. Als er meerdere afzenders worden gebruikt, verhoogt u het batch-interval tot 100 MS.
* De toegang tot de batch Store ingeschakeld laten. Deze toegang verhoogt het totale aantal berichten dat naar de wachtrij kan worden geschreven.
* Stel het aantal prefetch in op 20 keer de maximale verwerkings snelheid van alle receivers van een Factory. Dit aantal vermindert het aantal Service Bus-client protocol overdrachten.

### <a name="multiple-high-throughput-queues"></a>Meerdere wacht rijen voor hoge door Voer

Doel: Maximaliseer de algehele door Voer van meerdere wacht rijen. De door Voer van een afzonderlijke wachtrij is gemiddeld of hoog.

Als u maximale door Voer in meerdere wacht rijen wilt verkrijgen, gebruikt u de instellingen die worden beschreven om de door Voer van een enkele wachtrij te maximaliseren. Daarnaast kunt u verschillende fabrieken gebruiken om clients te maken die vanuit verschillende wacht rijen worden verzonden of ontvangen.

### <a name="low-latency-queue"></a>Wachtrij met lage latentie

Doel: de end-to-end latentie van een wachtrij of onderwerp minimaliseren. Het aantal afzenders en ontvangers is klein. De door Voer van de wachtrij is klein of gemiddeld.

* Schakel batch verwerking aan client zijde uit. De client verzendt onmiddellijk een bericht.
* Schakel de toegang tot de batch-Store uit. De service schrijft direct het bericht naar de Store.
* Als u één client gebruikt, stelt u het aantal prefetch in op 20 keer de verwerkings factor van de ontvanger. Als meerdere berichten tegelijkertijd in de wachtrij arriveren, verzendt het Service Bus-client protocol deze allemaal tegelijk. Wanneer de client het volgende bericht ontvangt, bevindt het bericht zich al in de lokale cache. De cache moet klein zijn.
* Als u meerdere clients gebruikt, stelt u het aantal prefetch in op 0. Door het aantal in te stellen, kan de tweede client het tweede bericht ontvangen terwijl de eerste client nog bezig is met het verwerken van het eerste bericht.

### <a name="queue-with-a-large-number-of-senders"></a>Wachtrij met een groot aantal afzenders

Doel: Maximaliseer de door Voer van een wachtrij of onderwerp met een groot aantal afzenders. Elke afzender verzendt berichten met een gemiddeld aantal. Het aantal ontvangers is klein.

Service Bus maakt Maxi maal 1000 gelijktijdige verbindingen met een bericht entiteit mogelijk. Deze limiet wordt afgedwongen op naam ruimte niveau, en wacht rijen/onderwerpen/abonnementen worden beperkt door de limiet van gelijktijdige verbindingen per naam ruimte. Voor wacht rijen wordt dit nummer gedeeld tussen afzenders en ontvangers. Als alle 1000-verbindingen voor afzenders zijn vereist, vervangt u de wachtrij door een onderwerp en één abonnement. Een onderwerp accepteert Maxi maal 1000 gelijktijdige verbindingen van afzenders, terwijl het abonnement een extra 1000 gelijktijdige verbindingen van ontvangers accepteert. Als meer dan 1000 gelijktijdige afzenders zijn vereist, moeten de afzenders via HTTP berichten verzenden naar het Service Bus Protocol.

Als u de door voer wilt maximaliseren, voert u de volgende stappen uit:

* Als elke afzender zich in een ander proces bevindt, gebruikt u slechts één fabriek per proces.
* Gebruik asynchrone bewerkingen om te profiteren van batch verwerking aan de client zijde.
* Gebruik het standaard batch-interval van 20 MS om het aantal Service Bus-client protocol-verzen dingen te verminderen.
* De toegang tot de batch Store ingeschakeld laten. Deze toegang verhoogt het totale aantal berichten dat kan worden geschreven naar de wachtrij of het onderwerp.
* Stel het aantal prefetch in op 20 keer de maximale verwerkings snelheid van alle receivers van een Factory. Dit aantal vermindert het aantal Service Bus-client protocol overdrachten.

### <a name="queue-with-a-large-number-of-receivers"></a>Wachtrij met een groot aantal ontvangers

Doel: Maximaliseer de ontvangst frequentie van een wachtrij of abonnement met een groot aantal ontvangers. Elke ontvanger ontvangt berichten met een gemiddeld snelheid. Het aantal afzenders is klein.

Service Bus maakt Maxi maal 1000 gelijktijdige verbindingen met een entiteit mogelijk. Als een wachtrij meer dan 1000 ontvangers vereist, vervangt u de wachtrij door een onderwerp en meerdere abonnementen. Elk abonnement kan Maxi maal 1000 gelijktijdige verbindingen ondersteunen. Ontvangers kunnen ook toegang krijgen tot de wachtrij via het HTTP-protocol.

Ga als volgt te werk om door voer te maximaliseren:

* Als elke ontvanger zich in een ander proces bevindt, gebruikt u slechts één fabriek per proces.
* Ontvangers kunnen synchrone of asynchrone bewerkingen gebruiken. Gezien de gemiddelde ontvangst snelheid van een afzonderlijke ontvanger, heeft het uitvoeren van batch verwerking aan de client zijde van een volledige aanvraag geen invloed op de door Voer van de ontvanger.
* De toegang tot de batch Store ingeschakeld laten. Deze toegang vermindert de algehele belasting van de entiteit. Het vermindert ook de algehele snelheid waarmee berichten kunnen worden geschreven naar de wachtrij of het onderwerp.
* Stel het aantal prefetch in op een kleine waarde (bijvoorbeeld PrefetchCount = 10). Dit aantal voor komt dat ontvangers inactief zijn terwijl andere ontvangers een groot aantal berichten in de cache hebben opgeslagen.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Onderwerp met een klein aantal abonnementen

Doel: Maximaliseer de door Voer van een onderwerp met een klein aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de gecombineerde ontvangst frequentie voor alle abonnementen groter is dan de verzend frequentie. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Ga als volgt te werk om door voer te maximaliseren:

* Als u de totale verzend snelheid in het onderwerp wilt verhogen, gebruikt u meerdere bericht fabrieken om afzenders te maken. Gebruik voor elke afzender asynchrone bewerkingen of meerdere threads.
* Als u de totale ontvangst snelheid van een abonnement wilt verhogen, gebruikt u meerdere bericht fabrieken om ontvangers te maken. Gebruik voor elke ontvanger asynchrone bewerkingen of meerdere threads.
* Gebruik asynchrone bewerkingen om te profiteren van batch verwerking aan de client zijde.
* Gebruik het standaard batch-interval van 20 MS om het aantal Service Bus-client protocol-verzen dingen te verminderen.
* De toegang tot de batch Store ingeschakeld laten. Deze toegang verhoogt het totale aantal berichten dat in het onderwerp kan worden geschreven.
* Stel het aantal prefetch in op 20 keer de maximale verwerkings snelheid van alle receivers van een Factory. Dit aantal vermindert het aantal Service Bus-client protocol overdrachten.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Onderwerp met een groot aantal abonnementen

Doel: Maximaliseer de door Voer van een onderwerp met een groot aantal abonnementen. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de gecombineerde ontvangst frequentie voor alle abonnementen veel groter is dan de verzend frequentie. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Onderwerpen met een groot aantal abonnementen bieden doorgaans een lage algehele door Voer als alle berichten naar alle abonnementen worden doorgestuurd. Deze lage door Voer wordt veroorzaakt door het feit dat elk bericht meerdere keren wordt ontvangen en alle berichten die zijn opgenomen in een onderwerp en alle bijbehorende abonnementen, worden opgeslagen in dezelfde Store. Er wordt van uitgegaan dat het aantal afzenders en het aantal ontvangers per abonnement klein is. Service Bus ondersteunt Maxi maal 2.000 abonnementen per onderwerp.

Als u de door voer wilt maximaliseren, voert u de volgende stappen uit:

* Gebruik asynchrone bewerkingen om te profiteren van batch verwerking aan de client zijde.
* Gebruik het standaard batch-interval van 20 MS om het aantal Service Bus-client protocol-verzen dingen te verminderen.
* De toegang tot de batch Store ingeschakeld laten. Deze toegang verhoogt het totale aantal berichten dat in het onderwerp kan worden geschreven.
* Stel het aantal prefetch in op 20 keer de verwachte ontvangst frequentie in seconden. Dit aantal vermindert het aantal Service Bus-client protocol overdrachten.

<!-- .NET Standard SDK, Microsoft.Azure.ServiceBus -->
[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender

<!-- .NET Framework SDK, Microsoft.Azure.ServiceBus -->
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnableFiltering]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing

<!-- Local links -->
[Partitioned messaging entities]: service-bus-partitioning.md