---
title: Failback naar on-premises oplossen tijdens het herstel van VMware-VM na noodgevallen naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel worden manieren voor het oplossen van problemen met failback en opnieuw beveiligen tijdens het herstel van VMware-VM na noodgevallen naar Azure met Azure Site Recovery beschreven.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 20cb7a446befb1d31f0e069d91d0230fc4a2a901
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565596"
---
# <a name="troubleshoot-failback-to-on-premises-from-azure"></a>Failback naar on-premises van Azure oplossen

In dit artikel wordt beschreven hoe u aan het oplossen van problemen die optreden bij het uitvoeren van Azure-VM naar uw on-premises VMware-infrastructuur, na een failover naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

Failback wordt in feite twee belangrijke stappen omvat. Voor de eerste stap moet na een failover, u virtuele machines van Azure naar on-premises beveiligen zodat ze gaan repliceren. De tweede stap wordt een failover uitgevoerd van Azure naar een failback uitvoeren naar uw on-premises site.

## <a name="troubleshoot-reprotection-errors"></a>Opnieuw beveiligen fouten oplossen

In deze sectie worden veelvoorkomende problemen met opnieuw beveiligen en hoe u om ze te corrigeren.

### <a name="error-code-95226"></a>Foutcode 95226

**Opnieuw beveiligen is mislukt omdat de virtuele machine van Azure kan niet tot de on-premises configuratieserver.**

Deze fout treedt op wanneer:

* De Azure-VM kan de on-premises configuratieserver niet bereiken. De virtuele machine kan niet worden gedetecteerd en geregistreerd bij de configuratieserver.
* De service InMage Scout application niet wordt uitgevoerd op de virtuele Azure-machine na een failover. De service is vereist voor communicatie met de on-premises configuratieserver.

Los dit probleem als volgt op:

* Controleer of de Azure VM-netwerk de virtuele Azure-machine om te communiceren met de on-premises configuratieserver toestaat. U kunt een site-naar-site-VPN-verbinding instellen voor uw on-premises datacenter of een Azure ExpressRoute-verbinding configureren met persoonlijke peering in het virtuele netwerk van de virtuele machine van Azure.
* Als de virtuele machine met de on-premises configuratieserver communiceren kan, moet u zich aanmelden bij de virtuele machine. Controleer de service InMage Scout application. Als u ziet dat deze niet wordt uitgevoerd, start u de service handmatig. Controleer dat de service het opstarttype is ingesteld op **automatische**.

### <a name="error-code-78052"></a>Foutcode 78052

**Beveiliging kan niet worden voltooid voor de virtuele machine.**

Dit probleem kan gebeuren als er al een virtuele machine met dezelfde naam op de hoofddoelserver waarop u bent failback.

Los dit probleem als volgt op:

* Selecteer een andere hoofddoelserver op een andere host zodat de machine opnieuw beveiligen op een andere host maakt, waarbij de namen niet conflicteren.
* U kunt ook vMotion gebruiken het hoofddoel verplaatsen naar een andere host waar de conflicterende namen niet uitgevoerd. Als de bestaande virtuele machine een losse virtuele machine is, de naam wijzigen zodat de nieuwe virtuele machine kan worden gemaakt op dezelfde ESXi-host.


### <a name="error-code-78093"></a>Foutcode 78093

**De virtuele machine wordt niet uitgevoerd, vastgelopen, of is niet toegankelijk.**

Los dit probleem als volgt op:

Als u wilt een failover-virtuele machine opnieuw beveiligen, moet de virtuele machine van Azure worden uitgevoerd zodat de Mobility-Service wordt geregistreerd bij de on-premises configuratieserver en kan beginnen met het repliceren door te communiceren met de processerver. Als de machine zich op een netwerk onjuist of is niet uitgevoerd (niet meer reageert of afsluiten), kan de configuratieserver niet bereiken Mobility-Service op de virtuele machine om te beginnen met opnieuw beveiligen.

* Start de VM opnieuw zodat deze weer on-premises communicatie kan starten.
* Start opnieuw op de taak opnieuw beveiligen na het starten van de virtuele machine van Azure.

### <a name="error-code-8061"></a>Foutcode 8061

**De gegevensopslag is niet toegankelijk is vanaf de ESXi-host.**

Controleer de [master target-vereisten en ondersteunde gegevensarchieven](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) voor failback.


## <a name="troubleshoot-failback-errors"></a>Failback fouten oplossen

Deze sectie beschrijft veelvoorkomende fouten die tijdens de failback optreden.

### <a name="error-code-8038"></a>Foutcode 8038

**Kan de zetten van de on-premises virtuele machine vanwege de fout.**

Dit probleem treedt op wanneer de on-premises VM op een host die niet over voldoende geheugen ingericht naar is gebracht. 

Los dit probleem als volgt op:

* Inrichten van meer geheugen op de ESXi-host.
* Bovendien kunt u vMotion op de virtuele machine verplaatsen naar een andere ESXi-host die voldoende geheugen heeft voor het opstarten van de VM.
