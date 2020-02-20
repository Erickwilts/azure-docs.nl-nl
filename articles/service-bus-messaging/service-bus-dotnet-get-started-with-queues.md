---
title: Aan de slag met Azure Service Bus-wachtrijen | Microsoft Docs
description: In deze zelf studie maakt u .NET Core-Console toepassingen om berichten te verzenden naar en berichten van een Service Bus wachtrij te ontvangen.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 5718106aee0e60d111398efdb839945c2c7a8a06
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471734"
---
# <a name="get-started-with-service-bus-queues"></a>Aan de slag met Service Bus-wachtrijen
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]
In deze zelf studie maakt u .NET Core-Console toepassingen om berichten te verzenden naar en berichten van een Service Bus wachtrij te ontvangen.

## <a name="prerequisites"></a>Vereisten

- [Visual Studio 2019](https://www.visualstudio.com/vs).
- [NET Core SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of later.
- Een Azure-abonnement. U hebt een Azure-account nodig om deze zelfstudie te voltooien. U kunt de [voor delen](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) van uw MSDN-abonnee activeren of zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Als u geen wachtrij hebt om mee te werken, voert u de stappen in het [Azure Portal gebruik uit om een service bus wachtrij](service-bus-quickstart-portal.md) artikel te maken om een wachtrij te maken.

  - Lees het kort overzicht van Service Bus-wacht rijen.
  - Maak een Service Bus naam ruimte.
  - Haal de connection string op.
  - Maak een Service Bus wachtrij.

## <a name="send-messages-to-the-queue"></a>Berichten naar de wachtrij verzenden

Maak een C#-consoletoepassing met Visual Studio om berichten naar de wachtrij te verzenden.

### <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio en maak een nieuw project voor de **console-app (.net core)** voor C#. In dit voor beeld wordt de app- *CoreSenderApp*naam.

### <a name="add-the-service-bus-nuget-package"></a>Het Service Bus NuGet-pakket toevoegen

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
1. Selecteer **Bladeren**. Zoek en selecteer **[micro soft. Azure. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)** .
1. Selecteer **installeren** om de installatie te volt ooien en sluit NuGet Package Manager.

    ![Een NuGet-pakket selecteren][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>Code schrijven om berichten naar de wachtrij te verzenden

1. Voeg in *Program.cs*de volgende `using`-instructies toe boven aan de definitie van de naam ruimte, vóór de klassen declaratie:

    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. Declareer de volgende variabelen in de klasse `Program`:

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    Voer uw connection string in voor de naam ruimte als de `ServiceBusConnectionString` variabele. Voer de naam van uw wachtrij in.

1. Vervang de `Main()`-methode door de volgende **async** -`Main` methode. Hiermee wordt de `SendMessagesAsync()` methode aangeroepen die u in de volgende stap toevoegt om berichten naar de wachtrij te verzenden. 

    ```csharp
    public static async Task Main(string[] args)
    {    
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```
1. Voeg direct na de `MainAsync()` methode de volgende `SendMessagesAsync()` methode toe waarmee het aantal berichten wordt verzonden dat is opgegeven door `numberOfMessagesToSend` (momenteel ingesteld op 10):

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```

Het *Program.cs* -bestand moet er als volgt uitzien.

```csharp
namespace CoreSenderApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        public static async Task Main(string[] args)
        {    
            const int numberOfMessages = 10;
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    
            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after sending all the messages.");
            Console.WriteLine("======================================================");
    
            // Send messages.
            await SendMessagesAsync(numberOfMessages);
    
            Console.ReadKey();
    
            await queueClient.CloseAsync();
        }

        static async Task SendMessagesAsync(int numberOfMessagesToSend)
        {
            try
            {
                for (var i = 0; i < numberOfMessagesToSend; i++)
                {
                    // Create a new message to send to the queue
                    string messageBody = $"Message {i}";
                    var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                    // Write the body of the message to the console
                    Console.WriteLine($"Sending message: {messageBody}");

                    // Send the message to the queue
                    await queueClient.SendAsync(message);
                }
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
            }
        }
    }
}
```

Voer het programma uit en controleer de Azure Portal.

Selecteer de naam van uw wachtrij in het venster **overzicht** van de naam ruimte om de wachtrij- **essentiële**elementen weer te geven.

![Berichten ontvangen met aantal en grootte][queue-message]

De waarde voor het **aantal actieve berichten** voor de wachtrij is nu **10**. Telkens wanneer u deze app Sender uitvoert zonder de berichten op te halen, wordt deze waarde verhoogd met 10.

De huidige grootte van de wachtrij verhoogt de **huidige** waarde in **Essentials** telkens wanneer de app berichten aan de wachtrij toevoegt.

In de volgende sectie wordt beschreven hoe u deze berichten ophaalt.

## <a name="receive-messages-from-the-queue"></a>Berichten ontvangen uit de wachtrij

Als u de berichten wilt ontvangen die u hebt verzonden, maakt u een andere **console app (.net core)** -toepassing. Installeer het **micro soft. Azure. ServiceBus** NuGet-pakket, zoals u voor de afzender toepassing hebt gedaan.

### <a name="write-code-to-receive-messages-from-the-queue"></a>Schrijven van code voor het ontvangen van berichten van de wachtrij

1. Voeg in *Program.cs*de volgende `using`-instructies toe boven aan de definitie van de naam ruimte, vóór de klassen declaratie:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. Declareer de volgende variabelen in de klasse `Program`:

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    Voer uw connection string in voor de naam ruimte als de `ServiceBusConnectionString` variabele. Voer de naam van uw wachtrij in.

1. Vervang de `Main()`-methode door de volgende code:

    ```csharp
    static void Main(string[] args)
    {
        MainAsync().GetAwaiter().GetResult();
    }

    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register QueueClient's MessageHandler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

1. Voeg direct na de `MainAsync()` methode de volgende methode toe, waarmee de bericht afhandeling wordt geregistreerd en de berichten worden ontvangen die worden verzonden door de toepassing van de afzender:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

1. Voeg direct na de vorige methode de volgende methode `ProcessMessagesAsync()` toe voor het verwerken van de ontvangen berichten:

    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

1. Voeg tot slot de volgende methode toe om eventuele uitzonderingen af te handelen:

    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }
    ```

Het *Program.cs* -bestand moet er als volgt uitzien:

```csharp
namespace CoreReceiverApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        static void Main(string[] args)
        {
            MainAsync().GetAwaiter().GetResult();
        }

        static async Task MainAsync()
        {
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
            Console.WriteLine("======================================================");

            // Register QueueClient's MessageHandler and receive messages in a loop
            RegisterOnMessageHandlerAndReceiveMessages();
 
            Console.ReadKey();

            await queueClient.CloseAsync();
        }

        static void RegisterOnMessageHandlerAndReceiveMessages()
        {
            // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
            var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
            {
                // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                // Set it according to how many messages the application wants to process in parallel.
                MaxConcurrentCalls = 1,

                // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                AutoComplete = false
            };

            // Register the function that will process messages
            queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
        }

        static async Task ProcessMessagesAsync(Message message, CancellationToken token)
        {
            // Process the message
            Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

            // Complete the message so that it is not received again.
            // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
            await queueClient.CompleteAsync(message.SystemProperties.LockToken);

            // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
            // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
            // to avoid unnecessary exceptions.
        }

        static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
        {
            Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
            var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
            Console.WriteLine("Exception context for troubleshooting:");
            Console.WriteLine($"- Endpoint: {context.Endpoint}");
            Console.WriteLine($"- Entity Path: {context.EntityPath}");
            Console.WriteLine($"- Executing Action: {context.Action}");
            return Task.CompletedTask;
        }
    }
}
```

Voer het programma uit en controleer de portal opnieuw. Het **aantal actieve berichten** en **huidige** waarden zijn nu **0**.

![Wachtrij nadat berichten zijn ontvangen][queue-message-receive]

Gefeliciteerd! U hebt nu een wachtrij gemaakt, een set berichten naar die wachtrij verzonden en die berichten ontvangen van dezelfde wachtrij.

> [!NOTE]
> U kunt Service Bus-resources beheren met [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). Met de Service Bus Explorer kunnen gebruikers eenvoudig verbinding maken met een Service Bus naam ruimte en berichten entiteiten beheren. Het hulp programma biedt geavanceerde functies zoals de functionaliteit voor importeren/exporteren of de mogelijkheid om onderwerpen, wacht rijen, abonnementen, relay-Services, Notification hubs en Event hubs te testen.

## <a name="next-steps"></a>Volgende stappen

Bekijk onze [GitHub-opslagplaats met voorbeelden](https://github.com/Azure/azure-service-bus/tree/master/samples) die enkele van de meer geavanceerde functies van Service Bus Messaging laten zien.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/messages-sent-to-essentials.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive-in-essentials.png

