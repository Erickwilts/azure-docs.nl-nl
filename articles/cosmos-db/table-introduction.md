---
title: Inleiding tot de Table-API van Azure Cosmos DB
description: Meer informatie over hoe u Azure Cosmos DB kunt gebruiken om grote hoeveelheden sleutelwaardegegevens met lage latentie op te slaan en op te vragen met behulp van de Azure Table-API.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 07/26/2019
ms.author: sngun
ms.openlocfilehash: 6cf9dddbfa6eb6f754c529981307f8ee80cfe7d1
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89376992"
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Inleiding tot Azure Cosmos DB: Tabel-API

[Azure Cosmos DB](introduction.md) biedt de Table-API voor toepassingen die zijn geschreven voor Azure Table-opslag en die premium-mogelijkheden nodig hebben, zoals:

* [Kant-en-klare wereldwijde distributie](distribute-data-globally.md).
* [Speciale doorvoer](partition-data.md) overal ter wereld.
* Latentie van slechts enkele milliseconden op het 99e percentiel.
* Gegarandeerde hoge beschikbaarheid.
* Automatische secundaire indexering.

Toepassingen die zijn geschreven voor Azure Table-opslag kunnen met behulp van de Table-API zonder codeaanpassingen worden gemigreerd naar Azure Cosmos DB en zo gebruikmaken van premium-mogelijkheden. In Table-API zijn client-SDK's beschikbaar voor .NET, Java, Python en Node.js.

> [!IMPORTANT]
> De .NET Framework SDK [Microsoft.Azure.CosmosDB.Table ](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) bevindt zich in de onderhoudsmodus en wordt binnenkort beëindigd. Voer een upgrade uit naar de nieuwe .NET Standaard bibliotheek [Microsoft.Azure.Cosmos.table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) om door te gaan met de nieuwste functies die door de Table-API worden ondersteund.

## <a name="table-offerings"></a>Aanbiedingen voor Table
Als u momenteel gebruikmaakt van Azure Table-opslag, levert overstappen naar de Azure Cosmos DB Table-API de volgende voordelen op:

| Functie | Azure Table Storage | Azure Cosmos DB Table-API |
| --- | --- | --- |
| Latentie | Snel, maar geen bovengrens voor latentie. | Latentie van slechts enkele milliseconden voor lees- en schrijfbewerkingen, ondersteund door <10 ms latentie voor lees- en schrijfbewerkingen in het 99e percentiel, op elke schaal, overal ter wereld. |
| Doorvoer | Model voor variabele doorvoersnelheid. Tabellen hebben een schaalbaarheidslimiet van 20.000 bewerkingen/sec. | Zeer schaalbaar met [toegewezen gereserveerde doorvoer per tabel](request-units.md), op basis van serviceovereenkomsten. Accounts hebben geen bovengrens voor doorvoer en bieden ondersteuning voor > 10 miljoen bewerkingen/sec per tabel. |
| Wereldwijde distributie | Eén regio met één optioneel leesbaar secundair leesgebied voor hoge beschikbaarheid. | [Kant en klare wereldwijde distributie](distribute-data-globally.md) van één tot een willekeurig aantal regio's. Ondersteuning voor [kant en klare wereldwijde distributie](high-availability.md), op elk moment en overal ter wereld. Mogelijkheid voor meerdere masters waardoor elke regio schrijfbewerkingen kan accepteren. |
| Indexeren | Alleen primaire index op PartitionKey en RowKey. Geen secundaire indexen. | Standaard automatische en volledige indexering op alle eigendommen, geen indexbeheer. |
| Query’s uitvoeren | Voor de queryuitvoering wordt een index gebruikt als primaire sleutel. In andere gevallen wordt er gescand. | Query's kunnen profiteren van de automatische indexering van eigenschappen voor een snelle uitvoertijden van query's. |
| Consistentie | Sterke in primaire regio. Mogelijk in secundaire regio. | [Vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor een wisselwerking tussen beschikbaarheid, latentie, doorvoer en consistentie op basis van uw toepassingsvereisten. |
| Prijzen | Geoptimaliseerd voor opslag. | Geoptimaliseerd voor doorvoer. |
| SLA's | 99,9% tot 99,99% beschikbaarheid, afhankelijk van de replicatiestrategie. | 99,999% leesbeschikbaarheid, 99,99% schrijfbeschikbaarheid van een account met één regio en 99,999% schrijfbeschikbaarheid voor accounts met meerdere regio's. [Uitgebreide SLA's](https://azure.microsoft.com/support/legal/sla/cosmos-db/) voor beschikbaarheid, latentie, doorvoer en consistentie. |

## <a name="get-started"></a>Aan de slag

Maak een Azure Cosmos DB-account in de [Azure-portal](https://portal.azure.com). Ga dan aan de slag met onze [Snelstartgids voor Table-API met behulp van .NET](create-table-dotnet.md). 

> [!IMPORTANT]
> Als u tijdens de preview een tabel-API-account hebt gemaakt, moet u een [nieuw tabel-API-account](create-table-dotnet.md#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK’s voor tabel-API's.
>

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanwijzingen om aan de slag te gaan:
* [Een .NET-toepassing ontwikkelen met de Table-API](create-table-dotnet.md)
* [Ontwikkelen met de Table-API in .NET](tutorial-develop-table-dotnet.md)
* [Tabelgegevens opvragen met de Table-API](tutorial-query-table.md)
* [Meer informatie over het instellen van wereldwijde distributie met Azure Cosmos DB met behulp van de Table-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table .NET Standard SDK](table-sdk-dotnet-standard.md)
* [Azure Cosmos DB Table .NET SDK](table-sdk-dotnet.md)
* [Azure Cosmos DB Table Java SDK](table-sdk-java.md)
* [Azure Cosmos DB Table Node.js SDK](table-sdk-nodejs.md)
* [Inleiding tot Azure Cosmos DB Table SDK voor Python](table-sdk-python.md)
