---
title: 'Azure Cosmos DB: Java-voorbeelden voor de SQL API'
description: Vind Java-voorbeelden op GitHub voor veelvoorkomende taken met de SQL API in Azure Cosmos DB, zoals CRUD-bewerkingen.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: sample
ms.date: 02/08/2019
ms.author: sngun
ms.openlocfilehash: 1b60df197e83fa480a3fdaed60552a6c977845b1
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68250042"
---
# <a name="azure-cosmos-db-java-examples-for-the-sql-api"></a>Azure Cosmos DB: Java-voorbeelden voor de SQL API

> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Async Java-voorbeelden](sql-api-async-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Python-voorbeelden](sql-api-python-samples.md)
> * [Galerie met codevoorbeelden voor Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

De nieuwste voorbeeldtoepassingen waarmee CRUD-bewerkingen en andere veelvoorkomende bewerkingen in Azure Cosmos DB-resources worden uitgevoerd, zijn opgenomen in de GitHub-opslagplaats [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java). Dit artikel bevat:

* Koppelingen naar de taken in elk van de Java-voorbeeldprojectbestanden. 
* Koppelingen naar het bijbehorende API-referentiemateriaal.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- U kunt [voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Via uw Visual Studio-abonnement ontvangt u elke maand een tegoed dat u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U hebt het volgende nodig om deze voorbeeldtoepassing uit te voeren:

* Java Development Kit 7
* Microsoft Azure DocumentDB Java SDK

U kunt eventueel Maven gebruiken om de recentste binaire Microsoft Azure DocumentDB Java SDK-bestanden op te halen voor gebruik in uw project. Maven voegt automatisch eventuele vereiste afhankelijkheden toe. Anders kunt u de afhankelijkheden die worden vermeld in het bestand pom.xml, rechtstreeks downloaden en toevoegen aan uw build-pad.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>LATEST</version>
</dependency>
```

**De voorbeeldtoepassingen uitvoeren**

De voorbeeldopslagplaats klonen:
```bash
$ git clone https://github.com/Azure/azure-documentdb-java.git

$ cd azure-documentdb-java
```

U kunt de voorbeelden uitvoeren met behulp van Eclipse of vanaf de opdrachtregel met behulp van Maven.

Vanaf Eclipse uitvoeren:
* Laad het bovenliggende hoofdprojectbestand pom.xml in Eclipse. De documentdb-voorbeelden moeten automatisch worden geladen.
* Voor het uitvoeren van de voorbeelden hebt u een geldig Azure Cosmos DB-eindpunt nodig. De eindpunten worden gelezen uit `src/test/java/com/microsoft/azure/documentdb/examples/AccountCredentials.java`.
* U kunt de eindpuntreferenties als VM-argumenten in Eclipse JUnit Run Config doorgeven of u kunt de eindpuntreferenties in AccountCredentials.java plaatsen.
    ```bash
    -DACCOUNT_HOST="https://REPLACE_THIS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS"
    ```
* U kunt nu de voorbeelden uitvoeren als JUnit-tests in Eclipse.

Uitvoeren vanaf de opdrachtregel:
* U kunt voorbeelden ook uitvoeren met behulp van Maven:
* Voer Maven uit en geef uw Azure Cosmos DB-eindpuntreferenties door:
    ```bash
    mvn test -DACCOUNT_HOST="https://REPLACE_THIS_WITH_YOURS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS_WITH_YOURS"
    ```

   > [!NOTE]
   > Elk voorbeeld staat op zichzelf. Het stelt zichzelf in en aan het einde worden de gegevens automatisch opgeschoond. In de voorbeelden wordt [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createcollection) meerdere keren aangeroepen. Telkens wanneer dit wordt gedaan, wordt uw abonnement gefactureerd voor 1 uur gebruik voor de prestatielaag van de gemaakte verzameling. 
   > 
   > 

## <a name="database-examples"></a>Voorbeelden voor databases
De [DatabaseCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-databases voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken en lezen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L64-L79) | [DocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createdatabase)<br>[DocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readdatabase)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.resource.setid) |
| [Een database maken en verwijderen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L82-L93) | [DocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.deletedatabase) |
| [Een database maken en een query erop uitvoeren](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L96-L111) | [DocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.querydatabases) |

## <a name="collection-examples"></a>Voorbeelden voor verzamelingen
De [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-verzamelingen voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel.

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling met één partitie maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L74-L84) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createcollection) |
| [Een aangepaste verzameling met meerdere partities maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L103-L155) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentcollection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.partitionkeydefinition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.requestoptions) |
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L97-L99) | [DocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.deletecollection) |

## <a name="document-examples"></a>Voorbeelden voor documenten
De [DocumentCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-documenten voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel.

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken, lezen en verwijderen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L84-L122) | [DocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createdocument)<br>[DocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readdocument)<br>[DocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.deletedocument) |
| [Een document maken met een programmeerbare documentdefinitie](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L126-L147) | [Document](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.resource.setid) |

## <a name="indexing-examples"></a>Voorbeelden van indexen
De [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over voordat de volgende voorbeelden wordt uitgevoerd in Azure Cosmos DB-indexering [indexeringsbeleid](index-policy.md), [indexeren typen](index-types.md), en [indexeren paden](index-paths.md) conceptuele artikelen. 

| Taak | API-verwijzing |
| --- | --- |
| [Een index maken en een indexeringsbeleid instellen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L125-L141) | [Index](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy) |

Zie [Azure Cosmos DB-indexeringsbeleid](index-policy.md) voor meer informatie over indexering.

## <a name="query-examples"></a>Voorbeelden van query's
De [DocumentQuerySamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over een verwijzing naar de SQL-query in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [SQL-queryvoorbeelden](how-to-sql-query.md) conceptueel artikel. 


| Taak | API-verwijzing |
| --- | --- |
| [Een eenvoudige documentquery uitvoeren op verschillende partities](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L108-L129) | [DocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.querydocuments)<br>[FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setenablecrosspartitionquery) |
| [Gesorteerd op-query](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L132-L154) | [FeedResponse\<T > .getQueryIterator](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedresponse.getqueryiterator) |

Zie [SQL-query in Azure Cosmos DB](how-to-sql-query.md) voor meer informatie over het schrijven van query's.

## <a name="offer-examples"></a>Voorbeelden van aanbieding
Het bestand [OfferCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) laat u zien hoe u de volgende taken uitvoert:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken en doorvoer instellen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L76-L102) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.requestoptions.setofferthroughput) |
| [Een verzameling lezen om de bijbehorende aanbieding te vinden](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L108-L132) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.offer.getcontent)<br>[DocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.replaceoffer)<br>[DocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readcollection)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.queryoffers) |

## <a name="partition-key-examples"></a>Voorbeelden van partitiesleutels
De [SinglePartitionCollectionDocumentCrudSample](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over partitionering en partitiesleutels in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [partitioneren](partitioning-overview.md) conceptueel artikel. 


| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling met één partitie maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L164-L207) | [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createcollection) |
| [De doorvoeraanbieding voor een verzameling met één partitie wijzigen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L209-L223) | [DocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.replaceoffer) |

## <a name="stored-procedure-examples"></a>Voorbeelden van opgeslagen procedures
De [StoredProcedureSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over programmeren-Server in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [opgeslagen procedures, triggers en gebruikersgedefinieerde functies](stored-procedures-triggers-udfs.md) conceptueel artikel. 


| Taak | API-verwijzing |
| --- | --- |
| [Een opgeslagen procedure maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L85-L118) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.storedprocedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.createstoredprocedure) |
| [Een opgeslagen procedure met argumenten uitvoeren](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L121-L144) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.executestoredprocedure) |
| [Een opgeslagen procedure met een objectargument uitvoeren](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L147-L177) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.executestoredprocedure) |
