---
title: Azure Cosmos DB-bindingen voor Functions 2.x
description: Over het gebruik van Azure Cosmos DB-triggers en bindingen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 5762e934d7735dd9617cefc1f56105823d74312f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342857"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x"></a>Azure Cosmos DB-bindingen voor Azure Functions 2.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versie 1:](functions-bindings-cosmosdb.md)
> * [Versie 2](functions-bindings-cosmosdb-v2.md)

Dit artikel wordt uitgelegd hoe u werkt met [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) bindingen in Azure Functions 2.x. Azure Functions ondersteunt activeren, invoeren en uitvoerbindingen voor Azure Cosmos DB.

> [!NOTE]
> In dit artikel is bedoeld voor [Azure Functions versie 2.x](functions-versions.md).  Voor informatie over het gebruik van deze bindingen in functies 1.x, Zie [Azure Cosmos DB-bindingen voor Azure Functions 1.x](functions-bindings-cosmosdb.md).
>
> Deze binding heette oorspronkelijk DocumentDB. In functies versie zijn 2.x, de trigger, bindingen en pakket alle benoemde Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-apis"></a>Ondersteunde API 's

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Azure Cosmos DB-bindingen voor Functions versie 2.x vindt u in de [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) NuGet-pakket versie 3.x. Broncode voor de bindingen is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

De Azure Cosmos DB-Trigger gebruikt de [Azure Cosmos DB-Wijzigingenfeed](../cosmos-db/change-feed.md) om te luisteren naar invoegingen en updates over meerdere partities. De wijzigingenfeed publiceert invoegingen en updates, niet verwijderen.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

Voorbeelden van de trigger overslaan

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt aangeroepen wanneer er zijn ingevoegd of bijgewerkt in de opgegeven database en verzameling.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

Voorbeelden van de trigger overslaan

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Cosmos DB-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie schrijft logboekberichten wanneer Cosmos DB-records zijn gewijzigd.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Dit is de C#-scriptcode:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

Voorbeelden van de trigger overslaan

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Cosmos DB-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft logboekberichten wanneer Cosmos DB-records zijn gewijzigd.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Dit is de JavaScript-code:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

### <a name="trigger---java-example"></a>Trigger - Java-voorbeeld

Het volgende voorbeeld ziet u een binding in Cosmos DB-trigger *function.json* bestand en een [Java functie](functions-reference-java.md) die gebruikmaakt van de binding. De functie is betrokken wanneer er ingevoegd zijn of bijgewerkt in de opgegeven database en verzameling.

```json
{
    "type": "cosmosDBTrigger",
    "name": "items",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "AzureCosmosDBConnection",
    "databaseName": "ToDoList",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": false
}
```

