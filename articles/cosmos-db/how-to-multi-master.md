---
title: Schrijf bewerkingen voor meerdere regio's configureren in Azure Cosmos DB
description: Meer informatie over het configureren van schrijf bewerkingen voor meerdere regio's voor uw toepassingen met behulp van verschillende Sdk's in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/10/2020
ms.author: mjbrown
ms.custom: devx-track-python, devx-track-js, devx-track-csharp
ms.openlocfilehash: 95337f88133c9493250e9197654288dc0af59ed1
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2020
ms.locfileid: "92486137"
---
# <a name="configure-multi-region-writes-in-your-applications-that-use-azure-cosmos-db"></a>Schrijf bewerkingen in meerdere regio's configureren in uw toepassingen die gebruikmaken van Azure Cosmos DB

Als een account is gemaakt terwijl meerdere schrijf regio's zijn ingeschakeld, moet u twee wijzigingen aanbrengen in uw toepassing voor de Connection Policy voor de DocumentClient om de multi-regio schrijf bewerkingen en mogelijkheden voor multi-multihoming in te scha kelen in Azure Cosmos DB. In de Connection Policy stelt u UseMultipleWriteLocations in op True en geeft u de naam van de regio waar de toepassing wordt geïmplementeerd naar SetCurrentLocation. Hiermee wordt de eigenschap PreferredLocations ingevuld op basis van de geografische nabijheid van de locatie die wordt door gegeven in. Als er later een nieuwe regio wordt toegevoegd aan het account, de toepassing niet hoeft te worden bijgewerkt of opnieuw wordt geïmplementeerd, wordt de regio dichter automatisch gedetecteerd en wordt er automatisch een regionale gebeurtenis weer gegeven.

> [!Note]
> Cosmos-accounts die in eerste instantie zijn geconfigureerd met één schrijf regio kunnen worden geconfigureerd voor meerdere schrijf regio's met een onbepaalde tijd. Zie [meerdere regio's configureren](how-to-manage-database-account.md#configure-multiple-write-regions) voor meer informatie.

## <a name="net-sdk-v2"></a><a id="netv2"></a>.NET-SDK v2

Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, stelt `UseMultipleWriteLocations` u in op `true` . Stel ook `SetCurrentLocation` de regio in waarin de toepassing wordt geïmplementeerd en waar Azure Cosmos DB worden gerepliceerd:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a name="net-sdk-v3"></a><a id="netv3"></a>.NET-SDK v3

Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, stelt `ApplicationRegion` u in op de regio waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

```csharp
CosmosClient cosmosClient = new CosmosClient(
    "<connection-string-from-portal>", 
    new CosmosClientOptions()
    {
        ApplicationRegion = Regions.WestUS2,
    });
```

U kunt ook de gebruiken `CosmosClientBuilder` `WithApplicationRegion` om hetzelfde resultaat te krijgen:

```csharp
CosmosClientBuilder cosmosClientBuilder = new CosmosClientBuilder("<connection-string-from-portal>")
            .WithApplicationRegion(Regions.WestUS2);
CosmosClient client = cosmosClientBuilder.Build();
```

## <a name="java-v4-sdk"></a><a id="java4-multi-region-writes"></a> Java V4 SDK

Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, roept u `.multipleWriteRegionsEnabled(true)` `.preferredRegions(preferredRegions)` de client-opbouw functie op en gebruikt u deze, waarbij `preferredRegions` een `List` bevat één element, dat de regio is waarin de toepassing wordt geïmplementeerd en waar Cosmos DB wordt gerepliceerd:

# <a name="async"></a>[Async](#tab/api-async)

   [Java SDK V4](sql-api-sdk-java-v4.md) (Maven [com.azure::azure-cosmos](https://mvnrepository.com/artifact/com.azure/azure-cosmos)) Async API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=ConfigureMultimasterAsync)]

# <a name="sync"></a>[Sync](#tab/api-sync)

   [Java SDK V4](sql-api-sdk-java-v4.md) (Maven [com.azure::azure-cosmos](https://mvnrepository.com/artifact/com.azure/azure-cosmos)) Sync API

   [!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/sync/SampleDocumentationSnippets.java?name=ConfigureMultimasterSync)]

--- 

## <a name="async-java-v2-sdk"></a><a id="java2-multi-region-writes"></a> Asynchrone Java v2 SDK

De Java v2 SDK heeft de Maven [com. micro soft. Azure:: Azure-cosmosdb](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)gebruikt. Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, stelt `policy.setUsingMultipleWriteLocations(true)` u in en stelt `policy.setPreferredLocations` u deze in op de regio waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a name="nodejs-javascript-and-typescript-sdks"></a><a id="javascript"></a>Node.js, java script en type script Sdk's

Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, stelt `connectionPolicy.UseMultipleWriteLocations` u in op `true` . Stel ook `connectionPolicy.PreferredLocations` de regio in waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a name="python-sdk"></a><a id="python"></a>Python-SDK

Als u schrijf bewerkingen in meerdere regio's in uw toepassing wilt inschakelen, stelt `connection_policy.UseMultipleWriteLocations` u in op `true` . Stel ook `connection_policy.PreferredLocations` de regio in waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {
                                    'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen:

* [Sessie tokens gebruiken voor het beheren van consistentie in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Conflictsoorten en oplossingsbeleid in Azure Cosmos DB](conflict-resolution-policies.md)
* [Hoge beschikbaarheid in Azure Cosmos DB](high-availability.md)
* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Kies het juiste consistentie niveau in Azure Cosmos DB](./consistency-levels.md)
* [Consistentie, Beschik baarheid en prestatie afweging in Azure Cosmos DB](./consistency-levels.md)
* [Beschik baarheid en prestaties voor diverse consistentie niveaus](./consistency-levels.md)
* [Wereldwijd schalen van ingerichte doorvoer](./request-units.md)
* [Wereld wijde distributie: onder de motorkap](global-dist-under-the-hood.md)