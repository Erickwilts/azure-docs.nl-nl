---
title: Gegevens globaal distribueren met Azure Cosmos DB
description: Meer informatie over wereldwijde geo-replicatie, meerdere masters, failover en gegevens herstellen met behulp van de globale databases van Azure Cosmos DB, een wereldwijd gedistribueerde, multi-model databaseservice.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: e58a8cd286e4d416dd5f4e6d3fddedf1897fed1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65954170"
---
# <a name="global-data-distribution-with-azure-cosmos-db---overview"></a>Globale gegevensdistributie met Azure Cosmos DB - overzicht

Van toepassingen wordt tegenwoordig vereist dat ze zeer responsief en altijd online zijn. Voor het bereiken van lage latentie en hoge beschikbaarheid moeten de instanties van deze toepassingen worden geïmplementeerd in datacenters die zich dicht bij de gebruikers ervan bevinden. Deze toepassingen worden doorgaans geïmplementeerd in meerdere datacenters en wereldwijd gedistribueerde worden genoemd. Wereldwijd gedistribueerde toepassingen moeten een globaal gedistribueerde database die de gegevens overal ter wereld om in te schakelen van de toepassingen die moeten worden uitgevoerd op een kopie van de gegevens die zich dicht bij de gebruikers daarvan transparant kunt repliceren. 

Azure Cosmos DB is een wereldwijd gedistribueerde databaseservice die is ontwikkeld voor lage latentie, elastische schaalbaarheid van doorvoer, goed gedefinieerde semantiek voor de consistentie van gegevens en hoge beschikbaarheid. Kort gezegd, als uw toepassing nodig gegarandeerde snelle reactietijd overal ter wereld, heeft als dit is vereist om altijd online en het moet onbeperkte en flexibele schaalbaarheid van de doorvoer en opslag, moet u uw toepassing op Azure Cosmos DB bouwen.

U kunt uw databases zijn wereldwijd gedistribueerde en beschikbaar zijn in een van de Azure-regio's configureren. Als u wilt de latentie verlagen, moet u de gegevens dicht bij waar uw gebruikers zijn geplaatst. De vereiste regio's te kiezen, is afhankelijk van het wereldwijde bereik van uw toepassing en waar uw gebruikers zich bevinden. Cosmos DB repliceert de gegevens transparant naar alle regio's die zijn gekoppeld aan uw Cosmos-account. Het biedt één integraal beeld van de wereldwijd gedistribueerde Azure Cosmos-database en de containers die uw toepassing kunt lezen en schrijven naar lokaal. 

U kunt met Azure Cosmos DB, toevoegen of verwijderen van de regio's die zijn gekoppeld aan uw account op elk gewenst moment. Uw toepassing hoeft niet te worden onderbroken of opnieuw te worden geïnstalleerd als u een regio wilt toevoegen of verwijderen. Worden maximaal beschikbare voortdurend vanwege de multi-homingmogelijkheden die de service systeemeigen biedt blijft.

![Maximaal beschikbare implementatietopologie](./media/distribute-data-globally/deployment-topology.png)

## <a name="key-benefits-of-global-distribution"></a>Belangrijkste voordelen van wereldwijde distributie

**Bouw wereldwijd actief / actief-apps.** Met de replicatie van informatiestructuren meerdere masters-protocol biedt ondersteuning voor elke regio schrijfbewerkingen en leesbewerkingen. Er kunnen ook de mogelijkheid meerdere masters:

- Onbeperkt elastisch schrijven en lezen van de schaalbaarheid. 
- 99,999% lezen en schrijven van de beschikbaarheid van de hele wereld.
- Gegarandeerde schrijf- en leesbewerkingen geleverd in minder dan 10 milliseconden in het 99e percentiel.

Met behulp van de Azure Cosmos DB-multihoming-API's, uw toepassing is op de hoogte van de dichtstbijzijnde regio en aanvragen kunt verzenden naar deze regio. De dichtstbijzijnde regio wordt zonder eventuele wijzigingen in de configuratie geïdentificeerd. Het toevoegen en verwijderen van regio's en naar uw Azure Cosmos-account, uw toepassing hoeft niet te worden geïmplementeerd of is onderbroken, blijft te allen tijde maximaal beschikbaar zijn.

**Bouw uiterst snel reagerende apps.** Uw toepassing kunt uitvoeren in de buurt van realtime leest en schrijfbewerkingen ten opzichte van de regio's die u hebt gekozen voor uw database. Azure Cosmos DB wordt intern verwerkt de gegevensreplicatie tussen regio's met garanties voor consistentie niveau van het niveau dat u hebt geselecteerd.

**Maak maximaal beschikbare apps.** Een database in meerdere regio's over de hele wereld uitgevoerd verhoogt de beschikbaarheid van een database. Als één regio niet beschikbaar is, wordt in andere regio's automatisch aanvragen verwerken. Azure Cosmos DB biedt voor 99,999% lezen en schrijven van beschikbaarheid voor databases van meerdere regio's.

**Zakelijke continuïteit onderhouden tijdens een regionale onderbreking.** Azure Cosmos DB ondersteunt [automatische failover](how-to-manage-database-account.md#automatic-failover) tijdens een regionale onderbreking. Tijdens een regionale storing blijft Azure Cosmos DB onderhouden van de SLA's voor latentie, beschikbaarheid, consistentie en doorvoer. Om ervoor te zorgen dat de gehele toepassing maximaal beschikbaar is, biedt Cosmos DB een handmatige failover API voor het simuleren van een regionale onderbreking. Met behulp van deze API kunt u normale zakelijke continuïteit oefeningen uitvoeren.

**Schaal lezen en schrijven doorvoer wereldwijd.** U kunt elke regio schrijfbaar en lees- en schrijfbewerkingen flexibel schalen over de hele wereld kunt inschakelen. De doorvoer die uw toepassing wordt geconfigureerd op een Azure Cosmos-database of een container kan worden gegarandeerd moet worden geleverd in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. De ingerichte doorvoer van wordt gegarandeerd door [met financiële Sla's ondersteund](https://aka.ms/acdbsla).

**Kies uit meerdere duidelijk gedefinieerde consistentiemodellen.** Het protocol van de replicatie Azure Cosmos DB biedt vijf duidelijk omschreven, praktische en intuïtieve consistentiemodellen. Elk model heeft een balans tussen consistentie en prestaties. Gebruik deze consistentiemodellen om wereldwijd gedistribueerde toepassingen met gemak.

## <a id="Next Steps"></a>Volgende stappen

Meer informatie over globale distributie in de volgende artikelen:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Meerdere masters in uw toepassingen configureren](how-to-multi-master.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Toevoegen of verwijderen van regio's van uw Azure Cosmos DB-account](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)