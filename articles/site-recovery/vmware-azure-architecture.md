---
title: VMware naar Azure disaster recovery-architectuur in Azure Site Recovery | Microsoft Docs
description: In dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het instellen van herstel na noodgevallen van on-premises VMware-machines naar Azure met Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: f1fdbd143093beb9736e86b24b76843ad82b89f2
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418367"
---
# <a name="vmware-to-azure-disaster-recovery-architecture"></a>VMware naar Azure disaster recovery-architectuur

Dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u disaster recovery-replicatie, failover en herstel van virtuele VMware-machines (VM's) implementeert tussen een on-premises VMware-site en Azure met de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor noodherstel van VMware naar Azure.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure Storage-account voor cache, beheerde schijf en Azure-netwerk. | Gerepliceerde gegevens van on-premises VM's worden opgeslagen in Azure storage. Azure VM's worden gemaakt met de gerepliceerde gegevens wanneer u een failover van on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratie van server-machine** | Een enkele on-premises computer. Het is raadzaam dat u uitvoert als een VMware-VM die kan worden geïmplementeerd vanaf een gedownloade OVF-sjabloon.<br/><br/> De machine wordt uitgevoerd van alle on-premises Site Recovery-onderdelen, waaronder de configuratieserver, processerver en hoofddoelserver. | **Configuratieserver**: Coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.<br/><br/> **Processerver**: standaard geïnstalleerd op de configuratieserver. Deze ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt dit naar Azure Storage. De processerver installeert ook Azure Site Recovery Mobility Service op virtuele machines die u repliceren wilt, en wordt automatische detectie van on-premises machines uitgevoerd. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.<br/><br/> **Hoofddoelserver**: standaard geïnstalleerd op de configuratieserver. Replicatiegegevens worden verwerkt tijdens de failback vanuit Azure. U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**VMware-servers** | Virtuele VMware-machines worden gehost op on-premises vSphere ESXi-servers. U wordt aangeraden een vCenter-server voor het beheren van de hosts. | Tijdens de implementatie van Site Recovery kunt u VMware-servers toevoegen aan de Recovery Services-kluis.
**Gerepliceerde machines** | Mobility-Service is geïnstalleerd op elke VMware-VM die u wilt repliceren. | Het is raadzaam dat u de automatische installatie van de processerver toestaan. U kunt ook de service handmatig installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.

**VMware naar Azure-architectuur**

![Onderdelen](./media/vmware-azure-architecture/arch-enhanced.png)



## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint initiële replicatie naar Azure-opslag, met behulp van het opgegeven replicatiebeleid. Houd rekening met het volgende:
    - Voor virtuele VMware-machines is de replicatie op blokniveau, in de buurt van continue, met behulp van de Mobility-service-agent die wordt uitgevoerd op de virtuele machine.
    - De replicatie-beleidsinstellingen worden toegepast:
        - **RPO-drempelwaarde**. Deze instelling heeft geen invloed op de replicatie. Dit helpt bij de bewaking. Een gebeurtenis geactiveerd en eventueel een e-mailbericht verzonden, als de huidige RPO overschrijdt de drempelwaarde die u opgeeft.
        - **Bewaarperiode voor herstelpunten**. Deze instelling bepaalt hoe ver terug in de tijd die u gaan wilt wanneer er een onderbreking optreedt. Maximum bewaartermijn op premium storage is 24 uur. Op de standard-opslag is dit 72 uur. 
        - **App-consistente momentopnamen**. App-consistente momentopname tilt elke 1 tot 12 uur, afhankelijk van de behoeften van uw app. Momentopnamen zijn standaard Azure blob-momentopnamen. De Mobility-agent die wordt uitgevoerd op een virtuele machine vraagt een VSS-momentopname in overeenstemming met deze instelling en bladwijzers die point-in-time als een toepassing die consistent punt in de stream van replicatie.

2. Verkeer repliceert naar Azure storage-openbare eindpunten via internet. U kunt ook kunt u Azure ExpressRoute met [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering). Het repliceren van verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site naar Azure wordt niet ondersteund.
3. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Bijgehouden wijzigingen voor een virtuele machine worden verzonden naar de processerver.
4. Communicatie gebeurt er als volgt uit:

    - Virtuele machines communiceren met de on-premises configuratieserver op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.
    - De configuratieserver coördineert de replicatie met Azure via HTTPS-poort 443 uitgaande.
    - Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op de server configuration) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
    - De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.
5. De replicatiegegevens registreert eerste land in een cache-opslagaccount in Azure. Deze logboeken worden verwerkt en de gegevens worden opgeslagen in een door Azure beheerde schijf (met de naam als asr seed-schijf). De herstelpunten worden gemaakt op deze schijf.




**VMware naar Azure replicatieproces**

![Replicatieproces](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat replicatie is ingesteld en u een Dr-herstelanalyse (testfailover) om te controleren of alles werkt zoals verwacht worden uitgevoerd, kunt u failover en failback uitvoeren als u wilt.

1. U mislukt voor één machine uitvoeren of plannen om meerdere virtuele machines op hetzelfde moment een failover van een herstelpunt maken. Het voordeel van een plan voor herstel in plaats van één machine failover zijn onder andere:
    - U kunt app-afhankelijkheden model door alle virtuele machines in de app in één herstelplan.
    - U kunt scripts, Azure-runbooks, toevoegen en onderbreken voor de handmatige acties.
2. Na de initiële failover wordt geactiveerd, wijst u het toe om toegang tot de workload via de Azure-VM.
3. Als uw primaire on-premises site weer beschikbaar is, kunt u voorbereiden voor failback. Als u wilt uitvoeren van een failback, moet u voor het instellen van een infrastructuur voor failback, met inbegrip van:

    * **Tijdelijke processerver in Azure**: Failback van Azure, voor het instellen van een Azure-VM om te fungeren als een processerver voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    * **VPN-verbinding**: Als u wilt uitvoeren van een failback, moet u een VPN-verbinding (of ExpressRoute) van het Azure-netwerk de on-premises site.
    * **Afzonderlijke hoofddoelserver**: De hoofddoelserver die is geïnstalleerd met de configuratieserver op de on-premises virtuele VMware-machine wordt standaard de failback afgehandeld. Als u back grote hoeveelheden verkeer mislukt wilt, kunt u instellen van een afzonderlijke on-premises hoofddoelserver voor dit doel.
    * **Failbackbeleid**: Als u wilt repliceren naar uw on-premises site, moet u een beleid voor failback. Dit beleid wordt automatisch gemaakt wanneer u een beleid voor replicatie van on-premises naar Azure maakt.
4. Nadat de onderdelen in plaats daarvan de failback in drie acties zijn:

    - Fase 1: De Azure VM's opnieuw beveiligen zodat ze van Azure worden gerepliceerd naar de on-premises VMware-machines.
    -  Fase 2: Een failover uitvoeren naar de on-premises site.
    - Fase 3: Nadat de werkbelastingen terug zijn mislukt, opnieuw u replicatie voor de on-premises VM's inschakelen.
    
 
**VMware-failback van Azure**

![Failback](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](vmware-azure-tutorial.md) om in te schakelen van VMware naar Azure-replicatie.
