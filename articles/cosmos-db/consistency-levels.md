---
title: Consistentieniveaus in Azure Cosmos DB
description: Azure Cosmos DB biedt vijf consistentieniveaus te verdelen over uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: f9de37c04e5e791445659de0ab667b51f44a4024
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839823"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Consistentieniveaus in Azure Cosmos DB

Gedistribueerde databases die afhankelijk van de replicatie voor hoge beschikbaarheid, lage latentie, of beide zijn, moeten het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commercieel verkrijgbaar gedistribueerde databases stellen ontwikkelaars om te kiezen tussen de twee extreme consistentiemodellen: *sterke* consistentie en *uiteindelijke* consistentie. De  [verwerkingen](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) of het model sterke consistentie is de standaard gold van gegevens programmeren. Maar een prijs van hogere latentie (in onveranderlijke) toegevoegd en verminderde beschikbaarheid (bij storingen). Aan de andere kant uiteindelijke consistentie biedt een hogere beschikbaarheid en betere prestaties, maar is het moeilijk het programmeren van toepassingen. 

Azure Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van twee extreme. Sterke consistentie en uiteindelijke consistentie zijn aan het einde van het spectrum, maar er zijn veel keuzes in consistentie langs het spectrum krijgen. Ontwikkelaars kunnen deze opties gebruiken om nauwkeurige keuzes en gedetailleerde compromissen met betrekking tot hoge beschikbaarheid en prestaties te maken. 

Met Azure Cosmos DB kunnen ontwikkelaars kiezen uit vijf duidelijk gedefinieerde consistentiemodellen op het spectrum consistentie. Van sterk naar meer soepele, de modellen zijn *sterke*, *gebonden veroudering*, *sessie*, *consistent voorvoegsel*, en *uiteindelijke* consistentie. De modellen zijn goed gedefinieerde en intuïtieve en kunnen worden gebruikt voor specifieke praktijkscenario's. Elk model biedt [zorgen voor beschikbaarheid en prestaties van een balans](consistency-levels-tradeoffs.md) en wordt ondersteund door de SLA's. De volgende afbeelding ziet u de verschillende consistentieniveaus als een breed spectrum aan mogelijkheden.

![Consistentie als een breed spectrum aan mogelijkheden](./media/consistency-levels/five-consistency-levels.png)

De consistentieniveaus regioneutrale zijn en worden gegarandeerd voor alle bewerkingen, ongeacht de regio van waaruit de lees- en schrijfbewerkingen plaatsvindt, het aantal regio's die zijn gekoppeld aan uw Azure Cosmos-account, of of uw account is geconfigureerd met één of meerdere regio's voor schrijven.

## <a name="scope-of-the-read-consistency"></a>Bereik van het lezen van consistentie

Lezen van consistentie van toepassing op een enkele leesbewerking binnen het bereik van binnen het bereik van een partitiesleutel of een logische partitie. De leesbewerking kan worden uitgegeven door een externe client of een opgeslagen procedure.

## <a name="configure-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren

U kunt het standaardconsistentieniveau configureren op uw Azure Cosmos-account op elk gewenst moment. Het standaardconsistentieniveau geconfigureerd in uw account is van toepassing op alle Azure-Cosmos-databases en containers onder dat account. Alle leesbewerkingen en query's die zijn uitgegeven voor een container of een database wordt de opgegeven consistentieniveau standaard gebruikt. Zie voor meer informatie over het [het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garanties die zijn gekoppeld aan consistentieniveaus

De uitgebreide Sla's geleverd door Azure Cosmos DB gegarandeerd dat 100 procent van de leesaanvragen voldoet aan de consistentiegarantie voor elk consistentieniveau die u kiest. Een leesaanvraag voldoet aan de SLA van de consistentie als alle de consistentiegarantie die zijn gekoppeld aan het consistentieniveau is voldaan. De precieze definities van de vijf consistentieniveaus in Azure Cosmos DB met behulp van de [TLA + specificatietaal](https://lamport.azurewebsites.net/tla/tla.html) vindt u in de [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) GitHub-opslagplaats. 

De semantiek van de vijf consistentieniveaus worden hier beschreven:

