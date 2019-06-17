---
title: Prestaties op Lsv2-serie van Azure virtual machines - opslag te optimaliseren | Microsoft Docs
description: Informatie over het optimaliseren van prestaties voor uw oplossing op de virtuele machines van de Lsv2-serie.
services: virtual-machines-windows
author: laurenhughes
manager: jeconnoc
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: ec6a6ea5d00d43f6dffa3aabc367d94ebb8db198
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075610"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Prestaties op de Lsv2-serie virtuele machines optimaliseren

Lsv2-serie virtuele machines ondersteunen verschillende workloads die hoge i/o en doorvoer voor lokale opslag voor een breed scala van toepassingen en bedrijven nodig hebben.  De Lsv2-serie is ideaal voor Big Data, SQL, NoSQL-databases, gegevensopslag en grote transactionele databases, met inbegrip van Cassandra, MongoDB, Cloudera en Redis.

Het ontwerp van de Lsv2-serie virtuele Machines (VM's) maximaliseert de processor AMD EPYC™ 7551 voor de beste prestaties tussen de processor, geheugen, NVMe-apparaten en de virtuele machines. Naast de Hardwareprestaties maximaliseren, worden de Lsv2-serie VM's ontworpen voor gebruik met de behoeften van Windows en Linux-besturingssystemen voor betere prestaties met de hardware en software.

De geoptimaliseerde versie van de software en hardware afstemmen leidde [Windows Server 2019 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), uitgebracht in begin December 2018 in de Azure Marketplace, die ondersteuning biedt voor maximale prestaties op de NVMe-apparaten in Lsv2-serie Virtuele machines.

Dit artikel vindt u tips en suggesties om te controleren of uw workloads en toepassingen maar liefst de maximale prestaties ontworpen bij de virtuele machines. De informatie op deze pagina wordt steeds bijgewerkt naarmate meer Lsv2 geoptimaliseerd kopieën zijn toegevoegd aan de Azure Marketplace.

## <a name="amd-eypc-chipset-architecture"></a>AMD EYPC™-chipset-architectuur

Lsv2-serie VM's AMD EYPC™ server processors op basis van de microarchitectuur Zen gebruiken. AMD oneindig Fabric (als) die zijn ontwikkeld voor EYPC™ als schaalbare InterConnect vereist is voor de NUMA-model dat kan worden gebruikt voor communicatie op-chip, op pakket- en meerdere pakket. Vergeleken met QPI (pad voor snelle InterConnect vereist is) en UPI (Ultra-Path Interconnect) van Intel-processors voor moderne monolithische die wordt gebruikt, AMD van veel NUMA kleine-die architectuur kan doen om beide prestatievoordelen en uitdagingen. De werkelijke impact van bandbreedte en de latentie geheugenbeperkingen kan variëren afhankelijk van het type van workloads die worden uitgevoerd.

## <a name="tips-for-maximizing-performance"></a>Tips voor prestaties maximaliseren

* De hardware die wordt gebruikt door de Lsv2-serie VM's maakt gebruik van NVMe-apparaten met acht paren voor i/o-wachtrij (QP) s. Elke wachtrij NVMe-i/o-apparaat is eigenlijk een paar: een wachtrij voor verzending en een voltooiingswachtrij. De NVMe-stuurprogramma is ingesteld voor het optimaliseren van het gebruik van deze acht i/o-QPs door te distribueren ik / O van in een round-robin plannen. Voor maximale prestaties krijgen, moet u acht taken per apparaat zodat deze overeenkomen met uitvoeren.

* Voorkom dan dat u NVMe-beheerder opdrachten (bijvoorbeeld NVMe slimme info query, enzovoort) met NVMe i/o-opdrachten tijdens actieve werkbelastingen. Lsv2 NVMe-apparaten worden ondersteund door Hyper-V NVMe Direct-technologie, over te schakelen in 'trage modus' wanneer alle NVMe-admin-opdrachten in behandeling zijn. Lsv2-gebruikers kunnen een aanzienlijke prestaties neerzetten in NVMe i/o-prestaties als dit gebeurt zien.

* Lsv2 gebruikers afhankelijk zijn niet NUMA apparaatgegevens (alle 0) doorgegeven vanuit de virtuele machine voor gegevensstations om te bepalen van de NUMA-affiniteit voor hun apps. De aanbevolen manier voor betere prestaties is Verdeel werkbelastingen indien mogelijk over CPU's. 

* De maximale ondersteunde wachtrijdiepte per combinatie van i/o-wachtrij voor Lsv2 VM NVMe-apparaat is 1024 (vs. Amazon i3 Wachtrijdiepte 32 limiet). Lsv2-gebruikers te beperken van hun (synthetische) benchmarking werklast laten opvangen wachtrijdiepte 1024 of lager om te voorkomen dat het activeren van de wachtrij volledige voorwaarden, waardoor prestaties kunnen verminderen.

## <a name="utilizing-local-nvme-storage"></a>Gebruik van lokale NVMe-opslag

Lokale opslag op de schijf van de NVMe 1,92 TB op alle Lsv2-VM's is tijdelijk. Tijdens een geslaagde standaard opnieuw opstarten van de virtuele machine blijft de gegevens op de lokale NVMe-schijf. De gegevens blijven niet behouden in de NVMe als de virtuele machine is geïmplementeerd, toewijzing ongedaan gemaakt of verwijderd. Gegevens blijven niet behouden als een ander probleem zorgt ervoor dat de virtuele machine of de hardware die deze wordt uitgevoerd, worden niet in orde. Als dit gebeurt, wordt alle gegevens op de oude host veilig gewist.

Ook wordt zijn er gevallen wanneer de virtuele machine moet worden verplaatst naar een andere hostmachine, bijvoorbeeld tijdens gepland onderhoud. Gepland onderhoudsbewerkingen en sommige hardwarestoringen kunnen worden verwacht met [geplande gebeurtenissen](scheduled-events.md). Geplande gebeurtenissen moeten worden gebruikt om te worden bijgewerkt op alle voorspeld onderhoud en recovery-bewerkingen.

In het geval dat een geplande onderhoudsgebeurtenis is vereist voor de virtuele machine opnieuw op een nieuwe host met lege lokale schijven worden gemaakt, moet de gegevens opnieuw worden gesynchroniseerd (nogmaals, met alle gegevens op de oude host wordt een veilige manier gewist). Dit gebeurt omdat Lsv2-serie VM's momenteel geen live migratie op de lokale NVMe-schijf ondersteunen.

Er zijn twee modi voor gepland onderhoud.

### <a name="standard-vm-customer-controlled-maintenance"></a>Standard VM door de klant bewaakte onderhoud

- De virtuele machine wordt verplaatst naar een bijgewerkte host tijdens een periode van 30 dagen.
- Gegevens van de lokale opslag Lsv2 kunnen verloren gaan, zodat back-ups van gegevens voorafgaand aan de gebeurtenis wordt aanbevolen.

### <a name="automatic-maintenance"></a>Automatisch onderhoud

- Treedt op als de klant niet door de klant bewaakte onderhoud wordt uitgevoerd, of in het geval van noodsituaties procedures, zoals een beveiligingsgebeurtenis nul dagen.
- Bedoeld voor het behouden van gegevens van de klant, maar er is een klein risico van een blokkering van de virtuele machine of opnieuw opstarten.
- Gegevens van de lokale opslag Lsv2 kunnen verloren gaan, zodat back-ups van gegevens voorafgaand aan de gebeurtenis wordt aanbevolen.

Gebruik het onderhoudsproces voor beheerde op een moment meest handig is dat u voor de update voor alle toekomstige servicegebeurtenissen. Voorafgaand aan de gebeurtenis, mag u back-up van uw gegevens in premium-opslag. Nadat de onderhoudsgebeurtenis is voltooid, kunt u uw gegevens kunt terugkeren naar de vernieuwd Lsv2 VMs lokale NVMe opslag.

Scenario's die gegevens op de lokale NVMe-schijven beheren zijn onder andere:

- De virtuele machine wordt uitgevoerd en in orde.
- De virtuele machine opnieuw wordt opgestart in plaats (door u of Azure).
- De virtuele machine is onderbroken (gestopt zonder toewijzing ongedaan maken).
- Het merendeel van het geplande onderhoud onderhoudsbewerkingen.

Scenario's die gegevens ter bescherming van de klant veilig wissen zijn:

- De virtuele machine is geïmplementeerd, gestopt (toewijzing ongedaan gemaakt), of is verwijderd (door u).
- De virtuele machine wordt niet in orde en heeft tot de service herstel knooppuntservice naar een ander knooppunt vanwege een hardwareprobleem.
- Een klein aantal het geplande onderhoud onderhoudsbewerkingen die vereist worden meegenomen naar een andere host voor het onderhoud van de virtuele machine.

Zie voor meer informatie over opties voor back-ups van gegevens in de lokale opslag, [back-up en herstel na noodgevallen voor Azure IaaS-schijven](backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

* **Hoe start ik Lsv2-serie VM's implementeren?**  
   Veel net als andere VM's, gebruiken de [Portal](quick-create-portal.md), [Azure CLI](quick-create-cli.md), of [PowerShell](quick-create-powershell.md) om een VM te maken.

* **Een enkele schijffout van NVMe, zullen alle virtuele machines op de host mislukken?**  
   Als er een schijffout wordt gedetecteerd op de hardware-knooppunt, wordt de hardware is in een foutstatus. Wanneer dit het geval is, worden alle virtuele machines op het knooppunt automatisch toewijzing ongedaan gemaakt en naar een gezonde knooppunt verplaatst. Voor Lsv2-serie VM's betekent dit dat de gegevens van de klant op het knooppunt mislukt ook veilig worden gewist en moet opnieuw worden gemaakt door de klant op het nieuwe knooppunt. Zoals opgemerkt, live migratie is pas beschikbaar op Lsv2, worden de gegevens op het knooppunt mislukte proactief verplaatst met de virtuele machines als ze worden overgedragen naar een ander knooppunt.

* **Moet ik polling wijzigingen aanbrengen in Windows in Windows Server 2012 of Windows Server 2016?**  
   NVMe polling is alleen beschikbaar op Windows Server 2019 op Azure.  

* **Kan ik overschakelen naar een traditionele interrupt service routine (ISR)-model?**  
   Lsv2-serie VM's zijn geoptimaliseerd voor NVMe polling. Updates vindt u continu om polling prestaties te verbeteren.

* **Kan ik de navraaginstellingen in Windows Server 2019 aanpassen?**  
   De poll-instellingen zijn niet aanpasbare gebruiker.
   
## <a name="next-steps"></a>Volgende stappen

* Zie de specificaties voor alle [virtuele machines die zijn geoptimaliseerd voor prestaties van opslagruimte](sizes-storage.md) op Azure
