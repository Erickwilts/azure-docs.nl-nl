---
title: Azure Cosmos DB Table-API .NET Standard SDK & resources
description: Meer informatie over de Azure Cosmos DB Table-API en de .NET Standard-SDK, inclusief release datums, pensioen datums en wijzigingen die zijn aangebracht tussen elke versie.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/18/2019
ms.openlocfilehash: b3ebd8b6f65ec86d338932579f5d4ccec6a90be3
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704735"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB tabel .NET Standard API: Down load en release-opmerkingen
> [!div class="op_single_selector"]
> 
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Voorbeeld**|[Cosmos DB Table-API .NET-voor beeld](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started)|
|**Snelstartgids**|[Snelstartgids](create-table-dotnet.md)|
|**Zelfstudie**|[Zelfstudie](tutorial-develop-table-dotnet.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET standaard 2,0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name104-preview104-preview"></a><a name="1.0.4-preview"/>1.0.4-preview
* Opgeloste fouten
* Geef de HttpClientTimeout-optie voor RestExecutorConfiguration op.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Opgeloste fouten

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Versie met algemene beschikbaarheid

### <a name="a-name0110-preview0110-preview"></a><a name="0.11.0-preview"/>0.11.0-Preview
* Er zijn wijzigingen aangebracht in de manier waarop CloudTableClient kan worden geconfigureerd. Er wordt nu een TableClientConfiguration-object gebruikt tijdens het bouwen. TableClientConfiguration biedt verschillende eigenschappen voor het configureren van het client gedrag, afhankelijk van het feit of het doel eindpunt Cosmos DB Table-API of Azure Storage Table-API.
* Ondersteuning toegevoegd aan TableQuery om resultaten te retour neren in de gesorteerde volg orde van een aangepaste kolom. Deze functie wordt alleen ondersteund voor Cosmos DB Table-eind punten.
* Er is ondersteuning toegevoegd om RequestCharges beschikbaar te maken voor verschillende resultaat typen. Deze functie wordt alleen ondersteund voor Cosmos DB Table-eind punten.

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1-preview
* Voeg ondersteuning toe voor SAS-token, bewerkingen van TablePermissions, ServiceProperties en ServiceStats tegen Azure Storage Table-eind punten. 
   > [!NOTE]
   > Sommige functies in eerdere Azure Storage tabel-Sdk's worden nog niet ondersteund, zoals versleuteling aan de client zijde.

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0-Preview
* Voeg ondersteuning toe voor belangrijkste ruwe, batch-en query bewerkingen voor Azure Storage tabel-eind punten. 
   > [!NOTE]
   > Sommige functies in eerdere Azure Storage tabel-Sdk's worden nog niet ondersteund, zoals versleuteling aan de client zijde.

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1 tot en-preview
* Azure Cosmos DB tabel .NET Standard SDK is een platformoverschrijdende .NET-bibliotheek die efficiënte toegang biedt tot het tabel gegevens model op Cosmos DB. Deze eerste release biedt ondersteuning voor de volledige set van tabel-en entiteits functies, met vergelijk bare Api's als de [Cosmos db table SDK voor .NET Framework](table-sdk-dotnet.md). 
   > [!NOTE]
   >  Azure Storage Table-eind punten worden nog niet ondersteund in de 0.9.1 tot en-preview-versie.

## <a name="release-and-retirement-dates"></a>Release-en pensioen datums
Micro soft biedt een kennisgeving van ten minste **twaalf maanden** voor het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie te versoepelen.

In deze platformoverschrijdende .NET Standard-bibliotheek [micro soft. Azure. Cosmos. table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) wordt de .NET Framework bibliotheek [micro soft. Azure. CosmosDB. table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table)vervangen.

| Version | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.0.4-preview](#1.0.4-preview) |26 juli 2019 |--- |
| 1.0.2-voorbeeld |2 mei 2019 |--- |
| [1.0.1](#1.0.1) |19 april 2019 |--- |
| [1.0.0](#1.0.0) |13 maart 2019 |--- |
| [0.11.0-preview](#0.11.0-preview) |5 maart 2019 |--- |
| [0.10.1-preview](#0.10.1-preview) |22 januari 2019 |--- |
| [0.10.0-preview](#0.10.0-preview) |18 december 2018 |--- |
| [0.9.1 tot en-preview](#0.9.1-preview) |18 oktober 2018 |--- |


## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie [Inleiding tot Azure Cosmos DB Table-API](table-introduction.md)voor meer informatie over de Azure Cosmos db table-API. 