- **Sterke**: Sterke consistentie biedt een [verwerkingen](https://aphyr.com/posts/313-strong-consistency-models) garanderen. De leesbewerkingen gegarandeerd de meest recente doorgevoerde versie van een item geretourneerd. Een client ziet nooit het terugschrijven van een niet-doorgevoerde of gedeeltelijke. Gebruikers zijn altijd gegarandeerd de meest recente toegezegde schrijven.

- **Gebonden veroudering**: De leesbewerkingen gegarandeerd de garantie consistent voorvoegsel in acht neemt. De leesbewerkingen kunnen volgen op schrijfbewerkingen met maximaal *'K'* versies (dat wil zeggen, "updates") van een item of door *"T"* tijdsinterval. Met andere woorden, als u gebonden veroudering kiest, kan de "veroudering' kan worden geconfigureerd op twee manieren: 

  * Het aantal versies (*K*) van het item
  * Het tijdsinterval (*T*) op waarop de leesbewerkingen schrijfbewerkingen achterblijven mogelijk 

  Gebonden veroudering aanbiedingen totale globale volgorde, behalve binnen de "veroudering venster." De monotone lezen garanties bestaan binnen een regio, zowel binnen als buiten het venster veroudering. Sterke consistentie is dezelfde semantiek als het account dat wordt aangeboden door gebonden veroudering. Het venster veroudering is gelijk aan nul. Gebonden veroudering wordt ook wel tijd uitgesteld verwerkingen genoemd. Wanneer een client leesbewerkingen binnen een regio die schrijfbewerkingen accepteert uitvoert, zijn de garanties geboden door consistentie voor gebonden veroudering identiek zijn aan de garanties door de sterke consistentie.

- **Sessie**:  In een sessie voor één client bij leesbewerkingen gegarandeerd de consistent voorvoegsel (uitgaande van een sessie voor één 'auteur'), monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes en write-follows-reads garanties in acht neemt. Clients buiten de sessie schrijfbewerkingen uitvoeren ziet uiteindelijke consistentie.

- **Consistent voorvoegsel**: Updates die worden geretourneerd, een prefix van alle updates, zonder hiaten bevatten. Consistent voorvoegsel consistentieniveau zorgt ervoor dat leesbewerkingen nooit out volgorde schrijfbewerkingen te zien.

- **Uiteindelijke**: Er is geen bestellen garantie voor leesbewerkingen. De replica's worden in de afwezigheid van geen schrijfbewerkingen meer kunnen uiteindelijk geconvergeerd.

## <a name="consistency-levels-explained-through-baseball"></a>Consistentieniveaus uitgelegd honkbal

We gaan een baseball game scenario als voorbeeld. Stel dat een reeks schrijfbewerkingen die staan voor de score van een game baseball. De regel inning door inning score wordt beschreven in de [gerepliceerd gegevensconsistentie honkbal](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) papier. Dit spel hypothetische baseball is momenteel in het midden van de zevende inning. Het is de zevende--inning stretch. De bezoekers zich achter met een score van 2 tot en met 5, zoals hieronder weergegeven:

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Wordt uitgevoerd** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Bezoekers** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **startpagina** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Een Azure Cosmos-container bevat de totalen uitvoeren voor de bezoekers en thuis teams. Terwijl het spel uitgevoerd wordt, lezen verschillende garanties kunnen leiden tot clients verschillende scores lezen. De volgende tabel bevat de volledige set van scores die door het lezen van de bezoekers en thuis scores met elk van de vijf consistentiegarantie kan worden geretourneerd. De bezoekers score wordt eerst weergegeven. Verschillende mogelijke geretourneerde waarden worden gescheiden door komma's.

| **Consistentieniveau** | **Beoordeelt (bezoekers, thuis)** |
| - | - |
| **Sterke** | 2-5 |
| **Gebonden veroudering** | Scores die maximaal één inning verouderd: 2-3, 2-4, 2-5 |
| **Sessie** | <ul><li>Voor de schrijver: 2-5</li><li> Voor iedereen behalve de writer: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5</li><li>Na het lezen van 1-3: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Consistent voorvoegsel** | 0-0, 0-1, 1-1, 1-2, 1-3, 2 en 3, 2-4, 2-5 |
| **Uiteindelijke** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5 |

## <a name="additional-reading"></a>Meer lezen

Lees voor meer informatie over concepten van de consistentie van de volgende artikelen:

- [Op hoog niveau TLA + specificaties voor de vijf consistentieniveaus die worden aangeboden door Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (video) door Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (technisch document) door Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Sessie-garanties voor zwak consistente gerepliceerde gegevens](https://dl.acm.org/citation.cfm?id=383631)
- [Optimalisatie van de consistentie in moderne gedistribueerde systemen ontwerpen van databases: LIMIET is slechts een deel van het artikel](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Uiteindelijk Consistent - herzien](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen voor meer informatie over consistentieniveaus in Azure Cosmos DB:

* [Kies de juiste consistentieniveau voor uw toepassing](consistency-levels-choosing.md)
* [Consistentieniveaus in Azure Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Het standaardconsistentieniveau overschrijven](how-to-manage-consistency.md#override-the-default-consistency-level)

