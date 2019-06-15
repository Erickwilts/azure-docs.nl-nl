---
title: Verzenden en ontvangen van gebeurtenissen met .NET Framework - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een .NET Framework-toepassing die gebeurtenissen naar Azure Event Hubs verzendt.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 96ce71a7b3076adec169f103060a167b61c42d5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65603508"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-net-framework"></a>Gebeurtenissen verzenden of ontvangen van gebeurtenissen uit Azure Event Hubs met behulp van .NET Framework
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie laat zien hoe u .NET Framework-consoletoepassingen in maakt C# gebeurtenissen te verzenden of ontvangen van gebeurtenissen van een Event hub. 

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- [Microsoft Visual Studio 2019](https://visualstudio.com).
- **Maak een Event Hubs-naamruimte en een event hub**. In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md). Vervolgens kunt u de **verbindingsreeks voor de event hub-naamruimte** met de instructies in het artikel: [Verbindingstekenreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U de verbindingsreeks later in deze zelfstudie.

## <a name="send-events"></a>Gebeurtenissen verzenden 
Deze sectie leest u hoe u een .NET Framework-consoletoepassing voor het verzenden van gebeurtenissen naar een event hub maakt. 

### <a name="create-a-console-application"></a>Een consoletoepassing maken

Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Afzender**.
   
![Consoletoepassing maken](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)

### <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

1. Klik in Solution Explorer met de rechtermuisknop op het project **Afzender** en klik op **NuGet-pakketten beheren voor oplossing**. 
2. Klik op het tabblad **Bladeren** en zoek vervolgens naar `WindowsAzure.ServiceBus`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden. 
   
    ![Service Bus NuGet-pakket installeren](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Er wordt door Visual Studio een verwijzing naar het [ NuGet-pakket Azure Service Bus-bibliotheek](https://www.nuget.org/packages/WindowsAzure.ServiceBus) gedownload, geïnstalleerd en toegevoegd.

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Code schrijven om berichten te verzenden naar de event hub

1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
2. Voeg de volgende velden toe aan de klasse **Program**, waarbij u de waarden van de tijdelijke aanduiding vervangt door de naam van de Event Hub die u in de vorige sectie hebt gemaakt, en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen. U kunt de verbindingsreeks kopiëren voor uw event hub uit **verbinding verbindingsreeks-primaire** key onder **RootManageSharedAccessKey** op de Event Hub-pagina in de Azure-portal. Zie voor gedetailleerde stappen [-verbindingsreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal).
   
    ```csharp
    static string eventHubName = "Your Event Hub name";
    static string connectionString = "namespace connection string";
    ```
3. Voeg de volgende methode toe aan de klasse **Program**:
   
      ```csharp
      static void SendingRandomMessages()
      {
          var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
          while (true)
          {
              try
              {
                  var message = Guid.NewGuid().ToString();
                  Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                  eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
              }
              catch (Exception exception)
              {
                  Console.ForegroundColor = ConsoleColor.Red;
                  Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                  Console.ResetColor();
              }
       
              Thread.Sleep(200);
          }
      }
      ```
   
      Met deze methode worden er continu gebeurtenissen naar uw Event Hub verzonden, met een vertraging van 200 ms.
4. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
      ```csharp
      Console.WriteLine("Press Ctrl-C to stop the sender process");
      Console.WriteLine("Press Enter to start now");
      Console.ReadLine();
      SendingRandomMessages();
      ```
5. Voer het programma uit en controleer of er geen fouten zijn.
  
## <a name="receive-events"></a>Gebeurtenissen ontvangen
In deze sectie schrijft u een .NET Framework-consoletoepassing die berichten van een event hub met behulp van ontvangt de [Event Processor Host](event-hubs-event-processor-host.md). De [EventProcessorHost](event-hubs-event-processor-host.md) is een .NET-klasse die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangst van deze Event Hubs te beheren. Met de EventProcessorHost kunt u gebeurtenissen splitsen over meerdere ontvangers, zelfs als deze worden gehost in verschillende knooppunten. 

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

### <a name="create-a-console-application"></a>Een consoletoepassing maken

Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Ontvanger**.
   
![Consoletoepassing maken](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)

### <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

1. Klik in Solution Explorer met de rechtermuisknop op het project **Ontvanger** en klik op **NuGet-pakketten beheren voor oplossing**.
2. Klik op het tabblad **Bladeren** en zoek vervolgens naar `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.
   
    ![Zoeken naar Event Processor Host NuGet-pakket](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Door Visual Studio wordt een verwijzing naar het [ NuGet-pakket Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), inclusief alle bijbehorende afhankelijkheden, gedownload, geïnstalleerd en toegevoegd.

### <a name="implement-the-ieventprocessor-interface"></a>De IEventProcessor-interface implementeren

1. Klik met de rechtermuisknop op het project **Ontvanger**, klik op **Toevoegen** en op **Klasse**. Noem de nieuwe klasse **SimpleEventProcessor** en klik op **Toevoegen** om de klasse te maken.
   
    ![De klasse SimpleEventProcessor toevoegen](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
2. Voeg de volgende instructies toe aan het begin van het bestand SimpleEventProcessor.cs:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      using System.Diagnostics;
      ```
    
3. Vervang de volgende code voor de hoofdtekst van de klasse:
    
      ```csharp
      class SimpleEventProcessor : IEventProcessor
      {
        Stopwatch checkpointStopWatch;
        
        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }
        
        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }
        
        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
        
                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }
        
            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
      }
      ```
    
      Deze klasse wordt aangeroepen door de **EventProcessorHost** om gebeurtenissen te verwerken die worden ontvangen van de Event Hub. De klasse `SimpleEventProcessor` gebruikt een stopwatch om periodiek de controlepuntmethode voor de context **EventProcessorHost** aan te roepen. Op deze manier gaat er nooit meer werk verloren dan in vijf minuten kan worden verwerkt, als de ontvanger opnieuw wordt opgestart.

### <a name="update-the-main-method-to-use-simpleeventprocessor"></a>De Main-methode bijwerken voor gebruik van SimpleEventProcessor

1. Voeg in de klasse **Program** de volgende `using`-instructie toe aan het begin van het bestand:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      ```
    
2. Vervang de `Main` methode in de `Program` klasse met de volgende code wordt vervangen door de naam van de event hub en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen, en de storage-account en de sleutel die u in de vorige secties hebt gekopieerd. 
    
      ```csharp
      static void Main(string[] args)
      {
        string eventHubConnectionString = "{Event Hubs namespace connection string}";
        string eventHubName = "{Event Hub name}";
        string storageAccountName = "{storage account name}";
        string storageAccountKey = "{storage account key}";
        string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
        
        string eventProcessorHostName = Guid.NewGuid().ToString();
        EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
        Console.WriteLine("Registering EventProcessor...");
        var options = new EventProcessorOptions();
        options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
        eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
        
        Console.WriteLine("Receiving. Press enter key to stop worker.");
        Console.ReadLine();
        eventProcessorHost.UnregisterEventProcessorAsync().Wait();
      }
      ```
    
3. Voer het programma uit en controleer of er geen fouten zijn.
  
## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen: 

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Functies en de belangrijkste termen in de Azure Event Hubs](event-hubs-features.md).
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)


<!-- Links -->
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Event Hubs overview]: event-hubs-about.md
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: event-hubs-event-processor-host.md
[Azure portal]: https://portal.azure.com
