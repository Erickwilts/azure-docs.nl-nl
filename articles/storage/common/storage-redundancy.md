---
title: De gegevensredundantie
titleSuffix: Azure Storage
description: Gegevens in uw Microsoft Azure Storage-account worden gerepliceerd voor duurzaamheid en maximale Beschik baarheid. Redundantie configuraties zijn lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografisch redundante opslag (GRS), geografisch redundante opslag met lees toegang (RA-GRS), geo-zone-redundante opslag (GZRS) en geo-zone-redundante opslag met lees toegang (RA-GZRS).
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 903560f5c0400a906918f0c17eafb2e1e09bdd30
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518501"
---
# <a name="azure-storage-redundancy"></a>Azure Storage redundantie

Azure Storage slaat altijd meerdere kopieën van uw gegevens op, zodat deze beschermd zijn tegen geplande en niet-geplande gebeurtenissen, waaronder tijdelijke hardwarestoringen, netwerk-of energie storingen en enorme natuur rampen. Redundantie zorgt ervoor dat uw opslag account voldoet aan de [Service Level Agreement (Sla) voor Azure Storage,](https://azure.microsoft.com/support/legal/sla/storage/) zelfs in het geval van fouten.

Wanneer u bepaalt welke optie voor redundantie het beste is voor uw scenario, kunt u rekening houden met de compromissen met lagere kosten en een hogere Beschik baarheid en duurzaamheid. De factoren waarmee u de gewenste redundantie optie kunt bepalen, zijn onder andere:  

- Hoe uw gegevens worden gerepliceerd in de primaire regio
- Of uw gegevens worden gerepliceerd naar een tweede locatie die geografisch onbereikbaar is naar de primaire regio, ter bescherming tegen regionale rampen
- Of voor uw toepassing Lees toegang is vereist voor de gerepliceerde gegevens in de secundaire regio als de primaire regio om een of andere reden niet beschikbaar is

## <a name="redundancy-in-the-primary-region"></a>Redundantie in de primaire regio

Gegevens in een Azure Storage-account worden altijd drie keer gerepliceerd in de primaire regio. Azure Storage biedt twee opties voor het repliceren van uw gegevens in de primaire regio:

- **Lokaal redundante opslag (LRS)** kopieert uw gegevens synchroon drie keer binnen één fysieke locatie in de primaire regio. LRS is de minst dure replicatie optie, maar wordt niet aanbevolen voor toepassingen die hoge Beschik baarheid vereisen.
- **Zone-redundante opslag (ZRS)** kopieert uw gegevens synchroon over drie Azure-beschikbaarheids zones in de primaire regio. Voor toepassingen die hoge Beschik baarheid vereisen, raadt micro soft aan ZRS te gebruiken in de primaire regio en ook te repliceren naar een secundaire regio.

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag

Met lokaal redundante opslag (LRS) worden uw gegevens drie keer gerepliceerd binnen één fysieke locatie in de primaire regio. LRS biedt ten minste 99,999999999% (11 Nines) duurzaamheid van objecten in een bepaald jaar.

LRS is de optie voor de laagste kosten redundantie en biedt de minste duurzaamheid ten opzichte van andere opties. LRS beschermt uw gegevens tegen server rack-en schijf storingen. Als er echter sprake is van een ramp, zoals brand of Flooding in het Data Center, zijn alle replica's van een opslag account met LRS mogelijk verloren of onherstelbaar. Om dit risico te beperken, raadt micro soft aan gebruik te maken van [zone-redundante opslag](#zone-redundant-storage) (ZRS), [geografisch redundante](#geo-redundant-storage) opslag (GRS) of [geo-zone-redundante opslag](#geo-zone-redundant-storage) (GZRS).

Een schrijf aanvraag naar een opslag account dat LRS gebruikt, gebeurt synchroon. De schrijf bewerking wordt alleen geretourneerd nadat de gegevens naar alle drie de replica's zijn geschreven.

LRS is een goede keuze voor de volgende scenario's:

- Als uw toepassing gegevens opslaat die gemakkelijk kunnen worden geconstrueerd als er gegevens verlies optreedt, kunt u kiezen voor LRS.
- Als uw toepassing is beperkt tot het repliceren van gegevens binnen een land of regio als gevolg van vereisten voor gegevens beheer, kunt u kiezen voor LRS. In sommige gevallen kunnen de gekoppelde regio's waarvan de gegevens geografisch worden gerepliceerd, zich in een ander land of andere regio bevinden. Zie [Azure-regio's](https://azure.microsoft.com/regions/)voor meer informatie over gekoppelde regio's.

### <a name="zone-redundant-storage"></a>Zone-redundante opslag

Zone-redundante opslag (ZRS) repliceert uw Azure Storage gegevens synchroon over drie Azure-beschikbaarheids zones in de primaire regio. Elke beschikbaarheids zone is een afzonderlijke fysieke locatie met onafhankelijke voeding, koeling en netwerken. ZRS biedt duurzaamheid voor Azure Storage gegevens objecten van ten minste 99,9999999999% (12 9) gedurende een bepaald jaar.

Met ZRS zijn uw gegevens nog steeds toegankelijk voor zowel lees-als schrijf bewerkingen, zelfs als een zone niet meer beschikbaar is. Als een zone niet meer beschikbaar is, onderneemt Azure netwerk updates, zoals het opnieuw aanwijzen van DNS. Deze updates kunnen van invloed zijn op uw toepassing als u gegevens opent voordat de updates zijn voltooid. Bij het ontwerpen van toepassingen voor ZRS volgt u de procedures voor het afhandelen van tijdelijke fouten, waaronder het implementeren van beleid voor opnieuw proberen met exponentiële back-ups.

Een schrijf aanvraag naar een opslag account dat ZRS gebruikt, gebeurt synchroon. De schrijf bewerking wordt alleen geretourneerd nadat de gegevens zijn geschreven naar alle replica's in de drie beschikbaarheids zones.

Micro soft raadt aan om ZRS te gebruiken in de primaire regio voor scenario's die consistentie, duurzaamheid en hoge Beschik baarheid vereisen. ZRS biedt uitstekende prestaties, lage latentie en tolerantie voor uw gegevens als deze tijdelijk niet beschikbaar is. Het is echter mogelijk dat ZRS uw gegevens niet beveiligt tegen een regionale ramp waarbij meerdere zones permanent worden beïnvloed. Voor beveiliging tegen regionale nood gevallen raadt micro soft aan gebruik te maken van [geo-zone-redundante opslag](#geo-zone-redundant-storage) (GZRS), die gebruikmaakt van ZRS in de primaire regio en dat uw gegevens in geo-replicatie naar een secundaire regio worden gerepliceerd.

In de volgende tabel ziet u welke typen opslag accounts ZRS ondersteunen in welke regio's:

|    Type opslagaccount    |    Ondersteunde regio’s    |    Ondersteunde services    |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
|    Algemeen gebruik v2<sup>1</sup>    | Azië - zuidoost<br /> Australië - oost<br /> Europa - noord<br />  Europa - west<br /> Frankrijk - centraal<br /> Japan - oost<br /> Zuid-Afrika - noord<br /> Verenigd Koninkrijk Zuid<br /> US - centraal<br /> US - oost<br /> US - oost 2<br /> US - west 2    |    Blok-blobs<br /> Pagina-blobs<sup>2</sup><br /> Bestands shares (standaard)<br /> Tabellen<br /> Wachtrijen<br /> |
|    BlockBlobStorage<sup>1</sup>    | Azië - zuidoost<br /> Australië - oost<br /> Europa - west<br /> US - oost    |    Alleen blok-blobs    |
|    FileStorage    | Azië - zuidoost<br /> Australië - oost<br /> Europa - west<br /> US - oost    |    Alleen Azure Files    |

<sup>1</sup> de Archive-laag wordt momenteel niet ondersteund voor ZRS-accounts.<br />
<sup>2</sup> opslag accounts die Azure Managed disks voor virtuele machines bevatten, gebruiken altijd LRS. Onbeheerde schijven van Azure moeten ook LRS gebruiken. Het is mogelijk om een opslag account te maken voor Azure unmanaged disks die gebruikmaken van GRS, maar dit wordt niet aanbevolen vanwege mogelijke problemen met de consistentie van de asynchrone geo-replicatie. Geen van de beheerde schijven of niet-Managed disks ondersteunen ZRS of GZRS. Zie [prijzen voor Azure Managed disks](https://azure.microsoft.com/pricing/details/managed-disks/)(Engelstalig) voor meer informatie over Managed disks.

Voor informatie over welke regio's ZRS ondersteunen, Zie **Services ondersteuning per regio** in [Wat zijn Azure-beschikbaarheidszones?](../../availability-zones/az-overview.md).

## <a name="redundancy-in-a-secondary-region"></a>Redundantie in een secundaire regio

Voor toepassingen die hoge Beschik baarheid vereisen, kunt u de gegevens in uw opslag account ook kopiëren naar een secundaire regio die honderden kilo meters uit de primaire regio bestaat. Als uw opslag account wordt gekopieerd naar een secundaire regio, zijn uw gegevens duurzaam, zelfs in het geval van een volledige regionale onderbreking of een ramp waarbij de primaire regio niet kan worden hersteld.

Wanneer u een opslag account maakt, selecteert u de primaire regio voor het account. De gekoppelde secundaire regio wordt bepaald op basis van de primaire regio en kan niet worden gewijzigd. Zie [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/)voor meer informatie over regio's die door Azure worden ondersteund.

Azure Storage biedt twee opties voor het kopiëren van uw gegevens naar een secundaire regio:

- **Geografisch redundante opslag (GRS)** kopieert uw gegevens synchroon drie keer binnen één fysieke locatie in de primaire regio met behulp van LRS. Vervolgens worden uw gegevens asynchroon gekopieerd naar één fysieke locatie in de secundaire regio.
- **Geo-zone-redundante opslag (GZRS)** kopieert uw gegevens synchroon over drie Azure-beschikbaarheids zones in de primaire regio met behulp van ZRS. Vervolgens worden uw gegevens asynchroon gekopieerd naar één fysieke locatie in de secundaire regio.

Het belangrijkste verschil tussen GRS en GZRS is hoe gegevens worden gerepliceerd in de primaire regio. Binnen de secundaire locatie worden gegevens altijd synchroon drie keer gerepliceerd met behulp van LRS. LRS in de secundaire regio beschermt uw gegevens tegen hardwarestoringen.

Met GRS of GZRS zijn de gegevens op de secundaire locatie niet beschikbaar voor lees-of schrijf toegang, tenzij er een failover naar de secundaire regio wordt gemaakt. Configureer voor lees toegang tot de secundaire locatie uw opslag account voor het gebruik van geografisch redundante opslag met lees toegang (RA-GRS) of geo-zone-redundante opslag met lees toegang (RA-GZRS). Zie [Lees toegang tot gegevens in de secundaire regio](#read-access-to-data-in-the-secondary-region)voor meer informatie.

Als de primaire regio niet beschikbaar is, kunt u een failover uitvoeren naar de secundaire regio. Nadat de failover is voltooid, wordt de secundaire regio de primaire regio en kunt u opnieuw gegevens lezen en schrijven. Zie [nood herstel en failover van het opslag account](storage-disaster-recovery-guidance.md)voor meer informatie over herstel na nood gevallen en voor informatie over het uitvoeren van een failover naar de secundaire regio.

> [!IMPORTANT]
> Omdat gegevens asynchroon naar de secundaire regio worden gerepliceerd, kan een storing die van invloed is op de primaire regio, leiden tot gegevens verlies als de primaire regio niet kan worden hersteld. Het interval tussen de meest recente schrijf bewerkingen naar de primaire regio en de laatste schrijf bewerking naar de secundaire regio wordt de Recovery Point Objective (RPO) genoemd. De RPO geeft het tijdstip aan waarop gegevens kunnen worden hersteld. Azure Storage heeft doorgaans een RPO van minder dan 15 minuten, maar er is momenteel geen SLA over hoe lang het duurt om gegevens naar de secundaire regio te repliceren.

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag

Geografisch redundante opslag (GRS) kopieert uw gegevens synchroon drie keer binnen één fysieke locatie in de primaire regio met behulp van LRS. Vervolgens worden uw gegevens asynchroon gekopieerd naar één fysieke locatie in een secundaire regio die honderden kilo meters van de primaire regio is verwijderd. GRS biedt duurzaamheid voor Azure Storage gegevens objecten van ten minste 99.99999999999999% (16 negen) gedurende een bepaald jaar.

Een schrijf bewerking wordt eerst doorgevoerd op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio. Wanneer gegevens naar de secundaire locatie worden geschreven, worden deze ook gerepliceerd binnen die locatie met behulp van LRS.

### <a name="geo-zone-redundant-storage"></a>Geografisch zone-redundante opslag

Geo-zone-redundante opslag (GZRS) is een combi natie van de hoge Beschik baarheid die wordt verschaft door redundantie in verschillende beschikbaarheids zones met beveiliging tegen regionale storingen die worden verschaft door Geo-replicatie. Gegevens in een GZRS-opslag account worden gekopieerd over drie [Azure-beschikbaarheids zones](../../availability-zones/az-overview.md) in de primaire regio en worden ook gerepliceerd naar een secundaire geografische regio voor beveiliging tegen regionale rampen. Micro soft raadt aan om GZRS te gebruiken voor toepassingen die maximale consistentie, duurzaamheid en beschik baarheid, uitstekende prestaties en flexibiliteit voor herstel na nood gevallen vereisen.

Met een GZRS-opslag account kunt u door gaan met het lezen en schrijven van gegevens als een beschikbaarheids zone niet meer beschikbaar is of niet kan worden hersteld. Daarnaast zijn uw gegevens ook duurzaam in het geval van een volledige regionale onderbreking of een nood situatie waarin de primaire regio niet kan worden hersteld. GZRS is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten in een bepaald jaar te bieden.

Alleen voor algemeen gebruik v2-opslag accounts bieden ondersteuning voor GZRS en RA-GZRS. Zie [Overzicht van Azure-opslagaccounts](storage-account-overview.md) voor meer informatie over de typen opslagaccounts. GZRS en RA-GZRS ondersteunen blok-blobs, pagina-blobs (met uitzonde ring van VHD-schijven), bestanden, tabellen en wacht rijen.

GZRS en RA-GZRS worden ondersteund in de volgende regio's:

- Azië - zuidoost
- Europa - noord
- Europa - west
- Japan - oost
- Verenigd Koninkrijk Zuid
- US - centraal
- US - oost
- US - oost 2
- US - west 2

Zie de prijs informatie voor [blobs](https://azure.microsoft.com/pricing/details/storage/blobs), [bestanden](https://azure.microsoft.com/pricing/details/storage/files/), [wacht rijen](https://azure.microsoft.com/pricing/details/storage/queues/)en [tabellen](https://azure.microsoft.com/pricing/details/storage/tables/)voor meer informatie over prijzen.

## <a name="read-access-to-data-in-the-secondary-region"></a>Lees toegang tot de gegevens in de secundaire regio

Geografisch redundante opslag (met GRS of GZRS) repliceert uw gegevens naar een andere fysieke locatie in de secundaire regio om te beschermen tegen regionale storingen. Deze gegevens zijn echter alleen beschikbaar als alleen-lezen als de klant of micro soft een failover initieert van de primaire naar de secundaire regio. Wanneer u lees toegang tot de secundaire regio inschakelt, zijn uw gegevens te allen tijde beschikbaar om te worden gelezen, met inbegrip van een situatie waarin de primaire regio niet beschikbaar is. Voor lees toegang tot de secundaire regio schakelt u geografisch redundante opslag met lees toegang (RA-GRS) of geo-zone-redundante opslag met lees toegang (RA-GZRS) in.

### <a name="design-your-applications-for-read-access-to-the-secondary"></a>Uw toepassingen ontwerpen voor lees toegang tot de secundaire

Als uw opslag account is geconfigureerd voor lees toegang tot de secundaire regio, kunt u uw toepassingen zo ontwerpen dat de gegevens van de secundaire regio naadloos worden gelezen als de primaire regio om welke reden dan ook niet beschikbaar is. 

De secundaire regio is beschikbaar voor lees toegang nadat u RA-GRS of RA-GZRS hebt ingeschakeld, zodat u de toepassing vooraf kunt testen om er zeker van te zijn dat deze in het geval van een storing op de secundaire manier kan worden gelezen. Zie [geo-redundantie gebruiken om Maxi maal beschik bare toepassingen te ontwerpen](geo-redundant-design.md)voor meer informatie over het ontwerpen van uw toepassingen voor maximale Beschik baarheid.

Wanneer lees toegang tot de secundaire is ingeschakeld, kan uw toepassing worden gelezen vanuit het secundaire eind punt en van het primaire eind punt. Het secundaire eind punt voegt het achtervoegsel *(secundair* ) toe aan de account naam. Als uw primaire eind punt voor Blob Storage bijvoorbeeld is `myaccount.blob.core.windows.net` , is het secundaire eind punt `myaccount-secondary.blob.core.windows.net` . De toegangs sleutels voor het account voor uw opslag account zijn hetzelfde voor de primaire en secundaire eind punten.

### <a name="check-the-last-sync-time-property"></a>De eigenschap Laatst gesynchroniseerd controleren

Omdat gegevens asynchroon naar de secundaire regio worden gerepliceerd, is de secundaire regio vaak achter de primaire regio. Als er een fout optreedt in de primaire regio, is het waarschijnlijk dat alle schrijf bewerkingen naar het primaire bestand nog niet zijn gerepliceerd naar de secundaire.

Om te bepalen welke schrijf bewerkingen zijn gerepliceerd naar de secundaire regio, kan de toepassing de **laatste synchronisatie tijd** -eigenschap voor uw opslag account controleren. Alle schrijf bewerkingen die zijn geschreven naar de primaire regio vóór de laatste synchronisatie tijd zijn gerepliceerd naar de secundaire regio, wat betekent dat ze kunnen worden gelezen van de secundaire. Schrijf bewerkingen die worden geschreven naar de primaire regio na de laatste synchronisatie tijd, kunnen al dan niet zijn gerepliceerd naar de secundaire regio, wat inhoudt dat ze mogelijk niet beschikbaar zijn voor lees bewerkingen.

U kunt een query uitvoeren op de waarde van de eigenschap **laatste synchronisatie tijd** met behulp van Azure PowerShell, Azure CLI of een van de Azure Storage-client bibliotheken. De eigenschap **laatste synchronisatie tijd** is een GMT-datum/-tijd waarde. Zie [de eigenschap laatste synchronisatie tijd voor een opslag account controleren](last-sync-time-get.md)voor meer informatie.

## <a name="summary-of-redundancy-options"></a>Overzicht van redundantie opties

De tabellen in de volgende secties bevatten een overzicht van de redundantie opties die beschikbaar zijn voor Azure Storage

### <a name="durability-and-availability-parameters"></a>Duurzaamheid en beschikbaarheids parameters

In de volgende tabel worden de belangrijkste para meters voor elke redundantie optie beschreven:

| Parameter                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Percentage van de duurzaamheid van objecten in een bepaald jaar<sup>1</sup>                                          | ten minste 99,999999999% (11 9) | ten minste 99,9999999999% (12 9) | ten minste 99.99999999999999% (16 9) | ten minste 99.99999999999999% (16 9) |
| SLA voor Beschik baarheid voor lees aanvragen<sup>1</sup>  | Ten minste 99,9% (99% voor de laag van de cool-toegang) | Ten minste 99,9% (99% voor de laag van de cool-toegang) | Ten minste 99,9% (99% voor de laag met coole toegang) voor GRS<br /><br />Ten minste 99,99% (99,9% voor de laag voor cool-toegang) voor RA-GRS | Ten minste 99,9% (99% voor de laag met coole toegang) voor GZRS<br /><br />Ten minste 99,99% (99,9% voor de laag voor cool-toegang) voor RA-GZRS |
| SLA voor Beschik baarheid voor schrijf aanvragen<sup>1</sup>  | Ten minste 99,9% (99% voor de laag van de cool-toegang) | Ten minste 99,9% (99% voor de laag van de cool-toegang) | Ten minste 99,9% (99% voor de laag van de cool-toegang) | Ten minste 99,9% (99% voor de laag van de cool-toegang) |

<sup>1</sup> zie de [Azure Storage Sla](https://azure.microsoft.com/support/legal/sla/storage/)voor informatie over Azure Storage garanties voor duurzaamheid en beschik baarheid.

### <a name="durability-and-availability-by-outage-scenario"></a>Duurzaamheid en beschik baarheid per uitval scenario

In de volgende tabel wordt aangegeven of uw gegevens duurzaam zijn en beschikbaar zijn in een bepaald scenario, afhankelijk van welk type redundantie van toepassing is op uw opslag account:

| Storings scenario                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Een knoop punt in een Data Center wordt niet meer beschikbaar                                                                 | Ja                             | Ja                              | Ja                                  | Ja                                 |
| Een volledig Data Center (zonegebonden of niet-zonegebonden) is niet meer beschikbaar                                           | Nee                              | Ja                              | Ja<sup>1</sup>                                  | Ja                                  |
| Er treedt een storing op de hele regio op in de primaire regio                                                                                     | Nee                              | Nee                               | Ja<sup>1</sup>                                  | Ja<sup>1</sup>                                  |
| Lees toegang tot de secundaire regio is beschikbaar als de primaire regio niet beschikbaar is | Nee                              | Nee                               | Ja (met RA-GRS)                                   | Ja (met RA-GZRS)                                 |

<sup>1</sup> failover van het account is vereist voor het herstellen van schrijf beschikbaarheid als de primaire regio niet beschikbaar is. Zie [nood herstel en failover van het opslag account](storage-disaster-recovery-guidance.md)voor meer informatie.

### <a name="supported-storage-account-types"></a>Ondersteunde typen opslag accounts

In de volgende tabel ziet u welke redundantie opties worden ondersteund door elk type opslag account. Zie [overzicht van opslag accounts](storage-account-overview.md)voor informatie over typen opslag accounts.

| LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS                              |
| :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Algemeen gebruik v2<br /> Algemeen gebruik v1<br /> Blob-opslag blok keren<br /> Blob Storage<br /> File Storage                | Algemeen gebruik v2<br /> Blob-opslag blok keren<br /> File Storage                             | Algemeen gebruik v2<br /> Algemeen gebruik v1<br /> Blob Storage                     | Algemeen gebruik v2                     |

Alle gegevens voor alle opslag accounts worden gekopieerd op basis van de redundantie optie voor het opslag account. Objecten inclusief blok-blobs, toevoeg-blobs, pagina-blobs, wacht rijen, tabellen en bestanden worden gekopieerd. Gegevens in alle lagen, inclusief de laag Archive, worden gekopieerd. Zie [Azure Blob Storage: warme, cool en archief toegangs lagen](../blobs/storage-blob-storage-tiers.md)voor meer informatie over blob-lagen.

Zie [Azure Storage prijzen](https://azure.microsoft.com/pricing/details/storage/)voor prijs informatie voor elke optie voor redundantie.

> [!NOTE]
> Azure Premium Disk Storage ondersteunt momenteel alleen lokaal redundante opslag (LRS). Blok-Blob Storage-accounts ondersteunen lokaal redundante opslag (LRS) en zone redundante opslag (ZRS) in bepaalde regio's.

## <a name="data-integrity"></a>Gegevensintegriteit

Azure Storage controleert regel matig de integriteit van gegevens die zijn opgeslagen met behulp van cyclische redundantie controles (CRCs). Als beschadigde gegevens worden gedetecteerd, wordt deze hersteld met behulp van redundante gegevens. Azure Storage berekent ook de controle sommen op al het netwerk verkeer om beschadiging van gegevens pakketten te detecteren bij het opslaan of ophalen van gegevens.

## <a name="see-also"></a>Zie tevens

- [De eigenschap van de laatste synchronisatie tijd voor een opslag account controleren](last-sync-time-get.md)
- [De redundantie optie voor een opslag account wijzigen](redundancy-migration.md)
- [Geo-redundantie gebruiken om Maxi maal beschik bare toepassingen te ontwerpen](geo-redundant-design.md)
- [Herstel na nood geval en failover van het opslag account](storage-disaster-recovery-guidance.md)
