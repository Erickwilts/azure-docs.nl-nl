---
title: Optimaliseer de kosten voor implementaties met meerdere regio's in Azure Cosmos DB
description: In dit artikel wordt uitgelegd hoe u de kosten van implementaties met meerdere regio's in Azure Cosmos DB kunt beheren.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: rimman
ms.openlocfilehash: 233eab1fc49d7ce4cbb1e5b98b67eda9a64aa195
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667597"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>De kosten voor meerdere regio's optimaliseren in Azure Cosmos DB

U kunt op elk gewenst moment regio's toevoegen aan en verwijderen uit uw Azure Cosmos-account. De door u geconfigureerde door Voer voor verschillende Azure Cosmos-data bases en containers is gereserveerd in elke regio die aan uw account is gekoppeld. Als de door Voer is ingericht per uur, is dat de som van ru/s die is geconfigureerd voor alle data bases en containers voor uw `T` Azure Cosmos-account, en het aantal Azure-regio's dat `N`aan uw database account is gekoppeld, en vervolgens het totaal de ingerichte door Voer voor uw Cosmos-account voor een gegeven uur is gelijk aan:

1. `T x N RU/s`Als uw Azure Cosmos-account is geconfigureerd met één schrijf regio. 

1. `T x (N+1) RU/s`Als uw Azure Cosmos-account is geconfigureerd met alle regio's die schrijf bewerkingen kunnen verwerken. 

Ingerichte door Voer met enkele schrijf regio kosten $0.008/uur per 100 RU/s en ingerichte door Voer met meerdere Beschrijf bare regio's kosten $0,016/per uur per 100 RU/s. Zie Azure Cosmos DB- [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)voor meer informatie.

## <a name="costs-for-multiple-write-regions"></a>Kosten voor meerdere schrijf regio's

In een systeem met meerdere masters neemt het net beschik bare RUs voor schrijf `N` bewerkingen tijden `N` toe, waarbij het aantal schrijf regio's is. In tegens telling tot de schrijf bewerkingen van één regio is elke regio nu schrijfbaar en moet er conflicten worden opgelost. De hoeveelheid werk belasting voor schrijvers is toegenomen. Van het kosten plannings punt van de weer gave `M` , voor het wereld wijd uitvoeren van ru/s voor schrijf bewerkingen, `RUs` moet u M inrichten op een container-of database niveau. U kunt vervolgens zoveel regio's toevoegen als u wilt en deze gebruiken voor schrijf bewerkingen voor het uitvoeren `M` van de wereld wijde schrijf bewerkingen van ru. 

### <a name="example"></a>Voorbeeld

Stel dat u een container hebt die is ingericht met een doorvoer snelheid van 10.000 RU/s en dat deze maand 1 TB aan gegevens opslaat. Stel dat u drie regio's, VS-Oost, Europa-noord en Azië-oost toevoegt, elk met dezelfde opslag en door Voer en u de mogelijkheid wilt geven om naar de containers in alle vier regio's te schrijven vanuit uw wereld wijd gedistribueerde app. Uw totale maandelijkse factuur (aangenomen 31 dagen) in een maand is als volgt:

|**Item**|**Gebruik (maandelijks)**|**Malen**|**Maandelijkse kosten**|
|----|----|----|----|
|Doorvoer factuur voor container in West-VS (meerdere schrijf regio's) |10.000 RU/s * 24 * 31 |$0,016 per 100 RU/s per uur |$1.190,40 |
|Doorvoer factuur voor drie extra regio's: VS-Oost, Europa-noord en Azië-oost (meerdere schrijf regio's) |(3 + 1) * 10.000 RU/s * 24 * 31 |$0,016 per 100 RU/s per uur |$4.761,60 |
|Opslagfactuur voor container in US - west |1 TB (of 1.024 GB) |$0,25/GB |$256 |
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost |3 * 1 TB (of 3.072 GB) |$0,25/GB |$768 |
|**Eind**|||**$6.976** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Het doorvoer gebruik per regio verbeteren

Als u een inefficiënt gebruik hebt, bijvoorbeeld een of meer ondergeschikte of meer gebruikte regio's, kunt u de volgende stappen uitvoeren om het doorvoer gebruik te verbeteren:  

1. Zorg ervoor dat u de ingerichte door Voer (RUs) in de schrijf regio eerst optimaliseert en maak het maximum gebruik van het RUs in Lees regio's met behulp van Change feed van het Lees gebied, enzovoort. 

2. Meerdere schrijf regio's Lees-en schrijf bewerkingen kunnen worden geschaald in alle regio's die zijn gekoppeld aan het Azure Cosmos-account. 

3. Bewaak de activiteit in uw regio's en u kunt regio's op aanvraag toevoegen en verwijderen om uw lees-en schrijf doorvoer te schalen.

## <a name="next-steps"></a>Volgende stappen

Daarna kunt u meer te weten komen over cost Optimization in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over het [optimaliseren voor ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [uw Azure Cosmos DB factuur](understand-your-bill.md)
* Meer informatie over het [optimaliseren van doorvoer kosten](optimize-cost-throughput.md)
* Meer informatie over het [optimaliseren van opslag kosten](optimize-cost-storage.md)
* Meer informatie over [het optimaliseren van de kosten van lees-en schrijf bewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [het optimaliseren van de kosten van query's](optimize-cost-queries.md)

