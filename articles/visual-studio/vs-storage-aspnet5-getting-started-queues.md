---
title: Aan de slag met Queue Storage en Visual Studio Connected Services (ASP.NET Core) | Microsoft Docs
description: Aan de slag met Azure Queue Storage in een ASP.NET Core-project in Visual Studio
services: storage
author: ghogen
manager: jillfra
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: d8e370c6f7c59da8522bb4fb1403b6107a9c9c41
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510979"
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met Queue Storage en Visual Studio Connected Services (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

In dit artikel wordt beschreven hoe u aan de slag gaat met Azure Queue Storage in Visual Studio nadat u een Azure-opslag account in een ASP.NET Core-project hebt gemaakt of ernaar hebt verwezen met behulp van de Visual Studio **Connected Services** -functie. De **Connected Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en voegt de connectiestring voor het opslagaccount toe aan uw project-configuratiebestanden. (Zie [opslag documentatie](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure Queue-opslag is een service voor het opslaan van grote aantallen berichten die overal ter wereld toegankelijk zijn via geverifieerde oproepen met HTTP of HTTPS. Eén wachtrij bericht kan Maxi maal 64 kB groot zijn en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteits limiet van een opslag account. Zie ook [aan de slag met Azure Queue Storage met behulp van .net](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie over het programmatisch bewerken van wacht rijen.

Maak eerst een Azure-wachtrij in uw opslag account om aan de slag te gaan. In C# dit artikel wordt uitgelegd hoe u een wachtrij maakt en hoe u eenvoudige wachtrij bewerkingen uitvoert, zoals het toevoegen, wijzigen, lezen en verwijderen van wachtrij berichten.  De code maakt gebruik van de Azure Storage-client bibliotheek voor .NET. Zie [ASP.net](https://www.asp.net)voor meer informatie over ASP.net.

Enkele van de Azure Storage-API's zijn asynchroon en de code in dit artikel gaat ervan uit dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-queues-in-code"></a>Toegang tot wacht rijen in code

Als u toegang wilt krijgen tot wacht rijen in ASP.NET Core projecten, neemt C# u de volgende items op in een bron bestand dat toegang heeft tot de Azure-wachtrij opslag. Gebruik al deze code vóór de code in de volgende secties.

1. De benodigde `using` instructies toevoegen:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Maak een `CloudStorageAccount` -object met gegevens van uw opslagaccount. Gebruik de volgende code om de gegevens van de opslag connection string en het opslag account op te halen uit de configuratie van de Azure-service:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Een `CloudQueueClient` object ophalen om te verwijzen naar de wachtrij objecten in uw opslag account:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Een `CloudQueue` object ophalen om naar een specifieke wachtrij te verwijzen:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Een wachtrij maken in code

Als u de Azure-wachtrij in code wilt `CreateIfNotExistsAsync`maken, roept u het volgende op:

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Een bericht aan een wachtrij toevoegen

Als u een bericht wilt invoegen in een bestaande wachtrij, maakt `CloudQueueMessage` u een nieuw object en `AddMessageAsync` roept u vervolgens de methode aan. U `CloudQueueMessage` kunt een object maken op basis van een teken reeks (in UTF-8-indeling) of een byte matrix.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen

U kunt het bericht aan de voor kant van een wachtrij bekijken zonder het uit de wachtrij te verwijderen door de `PeekMessageAsync` methode aan te roepen:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen en verwijderen

Met uw code kunt u een bericht uit een wachtrij in twee stappen verwijderen (dewachtrij).

1. Aanroep `GetMessageAsync` om het volgende bericht in een wachtrij op te halen. Een bericht dat wordt `GetMessageAsync` geretourneerd van is niet zichtbaar voor andere code die berichten uit deze wachtrij leest. Standaard blijft het bericht onzichtbaar gedurende 30 seconden.
1. Roep `DeleteMessageAsync`aan om het verwijderen van het bericht uit de wachtrij te volt ooien.

Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. De volgende code roept `DeleteMessageAsync` direct aan nadat het bericht is verwerkt:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>Aanvullende opties voor het dequeuing van berichten

Er zijn twee manieren om het ophalen van berichten aan te passen vanuit een wachtrij. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. In het volgende code voorbeeld wordt `GetMessages` de methode gebruikt om 20 berichten in één aanroep op te halen. Vervolgens wordt elk bericht met een `foreach` lus verwerkt. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de timer van vijf minuten voor alle berichten tegelijk wordt gestart, dus nadat de vijf minuten zijn verstreken, worden alle berichten die niet zijn verwijderd, weer zichtbaar.

```cs
// Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
//   delete each message after processing.

foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting ophalen van het aantal berichten in de wachtrij. De `FetchAttributes` -methode vraagt de wachtrij service om de wachtrij kenmerken op te halen, inclusief het aantal berichten. De `ApproximateMethodCount` eigenschap retourneert de laatste waarde die door de `FetchAttributes` methode is opgehaald, zonder de Queue-service aan te roepen.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Het async-await-patroon gebruiken met algemene wachtrij-Api's

In dit voor beeld ziet u hoe u het async-await-patroon gebruikt met algemene `Async`wachtrij-api's die eindigen op. Wanneer een asynchrone methode wordt gebruikt, wordt het async-await-patroon onderbroken totdat de aanroep is voltooid. Dit gedrag zorgt ervoor dat de huidige thread andere werkzaamheden kan uitvoeren, waardoor prestatie knelpunten worden voor komen en de algehele reactie snelheid van uw toepassing wordt verbeterd.

```cs
// Create a message to add to the queue.
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message.
await messageQueue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Als u een wachtrij en alle berichten erin wilt verwijderen, roept u de `Delete` methode aan in het wachtrij object:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