Dit is de Java-code:

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@CosmosDBTrigger` aantekening op parameters waarvan de waarde afkomstig van Cosmos DB zijn kan.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van optioneel<T>.


Voorbeelden van de trigger overslaan

### <a name="trigger---python-example"></a>Trigger - Python-voorbeeld

Het volgende voorbeeld ziet u een Cosmos DB-trigger binding in een *function.json* bestand en een [funkce Pythonu](functions-reference-python.md) die gebruikmaakt van de binding. De functie schrijft logboekberichten wanneer Cosmos DB-records zijn gewijzigd.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Hier volgt de Python-code:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

## <a name="trigger---c-attributes"></a>Trigger - C# attributes

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de databasenaam en verzamelingsnaam. Zie voor meer informatie over deze instellingen en andere eigenschappen die u kunt configureren, [Trigger - configuratie](#trigger---configuration). Hier volgt een `CosmosDBTrigger` kenmerk voorbeeld in een handtekeningmethode:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).


## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `CosmosDBTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** || Moet worden ingesteld op `cosmosDBTrigger`. |
|**direction** || Moet worden ingesteld op `in`. Deze parameter wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**name** || De naam van de variabele die wordt gebruikt in functiecode aan te geven dat de lijst met documenten met wijzigingen vertegenwoordigt. |
|**connectionStringSetting**|**connectionStringSetting** | De naam van een app-instelling met de verbindingsreeks waarmee verbinding met de Azure Cosmos DB-account dat wordt bewaakt. |
|**databaseName**|**DatabaseName**  | De naam van de Azure Cosmos DB-database met de verzameling die worden bewaakt. |
|**collectionName** |**collectionName** | De naam van de verzameling die worden bewaakt. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Optioneel) De naam van een app-instelling met de verbindingsreeks voor de service die de leaseverzameling bevat. Als niet is ingesteld, de `connectionStringSetting` waarde wordt gebruikt. Deze parameter is automatisch ingesteld wanneer de binding in de portal wordt gemaakt. De verbindingsreeks voor de verzameling leases moet schrijfmachtigingen hebben.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Optioneel) De naam van de database waarin de verzameling die wordt gebruikt voor het opslaan van de leases. Wanneer niet ingesteld, de waarde van de `databaseName` instelling wordt gebruikt. Deze parameter is automatisch ingesteld wanneer de binding in de portal wordt gemaakt. |
|**leaseCollectionName** | **LeaseCollectionName** | (Optioneel) De naam van de verzameling die wordt gebruikt voor het opslaan van de leases. Als niet is ingesteld, de waarde `leases` wordt gebruikt. |
|**createLeaseCollectionIfNotExists** | **createLeaseCollectionIfNotExists** | (Optioneel) Als de waarde `true`, de verzameling leases wordt automatisch gemaakt als deze nog niet bestaat. De standaardwaarde is `false`. |
|**leasesCollectionThroughput**| **leasesCollectionThroughput**| (Optioneel) Hiermee definieert u de hoeveelheid Aanvraageenheden om toe te wijzen wanneer de verzameling leases wordt gemaakt. Deze instelling is alleen gebruikt wanneer `createLeaseCollectionIfNotExists` is ingesteld op `true`. Deze parameter is automatisch ingesteld wanneer de binding wordt gemaakt met behulp van de portal.
|**leaseCollectionPrefix**| **leaseCollectionPrefix**| (Optioneel) Als de waarde, wordt een voorvoegsel toegevoegd aan de leases gemaakt in de verzameling van de Lease voor deze functie is effectief zodat twee afzonderlijke Azure-functies voor het delen van dezelfde leaseverzameling met behulp van verschillende voorvoegsels.
|**feedPollDelay**| **feedPollDelay**| (Optioneel) Wanneer is ingesteld, deze wordt gedefinieerd, in milliseconden, de vertraging tussen het opvragen van configuratiegegevens bij een partitie voor de nieuwe wijzigingen in de feed worden nadat alle huidige wijzigingen geleegd. De standaardwaarde is 5000 (5 seconden).
|**leaseAcquireInterval**| **leaseAcquireInterval**| (Optioneel) Als de waarde, deze wordt gedefinieerd, in milliseconden, het interval voor een vliegende start een taak voor het berekenen van als partities gelijkmatig zijn verdeeld over exemplaren bekende hosten. De standaardwaarde is 13000 (13 seconden).
|**leaseExpirationInterval**| **leaseExpirationInterval**| (Optioneel) Als de waarde, deze wordt gedefinieerd, in milliseconden, het interval waarvoor de lease op een lease voor een partitie wordt gemaakt. Als de lease niet binnen dit interval wordt vernieuwd, wordt het verlopen en eigendom van de partitie worden verplaatst naar een andere instantie. De standaardwaarde is 60000 (60 seconden).
|**leaseRenewInterval**| **leaseRenewInterval**| (Optioneel) Als de waarde, deze wordt gedefinieerd, in milliseconden, het vernieuwingsinterval voor alle leases voor partities die momenteel worden vastgehouden door een exemplaar. De standaardwaarde is 17000 (17 seconden).
|**checkpointFrequency**| **checkpointFrequency**| (Optioneel) Als de waarde, deze wordt gedefinieerd, in milliseconden, het interval tussen de lease controlepunten. Standaard is altijd na elke aanroep van de functie.
|**maxItemsPerInvocation**| **maxItemsPerInvocation**| (Optioneel) Als de waarde, past wordt de maximale hoeveelheid items ontvangen per aanroep van de functie.
|**startFromBeginning**| **StartFromBeginning**| (Optioneel) Als de waarde, krijgt de Trigger wilt beginnen met lezen wijzigingen vanaf het begin van de geschiedenis van de verzameling in plaats van de huidige tijd. Dit werkt alleen de eerste keer dat de Trigger wordt gestart, zoals in het volgende wordt uitgevoerd, de controlepunten worden al opgeslagen. Als u dit op `true` wanneer er zijn al gemaakt leases heeft geen effect.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

De trigger is vereist voor een tweede verzameling die worden gebruikt voor het opslaan van _leases_ via de partities. Zowel de verzameling die worden bewaakt en de verzameling waarin de leases moet beschikbaar zijn voor de trigger om te werken.

