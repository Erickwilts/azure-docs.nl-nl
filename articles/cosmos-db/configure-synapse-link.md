---
title: Azure Synapse-koppeling voor Azure Cosmos DB configureren en gebruiken (preview)
description: Meer informatie over het inschakelen van de Synapse-koppeling voor Azure Cosmos DB accounts, het maken van een container met een analytische opslag, het verbinden van de Azure Cosmos-Data Base naar Synapse-werk ruimte en het uitvoeren van query's.
author: Rodrigossz
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/31/2020
ms.author: rosouz
ms.custom: references_regions
ms.openlocfilehash: dee5a56f309dab8f09a598219f6302c88f4308e7
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2020
ms.locfileid: "92370709"
---
# <a name="configure-and-use-azure-synapse-link-for-azure-cosmos-db-preview"></a>Azure Synapse-koppeling voor Azure Cosmos DB configureren en gebruiken (preview)

De [Azure Synapse-koppeling voor Azure Cosmos DB](synapse-link.md) is een Cloud-native hybride transactionele en analytische verwerking (HTAP) waarmee u bijna realtime analyses kunt uitvoeren via operationele gegevens in azure Cosmos db. Synapse-koppeling maakt een strakkere integratie tussen Azure Cosmos DB en Azure Synapse Analytics.

> [!IMPORTANT]
> Als u Azure Synapse-koppeling wilt gebruiken, moet u uw Azure Cosmos DB-account inrichten & Azure Synapse Analytics-werk ruimte in een van de ondersteunde regio's. De koppeling naar Azure Synapse is momenteel beschikbaar in de volgende Azure-regio's: US West-Centraal, VS-Oost, West-VS2, Europa-noord, Europa-west, Zuid-Centraal VS, Zuidoost-Azië, Australië-oost, Oost-U2, UK-zuid.

De koppeling Azure Synapse is beschikbaar voor Azure Cosmos DB SQL API-containers of voor Azure Cosmos DB-API voor Mongo DB-verzamelingen. Gebruik de volgende stappen om analytische query's uit te voeren met de Azure Synapse-koppeling voor Azure Cosmos DB:

* [Synapse-koppeling voor uw Azure Cosmos DB-accounts inschakelen](#enable-synapse-link)
* [Een analytische archief maken Azure Cosmos DB container is ingeschakeld](#create-analytical-ttl)
* [Uw Azure Cosmos DB-Data Base koppelen aan een Synapse-werk ruimte](#connect-to-cosmos-database)
* [Een query uitvoeren op de analytische opslag met behulp van Synapse Spark](#query-analytical-store-spark)
* [Een query uitvoeren op de analytische opslag met Synapse SQL serverloos](#query-analytical-store-sql-on-demand)
* [Synapse SQL Server gebruiken voor het analyseren en visualiseren van gegevens in Power BI](#analyze-with-powerbi)

## <a name="enable-azure-synapse-link-for-azure-cosmos-db-accounts"></a><a id="enable-synapse-link"></a>Azure Synapse-koppeling voor Azure Cosmos DB accounts inschakelen

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure-account](create-sql-api-dotnet.md#create-account)of selecteer een bestaand Azure Cosmos DB-account.

1. Navigeer naar uw Azure Cosmos DB-account en open het deel venster **functies** .

1. Selecteer **Synapse link** in de lijst met functies.

   :::image type="content" source="./media/configure-synapse-link/find-synapse-link-feature.png" alt-text="Preview-functie voor Synapse link zoeken":::

1. Vervolgens wordt u gevraagd om de Synapse-koppeling in te scha kelen voor uw account. Selecteer **Inschakelen**. Dit proces kan 1 tot vijf minuten duren.

   :::image type="content" source="./media/configure-synapse-link/enable-synapse-link-feature.png" alt-text="Preview-functie voor Synapse link zoeken":::

1. Uw account is nu ingeschakeld voor het gebruik van de Synapse-koppeling. Zie voor meer informatie over het maken van containers voor analytische opslag om automatisch te beginnen met het repliceren van uw operationele gegevens uit het transactionele archief naar de analytische opslag.

> [!NOTE]
> Als u de Synapse-koppeling inschakelt, wordt de analytische opslag niet automatisch ingeschakeld. Wanneer u Synapse-koppeling op het Cosmos DB-account hebt ingeschakeld, moet u analytische opslag op containers inschakelen wanneer u deze maakt, om uw bewerkings gegevens te repliceren naar de analytische opslag. 

## <a name="create-an-azure-cosmos-container-with-analytical-store"></a><a id="create-analytical-ttl"></a> Een Azure Cosmos-container maken met een analytische opslag

U kunt de analytische opslag inschakelen op een Azure Cosmos-container tijdens het maken van de container. U kunt de Azure Portal gebruiken of de `analyticalTTL` eigenschap configureren tijdens het maken van de container met behulp van de Azure Cosmos DB sdk's.

> [!NOTE]
> Op dit moment kunt u het analytische archief inschakelen voor **nieuwe** containers (zowel in nieuwe als bestaande accounts). U kunt gegevens uit uw bestaande-containers migreren naar nieuwe containers met [Azure Cosmos DB-migratie hulpprogramma's.](cosmosdb-migrationchoices.md)

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) of [Azure Cosmos Explorer](https://cosmos.azure.com/).

1. Navigeer naar uw Azure Cosmos DB-account en open het tabblad **Data Explorer** .

1. Selecteer **nieuwe container** en voer een naam in voor uw data base, container, partitie sleutel en gegevens doorvoer. Schakel de optie voor de **analytische opslag** in. Nadat u het analytische archief hebt ingeschakeld, maakt het een container waarvan `AnalyicalTTL` de eigenschap is ingesteld op de standaard waarde-1 (oneindige retentie). Dit analytische archief behoudt alle historische versies van records.

   :::image type="content" source="./media/configure-synapse-link/create-container-analytical-store.png" alt-text="Preview-functie voor Synapse link zoeken":::

1. Als u de Synapse-koppeling voor dit account nog niet hebt ingeschakeld, wordt u gevraagd dit te doen omdat het een vereiste is om een container voor een analytische opslag te maken. Selecteer **Synapse-koppeling inschakelen**als u hierom wordt gevraagd. Dit proces kan 1 tot vijf minuten duren.

1. Selecteer **OK**om een Azure Cosmos-container met analytische opslag te maken.

1. Nadat de container is gemaakt, controleert u of het analytische archief is ingeschakeld door te klikken op **instellingen**, rechts onder documenten in Data Explorer en controleert u of de optie voor het **analytische archief time to Live** is ingeschakeld.

### <a name="net-sdk"></a>.NET SDK

Met de volgende code wordt een container met analytische opslag gemaakt met behulp van de .NET SDK. Stel de eigenschap van de analytische TTL in op de vereiste waarde. Voor de lijst met toegestane waarden raadpleegt u het artikel [analytische TTL-waarden](analytical-store-introduction.md#analytical-ttl) die worden ondersteund:

```csharp
// Create a container with a partition key, and analytical TTL configured to  -1 (infinite retention)
string containerId = “myContainerName”;
int analyticalTtlInSec = -1;
ContainerProperties cpInput = new ContainerProperties()
            {
Id = containerId,
PartitionKeyPath = "/id",
AnalyticalStorageTimeToLiveInSeconds = analyticalTtlInSec,
};
 await this. cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(cpInput);
```

### <a name="java-v4-sdk"></a>Java V4 SDK

Met de volgende code wordt een container met analytische opslag gemaakt met behulp van de Java v4-SDK. Stel de `AnalyticalStoreTimeToLiveInSeconds` eigenschap in op de vereiste waarde:

```java
// Create a container with a partition key and  analytical TTL configured to  -1 (infinite retention) 
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

containerProperties.setAnalyticalStoreTimeToLiveInSeconds(-1);

container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="python-v4-sdk"></a>Python v4-SDK

Python 2,7 en Azure Cosmos DB SDK 4.1.0 zijn de vereiste minimum versie en de SDK is alleen compatibel met de SQL-API.

De eerste stap is om ervoor te zorgen dat u ten minste versie 4.1.0 van de [python-SDK van Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos)gebruikt:

```python
import azure.cosmos as cosmos

print (cosmos.__version__)
```
Met de volgende stap maakt u een container met een analytische opslag met behulp van de Azure Cosmos DB python SDK:

```python
# Azure Cosmos DB Python SDK, for SQL API only.
# Creating an analytical store enabled container.

import azure.cosmos.cosmos_client as cosmos_client
import azure.cosmos.exceptions as exceptions
from azure.cosmos.partition_key import PartitionKey

HOST = 'your-cosmos-db-account-URI'
KEY = 'your-cosmos-db-account-key'
DATABASE = 'your-cosmos-db-database-name'
CONTAINER = 'your-cosmos-db-container-name'

client = cosmos_client.CosmosClient(HOST,  KEY )
# setup database for this sample. 
# If doesn't exist, creates a new one with the name informed above.
try:
    db = client.create_database(DATABASE)

except exceptions.CosmosResourceExistsError:
    db = client.get_database_client(DATABASE)

# Creating the container with analytical store enabled, using the name informed above.
# If a container with the same name exists, an error is returned.
#
# The 3 options for the analytical_storage_ttl parameter are:
# 1) 0 or Null or not informed (Not enabled).
# 2) -1 (The data will be stored in analytical store infinitely).
# 3) Any other number is the actual ttl, in seconds.

try:
    container = db.create_container(
        id=CONTAINER,
        partition_key=PartitionKey(path='/id', kind='Hash'),analytical_storage_ttl=-1
    )
    properties = container.read()
    print('Container with id \'{0}\' created'.format(container.id))
    print('Partition Key - \'{0}\''.format(properties['partitionKey']))

except exceptions.CosmosResourceExistsError:
    print('A container with already exists')
```

### <a name="update-the-analytical-store-time-to-live"></a><a id="update-analytical-ttl"></a> De time-outwaarde voor het analytische archief bijwerken

Nadat de analytische opslag met een bepaalde TTL-waarde is ingeschakeld, kunt u deze later bijwerken naar een andere geldige waarde. U kunt de waarde bijwerken met behulp van de Azure-portal of SDK's. Zie voor meer informatie over de verschillende configuratie opties voor de analyse-TTL het artikel [analytische TTL-waarden](analytical-store-introduction.md#analytical-ttl) .

#### <a name="azure-portal"></a>Azure Portal

Als u een container voor een analytische opslag hebt gemaakt via de Azure Portal, bevat deze een standaard analyse-TTL van-1. Gebruik de volgende stappen om deze waarde bij te werken:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) of [Azure Cosmos Explorer](https://cosmos.azure.com/).

1. Navigeer naar uw Azure Cosmos DB-account en open het tabblad **Data Explorer** .

1. Selecteer een bestaande container waarvoor het analytische archief is ingeschakeld. Vouw het item uit en wijzig de volgende waarden:

  * Open het venster **Schaal en instellingen**.
  * Onder **instelling** zoeken, * * analytische opslag time to Live * *.
  * Selecteer **On (no default)** of **Aan** en stel een TTL-waarde in
  * Klik op **Opslaan** om de wijzigingen op te slaan.

#### <a name="net-sdk"></a>.NET SDK

De volgende code laat zien hoe u de TTL voor de analyse opslag bijwerkt met behulp van de .NET SDK:

```csharp
// Get the container, update AnalyticalStorageTimeToLiveInSeconds 
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Update analytical store TTL
containerResponse.Resource. AnalyticalStorageTimeToLiveInSeconds = 60 * 60 * 24 * 180  // Expire analytical store data in 6 months;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

#### <a name="java-v4-sdk"></a>Java V4 SDK

De volgende code laat zien hoe u de TTL voor de analytische opslag bijwerkt met behulp van de Java v4 SDK:

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

// Update analytical store TTL to expire analytical store data in 6 months;
containerProperties.setAnalyticalStoreTimeToLiveInSeconds (60 * 60 * 24 * 180 );  
 
// Update container settings
container.replace(containerProperties).block();
```

## <a name="connect-to-a-synapse-workspace"></a><a id="connect-to-cosmos-database"></a> Verbinding maken met een Synapse-werk ruimte

Volg de instructies in [verbinding maken met Azure Synapse link](../synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db.md) voor toegang tot een Azure Cosmos DB Data Base vanuit Azure Synapse Analytics Studio met Azure Synapse link.

## <a name="query-analytical-store-using-apache-spark-for-azure-synapse-analytics"></a><a id="query-analytical-store-spark"></a> Een query uitvoeren op analytisch archief met Apache Spark voor Azure Synapse Analytics

Volg de instructies in het artikel [query Azure Cosmos DB Analytical Store](../synapse-analytics/synapse-link/how-to-query-analytical-store-spark.md) voor informatie over het uitvoeren van Query's met Synapse Spark. In dit artikel vindt u enkele voor beelden van de manier waarop u kunt communiceren met de analytische opslag vanuit Synapse-gebaren. Deze gebaren worden weer gegeven wanneer u met de rechter muisknop op een container klikt. Met penbewegingen kunt u snel code genereren en deze aanpassen aan uw behoeften. Ze zijn ook ideaal voor het detecteren van gegevens met één klik.

## <a name="query-the-analytical-store-using-synapse-sql-serverless"></a><a id="query-analytical-store-sql-on-demand"></a> Een query uitvoeren op de analytische opslag met Synapse SQL serverloos

Synapse SQL serverloos (een preview-functie die voorheen **SQL on-demand**werd genoemd) kunt u gegevens in uw Azure Cosmos DB containers die zijn ingeschakeld met de koppeling Azure Synapse, opvragen en analyseren. U kunt gegevens in bijna realtime analyseren zonder dat dit van invloed is op de prestaties van uw transactionele werk belastingen. Het biedt een bekende T-SQL-syntaxis voor het opvragen van gegevens uit de analytische opslag en de geïntegreerde connectiviteit met een breed scala aan BI-en ad-hoc hulp middelen voor query's via de T-SQL-interface. Zie voor meer informatie de [analytische query voor query's met Synapse SQL Server](../synapse-analytics/sql/query-cosmos-db-analytical-store.md) zonder artikelen.

## <a name="use-synapse-sql-serverless-to-analyze-and-visualize-data-in-power-bi"></a><a id="analyze-with-powerbi"></a>Synapse SQL Server gebruiken voor het analyseren en visualiseren van gegevens in Power BI

U kunt een Synapse SQL serverloze data base en weer gaven maken via de Synapse-koppeling voor Azure Cosmos DB. Later kunt u een query uitvoeren op de Azure Cosmos-containers en vervolgens een model bouwen met Power BI over die weer gaven om die query weer te geven. Zie [Synapse SQL Server gebruiken om Azure Cosmos DB gegevens te analyseren met Synapse-koppelings](synapse-link-power-bi.md) artikel voor meer informatie.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

Met de [Azure Resource Manager-sjabloon](manage-sql-with-resource-manager.md#azure-cosmos-account-with-analytical-store) wordt een Synapse-koppeling ingeschakeld Azure Cosmos DB account voor de SQL-API. Met deze sjabloon maakt u een core-API-account (SQL) in één regio met een container die is geconfigureerd met analytische TTL ingeschakeld en een optie om de door Voer van hand matig of automatisch schalen te gebruiken. Als u deze sjabloon wilt implementeren, klikt **u op implementeren in azure** op de pagina README.

## <a name="getting-started-with-azure-synpase-link---samples"></a><a id="cosmosdb-synapse-link-samples"></a> Aan de slag met Azure Synpase-koppeling-voor beelden

U kunt voor beelden vinden om aan de slag te gaan met de koppeling van Azure Synapse op [github](https://aka.ms/cosmosdb-synapselink-samples). In deze etalage worden end-to-end-oplossingen gedemonstreerd met IoT-en retail-scenario's. U kunt ook de voor beelden vinden die overeenkomen met Azure Cosmos DB-API voor MongoDB in dezelfde opslag plaats in de map [MongoDb](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/MongoDB) . 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende documenten voor meer informatie:

* [Azure Synapse-koppeling voor Azure Cosmos DB.](synapse-link.md)

* [Overzicht van Azure Cosmos DB Analytical Store.](analytical-store-introduction.md)

* [Veelgestelde vragen over de Synapse-koppeling voor Azure Cosmos DB.](synapse-link-frequently-asked-questions.md)

* [Apache Spark in azure Synapse Analytics](../synapse-analytics/spark/apache-spark-concepts.md).

* [Ondersteuning voor SQL serverloze runtime in azure Synapse Analytics](../synapse-analytics/sql/on-demand-workspace-overview.md).
