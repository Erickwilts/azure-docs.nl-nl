---
title: Concepten-persoonlijke Clouds en clusters
description: Meer informatie over de belangrijkste mogelijkheden van Azure VMware-software-gedefinieerde data centers en vSphere-clusters in VMware-oplossing in azure op VMware.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 715293d9951876ff0f794f8f6b580093f89571b3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316865"
---
#  <a name="azure-vmware-solution-private-cloud-and-cluster-concepts"></a>Azure VMware-oplossing persoonlijke Cloud-en cluster concepten

De Azure VMware-oplossing levert persoonlijke Clouds op basis van VMware in Azure. De persoonlijke Clouds zijn gebouwd op basis van clusters van toegewezen bare-metal hosts en worden geïmplementeerd en beheerd via de Azure Portal. Clusters in persoonlijke Clouds worden ingericht met VMware vSphere-, vCenter-, vSAN-en NSX-software. Azure VMware-oplossing, hardware-en software-implementaties in de privécloud zijn volledig geïntegreerd en geautomatiseerd in Azure.

Er is een logische relatie tussen Azure-abonnementen, persoonlijke Clouds van Azure VMware, vSAN-clusters en hosts. In het diagram worden twee persoonlijke Clouds in één Azure-abonnement weer gegeven. Persoonlijke Clouds vertegenwoordigen een ontwikkelings-en een productie omgeving, elk met hun eigen privécloud. In elk van deze persoonlijke Clouds zijn er twee clusters. Als u de lagere mogelijke behoeften van een ontwikkel omgeving wilt weer geven, worden kleinere clusters met hosts met een lagere capaciteit gebruikt. Al deze concepten worden beschreven in de volgende secties.

![Afbeelding van twee persoonlijke Clouds in een klant abonnement](./media/hosts-clusters-private-clouds-final.png)

## <a name="private-clouds"></a>Privéclouds

Persoonlijke Clouds bevatten vSAN-clusters die zijn gebouwd met specifieke, Bare-Metal Azure-hosts. Elke privécloud kan meerdere clusters hebben, die allemaal worden beheerd door dezelfde vCenter-Server en NSX-T-beheer. U kunt persoonlijke Clouds implementeren en beheren in de portal, vanuit de CLI of met Power shell. Net als bij andere resources worden persoonlijke Clouds geïnstalleerd en beheerd vanuit een Azure-abonnement.

Het aantal privé-Clouds in een abonnement is schaalbaar. In eerste instantie is er een limiet van één privécloud per abonnement.

## <a name="clusters"></a>Clusters

U maakt ten minste één vSAN-cluster in elke privécloud. Wanneer u een privécloud maakt, is er standaard één cluster. U kunt extra clusters toevoegen aan een privécloud met behulp van de Azure Portal of via de API. Alle clusters hebben een standaard grootte van drie hosts en kunnen worden geschaald van 3 naar 16 hosts. Het type hosts dat in een cluster wordt gebruikt, moet van hetzelfde type zijn. De typen hosts worden in de volgende sectie beschreven.

Proef clusters zijn beschikbaar voor evaluatie en ze zijn beperkt tot drie hosts en één proef cluster per privécloud. U kunt een proef cluster op één host schalen tijdens de evaluatie periode.

U kunt clusters maken, verwijderen en schalen via de portal of API. U kunt nog steeds vSphere en NSX-T-beheer gebruiken om de meeste andere aspecten van de cluster configuratie of-bewerking te beheren. Alle lokale opslag van elke host in een cluster wordt beheerd door vSAN.

## <a name="hosts"></a>Hosts

De knoop punten met een Hyper-Metal-infrastructuur beheerder worden gebruikt in azure VMware-oplossingen voor particuliere cloud clusters. De geheugen-, CPU-en schijf capaciteit van de host vindt u in de volgende tabel. 

| Hosttype              |             CPU             |   RAM (GB)   |  vSAN NVMe-cache-laag (TB, RAW)  |  vSAN SSD-capaciteits tier (TB, RAW)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| High-End (HE)          |  Dual Intel 18 Core 2,3 GHz  |     576      |                3.2               |                15,20               |

Hosts die worden gebruikt voor het bouwen of schalen van clusters, worden verkregen van een geïsoleerde groep hosts. Deze hosts hebben hardware-tests door lopen en alle gegevens zijn veilig verwijderd uit de flash-schijven. Wanneer u een host uit een cluster verwijdert, worden de interne schijven veilig gewist en worden de hosts in de geïsoleerde groep hosts geplaatst. Wanneer u een host aan een cluster toevoegt, wordt een gezuiverde host uit de geïsoleerde groep gebruikt.

## <a name="vmware-software-versions"></a>VMware-software versies

De huidige software versies van de VMware-software die worden gebruikt in azure VMware-oplossingen persoonlijke Cloud clusters zijn:

| Software              |    Versie   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6,7 U2    | 
| ESXi                  |    6,7 U2    | 
| vSAN                  |    6,7 U2    |
| NSX-T                 |      2,5     |

Voor elk nieuw cluster in een privécloud komt de versie van de software overeen met wat er momenteel wordt uitgevoerd in de privécloud. Voor elke nieuwe privécloud in een klant abonnement is de nieuwste versie van de software stack geïnstalleerd.

Het algemene upgrade beleid en de processen voor de Azure VMware Solution platform-software worden beschreven in het document concepten bijwerken.

## <a name="host-maintenance-and-lifecycle-management"></a>Host onderhoud en levenscyclus beheer

Host onderhoud en levenscyclus beheer worden uitgevoerd zonder gevolgen voor de capaciteit of prestaties van particuliere cloud clusters. Voor beelden van automatische host onderhoud zijn onder andere firmware-upgrades en reparaties of vervangingen van hardware.

Micro soft is verantwoordelijk voor het levenscyclus beheer van NSX-T-apparaten, zoals NSX-T-beheer en NSX-T EDGE. Micro soft is ook verantwoordelijk voor het Boots trappen van de netwerk configuratie, zoals het maken van de laag-0-gateway en het inschakelen van North-South route ring. Als beheerder van uw Azure VMware-oplossing privécloud, bent u verantwoordelijk voor de configuratie van NSX-T SDN. Bijvoorbeeld netwerk segmenten, gedistribueerde firewall regels, laag 1-gateways en load balancers.

> [!IMPORTANT]
> Een Azure VMware-oplossings beheerder mag de configuratie van de NSX-T-rand of de laag-0-gateway niet wijzigen. Dit kan leiden tot een verlies van de service.

## <a name="backup-and-restoration"></a>Back-ups maken en herstellen

Er wordt elk uur een back-up gemaakt van de vCenter-en NSX-configuratie van de privécloud. Back-ups worden drie dagen bewaard. U wordt gevraagd om een back-up te herstellen via een service aanvraag in de Azure Portal.

## <a name="next-steps"></a>Volgende stappen

De volgende stap is het leren van [netwerken en concepten tussen de connectiviteit](concepts-networking.md).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