>[!IMPORTANT]
> Als meerdere functies zijn geconfigureerd voor het gebruik van een Cosmos DB-trigger voor dezelfde verzameling, moet elk van de functies gebruiken van een toegewezen leaseverzameling of geef een andere `LeaseCollectionPrefix` voor elke functie. Anders wordt slechts één van de functies worden geactiveerd. Zie voor meer informatie over het voorvoegsel de [configuratiesectie](#trigger---configuration).

De trigger wordt niet aangegeven dat of een document is bijgewerkt of ingevoegd, biedt deze alleen het document zelf. Als u nodig hebt voor het afhandelen van updates en toevoegingen anders, kunt u dat doen door het implementeren van timestamp velden voor het invoegen of bijwerken.

## <a name="input"></a>Invoer

De Azure Cosmos DB-Invoerbinding maakt gebruik van de SQL-API om een of meer Azure Cosmos DB-documenten te halen en geeft deze door aan de invoerparameter van de functie. De document-ID of query parameters kunnen worden bepaald op basis van de trigger die de functie activeert.

## <a name="input---examples"></a>Invoer - voorbeelden

Zie de voorbeelden taalspecifieke die één document worden gelezen door een id-waarde op te geven:

* [C#](#input---c-examples)
* [C# script (.csx)](#input---c-script-examples)
* [F#](#input---f-examples)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-examples)
* [Python](#input---python-examples)

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---c-examples"></a>Invoer - C#-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [Wachtrijtrigger, zoekt u de ID van JSON](#queue-trigger-look-up-id-from-json-c)
* [HTTP-trigger, uiterlijk-id van de query-tekenreeks](#http-trigger-look-up-id-from-query-string-c)
* [HTTP-trigger, ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-c)
* [HTTP-trigger, ID opzoeken van routegegevens, met behulp van SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP activeren, meerdere documenten, met behulp van SqlQuery ophalen](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP activeren, meerdere documenten, met behulp van DocumentClient ophalen](#http-trigger-get-multiple-docs-using-documentclient-c)

De voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Wachtrijtrigger, zoekt u de ID van JSON (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document worden opgehaald. De functie wordt geactiveerd door een wachtrijbericht met een JSON-object. De wachtrijtrigger parseert de JSON naar een object met de naam `ToDoItemLookup`, waarin de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP-trigger, uiterlijk-id van de query-tekenreeks (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

>[!NOTE]
>De HTTP-queryreeks-parameter is hoofdlettergevoelig.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP-trigger, ID opzoeken van routegegevens (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag dat maakt gebruik van gegevens om op te geven van de ID opzoeken routeren. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP-trigger, ID opzoeken van routegegevens, met behulp van SqlQuery (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag dat maakt gebruik van gegevens om op te geven van de ID opzoeken routeren. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Het voorbeeld ziet u hoe u een bindingexpressie in de `SqlQuery` parameter. U kunt doorgeven gegevens routeren naar de `SqlQuery` parameter zoals wordt weergegeven, maar momenteel [u querytekenreekswaarden niet doorgeven](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP activeren, krijgen meerdere documenten, met behulp van SqlQuery (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een lijst met documenten worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag. De query is opgegeven in de `SqlQuery` eigenschap van het kenmerk.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP activeren, krijgen meerdere documenten, met behulp van DocumentClient (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een lijst met documenten worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag. De code gebruikt een `DocumentClient` exemplaar geleverd door de Azure Cosmos DB-binding met een lijst met documenten lezen. De `DocumentClient` exemplaar kan ook worden gebruikt voor bewerkingen voor schrijven.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---c-script-examples"></a>Invoer - voorbeelden van C#-script

Deze sectie bevat de volgende voorbeelden:

* [Wachtrijtrigger, ID opzoeken van tekenreeks](#queue-trigger-look-up-id-from-string-c-script)
* [Trigger in de wachtrij, meerdere documenten, met behulp van SqlQuery ophalen](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP-trigger, uiterlijk-id van de query-tekenreeks](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP-trigger, ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP activeren, meerdere documenten, met behulp van SqlQuery ophalen](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP activeren, meerdere documenten, met behulp van DocumentClient ophalen](#http-trigger-get-multiple-docs-using-documentclient-c-script)

De HTTP-trigger voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Wachtrijtrigger, zoekt u ID van een tekenreeks (C#-script)

Het volgende voorbeeld ziet u een Cosmos DB-Invoerbinding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie leest één document en updates van de tekstwaarde van het document.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Trigger in de wachtrij, het ophalen van meerdere documenten, met behulp van SqlQuery (C#-script)

Het volgende voorbeeld ziet u een Azure Cosmos DB-Invoerbinding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie haalt meerdere documenten die zijn opgegeven door een SQL-query met behulp van een wachtrijtrigger voor het aanpassen van de queryparameters.

De wachtrijtrigger bevat een parameter `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` retourneert alle records voor de afdeling Financiën.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>HTTP-trigger, uiterlijk-id van de query-tekenreeks (C#-script)

Het volgende voorbeeld wordt een [C#-scriptfunctie](functions-reference-csharp.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Dit is de C#-scriptcode:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>HTTP-trigger, ID opzoeken van gegevens routeren (C#-script)

Het volgende voorbeeld wordt een [C#-scriptfunctie](functions-reference-csharp.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag dat maakt gebruik van gegevens om op te geven van de ID opzoeken routeren. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Dit is de C#-scriptcode:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>HTTP activeren, krijgen meerdere documenten, met behulp van SqlQuery (C#-script)

Het volgende voorbeeld wordt een [C#-scriptfunctie](functions-reference-csharp.md) die een lijst met documenten worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag. De query is opgegeven in de `SqlQuery` eigenschap van het kenmerk.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

Dit is de C#-scriptcode:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>HTTP activeren, krijgen meerdere documenten, met behulp van DocumentClient (C#-script)

Het volgende voorbeeld wordt een [C#-scriptfunctie](functions-reference-csharp.md) die een lijst met documenten worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag. De code gebruikt een `DocumentClient` exemplaar geleverd door de Azure Cosmos DB-binding met een lijst met documenten lezen. De `DocumentClient` exemplaar kan ook worden gebruikt voor bewerkingen voor schrijven.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

Dit is de C#-scriptcode:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---javascript-examples"></a>Invoer - JavaScript-voorbeelden

Deze sectie bevat de volgende voorbeelden die één document worden gelezen door een id-waarde uit diverse bronnen op te geven:

* [Wachtrijtrigger, zoekt u de ID van JSON](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP-trigger, uiterlijk-id van de query-tekenreeks](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP-trigger, ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-javascript)
* [Trigger in de wachtrij, meerdere documenten, met behulp van SqlQuery ophalen](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Wachtrijtrigger, zoekt u de ID van JSON (JavaScript)

Het volgende voorbeeld ziet u een Cosmos DB-Invoerbinding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie leest één document en updates van de tekstwaarde van het document.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>HTTP-trigger, uiterlijk-id van de query-tekenreeks (JavaScript)

Het volgende voorbeeld wordt een [JavaScript-functie](functions-reference-node.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>HTTP-trigger, uiterlijk-id van het routeren van gegevens (JavaScript)

Het volgende voorbeeld wordt een [JavaScript-functie](functions-reference-node.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Trigger in de wachtrij, het ophalen van meerdere documenten, met behulp van SqlQuery (JavaScript)

Het volgende voorbeeld ziet u een Azure Cosmos DB-Invoerbinding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie haalt meerdere documenten die zijn opgegeven door een SQL-query met behulp van een wachtrijtrigger voor het aanpassen van de queryparameters.

De wachtrijtrigger bevat een parameter `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` retourneert alle records voor de afdeling Financiën.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---python-examples"></a>Invoer - Python-voorbeelden

Deze sectie bevat de volgende voorbeelden die één document worden gelezen door een id-waarde uit diverse bronnen op te geven:

* [Wachtrijtrigger, zoekt u de ID van JSON](#queue-trigger-look-up-id-from-json-python)
* [HTTP-trigger, uiterlijk-id van de query-tekenreeks](#http-trigger-look-up-id-from-query-string-python)
* [HTTP-trigger, ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-python)
* [Trigger in de wachtrij, meerdere documenten, met behulp van SqlQuery ophalen](#queue-trigger-get-multiple-docs-using-sqlquery-python)

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-python"></a>Wachtrijtrigger, zoekt u de ID van JSON (Python)

Het volgende voorbeeld ziet u een Cosmos DB-Invoerbinding in een *function.json* bestand en een [funkce Pythonu](functions-reference-python.md) die gebruikmaakt van de binding. De functie leest één document en updates van de tekstwaarde van het document.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-python"></a>HTTP-trigger, uiterlijk-id van de query-tekenreeks (Python)

Het volgende voorbeeld wordt een [funkce Pythonu](functions-reference-python.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

Hier volgt de Python-code:

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-python"></a>HTTP-trigger, ID opzoeken van gegevens routeren (Python)

Het volgende voorbeeld wordt een [funkce Pythonu](functions-reference-python.md) die één document worden opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om op te halen een `ToDoItem` document van de opgegeven database en verzameling.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Hier volgt de Python-code:

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>Trigger in de wachtrij, het ophalen van meerdere documenten, met behulp van SqlQuery (Python)

Het volgende voorbeeld ziet u een Azure Cosmos DB-Invoerbinding in een *function.json* bestand en een [funkce Pythonu](functions-reference-python.md) die gebruikmaakt van de binding. De functie haalt meerdere documenten die zijn opgegeven door een SQL-query met behulp van een wachtrijtrigger voor het aanpassen van de queryparameters.

De wachtrijtrigger bevat een parameter `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` retourneert alle records voor de afdeling Financiën.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```


[Invoer voorbeelden overslaan](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Invoer - F# voorbeelden

Het volgende voorbeeld ziet u een Cosmos DB-Invoerbinding in een *function.json* bestand en een [ F# functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie leest één document en updates van de tekstwaarde van het document.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de F# code:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

In dit voorbeeld heeft een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet-afhankelijkheden:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Om toe te voegen een `project.json` bestand, Zie [ F# van Pakketbeheer](functions-reference-fsharp.md#package).

### <a name="input---java-examples"></a>Invoer - Java-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [HTTP-trigger, uiterlijk-id van de queryreeks - parameter String](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP-trigger, ID van de queryreeks - parameter POJO zoeken](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP-trigger, ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-java)
* [HTTP-trigger, ID opzoeken van routegegevens, met behulp van SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP activeren, meerdere docs uit routeren van gegevens, met behulp van SqlQuery](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

De voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```java
public class ToDoItem {

  private String id;
  private String description;  

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

#### <a name="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>HTTP-trigger, uiterlijk-id van de queryreeks - parameter String (Java)

Het volgende voorbeeld ziet een Java-functie die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Deze ID wordt gebruikt om een document ophalen uit de opgegeven database en verzameling, in de vorm van een tekenreeks.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@CosmosDBInput` aantekening op functieparameters waarvan de waarde afkomstig van Cosmos DB zijn kan.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van optioneel<T>.

#### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>HTTP-trigger, uiterlijk-id van de queryreeks - parameter POJO (Java)

Het volgende voorbeeld ziet een Java-functie die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een query-tekenreeks om op te geven van de ID om te zoeken. Deze ID wordt gebruikt om een document ophalen uit de opgegeven database en verzameling. Het document wordt vervolgens geconverteerd naar een exemplaar van de ```ToDoItem``` POJO eerder hebt gemaakt, en als een argument doorgegeven aan de functie.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-java"></a>HTTP-trigger, uiterlijk-id van het routeren van gegevens (Java)

Het volgende voorbeeld ziet een Java-functie die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een route-parameter om op te geven van de ID om te zoeken. Dat ID wordt gebruikt om een document ophalen uit de opgegeven database en verzameling, retourneren als een ```Optional<String>```.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{id}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>HTTP-trigger, ID opzoeken van routegegevens, met behulp van SqlQuery (Java)

Het volgende voorbeeld ziet een Java-functie die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een route-parameter om op te geven van de ID om te zoeken. Dat een document ophalen uit de opgegeven database en verzameling-ID gebruikt is, conversie van het resultaat ingesteld op een ```ToDoItem[]```, aangezien veel documenten kunnen worden geretourneerd, afhankelijk van de querycriteria.

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}") 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>HTTP activeren, meerdere docs uit routeren van gegevens, met behulp van SqlQuery (Java)

Het volgende voorbeeld ziet u een Java-functie die meerdere documenten. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een route-parameter ```desc``` om op te geven van de tekenreeks die moet worden gezocht de ```description``` veld. De zoekterm die wordt gebruikt om op te halen van een verzameling documenten uit de opgegeven database en verzameling, het converteren van het resultaat ingesteld op een ```ToDoItem[]``` en deze als een argument doorgegeven aan de functie.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] items,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de databasenaam en verzamelingsnaam. Zie voor meer informatie over deze instellingen en andere eigenschappen die u kunt configureren, [de volgende configuratiesectie](#input---configuration).

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `CosmosDB` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**     || Moet worden ingesteld op `cosmosDB`.        |
|**direction**     || Moet worden ingesteld op `in`.         |
|**name**     || De naam van de bindingsparameter die het document in de functie vertegenwoordigt.  |
|**databaseName** |**DatabaseName** |De database met het document.        |
|**collectionName** |**collectionName** | De naam van de verzameling waarin het document. |
|**id**    | **Id** | De ID van het document om op te halen. Deze eigenschap ondersteunt [expressies binding](./functions-bindings-expressions-patterns.md). Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u niet uit, één instelt, wordt de volledige verzameling worden opgehaald. |
|**sqlQuery**  |**SqlQuery**  | Een Azure Cosmos DB SQL-query die wordt gebruikt voor het ophalen van meerdere documenten. De eigenschap biedt ondersteuning voor runtime-bindingen, zoals in dit voorbeeld: `SELECT * FROM c where c.departmentId = {departmentId}`. Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u niet uit, één instelt, wordt de volledige verzameling worden opgehaald.|
|**connectionStringSetting**     |**connectionStringSetting**|De naam van de app-instelling met daarin uw Azure Cosmos DB-verbindingsreeks.        |
|**partitionKey**|**partitionKey**|Hiermee geeft u de waarde voor de partitiesleutel voor de zoekopdracht. Kan omvatten bindende parameters.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

In C# en F# functioneert, wanneer de functie wordt afgesloten, alle wijzigingen aan het ingevoerde document via invoer van de benoemde parameters worden automatisch doorgevoerd.

In JavaScript-functies worden niet automatisch updates aangebracht bij afsluiten van de functie. In plaats daarvan gebruik `context.bindings.<documentName>In` en `context.bindings.<documentName>Out` om updates te maken. Zie het voorbeeld JavaScript.

## <a name="output"></a>Uitvoer

De Azure Cosmos DB-uitvoer binding kunt schrijven u een nieuw document naar een Azure Cosmos DB-database met behulp van de SQL-API.

## <a name="output---examples"></a>Uitvoer - voorbeelden

Zie de voorbeelden taalspecifieke:

* [C#](#output---c-examples)
* [C# script (.csx)](#output---c-script-examples)
* [F#](#output---f-examples)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-examples)

Zie ook de [invoer voorbeeld](#input---c-examples) die gebruikmaakt van `DocumentClient`.

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---c-examples"></a>Uitvoer - C# voorbeelden

Deze sectie bevat de volgende voorbeelden:

* Wachtrijtrigger, één document schrijven
* Wachtrijtrigger, met behulp van IAsyncCollector schrijven-docs

De voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Wachtrijtrigger, schrijven één doc-bestand (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een document wordt toegevoegd aan een database met behulp van gegevens die zijn opgegeven in het bericht van Queue storage.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            ILogger log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.LogInformation($"C# Queue trigger function inserted one row");
            log.LogInformation($"Description={queueMessage}");
        }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Wachtrijtrigger, schrijven docs IAsyncCollector (C#) gebruiken

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) waarmee een verzameling documenten worden toegevoegd aan een database met behulp van gegevens die zijn opgegeven in een wachtrijbericht JSON.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.LogInformation($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---c-script-examples"></a>Uitvoer - voorbeelden van C#-script

Deze sectie bevat de volgende voorbeelden:

* Wachtrijtrigger, één document schrijven
* Wachtrijtrigger, met behulp van IAsyncCollector schrijven-docs

[Voorbeelden van uitvoer overslaan](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Wachtrijtrigger, schrijven één document (C#-script)

Het volgende voorbeeld ziet u een Azure Cosmos DB-Uitvoerbinding een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij-Invoerbinding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie wordt gemaakt van Azure Cosmos DB-documenten in de volgende indeling voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;
    using Microsoft.Extensions.Logging;

    public static void Run(string myQueueItem, out object employeeDocument, ILogger log)
    {
      log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Wachtrijtrigger, met behulp van IAsyncCollector schrijven-docs

Voor het maken van meerdere documenten, kunt u binden aan `ICollector<T>` of `IAsyncCollector<T>` waar `T` is een van de ondersteunde typen.

In dit voorbeeld verwijst naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Dit is het bestand function.json:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connectionStringSetting": "AzureWebJobsStorage"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Dit is de C#-scriptcode:

```cs
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.LogInformation($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---javascript-examples"></a>Uitvoer - JavaScript-voorbeelden

Het volgende voorbeeld ziet u een Azure Cosmos DB-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij-Invoerbinding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie wordt gemaakt van Azure Cosmos DB-documenten in de volgende indeling voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---f-examples"></a>Uitvoer - F# voorbeelden

Het volgende voorbeeld ziet u een Azure Cosmos DB-Uitvoerbinding een *function.json* bestand en een [ F# functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij-Invoerbinding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie wordt gemaakt van Azure Cosmos DB-documenten in de volgende indeling voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de F# code:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json
    open Microsoft.Extensions.Logging

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: ILogger) =
      log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

In dit voorbeeld heeft een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet-afhankelijkheden:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Om toe te voegen een `project.json` bestand, Zie [ F# van Pakketbeheer](functions-reference-fsharp.md#package).

### <a name="output---java-examples"></a>Uitvoer - Java-voorbeelden

* [Wachtrijtrigger bericht opslaan in de database via de geretourneerde waarde](#queue-trigger-save-message-to-database-via-return-value-java)
* [HTTP-trigger, een document met de database via de geretourneerde waarde opslaan](#http-trigger-save-one-document-to-database-via-return-value-java)
* [HTTP-trigger, een document met de database via OutputBinding opslaan](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [HTTP-trigger, meerdere documenten naar de database via OutputBinding opslaan](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


#### <a name="queue-trigger-save-message-to-database-via-return-value-java"></a>Wachtrijtrigger bericht opslaan in de database via de geretourneerde waarde (Java)

Het volgende voorbeeld ziet een Java-functie die een document wordt toegevoegd aan een database met gegevens van een bericht in Queue storage.

```java
@FunctionName("getItem")
@CosmosDBOutput(name = "database", 
  databaseName = "ToDoList", 
  collectionName = "Items", 
  connectionStringSetting = "AzureCosmosDBConnection")
public String cosmosDbQueryById(
    @QueueTrigger(name = "msg", 
      queueName = "myqueue-items", 
      connection = "AzureWebJobsStorage") 
    String message,
    final ExecutionContext context)  {
     return "{ id: \"" + System.currentTimeMillis() + "\", Description: " + message + " }";
   }
```

#### <a name="http-trigger-save-one-document-to-database-via-return-value-java"></a>HTTP-trigger, behalve één document met de database via de geretourneerde waarde (Java)

Het volgende voorbeeld ziet u een Java-functie waarvan handtekening aantekeningen is voorzien met ```@CosmosDBOutput``` en heeft de resultaatwaarde van het type ```String```. Het JSON-document dat is geretourneerd door de functie, automatisch worden geschreven naar de overeenkomstige cosmos DB-verzameling.

```java
    @FunctionName("WriteOneDoc")
    @CosmosDBOutput(name = "database", 
      databaseName = "ToDoList",
      collectionName = "Items", 
      connectionStringSetting = "Cosmos_DB_Connection_String")
    public String run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Parse query parameter        
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        return jsonDocument;
    }
```

#### <a name="http-trigger-save-one-document-to-database-via-outputbinding-java"></a>HTTP-trigger, behalve één document met de database via OutputBinding (Java)

Het volgende voorbeeld ziet u een Java-functie die een document naar CosmosDB via schrijft een ```OutputBinding<T>``` uitvoerparameter. Houd er rekening mee dat in deze configuratie is de ```outputItem``` parameter die worden voorzien van aantekeningen moet met ```@CosmosDBOutput```, niet de functiehandtekening. Met behulp van ```OutputBinding<T>``` kunt u uw functie te profiteren van de binding met het schrijven van het document in CosmosDB terwijl u ook een andere waarde retourneren aan de aanroepende functie, zoals een JSON of XML-document.

```java
    @FunctionName("WriteOneDocOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<String> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        // Set outputItem's value to the JSON document to be saved
        outputItem.setValue(jsonDocument);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Document created successfully.")
                      .build();
    }
```

#### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding-java"></a>HTTP-trigger, meerdere documenten opslaan op een van de database via OutputBinding (Java)

Het volgende voorbeeld ziet u een Java-functie die meerdere documenten naar CosmosDB via schrijft een ```OutputBinding<T>``` uitvoerparameter. Houd er rekening mee dat in deze configuratie is de ```outputItem``` parameter die worden voorzien van aantekeningen moet met ```@CosmosDBOutput```, niet de functiehandtekening. De uitvoerparameter ```outputItem``` heeft een lijst van ```ToDoItem``` objecten zoals het parametertype van de sjabloon. Met behulp van ```OutputBinding<T>``` kunt u uw functie te profiteren van de binding met de documenten in CosmosDB schrijven terwijl u ook een andere waarde retourneren aan de aanroepende functie, zoals een JSON of XML-document.

```java
    @FunctionName("WriteMultipleDocsOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<List<ToDoItem>> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate documents
        List<ToDoItem> items = new ArrayList<>();

        for (int i = 0; i < 5; i ++) {
          // Generate random ID
          final int id = Math.abs(new Random().nextInt());

          // Create ToDoItem
          ToDoItem item = new ToDoItem(String.valueOf(id), name);
          
          items.add(item);
        }

        // Set outputItem's value to the list of POJOs to be saved
        outputItem.setValue(items);
        context.getLogger().info("Document to be saved: " + items);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Documents created successfully.")
                      .build();
    }
```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@CosmosDBOutput` aantekening aan parameters die worden geschreven naar Cosmos DB.  Het parametertype van de aantekening moet ```OutputBinding<T>```, waarbij T een systeemeigen Java-type of een POJO.


## <a name="output---attributes"></a>Uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de databasenaam en verzamelingsnaam. Zie voor meer informatie over deze instellingen en andere eigenschappen die u kunt configureren, [uitvoer - configuratie](#output---configuration). Hier volgt een `CosmosDB` kenmerk voorbeeld in een handtekeningmethode:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Voor een compleet voorbeeld ziet u uitvoer - C# voorbeeld.

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `CosmosDB` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**     || Moet worden ingesteld op `cosmosDB`.        |
|**direction**     || Moet worden ingesteld op `out`.         |
|**name**     || De naam van de bindingsparameter die het document in de functie vertegenwoordigt.  |
|**databaseName** | **DatabaseName**|De database met de verzameling waarin het document wordt gemaakt.     |
|**collectionName** |**collectionName**  | De naam van de verzameling waarin het document wordt gemaakt. |
|**createIfNotExists**  |**createIfNotExists**    | Een Booleaanse waarde om aan te geven of de verzameling is gemaakt als deze nog niet bestaat. De standaardwaarde is *false* omdat nieuwe verzamelingen worden gemaakt met gereserveerde doorvoer, wat gevolgen heeft kosten. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.  |
|**partitionKey**|**partitionKey** |Wanneer `CreateIfNotExists` is ingesteld op true, wordt het pad van de partitie voor de gemaakte verzameling gedefinieerd.|
|**collectionThroughput**|**collectionThroughput**| Wanneer `CreateIfNotExists` is ingesteld op true, definieert de [doorvoer](../cosmos-db/set-throughput.md) van de gemaakte verzameling.|
|**connectionStringSetting**    |**connectionStringSetting** |De naam van de app-instelling met daarin uw Azure Cosmos DB-verbindingsreeks.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik

Wanneer u naar de output-parameter in de functie schrijft wordt een document standaard gemaakt in uw database. Dit document bevat een automatisch gegenereerde GUID als de document-ID. U kunt de document-ID van het uitvoerdocument opgeven door op te geven de `id` eigenschap in het JSON-object doorgegeven aan de output-parameter.

> [!Note]  
> Wanneer u de ID van een bestaand document opgeeft, wordt deze overschreven door het nieuwe uitvoerdocument.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| CosmosDB | [Foutcodes in CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>instellingen voor host.JSON

In deze sectie beschrijft de globale configuratie-instellingen beschikbaar voor deze binding in versie 2.x. Voor meer informatie over globale configuratie-instellingen in versie 2.x, Zie [naslaginformatie over host.json voor Azure Functions versie 2.x](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "cosmosDB": {
            "connectionMode": "Gateway",
            "protocol": "Https",
            "leaseOptions": {
                "leasePrefix": "prefix1"
            }
        }
    }
}
```  

|Eigenschap  |Standaard | Description |
|---------|---------|---------| 
|GatewayMode|Gateway|De verbindingsmodus die door de functie wordt gebruikt bij het verbinden met de Azure Cosmos DB-service. Opties zijn `Direct` en `Gateway`|
|Protocol|Https|De verbindingsprotocol wordt gebruikt door de functie wanneer verbinding met de Azure Cosmos DB-service.  Lezen [hier voor een uitleg van beide modi](../cosmos-db/performance-tips.md#networking)| 
|leasePrefix|N.v.t.|Lease-voorvoegsel moet worden gebruikt voor alle functies in een app.| 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over serverloze computing met Cosmos DB-database](../cosmos-db/serverless-computing-database.md)
* [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
