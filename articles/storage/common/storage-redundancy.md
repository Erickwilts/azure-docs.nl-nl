---
title: Redundantie van de gegevens in Azure Storage | Microsoft Docs
description: Gegevens in uw Microsoft Azure Storage-account wordt gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Opties voor gegevensredundantie zijn lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geo-redundante opslag met leestoegang (RA-GRS).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/18/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 078c62913b903eafe9e0fcfcef4189f5ca735d0f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002828"
---
# <a name="azure-storage-redundancy"></a>Azure Storage-redundantie

De gegevens in uw Microsoft Azure Storage-account worden altijd gerepliceerd om duurzaamheid en hoge beschikbaarheid te garanderen. Azure Storage worden gekopieerd van uw gegevens zodat deze wordt beschermd tegen geplande en ongeplande gebeurtenissen, met inbegrip van tijdelijke hardwarefouten, het netwerk of stroomstoringen en enorme natuurrampen. U kunt uw gegevens binnen hetzelfde Datacenter repliceren in zonegebonden datacenters binnen dezelfde regio bevinden of geografisch gescheiden regio's.

Replicatie zorgt ervoor dat uw opslagaccount voldoet aan de [Service-Level Agreement (SLA) voor opslag](https://azure.microsoft.com/support/legal/sla/storage/), zelfs wanneer er fouten optreden. Raadpleeg de SLA voor informatie over de garanties van Azure Storage voor duurzaamheid en beschikbaarheid.

Azure Storage controleert regelmatig de integriteit van gegevens die zijn opgeslagen met cyclische redundantie controles (CRC's). Als beschadiging van gegevens wordt gedetecteerd, is deze met behulp van redundante gegevens hersteld. Azure-opslag wordt ook berekend controlesommen op al het netwerkverkeer voor het detecteren van beschadiging van gegevenspakketten wanneer op te slaan of het ophalen van gegevens.

## <a name="choosing-a-redundancy-option"></a>Een optie voor redundantie te kiezen

Wanneer u een opslagaccount maakt, kunt u een van de volgende opties voor redundantie selecteren:

* [Lokaal redundante opslag (LRS)](storage-redundancy-lrs.md)
* [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md)
* [Geografisch redundante opslag (GRS)](storage-redundancy-grs.md)
* [Geografisch redundante opslag met leestoegang (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

De volgende tabel geeft een kort overzicht van het bereik van duurzaamheid en beschikbaarheid die elke replicatiestrategie u voor een bepaald type gebeurtenis (of een gebeurtenis van vergelijkbare impact krijgt).

| Scenario                                                                                                 | LRS                             | ZRS                              | GRS                                  | RA-GRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Knooppunt niet beschikbaar zijn binnen een datacenter                                                                 | Ja                             | Ja                              | Ja                                  | Ja                                  |
| Een heel Datacenter (zonegebonden of niet-zonegebonden) niet beschikbaar                                           | Nee                              | Ja                              | Ja                                  | Ja                                  |
| Een storing in de gehele regio                                                                                     | Nee                              | Nee                               | Ja                                  | Ja                                  |
| Leestoegang tot uw gegevens (in de regio van een externe, geo-replicatie) in het geval van de gehele regio niet beschikbaar zijn | Nee                              | Nee                               | Nee                                   | Ja                                  |
| Die zijn bedoeld voor \_ \_ duurzaamheid van objecten in een bepaald jaar                                          | ten minste 99,999999999% (11 9's) | ten minste 99,9999999999% (12 9's) | ten minste 99,99999999999999% (16 9's) | ten minste 99,99999999999999% (16 9's) |
| Typen ondersteunde opslagaccounts weergegeven                                                                   | GPv2, GPv1, Blob                | GPv2                             | GPv2, GPv1, Blob                     | GPv2, GPv1, Blob                     |
| SLA voor beschikbaarheid voor lezen aanvragen | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,99% (99,9% voor de koude Toegangslaag) |
| Beschikbaarheids-SLA voor schrijfaanvragen voor | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) | Ten minste 99,9% (99% voor de koude toegangslaag) |

Zie voor informatie over prijzen voor elke optie voor redundantie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

Zie voor meer informatie over Azure Storage garanties voor duurzaamheid en beschikbaarheid, de [Azure Storage SLA](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> Premium Storage ondersteunt alleen lokaal redundante opslag (LRS).

## <a name="changing-replication-strategy"></a>Replicatiestrategie wijzigen
U kunt uw storage-account replicatiestrategie wijzigen met behulp van de [Azure-portal](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), of een van de [Azure-client bibliotheken](https://docs.microsoft.com/azure/index#pivot=sdkstools). Wijzigen van het replicatietype van uw opslagaccount leidt niet tot uitvaltijd.

   > [!NOTE]
   > U niet op dit moment de Portal of de API aan uw account converteren naar ZRS gebruiken. Als u de replicatie van uw account converteren naar ZRS wilt, Zie [Zone-redundante opslag (ZRS)](storage-redundancy-zrs.md) voor meer informatie.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Zijn er kosten voor het wijzigen van replicatiestrategie van mijn account?
Dat hangt ervan af op het pad voor de conversie. Volgorde van de goedkoopste op de duurste redundantie-aanbieding hebben we LRS-, ZRS, GRS en RA-GRS. Bijvoorbeeld, gaan *van* LRS met alles extra kosten in rekening gebracht, omdat u een meer geavanceerde redundantieniveau, zonereduntante gaat. Gaan *naar* GRS of RA-GRS wordt een uitgaande bandbreedte in rekening gebracht omdat uw gegevens (in de primaire regio) naar uw externe secundaire regio wordt gerepliceerd. Dit is een eenmalige kosten in rekening gebracht tijdens de eerste installatie. Nadat de gegevens worden gekopieerd, zijn er geen verdere kosten voor de conversie. U alleen gefactureerd voor het repliceren van een nieuwe of updates van bestaande gegevens. Zie voor meer informatie over de bandbreedte kosten in rekening gebracht, [prijzen voor Azure Storage-pagina](https://azure.microsoft.com/pricing/details/storage/blobs/).

Als u uw storage-account van GRS naar LRS converteert, er is geen extra kosten, maar uw gerepliceerde gegevens worden verwijderd uit de secundaire locatie.

Als u uw storage-account van RA-GRS geconverteerd naar GRS of LRS, wordt dat account als RA-GRS gefactureerd voor een nog 30 dagen na de datum waarop deze is geconverteerd.

## <a name="see-also"></a>Zie ook

- [Lokaal redundante opslag (LRS): Gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage](storage-redundancy-zrs.md)
- [Geografisch redundante opslag (GRS): Regio-overschrijdend-replicatie voor Azure Storage](storage-redundancy-grs.md)
- [Schaalbaarheids- en prestatiedoelen voor Azure Storage](storage-scalability-targets.md)
- [Het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Storage redundantie opties en leestoegang geografisch redundante opslag](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP-document - Azure-opslag: Een maximaal beschikbare cloudopslagservice met sterke consistentie](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
