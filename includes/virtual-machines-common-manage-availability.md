---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f57c2cacca9bb3e4526ec6261b8aa0ff6c18448a
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65546746"
---
## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>Informatie over het opnieuw opstarten van VM's - onderhoud versus downtime
Er zijn drie scenario's die kunnen leiden tot een virtuele machine in Azure wordt beïnvloed: niet-gepland hardwareonderhoud, onverwachte downtime en gepland onderhoud.

* **Gebeurtenis voor niet-gepland hardwareonderhoud** treedt op wanneer via het Azure-platform een fout wordt voorspeld op de hardware of in een platformonderdeel dat is gekoppeld aan een fysieke computer. Wanneer via het platform een fout wordt voorspeld, wordt een gebeurtenis voor niet-gepland hardwareonderhoud vrijgegeven om de impact op de virtuele machines die worden gehost op deze hardware, te beperken. Azure maakt gebruik van [livemigratie](https://docs.microsoft.com/azure/virtual-machines/linux/maintenance-and-updates) technologie voor het migreren van de virtuele Machines van de hardware waarop de naar een gezonde fysieke machine. Livemigratie is een bewerking ter behoud van VM's waardoor de werking van een virtuele machine slechts korte tijd wordt onderbroken. Het geheugen, de geopende bestanden en de netwerkverbindingen blijven behouden, maar de prestaties vóór en/of na de gebeurtenis kunnen minder zijn. In gevallen waarbij livemigratie niet kan worden gebruikt, treedt er onverwachte downtime op de VM op, zoals hieronder wordt beschreven.


* **Een onverwachte Downtime** is wanneer de hardware of de fysieke infrastructuur voor de virtuele machine is onverwacht mislukt. Dit kunnen bijvoorbeeld lokale netwerkproblemen, lokale schijffouten te voorkomen of andere fouten van rack-niveau. Wanneer wordt gedetecteerd, worden automatisch het Azure-platform gemigreerd (VM) uw virtuele machine naar een gezonde fysieke machine in hetzelfde datacenter. Tijdens deze procedure treedt downtime (opnieuw opstarten) op de virtuele machines op en in sommige gevallen gaat de tijdelijke schijf verloren. Het besturingssysteem en de gegevensschijven die zijn bijgevoegd, blijven altijd behouden. 

  Virtuele machines kunnen ook treedt downtime in het onwaarschijnlijke geval van een stroomstoring of ramp die betrekking heeft op een heel datacenter of zelfs een hele regio. Voor deze scenario's biedt Azure beveiligingsopties, met inbegrip van [beschikbaarheidszones](../articles/availability-zones/az-overview.md) en [gekoppelde regio's](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

* **Gepland onderhoud** bestaat uit periodieke updates van het onderliggende Azure-platform die door Microsoft worden doorgevoerd ter verbetering van de algemene betrouwbaarheid, prestaties en beveiliging van de platform-infrastructuur waarop de virtuele machines worden uitgevoerd. Veel van deze updates worden uitgevoerd zonder dat dit van invloed is op uw virtuele machines of Cloud Services (zie [Onderhoud ter behoud van VM's](https://docs.microsoft.com/azure/virtual-machines/windows/preserving-maintenance)). Wanneer mogelijk maakt het Azure-platform gebruik van Onderhoud ter behoud van VM's. In zeldzame gevallen kan het echter noodzakelijk zijn om de virtuele machine opnieuw op te starten om de vereiste updates toe te passen op de onderliggende infrastructuur. In dit geval kunt u gepland onderhoud van Azure gebruiken met de bewerking Onderhoud-Opnieuw implementeren door het onderhoud voor de betrokken VM's te initiëren in het geschikte tijdvenster. Zie [Gepland onderhoud voor virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/planned-maintenance/) voor meer informatie.


Om de gevolgen van downtime vanwege een of meer van deze gebeurtenissen te beperken, raden we aan voor uw virtuele machines de volgende aanbevolen procedures voor hoge beschikbaarheid te volgen:

* [Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie]
* [Beheerde schijven voor virtuele machines in een beschikbaarheidsset gebruiken]
* [Geplande gebeurtenissen te proactief reageren op gebeurtenissen die VM gebruiken](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events)
* [Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets]
* [Combineer het gebruik van een load balancer met beschikbaarheidssets]
* [Gebruik beschikbaarheidszones om te beschermen tegen storingen in datacenters niveau]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie
U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Deze configuratie in een datacenter zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de 99,95% Azure SLA. Zie de [SLA voor virtuele machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/) voor meer informatie.

> [!IMPORTANT]
> Voorkom dat een virtuele machine met een enkele instantie als enige deel uitmaakt van een beschikbaarheidsset. Virtuele machines in deze configuratie niet in aanmerking komen voor SLA-garantie en krijgen met downtime bij Azure gepland onderhoud, behalve wanneer een enkele virtuele machine gebruikmaakt [Azure premium SSD's](../articles/virtual-machines/windows/disks-types.md#premium-ssd). Voor één VM's met behulp van premium SSD's, de Azure SLA van toepassing is.

Elke virtuele machine in uw beschikbaarheidsset krijgt een **updatedomein** en een **foutdomein** toegewezen door het onderliggende Azure-platform. Voor iedere beschikbaarheidsset worden standaard vijf updatedomeinen toegewezen die niet door gebruiker te bewerken zijn (voor Resource Manager-implementaties kan dit aantal worden opgehoogd tot 20 updatedomeinen), om groepen virtuele machines en onderliggende fysieke hardware aan te duiden die op hetzelfde moment opnieuw kunnen worden opgestart. Wanneer in één beschikbaarheidsset meer dan vijf virtuele machines worden geconfigureerd, wordt de zesde virtuele machine in hetzelfde updatedomein geplaatst als de eerste virtuele machine, de zevende in hetzelfde updatedomein als de tweede virtuele machine, enzovoort. De volgorde waarin updatedomeinen opnieuw worden opgestart, verloopt tijdens gepland onderhoud niet altijd sequentieel, maar er wordt slechts één updatedomein tegelijk opnieuw opgestart. Een updatedomein dat opnieuw is opgestart, heeft 30 minuten om te herstellen voordat onderhoud wordt geïnitieerd op een ander updatedomein.

Foutdomeinen duiden de groep virtuele machines aan die een gemeenschappelijke voeding en switch delen. Standaard worden bij Resource Manager-implementaties de virtuele machines die zijn geconfigureerd in uw beschikbaarheidsset verdeeld over maximaal drie foutdomeinen (twee foutdomeinen bij klassieke implementaties). Hoewel het in een beschikbaarheidsset plaatsen van uw virtuele machines uw toepassing niet beschermt tegen problemen met het besturingssysteem of de toepassing zelf, worden zo wel de gevolgen van mogelijke problemen met de fysieke hardware, netwerkstoringen of stroomonderbrekingen beperkt.

<!--Image reference-->
   ![Concepttekening van de configuratie van de update-domein- en foutdomeinen](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Beheerde schijven voor VM's in een beschikbaarheidsset gebruiken
Als u momenteel VM's met niet-beheerde schijven gebruikt, raden wij u ten zeerste aan [VM's in een beschikbaarheidsset te converteren voor het gebruik van beheerde schijven](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md).

[Beheerde schijven](../articles/virtual-machines/windows/managed-disks-overview.md) bieden een betere betrouwbaarheid voor beschikbaarheidssets door ervoor te zorgen dat de schijven van VM's in een beschikbaarheidsset voldoende van elkaar zijn verwijderd, waardoor een SPOF (Single Point Of Failure) wordt voorkomen. Dit gebeurt door de schijven automatisch plaatsen in domeinen met fouten van andere storage (opslagclusters) en deze overeenkomt met het foutdomein van de virtuele machine. Als een foutdomein opslag is mislukt vanwege fout bij de hardware of software, wordt alleen het VM-exemplaar met schijven op het foutdomein storage mislukt.
![Beheerde schijven FD 's](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

> [!IMPORTANT]
> Het aantal foutdomeinen voor beheerde beschikbaarheidssets varieert per regio: twee of drie per regio. In de volgende tabel wordt het aantal per regio weergegeven

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

Als u van plan bent VM's met niet-beheerde schijven gebruiken, volgt u onderstaande aanbevolen procedures voor opslagaccounts waarbij virtuele harde schijven (VHD's) van virtuele machines worden opgeslagen als [pagina-blobs](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs).

1. **Zorg dat alle schijven (gegevens en besturingssysteem) worden gekoppeld aan een virtuele machine op hetzelfde opslagaccount**
2. **Controleer de [limieten](../articles/storage/common/storage-scalability-targets.md) voor het aantal niet-beheerde schijven in een opslagaccount** voordat u meer VHD's aan een opslagaccount toevoegt
3. **Gebruik een afzonderlijk opslagaccount voor elke virtuele machine in een beschikbaarheidsset.** Deel opslagaccounts met meerdere VM's niet in dezelfde beschikbaarheidsset. Het is aanvaardbaar als virtuele machines in verschillende Beschikbaarheidssets gegevensaccount delen, als aanbevolen procedures worden gevolgd ![niet-beheerde schijven FD's](./media/virtual-machines-common-manage-availability/umd-updated.png)

## <a name="use-scheduled-events-to-proactively-respond-to-vm-impacting-events"></a>Kunt u proactief te reageren op gebeurtenissen die VM met geplande gebeurtenissen

Wanneer u zich abonneert op [geplande gebeurtenissen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events), uw virtuele machine wordt geïnformeerd over aanstaande onderhoud-gebeurtenissen die kunnen invloed hebben op uw virtuele machine. Wanneer de geplande gebeurtenissen zijn ingeschakeld, krijgt uw virtuele machine een minimale hoeveelheid tijd voordat de activiteit onderhoud wordt uitgevoerd. Bijvoorbeeld, updates van het Host-besturingssysteem die mogelijk van invloed zijn op uw virtuele machine worden in de wachtrij geplaatst als gebeurtenissen die de impact opgeven, evenals een periode waarbinnen het onderhoud wordt uitgevoerd als er geen actie ondernomen. Schema-gebeurtenissen worden ook in de wachtrij geplaatst wanneer Azure dreigende hardwarefout waardoor het mogelijk van invloed zijn op uw virtuele machine, zodat u kunt bepalen wanneer het herstel moet worden uitgevoerd. Klanten kunnen de gebeurtenis voor het uitvoeren van taken voorafgaand aan het onderhoud, zoals opgeslagen status, gebruiken Failover-overschakeling uitvoeren naar de secundaire, enzovoort. Nadat u uw logica voor het verwerken van de onderhoudsgebeurtenis zonder problemen voltooid, kunt u de openstaande geplande gebeurtenis om toe te staan van het platform om door te gaan met onderhoud goedkeuren.

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets
Als uw virtuele machines allemaal bijna identiek zijn en hetzelfde doel dienen voor uw toepassing, raden wij aan dat u een beschikbaarheidsset configureert voor elke laag van uw toepassing.  Als u twee verschillende lagen in dezelfde beschikbaarheidsset plaatst, kunnen alle virtuele machines in dezelfde toepassingslaag in één keer opnieuw worden opgestart. Door voor elke laag ten minste twee virtuele machines in een beschikbaarheidsset te configureren, garandeert u dat in elke laag ten minste één virtuele machine beschikbaar is.

U kunt bijvoorbeeld alle virtuele machines in de front-end van uw toepassing waarop IIS, Apache en Nginx worden uitgevoerd, in één beschikbaarheidsset plaatsen. Zorg ervoor dat er alleen virtuele machines van de front-end in dezelfde beschikbaarheidsset worden geplaatst. Zorg er ook voor dat alleen virtuele machines uit de gegevenslaag in een eigen beschikbaarheidsset worden geplaatst, zoals gerepliceerde virtuele machines met SQL Server of virtuele machines met MySQL.

<!--Image reference-->
   ![Toepassingslagen](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Combineer het gebruik van een load balancer met beschikbaarheidssets
Combineer de [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) met een beschikbaarheidsset om uw toepassingen zo stabiel mogelijk te maken. De Azure Load Balancer verdeelt het verkeer tussen meerdere virtuele machines. De Azure Load Balancer is voor virtuele machines uit de prijscategorie Standard bij de prijs inbegrepen. De Azure Load Balancer is niet bij alle prijscategorieën voor virtuele machines inbegrepen. Zie voor meer informatie over het gebruik van load balancers voor uw virtuele machines [Taakverdeling voor virtuele machines](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Als de load balancer niet is geconfigureerd om het verkeer te verdelen over meerdere virtuele machines, heeft iedere geplande onderhoudsgebeurtenis invloed op de enkele virtuele machine die uw verkeer afhandelt, waardoor uw applicatielaag onbeschikbaar wordt. Door meerdere virtuele machines van dezelfde laag onder te brengen bij dezelfde load balancer en beschikbaarheidsset, zorgt u ervoor dat verkeer altijd door ten minste één instantie kan worden afgehandeld.

## <a name="use-availability-zones-to-protect-from-datacenter-level-failures"></a>Gebruik beschikbaarheidszones om te beschermen tegen storingen in datacenters niveau

[Beschikbaarheidszones](../articles/availability-zones/az-overview.md)Hiermee stelt u een alternatief voor beschikbaarheid, vouw de mate van controle die u hebt voor de beschikbaarheid van de toepassingen en gegevens op uw virtuele machines. Een beschikbaarheidszone is een fysiek afgescheiden zone binnen een Azure-regio. Er zijn drie Beschikbaarheidszones per ondersteunde Azure-regio. Elke Beschikbaarheidszone heeft een afzonderlijke power bron, het netwerk en koeling, en is logisch gescheiden van de andere Beschikbaarheidszones binnen de Azure-regio. Door het ontwikkelen van uw oplossingen voor het gebruik van gerepliceerde VM's in zones, kunt u uw apps en gegevens beschermen tegen het verlies van een datacenter. Als één zone is geknoeid, klikt u vervolgens zijn gerepliceerde apps en gegevens onmiddellijk beschikbaar in een andere zone. 

![Beschikbaarheidszones](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Meer informatie over het implementeren van een [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) of [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) virtuele machine in een Beschikbaarheidszone.


<!-- Link references -->
[Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets]: #configure-each-application-tier-into-separate-availability-sets
[Combineer het gebruik van een load balancer met beschikbaarheidssets]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Beheerde schijven voor virtuele machines in een beschikbaarheidsset gebruiken]: #use-managed-disks-for-vms-in-an-availability-set
[Gebruik beschikbaarheidszones om te beschermen tegen storingen in datacenters niveau]: #use-availability-zones-to-protect-from-datacenter-level-failures
