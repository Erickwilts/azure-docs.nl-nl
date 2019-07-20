---
title: Configureren voor meerdere masters in Azure Cosmos DB
description: Meer informatie over het configureren van meerdere masters in uw toepassingen in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: a5232101d4e5d13fb4c95268311e06b56fa81e65
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68356212"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Multi-Master configureren in uw toepassingen die gebruikmaken van Azure Cosmos DB

Als een account is gemaakt terwijl meerdere schrijf regio's zijn ingeschakeld, moet u twee wijzigingen aanbrengen in uw toepassing in de Connection Policy voor de DocumentClient om de mogelijkheden voor meerdere masters en multi-multihoming in te scha kelen in Azure Cosmos DB. In de Connection Policy stelt u UseMultipleWriteLocations in op True en geeft u de naam van de regio waar de toepassing wordt geïmplementeerd naar SetCurrentLocation. Hiermee wordt de eigenschap PreferredLocations ingevuld op basis van de geografische nabijheid van de locatie die wordt door gegeven in. Als er later een nieuwe regio wordt toegevoegd aan het account, de toepassing niet hoeft te worden bijgewerkt of opnieuw wordt geïmplementeerd, wordt de regio dichter automatisch gedetecteerd en wordt er automatisch een regionale gebeurtenis weer gegeven.

> [!Note]
> Cosmos-accounts die in eerste instantie zijn geconfigureerd met één schrijf regio kunnen worden geconfigureerd voor meerdere schrijf regio's (dat wil zeggen multi-master) met een nul-tijd. Zie [meerdere regio's configureren](how-to-manage-database-account.md#configure-multiple-write-regions) voor meer informatie.

## <a id="netv2"></a>.NET SDK v2

`UseMultipleWriteLocations` Stel`true`in op om meerdere masters in uw toepassing in te scha kelen. Stel `SetCurrentLocation` ook de regio in waarin de toepassing wordt geïmplementeerd en waar Azure Cosmos DB worden gerepliceerd:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3

Als u meerdere masters in uw toepassing wilt inschakelen, `ApplicationRegion` stelt u de regio in waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

```csharp
CosmosClient cosmosClient = new CosmosClient(
    "<connection-string-from-portal>", 
    new CosmosClientOptions()
    {
        ApplicationRegion = Regions.WestUS2,
    });
```

U kunt ook de `CosmosClientBuilder` `WithApplicationRegion` gebruiken om hetzelfde resultaat te krijgen:

```csharp
CosmosClientBuilder cosmosClientBuilder = new CosmosClientBuilder("<connection-string-from-portal>")
            .WithApplicationRegion(Regions.WestUS2);
CosmosClient client = cosmosClientBuilder.Build();
```

## <a id="java"></a>Java Async SDK

Als u meerdere masters in uw toepassing wilt inschakelen, `policy.setUsingMultipleWriteLocations(true)` stelt u `policy.setPreferredLocations` in en stelt u deze in op de regio waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

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

## <a id="javascript"></a>Node. js-, java script-en type script-Sdk's

`connectionPolicy.UseMultipleWriteLocations` Stel`true`in op om meerdere masters in uw toepassing in te scha kelen. Stel `connectionPolicy.PreferredLocations` ook de regio in waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

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

## <a id="python"></a>Python SDK

`connection_policy.UseMultipleWriteLocations` Stel`true`in op om meerdere masters in uw toepassing in te scha kelen. Stel `connection_policy.PreferredLocations` ook de regio in waarin de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd.

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
* [Consistentie niveaus in Azure Cosmos DB](consistency-levels.md)
* [Kies het juiste consistentie niveau in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Wereld wijd schalen van ingerichte door Voer](scaling-throughput.md)
* [Globale distributie: Kijkje achter de schermen](global-dist-under-the-hood.md)
