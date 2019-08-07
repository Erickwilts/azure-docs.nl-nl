---
title: Problemen met replicatie problemen oplossen voor nood herstel van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: In dit artikel vindt u informatie over het oplossen van problemen met veelvoorkomende replicatie problemen tijdens nood herstel van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/2/2019
ms.author: mayg
ms.openlocfilehash: 54686a96385532e17fe0ac6e59058b91b40c1342
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742572"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Replicatie problemen voor virtuele VMware-machines en fysieke servers oplossen

In dit artikel worden enkele veelvoorkomende problemen en specifieke fouten beschreven die u kunt tegen komen wanneer u on-premises virtuele VMware-machines en fysieke servers naar Azure repliceert met [site Recovery](site-recovery-overview.md).

## <a name="step-1-monitor-process-server-health"></a>Stap 1: Status van proces server bewaken

Site Recovery gebruikt de [proces server](vmware-physical-azure-config-process-server-overview.md#process-server) om gerepliceerde gegevens te ontvangen en te optimaliseren en deze naar Azure te verzenden.

We raden u aan de status van proces servers in de portal te controleren om ervoor te zorgen dat ze zijn verbonden en goed werken, en dat de replicatie wordt uitgevoerd voor de bron machines die aan de proces server zijn gekoppeld.

- [Meer informatie over](vmware-physical-azure-monitor-process-server.md) het bewaken van proces servers.
- [Best praktijken beoordelen](vmware-physical-azure-troubleshoot-process-server.md#best-practices-for-process-server-deployment)
- De status van de proces server [oplossen](vmware-physical-azure-troubleshoot-process-server.md#check-process-server-health) .

## <a name="step-2-troubleshoot-connectivity-and-replication-issues"></a>Stap 2: Verbindings-en replicatie problemen oplossen

Initiële en doorlopende replicatie fouten worden vaak veroorzaakt door verbindings problemen tussen de bron server en de proces server of tussen de proces server en Azure. 

Los deze problemen op door [verbindings-en replicatie problemen](vmware-physical-azure-troubleshoot-process-server.md#check-connectivity-and-replication)op te lossen.




## <a name="step-3-troubleshoot-source-machines-that-arent-available-for-replication"></a>Stap 3: Problemen met bron machines oplossen die niet beschikbaar zijn voor replicatie

Wanneer u de bron machine probeert te selecteren om de replicatie in te scha kelen met behulp van Site Recovery, is de computer mogelijk niet beschikbaar om een van de volgende redenen:

* **Twee virtuele machines met dezelfde exemplaar-uuid**: Als twee virtuele machines onder de vCenter dezelfde exemplaar-UUID hebben, wordt de eerste virtuele machine die is gedetecteerd door de configuratie server weer gegeven in de Azure Portal. Om dit probleem op te lossen, moet u ervoor zorgen dat er geen twee virtuele machines met dezelfde exemplaar-UUID zijn. Dit scenario wordt doorgaans weer gegeven in gevallen waarin een back-up-VM actief wordt en is aangemeld bij onze detectie records. [Raadpleeg Azure site Recovery VMware-naar-Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) voor het oplossen van problemen.
* **Onjuiste vCenter-gebruikers referenties**: Zorg ervoor dat u de juiste vCenter-referenties hebt toegevoegd tijdens het instellen van de configuratie server met behulp van de OVF-sjabloon of Unified Setup. Zie [referenties wijzigen voor automatische detectie](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery)om de referenties te controleren die u tijdens de installatie hebt toegevoegd.
* **vCenter onvoldoende bevoegdheden**: Als de machtigingen voor toegang tot vCenter niet over de vereiste machtigingen beschikken, kunnen de virtuele machines niet worden gedetecteerd. Zorg ervoor dat de machtigingen die zijn beschreven in [een account voorbereiden voor automatische detectie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) worden toegevoegd aan het vCenter-gebruikers account.
* **Azure site Recovery beheerser vers**: Als de virtuele machine wordt gebruikt als beheer server onder een of meer van de volgende rollen: Configuratie server/scale-out process server/Master doel server, dan kunt u de virtuele machine niet kiezen uit de portal. Beheer servers kunnen niet worden gerepliceerd.
* **Al beveiligd/mislukt via Azure site Recovery Services**: Als de virtuele machine al wordt beveiligd of mislukt via Site Recovery, is de virtuele machine niet beschikbaar om te selecteren voor beveiliging in de portal. Zorg ervoor dat de virtuele machine waarnaar u zoekt in de portal niet al wordt beveiligd door een andere gebruiker of een ander abonnement.
* **vCenter niet verbonden**: Controleer of de status van de vCenter is verbonden. Als u wilt controleren, gaat u naar Recovery Services kluis > Site Recovery infra structuur > configuratie servers > klikt u op de betreffende configuratie server > een Blade aan de rechter kant wordt geopend met de details van de bijbehorende servers. Controleer of vCenter is verbonden. Als de status niet verbonden is, lost u het probleem op en vernieuwt u vervolgens [de configuratie server](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Hierna wordt de virtuele machine in de portal weer gegeven.
* **ESXi uitgeschakeld**: Als de ESXi-host waaronder de virtuele machine zich bevindt, de status uitgeschakeld heeft, wordt de virtuele machine niet weer gegeven of kan deze niet worden geselecteerd op de Azure Portal. Schakel de ESXi-host in, [Vernieuw de configuratie server](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Hierna wordt de virtuele machine in de portal weer gegeven.
* **Opnieuw opstarten in behandeling**: Als het opnieuw opstarten in behandeling is op de virtuele machine, kunt u de machine niet selecteren op Azure Portal. Zorg ervoor dat u de in behandeling zijnde activiteiten voor opnieuw opstarten voltooit. [Vernieuw de configuratie server](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Hierna wordt de virtuele machine in de portal weer gegeven.
* **IP-adres niet gevonden**: Als er geen geldig IP-adres aan de virtuele machine is gekoppeld, kunt u de machine niet selecteren op Azure Portal. Zorg ervoor dat u een geldig IP-adres aan de virtuele machine toewijst en [Vernieuw de configuratie server](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Hierna wordt de virtuele machine in de portal weer gegeven.

### <a name="troubleshoot-protected-virtual-machines-greyed-out-in-the-portal"></a>Problemen met beveiligde virtuele machines in de portal oplossen

Virtuele machines die onder Site Recovery worden gerepliceerd, zijn niet beschikbaar in de Azure Portal als er dubbele vermeldingen in het systeem zijn. Raadpleeg [Azure site Recovery VMware-naar-Azure voor meer informatie over het verwijderen van verouderde vermeldingen en het oplossen van het probleem: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="common-errors-and-solutions"></a>Veelvoorkomende fouten en oplossingen

### <a name="initial-replication-issues-error-78169"></a>Problemen met de eerste replicatie [fout 78169]

Als u ervoor hebt gezorgd dat er geen problemen zijn met de verbinding, band breedte of tijd, moet u ervoor zorgen dat:

- Er wordt geen antivirus software geblokkeerd Azure Site Recovery. [Meer](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) informatie over de uitsluitingen van mappen die zijn vereist voor Azure site Recovery.

### <a name="missing-app-consistent-recovery-points-error-78144"></a>Ontbrekende toepassings consistente herstel punten [fout 78144]

 Dit gebeurt als gevolg van problemen met de Volume Shadow Copy service (VSS). U lost dit als volgt op: 
 
- Controleer of de geïnstalleerde versie van de Azure Site Recovery-agent ten minste 9.22.2 is. 
- Controleer of de VSS-provider is geïnstalleerd als een service in Windows-Services en Controleer ook de MMC van de component service om te controleren of Azure Site Recovery VSS-provider wordt weer gegeven.
- Als de VSS-provider niet is geïnstalleerd, raadpleegt u het [artikel problemen oplossen met de installatie fout](vmware-azure-troubleshoot-push-install.md#vss-installation-failures).

- Als VSS is uitgeschakeld,
    - Controleer of het opstart type van de VSS-Provider service is ingesteld op **automatisch**.
    - Start de volgende services opnieuw:
        - VSS-service
        - Azure Site Recovery VSS-provider
        - VDS-service

- Als u SQL-of Exchange-workloads uitvoert, controleert u de logboeken van deze schrijvers van toepassingen op fouten. Regel matig voorkomende fouten en de oplossing hiervan worden vastgelegd in de volgende artikelen:
    -  [De optie voor automatisch sluiten van SQL Server Data Base is ingesteld op waar](https://support.microsoft.com/help/4504104)
    - [SQL Server 2008 R2 een niet-herstel bare fout activeren](https://support.microsoft.com/help/4504103)
    - [Bekend probleem in SQL Server 2016 en 2017](https://support.microsoft.com/help/4493364)
    - [Algemeen probleem met Exchange-servers 2013 en 2016](https://support.microsoft.com/help/4037535)


### <a name="source-machines-with-high-churn-error-78188"></a>Bron machines met hoog verloop [fout 78188]

Mogelijke oorzaken:
- De wijzigings frequentie van gegevens (geschreven bytes per seconde) op de vermelde schijven van de virtuele machine is hoger dan de [Azure site Recovery ondersteunde limieten](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) voor het type opslag account voor replicatie doel.
- Er is een plotselinge piek in het verloop percentage, omdat de hoge hoeveelheid gegevens in behandeling is voor het uploaden.

Om het probleem op te lossen:
- Zorg ervoor dat het type van het doel opslag account (Standard of Premium) is ingericht conform de vereiste voor de verloop snelheid bij de bron.
- Als u al repliceert naar een Premium Managed disk (type asrseeddisk), moet u ervoor zorgen dat de grootte van de schijf het waargenomen verloop tarief per Site Recovery limieten ondersteunt. Indien nodig kunt u de grootte van de asrseeddisk verg Roten. Volg de onderstaande stappen:
    - Navigeer naar de Blade schijven van de betrokken gerepliceerde machine en kopieer de naam van de replica schijf
    - Ga naar deze door de replica beheerde schijf
    - Mogelijk ziet u een banner op de Blade overzicht met de melding dat er een SAS-URL is gegenereerd. Klik op deze banner en Annuleer de export. Negeer deze stap als u de banner niet ziet.
    - Zodra de SAS-URL is ingetrokken, gaat u naar de Blade configuratie van de beheerde schijf en verhoogt u de grootte zodat ASR het waargenomen verloop op de bron schijf ondersteunt
- Als het waargenomen verloop tijdelijk is, wacht u een paar uur totdat de gegevens in behandeling zijn geüpload en herstel punten maken.
- Als de schijf niet-kritieke gegevens bevat zoals tijdelijke logboeken, gegevens testen, enzovoort, overweeg dan om deze gegevens elders te verplaatsen of deze schijf volledig uit te sluiten van replicatie
- Als het probleem blijft bestaan, gebruikt u de Site Recovery [Deployment planner](site-recovery-deployment-planner.md#overview) om de replicatie te plannen.

### <a name="source-machines-with-no-heartbeat-error-78174"></a>Bron machines zonder heartbeat [fout 78174]

Dit gebeurt wanneer Azure Site Recovery Mobility-agent op de bron machine niet communiceert met de configuratie server (CS).

Om het probleem op te lossen, voert u de volgende stappen uit om de netwerk verbinding van de bron-VM naar de configuratie server te controleren:

1. Controleer of de bron machine wordt uitgevoerd.
2. Meld u aan bij de bron machine met een account met beheerders bevoegdheden.
3. Controleer of de volgende services worden uitgevoerd en of de services niet opnieuw worden gestart:
   - Svagents (inmage Scout-agent)
   - Inmage Scout-toepassings service
4. Controleer op de bron computer de logboeken op de locatie voor fout Details:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="process-server-with-no-heartbeat-error-806"></a>Proces server zonder heartbeat [fout 806]
Als er geen heartbeat van de proces server (PS) is, controleert u het volgende:
1. PS-VM is actief
2. Raadpleeg de volgende logboeken op de PS voor fout Details:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="master-target-server-with-no-heartbeat-error-78022"></a>Master doel server zonder heartbeat [fout 78022]

Dit gebeurt wanneer Azure Site Recovery Mobility-agent op het hoofd doel niet communiceert met de configuratie server.

U kunt het probleem oplossen door de volgende stappen uit te voeren om de status van de service te controleren:

1. Controleer of de virtuele Master doel-VM wordt uitgevoerd.
2. Meld u aan bij de Master doel-VM met een account met beheerders bevoegdheden.
    - Controleer of de svagents-service wordt uitgevoerd. Als deze wordt uitgevoerd, start u de service opnieuw
    - Raadpleeg de logboeken op de locatie voor de fout Details:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log

## <a name="error-id-78144---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Fout-ID 78144-geen app-consistent herstel punt beschikbaar voor de virtuele machine in de afgelopen ' XXX ' minuten

Hieronder vindt u enkele van de meest voorkomende problemen

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>Oorzaak 1: Bekend probleem in SQL Server 2008/2008 R2 
**Oplossen** : Er is een bekend probleem met SQL Server 2008/2008 R2. Raadpleeg dit KB-artikel [Azure site Recovery agent of andere VSS-back-ups van niet-onderdeel zijn mislukt voor een server die als host fungeert voor SQL Server 2008 R2](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-auto_close-dbs"></a>Oorzaak 2: Azure Site Recovery-taken mislukken op servers die fungeren als host voor elke versie van SQL Server instanties met AUTO_SLUITEN Db's 
**Oplossen** : Raadpleeg het KB- [artikel](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) 


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>Oorzaak 3: Bekend probleem in SQL Server 2016 en 2017
**Oplossen** : Raadpleeg het KB- [artikel](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) 


### <a name="more-causes-due-to-vss-related-issues"></a>Meer oorzaken vanwege VSS-problemen:

Als u verder wilt oplossen, controleert u de bestanden op de bron computer om de exacte fout code op te halen voor de fout:
    
    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Hoe kunt u de fouten in het bestand vinden?
Zoek naar de teken reeks ' vacpError ' door het bestand vacp. log te openen in een editor
        
    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

In bovenstaand voor beeld **2147754994** is de fout code die u vertelt over de fout, zoals hieronder wordt weer gegeven

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS Writer is niet geïnstalleerd-fout 2147221164 

*Oplossen*: Azure Site Recovery maakt gebruik van micro soft Volume Shadow Copy service (VSS) om een toepassings consistentie code te genereren. Er wordt een VSS-provider geïnstalleerd voor de werking ervan om app-consistentie momentopnamen te maken. Deze VSS-provider is geïnstalleerd als een service. Als de VSS-Provider service niet is geïnstalleerd, mislukt het maken van de toepassings consistentie van de moment opname met de fout-id 0x80040154 ' klasse is niet geregistreerd '. </br>
Raadpleeg het [artikel over het oplossen van problemen met de VSS Writer-installatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS Writer is uitgeschakeld-fout 2147943458

**Oplossen**: Azure Site Recovery maakt gebruik van micro soft Volume Shadow Copy service (VSS) om een toepassings consistentie code te genereren. Er wordt een VSS-provider geïnstalleerd voor de werking ervan om app-consistentie momentopnamen te maken. Deze VSS-provider is geïnstalleerd als een service. Als de VSS-Provider service is uitgeschakeld, mislukt het maken van de moment opname van de toepassings consistentie met de fout-id. de opgegeven service is uitgeschakeld en kan niet worden gestart (0x80070422). </br>

- Als VSS is uitgeschakeld,
    - Controleer of het opstart type van de VSS-Provider service is ingesteld op **automatisch**.
    - Start de volgende services opnieuw:
        - VSS-service
        - Azure Site Recovery VSS-provider
        - VDS-service

####  <a name="vss-provider-not_registered---error-2147754756"></a>VSS-PROVIDER NOT_REGISTERED-fout 2147754756

**Oplossen**: Azure Site Recovery maakt gebruik van micro soft Volume Shadow Copy service (VSS) om een toepassings consistentie code te genereren. Controleer of de Azure Site Recovery VSS-Provider service is geïnstalleerd of niet. </br>

- Voer de installatie van de provider opnieuw uit met de volgende opdrachten:
- Bestaande provider verwijderen: C:\Program Files (x86) \Microsoft Azure site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Vervolgens C:\Program Files (x86) \Microsoft Azure site Recovery\agent\InMageVSSProvider_Install.cmd
 
Controleer of het opstart type van de VSS-Provider service is ingesteld op **automatisch**.
    - Start de volgende services opnieuw:
        - VSS-service
        - Azure Site Recovery VSS-provider
        - VDS-service

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, kunt u uw vraag in het [Azure site Recovery forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)plaatsen. We hebben een actieve community en een van onze technici kan u helpen.
