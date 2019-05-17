---
title: Azure Site Recovery-updates | Microsoft Docs
description: Biedt een overzicht van service-updates en het upgraden van onderdelen die worden gebruikt in Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/25/2019
ms.author: rajanaki
ms.openlocfilehash: dc15c8935cd3c20037bbed01f0f24cd2b8642045
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540929"
---
# <a name="service-updates-in-azure-site-recovery"></a>Service-updates in de Azure Site Recovery
Als een organisatie, moet u bepalen hoe u van plan bent voor uw gegevens veilig en apps/workloads kunt uitvoeren wanneer geplande en ongeplande uitval optreedt. Azure Site Recovery draagt bij aan uw BCDR-strategie door uw apps uitgevoerd op virtuele machines en fysieke servers beschikbaar als een site uitvalt. Met Site Recovery repliceert u workloads die worden uitgevoerd op virtuele machines en fysieke servers, zodat deze beschikbaar blijven op een secundaire locatie als de primaire site niet beschikbaar is. Hiermee herstelt u werkbelastingen op de primaire site wanneer deze weer actief is.

Met Site Recovery kunt u replicatie beheren voor:

- [Virtuele Azure-machines repliceren tussen Azure-regio's](azure-to-azure-tutorial-dr-drill.md).
- On-premises virtuele machines en fysieke servers die worden gerepliceerd naar Azure of naar een secundaire site.
Om te weten meer Raadpleeg de documentatie [hier](https://docs.microsoft.com/azure/site-recovery) .

Azure Site Recovery publiceert service-updates regelmatig - waaronder het toevoegen van nieuwe functies, verbeteringen in de ondersteuningsmatrix en oplossingen voor problemen indien van toepassing. Als u wilt blijven huidige profiteren van alle de meest recente functies & verbeteringen en oplossingen voor problemen indien van toepassing, worden gebruikers aangeraden altijd naar de nieuwste versie van Azure SIte Recovery-onderdelen wilt bijwerken. 
 
## <a name="support-statement-for-azure-site-recovery"></a>Ondersteuningsverklaring voor Azure Site Recovery 

> [!IMPORTANT]
> **Bij elke nieuwe versie "n" van een Azure Site Recovery-onderdeel dat wordt uitgebracht, alle versies hieronder 'N-4' wordt beschouwd als ondersteuning vervalt**. Daarom is het altijd verstandig om te upgraden naar de nieuwste beschikbare versies.

> [!IMPORTANT]
> De officiële ondersteuning voor upgrades van afkomstig is van > N-4 N versie (N wordt de meest recente versie). Als u van N-6 gebruikmaakt, moet u eerst een upgrade naar N-4 en vervolgens een upgrade naar N.

### <a name="upgrading-when-the-difference-between-current-version-and-latest-released-version-is-greater-than-4"></a>Bijwerken van bij het verschil tussen de huidige versie en de meest recente versie groter dan 4 is

1. Als een eerste stap het momenteel geïnstalleerde onderdeel upgraden van versie zeg N N + 4 en verplaatst naar de volgende compatibele versie. Stel de huidige versie is 9,24 en u bent bij 9.16, eerst een upgrade naar 9.20 en vervolgens naar 9,24.
2. Doe hetzelfde voor alle onderdelen afhankelijk van het scenario.

### <a name="support-for-latest-oskernel-versions"></a>Ondersteuning voor de meest recente versies van de OS/kernel

> [!NOTE]
> Als u een gepland onderhoudsvenster en opnieuw opstarten onderdeel van dezelfde is, raden wij u eerst de Site Recovery-onderdelen updatet en doorgaan met de rest van de geplande activiteiten.

1. Vóór de upgrade van de Kernel/OS-versies, moet u eerst controleren of de doelversie wordt ondersteund door Azure Site Recovery. U kunt de informatie vinden in onze documentatie voor Azure VM's, [virtuele VMware-machines](vmware-physical-azure-support-matrix.md) & Hyper-V-machines in
2. Raadpleeg onze [Service-Updates](https://azure.microsoft.com/updates/?product=site-recovery) om erachter te komen welke versie van Site Recovery-onderdelen ondersteunen de specifieke versie die u overstappen wilt op.
3. Eerst een upgrade uitvoert naar de nieuwste versie van Site worden hersteld.
4. Nu de OS/Kernel upgraden naar de gewenste versies.
5. Opnieuw opstarten.
6. Dit zorgt ervoor dat de versie van de OS/Kernel op uw virtuele machines worden bijgewerkt naar de nieuwste versie, en ook dat ook de meest recente wijzigingen voor de Site Recovery die vereist zijn ter ondersteuning van de nieuwe versie worden geladen op de bronmachine.



## <a name="azure-vm-disaster-recovery-to-azure"></a>Herstel na noodgevallen van virtuele Azure-machines naar Azure
In dit scenario wordt sterk aangeraden u [inschakelen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-autoupdate) automatische updates. U kunt kiezen om toe te staan van Site Recovery voor het beheren van updates in de volgende manieren:

- Als onderdeel van de stap van de replicatie inschakelen
- Schakelen tussen de extensie-instellingen in de kluis bijwerken

U hebt gekozen voor het beheren van updates handmatig, als volgt te werk:

1. Ga naar Azure portal en navigeer vervolgens naar de 'Recovery services-kluis."
2. Ga naar het deelvenster 'Gerepliceerde Items' in de Azure-portal voor de 'Recovery services-kluis."
3. Klik op de volgende melding aan de bovenkant van het scherm:
    
    *Nieuwe Site Recovery-replicatie-agentupdate is beschikbaar*
    
    *Klik om te installeren ->*

4. Selecteer de virtuele machines die u wilt de update van toepassing, en klik vervolgens op **OK**.

## <a name="between-two-on-premises-vmm-sites"></a>Tussen twee on-premises VMM-sites
1. Download de meest recente update Rollup voor Microsoft Azure Site Recovery Provider.
2. Eerst updatepakket installeren op de on-premises VMM-server die de site recovery wordt beheerd.
3. Na het herstel site wordt bijgewerkt, updatepakket installeren op de VMM-server die de primaire site beheert.

> [!NOTE]
> Als de VMM is een maximaal beschikbare VMM (geclusterde VMM), zorg er dan voor dat u de upgrade installeren op alle knooppunten van het cluster waarop de VMM-service is geïnstalleerd.

## <a name="between-an-on-premises-vmm-site-and-azure"></a>Tussen een on-premises VMM-site en Azure
1. Updatepakket voor Microsoft Azure Site Recovery Provider downloaden.
2. Updatepakket installeren op de on-premises VMM-server.
3. Installeer de meest recente agent MARS-agent op alle Hyper-V-hosts.

> [!NOTE]
> Als uw VMM is een maximaal beschikbare VMM (geclusterde VMM), zorg er dan voor dat u de upgrade installeren op alle knooppunten van het cluster waarop de VMM-service is geïnstalleerd.

## <a name="between-an-on-premises-hyper-v-site-and-azure"></a>Tussen een on-premises Hyper-V-site en Azure

1. Updatepakket voor Microsoft Azure Site Recovery Provider downloaden.
2. De provider installeren op elk knooppunt van de Hyper-V-servers die u hebt geregistreerd in Azure Site Recovery.

> [!NOTE]
> Als uw Hyper-V een Host geclusterde Hyper-V-server is, zorg ervoor dat u de upgrade op alle knooppunten van het cluster installeren

## <a name="between-an-on-premises-vmware-or-physical-site-to-azure"></a>Tussen een on-premises VMware of fysieke-site naar Azure

Raadpleeg voordat u doorgaat met updates [Site Recovery-ondersteuningsverklaring](#support-statement-for-azure-site-recovery) om te begrijpen van de upgrade-pad.

1. Op basis van uw huidige versie en ondersteuning instructie hierboven, installeert u de update eerst op uw on-premises beheerserver door de richtlijnen gegeven [hier](vmware-azure-deploy-configuration-server.md#upgrade-the-configuration-server). Dit is de server waarop de configuratieserver en de proces-serverfuncties.
2. Als u scale-out verwerken van servers, naast bijwerken door de volgende richtlijnen gegeven [hier](vmware-azure-manage-process-server.md#upgrade-a-process-server).
3. Vervolgens voor het bijwerken van mobility-agent op elk beveiligd item, gaat u naar Azure portal en ga vervolgens naar de **beveiligde Items** > **gerepliceerde Items** pagina. Selecteer een virtuele machine op deze pagina. Selecteer de **Update-Agent** knop die wordt weergegeven aan de onderkant van de pagina voor elke virtuele machine. Hiermee wordt de Mobility-Service-Agent op alle beveiligde virtuele machines.

### <a name="reboot-of-source-machine-after-mobility-agent-upgrade"></a>Opnieuw opstarten van de bronmachine nadat de upgrade van de mobility-agent

Opnieuw opstarten wordt aanbevolen na elke upgrade van de Mobility-agent om ervoor te zorgen dat alle laatste wijzigingen op de bronmachine zijn geladen. Het is echter wel **niet verplicht**. Als het verschil tussen de agentversie tijdens de laatste keer opnieuw opstarten en de huidige versie is groter dan 4, klikt u vervolgens is een opnieuw opstarten verplicht. Raadpleeg de volgende tabel voor gedetailleerde uitleg.

|**Agentversie tijdens de laatste keer opnieuw opstarten** | **Een upgrade naar** | **Is opnieuw verplicht?**|
|---------|---------|---------|
|9.16 |  9.18 | Niet verplicht|
|9.16 | 9.19 | Niet verplicht|
| 9.16 | 9.20 | Niet verplicht
 | 9.16 | 9.21 | Ja, eerst een upgrade naar 9.20 en vervolgens opnieuw opstarten voordat u bijwerkt naar 9.21 als het verschil tussen de versies (9.16 waar de laatste keer opnieuw opstarten is uitgevoerd en de doelversie 9.21) is > 4

## <a name="links-to-currently-supported-update-rollups"></a>Koppelingen naar de momenteel ondersteunde updatepakketten

|Updatepakket  |Leverancier  |Geïntegreerde Setup| OVF  |MARS.|
|---------|---------|---------|---------|--------|
|[Update Rollup 36](https://support.microsoft.com/en-in/help/4503156)     |   5.1.4150.0  |  9.24.5211.1   |  5.1.4150.0  | 2.0.9160.0
|[Update Rollup 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery)     |   5.1.4000.0  |  9.23.5163.1   |  5.1.4000.0  | 2.0.9156.0
|[Update Rollup 34](https://support.microsoft.com/en-us/help/4490016/update-rollup-34-for-azure-site-recovery) -hotfix     |   5.1.3950.0  |  9.22.5142.1   |  5.1.3950.0  | 2.0.9155.0
|[Update Rollup 33](https://support.microsoft.com/en-us/help/4489582/update-rollup-33-for-azure-site-recovery)     |   5.1.3900.0  |  9.22.5109.1   |  5.1.3900.0  | 2.0.9155.0
|[Update Rollup 32](https://support.microsoft.com/en-us/help/4485985/update-rollup-32-for-azure-site-recovery)     |   5.1.3800.0  |  9.21.5091.1   |  5.1.3800.0  |2.0.9144.0
|[Update Rollup 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)     |     5.1.3700.0      |   9.20.5051.1      |     5.1.3700.0    |2.0.9144.0

## <a name="previous-update-rollups"></a>Vorige updatepakketten

- [Update Rollup 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30)
- [Update Rollup 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery)
- [Update Rollup 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery)
- [Update Rollup 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery)
- [Update Rollup 26](https://support.microsoft.com/help/4344054/update-rollup-26-for-azure-site-recovery)  
- [Update Rollup 25](https://support.microsoft.com/help/4278275/update-rollup-25-for-azure-site-recovery) 
- [Update Rollup 23](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) 
- [Update Rollup 22](https://support.microsoft.com/help/4072852/update-rollup-22-for-azure-site-recovery) 
- [Update Rollup 21](https://support.microsoft.com/help/4051380/update-rollup-21-for-azure-site-recovery) 
- [Update Rollup 20](https://support.microsoft.com/help/4041105/update-rollup-20-for-azure-site-recovery) 
- [Update Rollup 19](https://support.microsoft.com/help/4034599/update-rollup-19-for-azure-site-recovery) 
