---
title: 'Quickstart: de Azure-portal gebruiken om een Service Bus-wachtrij te maken | Microsoft Docs'
description: In deze quickstart leert u hoe u een Service Bus-wachtrij kunt maken met behulp van de Azure-portal. Vervolgens gebruikt u een voorbeeldclienttoepassing om berichten te verzenden naar en te ontvangen van de wachtrij.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/10/2019
ms.author: spelluru
ms.openlocfilehash: 315f8d30b7c7559947c599edd0e18eaa5a99ac22
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67513637"
---
# <a name="quickstart-use-azure-portal-to-create-a-service-bus-queue"></a>Quickstart: De Azure-portal gebruiken om een Service Bus-wachtrij te maken
In deze Quick Start wordt beschreven hoe u berichten naar en van een Service Bus-wachtrij, verzenden en ontvangen met behulp van de [Azure-portal][Azure portal] om een messaging-naamruimte en een wachtrij binnen deze naamruimte te maken en het ophalen van de autorisatiereferenties op die de naamruimte. De procedure laat vervolgens zien hoe u berichten naar deze wachtrij verzendt en van de wachtrij ontvangt met behulp van de [.NET Standard-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus).

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, kunt u een [gratis account][] maken voordat u begint.
- [Visual Studio 2017 update 3 (versie 15.3, 26730.01)](https://www.visualstudio.com/vs) of hoger. Visual Studio kunt u een voorbeeld waarmee berichten verzonden en ontvangen van berichten uit een wachtrij maken. Het voorbeeld is het testen van de wachtrij die u hebt gemaakt met behulp van PowerShell. 
- [NET Core SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of later.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="send-and-receive-messages"></a>Berichten verzenden en ontvangen

Nadat de naamruimte en de wachtrij zijn ingericht en u over de benodigde referenties beschikt, bent u klaar om berichten te verzenden en te ontvangen. U kunt de code controleren in [deze GitHub-voorbeeldmap](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters).

Ga als volgt te werk om de code uit te voeren:

1. Kloon de [Service Bus GitHub-opslagplaats](https://github.com/Azure/azure-service-bus/) met behulp van de volgende opdracht:

   ```
   git clone https://github.com/Azure/azure-service-bus.git
   ```
3. Navigeer naar de voorbeeldmap `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart`.
4. Kopieer de verbindingsreeks en de naam van de wachtrij die u hebt verkregen in de sectie De beheerreferenties ophalen.
5.  Typ de volgende opdracht bij een opdrachtprompt:

    ```
    dotnet build
    ```
6.  Navigeer naar de map `bin\Debug\netcoreapp2.0`.
7.  Typ de volgende opdracht om het programma uit te voeren. Zorg ervoor dat u `myConnectionString` vervangt door de waarde die u eerder hebt verkregen, en vervang `myQueueName` door de naam van de wachtrij die u hebt gemaakt:

    ```shell
    dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
    ``` 
8. Bekijk tien berichten die naar de wachtrij zijn verzonden en vervolgens van de wachtrij zijn ontvangen:

   ![programma-uitvoer](./media/service-bus-quickstart-portal/dotnet.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt via de portal de resourcegroep, de naamruimte en de wachtrij verwijderen.

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

Deze sectie bevat meer informatie over de werking van de voorbeeldcode. 

### <a name="get-connection-string-and-queue"></a>Verbindingsreeks en wachtrij ophalen

De naam van de connection string en wachtrij worden doorgegeven aan de `Main()` methode als opdrachtregelargumenten. `Main()` declareert twee tekenreeksvariabelen die deze waarden kunnen omvatten:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
De methode `Main()` start vervolgens de asynchrone berichtenlus `MainAsync()`.

### <a name="message-loop"></a>Berichtenlus

De methode MainAsync() maakt de client van een wachtrij met de opdrachtregelargumenten, roept een ontvangende berichtenhandler met de naam `RegisterOnMessageHandlerAndReceiveMessages()`, en de set berichten verzonden:

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

De methode `RegisterOnMessageHandlerAndReceiveMessages()` stelt gewoon enkele opties voor de berichtenhandler in en roept vervolgens de methode `RegisterMessageHandler()` van de wachtrijclient aan, waardoor het ontvangen begint:

```csharp
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
```

### <a name="send-messages"></a>Berichten verzenden

De bewerkingen voor het maken en verzenden van berichten vinden in de methode `SendMessagesAsync()` plaats:

```csharp
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
```

### <a name="process-messages"></a>Berichten verwerken

De methode `ProcessMessagesAsync()` bevestigt, verwerkt en voltooit de ontvangst van de berichten:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```
> [!NOTE]
> U kunt Service Bus-resources beheren [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). De Service Bus Explorer kunnen gebruikers verbinding maken met een Service Bus-naamruimte en berichtentiteiten op een eenvoudige manier te beheren. Het hulpprogramma biedt geavanceerde functies zoals import/export-functionaliteit of de mogelijkheid om te testen, onderwerp, wachtrijen, abonnementen, relayservices, notification hubs en gebeurtenissen hubs. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Service Bus-naamruimte en andere resources gemaakt die zijn vereist voor het verzenden en ontvangen van berichten in een wachtrij. Voor meer informatie over het schrijven van code voor het verzenden en ontvangen van berichten, blijven de zelfstudies in de **berichten verzenden en ontvangen** sectie. 

> [!div class="nextstepaction"]
> [Berichten verzenden en ontvangen](service-bus-dotnet-get-started-with-queues.md)


[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
