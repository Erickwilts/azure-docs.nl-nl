---
title: Zelfstudie voor het instellen van wereldwijde distributie met Azure Cosmos DB met behulp van de SQL-API
description: Lees hoe u wereldwijde distributie met Azure Cosmos DB kunt instellen met behulp van de SQL-API.
author: rimman
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 07/15/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: a566094f88ba9ffd25eadd046ae7254e26b9c2cf
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234602"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-sql-api"></a>Wereldwijde distributie van Azure Cosmos DB instellen met behulp van de SQL-API

In dit artikel laten we zien hoe Azure Portal kan worden gebruikt voor het instellen van wereldwijde distributie van Azure DB Cosmos en hoe u daarmee verbinding kunt maken met behulp van de SQL-API.

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Wereldwijde distributie configureren met behulp van de [SQL-API's](sql-api-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a>Verbinding maken met een voorkeursregio met behulp van de SQL-API

Om te profiteren van [wereldwijde distributie](distribute-data-globally.md), kunnen clienttoepassingen de geordende voorkeurslijst met regio's opgeven die moet worden gebruikt voor het uitvoeren van documentbewerkingen. Dat kan worden gedaan door het verbindingsbeleid in te stellen. Op basis van de Azure Cosmos DB-accountconfiguratie, de huidige regionale beschikbaarheid en de opgegeven voorkeurslijst wordt het optimale eindpunt door de SQL-SDK gekozen voor het uitvoeren van schrijf- en leesbewerkingen.

Deze voorkeurslijst wordt opgegeven bij het initialiseren van een verbinding met de SQL-SDK's. De SDK's accepteren de optionele parameter 'PreferredLocations', die een geordende lijst met Azure-regio's bevat.

De SDK verzendt alle schrijfbewerkingen automatisch naar de huidige schrijfregio.

Alle leesbewerkingen worden verzonden naar de eerst beschikbare regio in de lijst PreferredLocations. Als de aanvraag mislukt, gaat de client naar de volgende regio in de lijst, enzovoort.

De SDK's proberen alleen de regio's te lezen die zijn opgegeven in PreferredLocations. Dus als bijvoorbeeld het databaseaccount in vier verschillende regio's beschikbaar is, maar de client alleen twee leesregio's (waarnaar niet kan worden geschreven) voor PreferredLocations opgeeft, worden er geen leesbewerkingen geleverd vanuit de leesregio die niet is opgegeven in PreferredLocations. Als de leesregio's die zijn opgegeven in PreferredLocations niet beschikbaar zijn, worden leesbewerkingen geleverd vanuit de schrijfregio.

De toepassing kan het huidige, door de SDK gekozen eindpunt voor lezen en eindpunt voor schrijven verifiëren door het controleren van de twee eigenschappen, WriteEndpoint en ReadEndpoint, die beschikbaar zijn in SDK-versie 1.8 en hoger.

Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen verwerkt vanuit de huidige schrijfregio.

## <a name="net-sdk"></a>.NET SDK
De SDK kan worden gebruikt zonder codewijzigingen. In dit geval stuurt de SDK alle lees- en schrijfbewerkingen automatisch door naar de huidige schrijfregio.

In versie 1.8 en hoger van de .NET SDK heeft de ConnectionPolicy-parameter voor de DocumentClient-constructor de eigenschap Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Deze eigenschap is van het type Verzameling `<string>`, die een lijst met regionamen moet bevatten. De tekenreekswaarden zijn opgemaakt volgens de kolom regionaam op de [Azure-regio's][regions] pagina, zonder spaties vóór of na de eerste en laatste teken respectievelijk.

De huidige eindpunten voor schrijven en lezen zijn beschikbaar in respectievelijk DocumentClient.WriteEndpoint en DocumentClient.ReadEndpoint.

> [!NOTE]
> De URL's voor de eindpunten moeten niet worden beschouwd als constanten met een lange levensduur. De service kan deze op elk gewenst moment bijwerken. De SDK verwerkt deze wijziging automatisch.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejsjavascript"></a>Node.js/JavaScript

De SDK kan worden gebruikt zonder codewijzigingen. In dit geval stuurt de SDK alle lees- en schrijfbewerkingen automatisch door naar de huidige schrijfregio.

In versie 1.8 en hoger van elke SDK heeft de ConnectionPolicy-parameter voor de DocumentClient-constructor een nieuwe eigenschap, DocumentClient.ConnectionPolicy.PreferredLocations genaamd. Deze parameter is een matrix van tekenreeksen die een lijst met regionamen verwerkt. De namen zijn opgemaakt volgens de kolom regionaam op de [Azure-regio's][regions] pagina. U kunt ook gebruikmaken van de vooraf gedefinieerde constanten in het object AzureDocuments.Regions

De huidige eindpunten voor schrijven en lezen zijn beschikbaar in respectievelijk DocumentClient.getWriteEndpoint en DocumentClient.getReadEndpoint.

> [!NOTE]
> De URL's voor de eindpunten moeten niet worden beschouwd als constanten met een lange levensduur. De service kan deze op elk gewenst moment bijwerken. De SDK verwerkt deze wijziging automatisch.
>
>

Hieronder ziet u een codevoorbeeld voor Node.js/Javascript.

```JavaScript
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="python-sdk"></a>Python-SDK

De volgende code laat zien hoe voorkeurslocaties instellen met behulp van de Python-SDK:

```python

connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe']
client = cosmos_client.CosmosClient(ENDPOINT, {'masterKey': MASTER_KEY}, connectionPolicy)

```

## <a name="java-v2-sdk"></a>V2 Java SDK

De volgende code laat zien hoe voorkeurslocaties instellen met behulp van de Java SDK:

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Arrays.asList("East US", "West US", "Canada Central"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKeyOrResourceToken(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy)
                .build();
```

## <a name="rest"></a>REST
Zodra een databaseaccount in meerdere regio's beschikbaar komt, kunnen clients de beschikbaarheid ervan opvragen door een GET-aanvraag voor de volgende URI uit te voeren.

    https://{databaseaccount}.documents.azure.com/

De service retourneert een lijst met regio's en hun bijbehorende Azure Cosmos DB-eindpunt-URI's voor de replica's. De huidige schrijfregio wordt aangegeven in het antwoord. Vervolgens kan de client op de volgende wijze het juiste eindpunt voor alle verdere REST-API-aanvragen selecteren.

Voorbeeld van een antwoord

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Alle PUT-, POST- en DELETE-aanvragen moeten verwijzen naar de opgegeven URI voor schrijven
* Alle GET-aanvragen en andere alleen-lezen aanvragen (zoals query's) kunnen verwijzen naar elk eindpunt dat de client aangeeft

Schrijfaanvragen naar alleen-lezen regio's mislukken met HTTP-foutcode 403 ('Verboden').

Als de schrijfregio verandert na de initiële detectiefase van de client, mislukken daaropvolgende schrijfbewerkingen naar de vorige schrijfregio met HTTP-foutcode 403 ('Verboden'). De client moet dan de lijst met regio's opnieuw ophalen om de bijgewerkte schrijfregio te krijgen.

En daarmee is deze zelfstudie voltooid. Informatie over het beheren van de consistentie van uw wereldwijd gerepliceerde account kunt u vinden in [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En voor meer informatie over hoe wereldwijde databasereplicatie werkt in Azure Cosmos DB, gaat u naar [Gegevens wereldwijd distribueren met Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Wereldwijde distributie configureren met behulp van Azure Portal
> * Wereldwijde distributie configureren met behulp van de SQL-API's

U kunt nu doorgaan met de volgende zelfstudie voor informatie over lokaal ontwikkelen met behulp van de lokale Azure Cosmos DB-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

